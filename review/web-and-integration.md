# Ревью Java: web, интеграции и границы доверия

<!-- learning-nav:start -->
[← Все разделы](../README.md) · [Предыдущая тема](concurrency-and-data.md) · [Следующая тема →](../reasoning-questions.md)
<!-- learning-nav:end -->

Двенадцать задач проверяют способность восстановить контракт и закрыть конкретное окно ошибки. Сначала прочитайте условие и предложите исправление, затем откройте разбор. База — Java 17, Spring Boot 3.x / Spring Framework 6.x; в JWT-примере — Spring Security 6.5, в Kafka-примере — Spring Kafka 3.3. Учебные фрагменты с прикладными `repository`, DTO и сервисами не являются готовым приложением: их зависимости и контракты описаны рядом. Самодостаточные JDK-методы можно перенести в класс с указанными imports.

Методика ревью — в [основном разделе](../code-review.md), теория защиты — в [безопасности backend](../application-security.md), гарантии распределённых операций — в [микросервисах](../microservices.md). Здесь требуется показать ошибочный сценарий, рабочее изменение и проверку границы гарантии.

## Оглавление

<!-- toc:start -->
<details>
<summary>Показать темы и вопросы</summary>

- [Доступ и доверие к запросу](#доступ-и-доверие-к-запросу)
  - [1. Кто может удалить пакет черновиков?](#1-кто-может-удалить-пакет-черновиков)
  - [2. Какие ответы сервиса отчётов обработает клиент?](#2-какие-ответы-сервиса-отчётов-обработает-клиент)
  - [3. Какие токены примет платёжный API?](#3-какие-токены-примет-платёжный-api)
  - [4. Когда можно подтвердить уведомление о начислении бонуса?](#4-когда-можно-подтвердить-уведомление-о-начислении-бонуса)
- [Данные, файлы и диагностика](#данные-файлы-и-диагностика)
  - [5. Какие данные сохранятся в журнале входа?](#5-какие-данные-сохранятся-в-журнале-входа)
  - [6. Как загрузить файл с лимитом в пять мегабайт?](#6-как-загрузить-файл-с-лимитом-в-пять-мегабайт)
  - [7. Что проверить перед публикацией файлов из ZIP?](#7-что-проверить-перед-публикацией-файлов-из-zip)
  - [8. Что увидит клиент после ошибки регистрации?](#8-что-увидит-клиент-после-ошибки-регистрации)
- [Завершение операций и фоновые задачи](#завершение-операций-и-фоновые-задачи)
  - [9. Как повторить платёж после таймаута?](#9-как-повторить-платёж-после-таймаута)
  - [10. В какой момент подтверждать обработку сообщения Kafka?](#10-в-какой-момент-подтверждать-обработку-сообщения-kafka)
  - [11. Чьи права учитывать при отложенном экспорте?](#11-чьи-права-учитывать-при-отложенном-экспорте)
  - [12. Когда сохранятся отметка об оплате и запись аудита?](#12-когда-сохранятся-отметка-об-оплате-и-запись-аудита)

</details>
<!-- toc:end -->

## Доступ и доверие к запросу

<a id="1-массовое-удаление-проверяет-только-первый-документ"></a>

### 1. Кто может удалить пакет черновиков?

Контракт: пользователь удаляет от 1 до 100 своих черновиков внутри активного tenant. Если хотя бы один ID недоступен, не удаляется ничего; клиент получает одинаковый 404 для отсутствующего и чужого документа. Tenant и actor берутся из проверенного principal, права на членство в tenant уже проверены.

```java
@Transactional
public void delete(List<Long> ids, Actor actor) {
    var first = repository.findById(ids.get(0)).orElseThrow();
    if (!first.getOwnerId().equals(actor.id())) throw new Forbidden();
    repository.deleteAllByIdInBatch(ids);
}
```

Сравните запросы со своим, чужим и отсутствующим ID. Какие изменения останутся после ответа в каждом случае?

<details>
<summary>Разбор</summary>

Проверен один объект, а операция затрагивает всё множество. Даже проверка tenant первого документа отсутствует. Аннотация транзакции обеспечивает атомарность выполненных изменений, но не добавляет авторизацию к repository-методу. Перестановка ID меняет результат проверки при неизменном намерении пользователя. UUID затрудняет угадывание, однако чужой ID может быть известен из ссылки.

Достаточно одного условного удаления и проверки числа строк. Ниже SQL для PostgreSQL 16 и схема сервисного метода; `deleteOwnedDrafts` возвращает update count, параметры связываются, список не конкатенируется в SQL:

```sql
DELETE FROM document
WHERE id = ANY (:ids)
  AND tenant_id = :tenant
  AND owner_id = :actor
  AND status = 'DRAFT';
```

```java
@Transactional
public void delete(List<Long> ids, Actor actor) {
    if (ids.isEmpty() || ids.size() > 100 || ids.contains(null)) {
        throw new BadRequest();
    }
    Set<Long> distinct = Set.copyOf(ids);
    int removed = repository.deleteOwnedDrafts(
            distinct, actor.tenantId(), actor.id());
    if (removed != distinct.size()) throw new NotFound();
}
```

`NotFound` здесь — unchecked-исключение, выходящее за границу Spring proxy: оно откатывает уже удалённые свои строки. Дубли ID разрешены и означают одно удаление. Нельзя поймать исключение внутри этого метода и нормально вернуться: тогда частичное удаление подтвердится. Внешний advice преобразует ошибку после rollback в согласованный 404.

Интеграционная проверка вызывает реальный Spring-бин и читает БД из новой транзакции: свой плюс чужой, свой плюс отсутствующий, другой tenant, опубликованный документ, дубли и параллельная публикация. После отказа свой документ должен сохраниться. Это модель «только владелец»; общий доступ и делегирование требуют дополнительного предиката политики. При JPA bulk SQL также учитывают устаревший persistence context.

</details>

<a id="2-проверенный-http-адрес-перенаправляет-во-внутреннюю-сеть"></a>

### 2. Какие ответы сервиса отчётов обработает клиент?

Контракт: сервис получает PDF только из `https://reports.partner.example:443`; партнёрские redirect не поддерживаются. Пользователь присылает URL. Секретный заголовок добавляет backend. Инфраструктурная egress-политика разрешает клиенту соединяться только с партнёром, DNS и TLS управляются доверенной конфигурацией.

```java
URI uri = URI.create(input);
if (!uri.getHost().endsWith("partner.example")) throw new BadRequest();
HttpClient client = HttpClient.newBuilder()
        .followRedirects(HttpClient.Redirect.ALWAYS).build();
return client.send(HttpRequest.newBuilder(uri)
        .header("Authorization", "Bearer " + secret).build(),
        HttpResponse.BodyHandlers.ofByteArray()).body();
```

Пройдите обычную загрузку и ответ 302. Каким должен быть итог для каждого варианта по контракту?

<details>
<summary>Разбор</summary>

Суффикс допускает посторонние имена, а scheme и port не проверяются. Главное окно — смена адреса после проверки: redirect на loopback или metadata endpoint вызывает уже HTTP-клиент. Нельзя рассчитывать, что правила переноса `Authorization` конкретной реализации предотвратят сам доступ во внутреннюю сеть.

Для этого контракта пользовательский URL вообще не нужен: принимаем UUID отчёта и строим адрес из фиксированного origin. Исправленный JDK-фрагмент использует `UUID reportId`, `String secret`, imports из `java.net`, `java.net.http`, `java.io`, `java.time`:

```java
// Клиент создаётся один раз на жизненный цикл приложения.
HttpClient client = HttpClient.newBuilder()
        .followRedirects(HttpClient.Redirect.NEVER)
        .connectTimeout(Duration.ofSeconds(2)).build();
URI uri = URI.create("https://reports.partner.example/reports/"
        + reportId + ".pdf");
HttpRequest request = HttpRequest.newBuilder(uri)
        .timeout(Duration.ofSeconds(3))
        .header("Authorization", "Bearer " + secret)
        .GET().build();
HttpResponse<InputStream> response = client.send(
        request, HttpResponse.BodyHandlers.ofInputStream());
try (InputStream body = response.body()) {
    if (response.statusCode() != 200) {
        throw new IOException("Unexpected partner status");
    }
    // Ограничить поток байтов при записи; см. задачу 6.
    consumeBoundedPdf(body);
}
```

`consumeBoundedPdf` — прикладная функция с лимитом размера и времени чтения, здесь не реализована. Request timeout с потоковым body не следует считать полной гарантией срока чтения всего файла: для него нужен отдельный deadline/поддержка транспорта. Даже тело ошибки закрывается, redirect не выполняется.

Проверяют на заглушке транспорта переход к категории `UNLISTED_DESTINATION` и отсутствие следующего обращения; отдельно — лимит байтов и deadline на небольших синтетических потоках. Если произвольные внешние URL действительно обязательны, этого решения мало: нужны политика адресов всех переходов, защита от DNS rebinding и контроль фактического egress. Одиночный `InetAddress.getByName` перед новым DNS-разрешением клиента оставляет гонку.

</details>

<a id="3-новый-audience-validator-отключил-проверку-issuer"></a>

### 3. Какие токены примет платёжный API?

Контракт ресурса: принимать только RS256 access tokens доверенного issuer `https://id.example`, с audience `billing-api`, обязательными `exp` и `token_use=access`. Доверенный JWKS задан конфигурацией, ID tokens имеют `token_use=id`. Проверки ролей выполняются позже, отдельно от проверки токена.

```java
NimbusJwtDecoder decoder = NimbusJwtDecoder
        .withJwkSetUri(jwksFromConfiguration).build();
decoder.setJwtValidator(new JwtClaimValidator<List<String>>(
        "aud", aud -> aud != null && aud.contains("billing-api")));
```

Придумайте минимальный набор тестовых токенов, который проверит все требования к ресурсу. Какие из них пропустит эта конфигурация?

<details>
<summary>Разбор</summary>

Setter заменяет набор validators. Разработчик добавил audience, но потерял стандартную проверку времени и не проверяет ожидаемый issuer. Общий ключ подписи не доказывает назначение токена: подписанный токен другого API или другого профиля может быть криптографически корректен. Подмена токена не требует подделывать подпись.

Минимальная композиция для Spring Security 6.5; imports находятся в `oauth2.jwt`, `oauth2.core`, `oauth2.jose.jws`, `java.time` и `java.util`:

```java
NimbusJwtDecoder decoder = NimbusJwtDecoder
        .withJwkSetUri(jwksFromConfiguration)
        .jwsAlgorithms(algorithms -> {
            algorithms.clear();
            algorithms.add(SignatureAlgorithm.RS256);
        }).build();
OAuth2TokenValidator<Jwt> defaults =
        JwtValidators.createDefaultWithIssuer("https://id.example");
decoder.setJwtValidator(new DelegatingOAuth2TokenValidator<>(
        defaults,
        new JwtClaimValidator<List<String>>("aud",
                aud -> aud != null && aud.contains("billing-api")),
        new JwtClaimValidator<Instant>("exp", Objects::nonNull),
        new JwtClaimValidator<String>("token_use", "access"::equals)));
```

Это конфигурационный фрагмент для bean `JwtDecoder`, подключённого к resource-server filter chain, а не самостоятельный authentication filter. Default validators сохраняют проверки `exp`/`nbf` с допустимым clock skew; отдельный validator требует наличия `exp`. `token_use` — условие этого issuer, не универсальный claim OAuth. Если issuer различает профили по `typ`, необходимо валидировать именно согласованный профиль вместо придуманного поля.

Проверки выполняют настоящим decoder над подписанными тестовыми токенами: неверные issuer, audience, ключ, алгоритм, истёкший срок, будущее `nbf`, отсутствующий `exp`, ID token. Один положительный сценарий не обнаруживает замену validators. Малформированный тип claim тоже должен приводить к отказу. Сетевые ошибки JWKS требуют контролируемой обработки и ротации, но не разрешения токена «на время сбоя». После проверки JWT всё ещё нужна объектная авторизация из задачи 1.

</details>

<a id="4-правильная-подпись-webhook-не-мешает-повторному-начислению"></a>

### 4. Когда можно подтвердить уведомление о начислении бонуса?

Контракт собственного протокола: подписываются ASCII timestamp в секундах, точка и исходные байты JSON. Допуск времени — ±300 секунд; `eventId` внутри подписанного JSON уникален у провайдера. Доставка повторяется семь дней, бонус должен начислиться один раз. Это учебный протокол, а не реализация конкретного платёжного SDK.

```java
String canonical = mapper.writeValueAsString(parsedRequest);
if (hmac(secret, canonical).equals(signature)) {
    bonuses.add(parsedRequest.customerId(), parsedRequest.amount());
}
```

Разберите исходную доставку, повтор и падение между приёмом и фактическим начислением. Какой ответ получит отправитель?

<details>
<summary>Разбор</summary>

Порядок полей, пробелы и escape-последовательности меняют байты, даже если данные эквивалентны. Timestamp тоже должен быть подписан: иначе отправитель повтора заменит только время. Подлинность сообщения не делает его новым. Для чужого провайдера используют его SDK и точную спецификацию, не подгоняют подпись собственной нормализацией JSON.

JDK 17-фрагмент проверки при заранее ограниченном body и заголовках; imports: `javax.crypto.*`, `javax.crypto.spec.SecretKeySpec`, `java.security.*`, `java.nio.charset.StandardCharsets`, `java.time.*`, `java.util.HexFormat`:

```java
long seconds = Long.parseLong(timestamp); // ошибки парсинга -> отказ
Instant now = clock.instant();
Instant sent = Instant.ofEpochSecond(seconds);
if (sent.isBefore(now.minusSeconds(300))
        || sent.isAfter(now.plusSeconds(300))) throw new SecurityException();
Mac mac = Mac.getInstance("HmacSHA256");
mac.init(new SecretKeySpec(secretBytes, "HmacSHA256"));
mac.update((timestamp + ".").getBytes(StandardCharsets.US_ASCII));
byte[] expected = mac.doFinal(rawBody);
byte[] actual = HexFormat.of().parseHex(signature);
if (actual.length != 32 || !MessageDigest.isEqual(expected, actual)) {
    throw new SecurityException();
}
```

Только после проверки разбирают JSON, валидируют бизнес-поля и сохраняют событие в inbox. Схема транзакции PostgreSQL 16:

```sql
INSERT INTO webhook_inbox(provider, event_id, payload)
VALUES (:provider, :signed_event_id, :verified_payload)
ON CONFLICT (provider, event_id) DO NOTHING;
-- UNIQUE(provider, event_id); commit до HTTP 2xx.
```

Worker атомарно начисляет бонус и отмечает inbox обработанным; при падении повторяет. Таблица сохраняет дедупликацию минимум согласованные семь дней. Для повторного ID с отличающимся содержимым нужна диагностика конфликта, а не второе начисление. Timestamp ограничивает старые захваченные доставки; inbox подавляет повторы внутри окна и новые подписанные попытки провайдера.

Проверяют исходный body, изменение одного байта, старый и будущий timestamp, два одновременных события с одним ID, падение после inbox commit и до начисления. Нельзя вернуть 2xx сразу после помещения в память executor: процесс потеряет принятую работу.

</details>

## Данные, файлы и диагностика

<a id="5-маскирование-dto-не-спасло-секрет-в-исключении-клиента"></a>

### 5. Какие данные сохранятся в журнале входа?

Контракт: в логи интеграции разрешены серверный operation ID, статус партнёра, длительность и код исхода. Пароль, bearer token, URL с query и response body запрещены. У `LoginRequest.toString()` уже скрыт password. HTTP-клиент при ошибке включает URI и body в текст исключения.

```java
try {
    client.login(request);
} catch (PartnerHttpException ex) {
    log.warn("Login failed request={} reason={}", request, ex.getMessage());
    throw new GatewayException(ex);
}
```

Проверьте журнал при успешном логине и при ошибке партнёра. Какие диагностические данные нужны в этих двух случаях?

<details>
<summary>Разбор</summary>

Секрет проходит по независимому каналу: `ex.getMessage()` и цепочке cause. Маскирование DTO не меняет текст исключения клиента. Оборачивание с cause сохраняет исходный body; глобальный logger или APM позже выгрузит его целиком. Логирование уровня DEBUG тоже остаётся логированием с копиями в сборщиках и архиве.

Исправление делают на границе адаптера. Ниже прикладной Java-фрагмент: `statusCode()` возвращает только числовой HTTP-статус, `operationId` создан сервером, `GatewayFailure` содержит лишь безопасные поля и не сохраняет исходный cause:

```java
try {
    client.login(request);
} catch (PartnerHttpException ex) {
    int status = ex.statusCode();
    log.warn("partner_login_failed operationId={} status={} kind={}",
            operationId, status, "HTTP_ERROR");
    throw new GatewayFailure(operationId, status);
} catch (PartnerTimeoutException ex) {
    log.warn("partner_login_failed operationId={} kind={}",
            operationId, "TIMEOUT");
    throw new GatewayFailure(operationId, null);
}
```

Потеря исходного сообщения здесь осознанная: вместо него адаптер сохраняет тип безопасного исхода. Если нужна отдельная защищённая диагностика, её формат, доступ и редактирование проектируются явно; обычный error log не становится таким хранилищем автоматически. Лучше также убрать credentials из URL и отключить body/wire logging в клиенте, interceptors и tracing. Одного локального `catch` мало, если interceptor записывает запрос до него.

Проверка использует уникальные фиктивные маркеры секрета в password, header, query и ошибочном body. Через реальный logging appender и обработчик ошибки проверяют отсутствие всех маркеров во всём захваченном выводе, включая stack traces. Дополнительно проверяют метрику отказа и доступность operation ID для расследования. Нельзя «исправить» тест одним regex по слову password: секрет может лежать в любом поле и в произвольном тексте партнёра. Разрешающий набор полей устойчивее запретного списка названий.

</details>

<a id="6-upload-проверил-размер-после-выделения-памяти"></a>

### 6. Как загрузить файл с лимитом в пять мегабайт?

Контракт: загрузка до 5 MiB сохраняется в приватный staging-каталог; после превышения лимита не должно оставаться частичного файла. Каталог заранее создан и доступен на запись только сервису. Имя клиента используется только как метаданные, входной `InputStream` закрывает вызывающий код.

```java
byte[] bytes = input.readAllBytes();
if (bytes.length > 5 * 1024 * 1024) throw new TooLarge();
Files.write(root.resolve(originalFilename), bytes);
```

Оцените память и состояние каталога для пустого файла, файла ровно 5 MiB и потока большей длины без известного размера.

<details>
<summary>Разбор</summary>

Память уже выделена до проверки. Несколько одновременных потоков способны исчерпать heap раньше отказа. Отдельная проблема — путь из имени пользователя: абсолютный путь и переход к родительскому каталогу могут вывести запись за root. Сокращение имени до basename устраняет часть вариантов, но не даёт уникальности и не определяет поведение при совпадении имён.

Метод для Java 17, imports: `java.io.*`, `java.nio.file.*`. Он генерирует имя, закрывает output и удаляет файл после неуспеха:

```java
static Path store(InputStream input, Path root) throws IOException {
    long limit = 5L * 1024 * 1024;
    Path file = Files.createTempFile(root, "upload-", ".part");
    boolean complete = false;
    try {
        try (OutputStream output = Files.newOutputStream(file)) {
            byte[] buffer = new byte[8192];
            long total = 0;
            for (int n; (n = input.read(buffer)) != -1;) {
                if (n > limit - total) throw new IOException("Upload too large");
                output.write(buffer, 0, n);
                total += n;
            }
        }
        complete = true;
        return file;
    } finally {
        if (!complete) Files.deleteIfExists(file);
    }
}
```

Флаг ставится после успешного `close()`: ошибка закрытия тоже удаляет неполный результат. Если удаление само завершится ошибкой, требуется эксплуатационная уборка и сигнал о сбое; недоступный диск не позволяет обещать физическое удаление при любых обстоятельствах. Сохранённый файл ещё не опубликован: затем нужны проверка формата и содержимого, привязка владельца и перевод из staging в доступное состояние.

Этот метод ограничивает одну запись, а не multipart parser до вызова: в Spring Boot 3.x дополнительно задают `spring.servlet.multipart.max-file-size` и `spring.servlet.multipart.max-request-size`, лимиты proxy, число одновременных загрузок и дисковую квоту. Медленный бесконечный источник требует deadline чтения транспортного уровня. Проверяют пустой поток, ровно лимит, лимит плюс байт, исключение input и невозможность записи; после отказа staging пуст. Не следует объявлять файл безопасным лишь потому, что он помещается в 5 MiB.

</details>

<a id="7-распаковка-проверяет-путь-строковым-префиксом"></a>

### 7. Что проверить перед публикацией файлов из ZIP?

Контракт ZIP-импорта: максимум 100 обычных файлов, 20 MiB суммарно распакованных данных всех entries, включая данные directory entries. Каждый импорт получает новый приватный каталог без symlinks, конкурирующих писателей и вложенных архивов. При отказе весь каталог удаляется, ни одна запись не публикуется. Путь root — `/srv/import`, имя ZIP-entry недоверенное.

```java
Path target = root.resolve(entry.getName()).normalize();
if (!target.toString().startsWith(root.toString())) throw new BadArchive();
Files.copy(zip, target, StandardCopyOption.REPLACE_EXISTING);
```

Проверьте расположение результата и расход ресурсов для архива в пределах лимита и при превышении каждой установленной границы.

<details>
<summary>Разбор</summary>

Строковый префикс не сравнивает компоненты пути: соседний каталог может иметь похожее имя, не находясь внутри разрешённого корня. `Path.startsWith` решает именно эту ошибку, но лексическое сравнение не отслеживает symlinks на диске. Поэтому условие приватного нового каталога и отсутствие внешнего писателя — часть решения, а не мелкая оговорка.

Ниже псевдокод Java 17: цикл, helpers лимита и рекурсивной уборки сокращены, фрагмент сам по себе не runnable. `root` заранее приводится к абсолютному нормализованному пути:

```java
long remaining = 20L * 1024 * 1024;
int files = 0;
int entries = 0;
for (ZipEntry entry; (entry = zip.getNextEntry()) != null;) {
    if (++entries > 200 || entry.getName().length() > 1024) {
        throw new BadArchive();
    }
    Path relative = Path.of(entry.getName());
    Path target = root.resolve(relative).normalize();
    if (relative.isAbsolute() || !target.startsWith(root)
            || target.equals(root) || relative.getNameCount() > 20) {
        throw new BadArchive();
    }
    if (entry.isDirectory()) {
        remaining -= copyAtMost(zip, OutputStream.nullOutputStream(), remaining);
        continue; // entry дочитана с лимитом; каталоги создаём для файлов
    }
    if (++files > 100) throw new BadArchive();
    Files.createDirectories(target.getParent());
    try (OutputStream out = Files.newOutputStream(target,
            StandardOpenOption.CREATE_NEW, StandardOpenOption.WRITE)) {
        long written = copyAtMost(zip, out, remaining);
        remaining -= written;
    }
}
```

`copyAtMost` считает реальные распакованные байты, бросает исключение при первом лишнем байте и не закрывает общий ZIP stream; `entry.getSize()` не является проверкой. Directory entry тоже может содержать данные: их ограниченно читают в `nullOutputStream` и вычитают из общего бюджета. Простой `continue` опасен: следующий `getNextEntry()` вызывает `closeEntry()`, дочитывающий предыдущую entry без нашего счётчика. При превышении лимита импорт немедленно прекращают и закрывают сам stream, не пытаясь дочитать entry.

`CREATE_NEW` запрещает перезапись пути и повтор файла. Снаружи `try/finally` удаляет дерево при отказе; публикация разрешена после полного успеха. Лимиты entries и путей защищают от множества пустых каталогов; deadline ограничивает время.

Обычный `ZipInputStream` здесь пишет обычные файлы, не восстанавливая symlinks. При смене extractor это условие перепроверяют. Проверки политики используют категории `INSIDE_ROOT`, `OUTSIDE_ROOT`, `DUPLICATE_FILE`. На небольших безопасных данных и искусственно сниженном бюджете проверяют общий счётчик directory+file, точную границу, отказ при её превышении и предел числа entries. На разных ОС правила пути отличаются; тесты выполняют на целевой платформе.

</details>

<a id="8-ошибка-валидации-вернула-пароль-через-rejectedvalue"></a>

### 8. Что увидит клиент после ошибки регистрации?

Контракт регистрации: HTTP 400 содержит только стабильный код ошибки, имя публичного поля и случайный request ID. Значения password, reset token и внутренние сообщения валидатора не возвращаются. Разработчик добавил общий Spring MVC handler, чтобы UI мог подсвечивать неверные поля.

```java
@ExceptionHandler(MethodArgumentNotValidException.class)
ResponseEntity<?> invalid(MethodArgumentNotValidException ex) {
    return ResponseEntity.badRequest().body(ex.getBindingResult().getFieldErrors());
}
```

Сформулируйте ожидаемое тело ответа при отклонённом пароле и сравните с тем, что вернёт handler.

<details>
<summary>Разбор</summary>

`FieldError` — внутренний диагностический объект. Он содержит rejected value и дополнительные сведения, а не только пару field/message. Сериализация всего объекта отдаёт чувствительное значение обратно через error path, даже если успешный response DTO уже безопасен. Ответ могут дополнительно сохранить браузерная аналитика, gateway и системы диагностики клиента.

Нужна явная проекция. Ниже фрагмент Spring Framework 6.x / Boot 3.x; records являются DTO ответа, `MethodArgumentNotValidException` и аннотация импортируются из Spring MVC:

```java
record Violation(String field, String code) {}
record InvalidRequest(String code, String requestId,
                      List<Violation> violations) {}

@ExceptionHandler(MethodArgumentNotValidException.class)
ResponseEntity<InvalidRequest> invalid(MethodArgumentNotValidException ex) {
    Set<String> publicFields = Set.of("email", "password", "displayName");
    List<Violation> errors = ex.getBindingResult().getFieldErrors().stream()
            .filter(error -> publicFields.contains(error.getField()))
            .map(error -> new Violation(error.getField(), "INVALID_VALUE"))
            .distinct().limit(20).toList();
    return ResponseEntity.badRequest().body(new InvalidRequest(
            "VALIDATION_FAILED", UUID.randomUUID().toString(), errors));
}
```

Код ошибки здесь намеренно общий: более точные публичные коды можно получить разрешающим отображением типов constraint, не пересылкой `getDefaultMessage()`. Динамические пути полей массивов и map keys также недоверенны; их надо нормализовать в публичные имена либо исключать. Для общей 500-ошибки применяют тот же принцип: фиксированное описание, request ID, без exception message, SQL и внутренних URI.

Проверяют HTTP-ответ сериализованного advice, а не только объект до Jackson: невалидный длинный password с уникальным маркером, ошибка вложенного поля, class-level constraint, неизвестное поле и сбой БД. Тело и заголовки не должны содержать маркер или stack trace; request ID должен связываться с безопасной диагностикой. Этот handler не обрабатывает автоматически ошибки Spring Security filter chain: authentication entry point и access denied handler настраивают отдельно. Корректный статус помогает клиенту и мониторингу, но приватность определяется содержимым ответа.

</details>

## Завершение операций и фоновые задачи

<a id="9-retry-создаёт-новый-ключ-внутри-каждой-попытки"></a>

### 9. Как повторить платёж после таймаута?

Контракт провайдера: тот же ключ с тем же телом возвращает прежний платёж в течение семи дней; ключ с другим телом даёт конфликт. Локальный payment intent создан до вызова, хранится устойчиво. При timeout разрешено повторить тот же intent; новая покупка создаёт другой intent. Используется блокирующий `Retry.executeSupplier` Resilience4j 2.x.

```java
return retry.executeSupplier(() -> {
    String key = UUID.randomUUID().toString();
    PaymentBody body = new PaymentBody(cart.currentTotal(), Instant.now());
    return provider.charge(key, body);
});
```

Пройдите таймаут, рестарт приложения и повтор запроса. Сколько списаний допускает контракт и сколько допускает код?

<details>
<summary>Разбор</summary>

Supplier исполняется заново на каждой попытке, поэтому повтор превращается в новую операцию у провайдера. Даже при стабильном ключе заново собранное тело меняет timestamp и сумму: это уже другой запрос и конфликт контракта. Исправление должно зафиксировать идентичность и данные до первого сетевого эффекта, а не только до первого вызова данной JVM-функции.

Прикладной фрагмент: `loadAuthorized` проверяет принадлежность intent пользователю/tenant; intent хранит ключ и неизменяемый снимок параметров. `PaymentBody` — record с immutable-компонентами, клиент не мутирует объект:

```java
PaymentIntent intent = intents.loadAuthorized(intentId, actor);
String key = intent.providerKey();
PaymentBody body = new PaymentBody(intent.amountMinor(), intent.currency(),
        intent.merchantReference(), intent.createdAt());
return retry.executeSupplier(() -> provider.charge(key, body));
```

Загрузка и создание intent происходят в коротких локальных транзакциях, сетевой retry — вне них. Если протокол подписывает точные байты, а не семантические поля, сохраняют и повторно используют именно сериализованный body. После рестарта читают тот же intent; новый UUID при восстановлении не создают. При истечении семидневного окна автоматический повтор может быть небезопасен: нужна сверка статуса, а не бесконечный retry.

Retry-политика отдельно ограничивает попытки и общий deadline, разрешает только согласованные временные сбои. Ошибки валидации, конфликт тела и бизнес-отказ не становятся успешными от повторов. Исчерпание retry после timeout не доказывает отсутствие списания: intent остаётся с неизвестным исходом до reconciliation. `@Retry` также не обеспечивает дедупликацию входящих HTTP-запросов — они должны ссылаться на один intent.

Проверка провайдера-заглушки: первая попытка фиксирует платёж и теряет ответ; вторая должна получить ровно те же key и body. Затем перезапуск обработчика и изменение корзины: сохранённый intent всё равно повторяется без изменений. Проверяют один эффект, а не один вызов: два сетевых обращения допустимы.

</details>

<a id="10-kafka-listener-подтвердил-передачу-работы-в-executor"></a>

### 10. В какой момент подтверждать обработку сообщения Kafka?

Контракт: Spring Kafka 3.3, record listener, `enable.auto.commit=false`, `AckMode.MANUAL_IMMEDIATE`, `asyncAcks=false`. Сообщение начисляет бонус в PostgreSQL. Обработка допускает replay, но не потерю события и не двойной бонус. Успех listener означает завершённый DB commit, порядок одной партиции нужно сохранять.

```java
@KafkaListener(topics = "bonuses", groupId = "bonus-worker")
public void receive(BonusEvent event, Acknowledgment ack) {
    executor.submit(() -> bonusService.apply(event));
    ack.acknowledge();
}
```

Разберите сбой сразу после возврата из listener и сразу после фиксации в базе. Какие наблюдаемые результаты возможны?

<details>
<summary>Разбор</summary>

Подтверждена постановка задачи в память, хотя бизнес-обработка не завершена. Offset может быть зафиксирован, затем worker или процесс падает: Kafka уже не обязана доставлять запись заново этому group. Исключение спрятано в проигнорированном `Future`, поэтому контейнерный error handler его тоже не видит.

Простейшее исправление — синхронная обработка на listener thread:

```java
@KafkaListener(topics = "bonuses", groupId = "bonus-worker")
public void receive(BonusEvent event, Acknowledgment ack) {
    bonusService.applyOnce(event); // другой Spring-бин, commit до return
    ack.acknowledge();
}
```

В `applyOnce` одна DB-транзакция вставляет `(consumer_name, event_id)` с UNIQUE через `ON CONFLICT DO NOTHING`, при новой записи начисляет бонус и подтверждает обе записи вместе. Исключение выходит наружу. Это не готовый repository-код: SQL и сервис реализуются для своей схемы. При падении после DB commit и до ack придёт дубль, который остановит маркер. Долгая обработка требует настройки `max.poll.interval.ms` и `max.poll.records`; дополнительную параллельность обычно получают партициями и concurrency контейнера.

Механический ack из произвольного executor опасен: задачи завершаются не по порядку, подтверждение более позднего offset может перескочить незавершённую запись. `asyncAcks` поддерживает отложенные подтверждения внутри poll, но меняет управление потоком и допускает повторы; он не добавляет атомарности с БД. Собственный вызов методов `KafkaConsumer` с worker тоже нарушает его модель потоков.

Проверка с реальным broker и DB воспроизводит падение до commit, после commit до ack, бизнес-исключение и повтор одного event ID. В каждом случае итоговый бонус один; исключение до commit должно достигать контейнерного обработчика. Для poison message явно определяют quarantine/DLT и условия подтверждения: «записать warning и ack в finally» незаметно меняет контракт на потерю.

</details>

<a id="11-фоновый-экспорт-использует-права-на-момент-http-запроса"></a>

### 11. Чьи права учитывать при отложенном экспорте?

Контракт: пользователь создаёт отложенный экспорт; перед началом чтения данных права проверяются заново. Отозванное членство запрещает запуск. Уже начатый экспорт завершает согласованный снимок, скачивание всегда требует актуальных прав. Job устойчиво сохранён до HTTP 202; executor лишь ускоряет запуск worker.

```java
SecurityContext captured = SecurityContextHolder.getContext();
executor.submit(() -> {
    SecurityContextHolder.setContext(captured);
    exporter.exportAllForCurrentTenant();
});
```

Сопоставьте три момента: постановку задания, начало чтения и скачивание. В каком случае пользователь получит файл, если его права изменились?

<details>
<summary>Разбор</summary>

Захвачена ссылка на изменяемый security context, а worker не восстанавливает прежнее значение. Но даже корректная инфраструктурная обёртка решает только перенос/очистку контекста: старое authentication не гарантирует актуальных прав спустя час. В условии задана конкретная точка проверки, поэтому её необходимо выразить в бизнес-протоколе задания.

Исправленная схема Java 17, прикладные helpers явно сокращены:

```java
record ExportCommand(UUID jobId, UUID actorId, UUID tenantId) {}

void run(ExportCommand command) {
    // claimAndAuthorize: одна транзакция с проверкой живого membership.
    // Только успешный claim переводит PENDING -> RUNNING.
    if (!jobs.claimAndAuthorize(command)) return;
    try {
        exporter.exportSnapshot(command.jobId(), command.tenantId());
        jobs.complete(command.jobId());
    } catch (RuntimeException ex) {
        jobs.failOrScheduleRetry(command.jobId());
        throw ex;
    }
}
```

Команда содержит идентификаторы, не bearer token и не servlet request. `claimAndAuthorize` проверяет сохранённые данные job, tenant, actor и текущую membership; значения команды не являются самостоятельным доказательством доступа. Все запросы экспорта явно ограничиваются tenant. Если отзыв и запуск конкурируют, общая блокировка строки membership и единый протокол обновления задают порядок: кто первым подтвердил транзакцию, тот определяет допустимость старта. Проверка членства и смена статуса в независимых транзакциях оставят окно гонки.

Результат хранится приватно; download endpoint повторно проверяет доступ. Для падения worker нужен lease и восстановление `RUNNING`, а публикация файла должна быть идемпотентна по job ID. Иначе ошибка после записи файла до `complete` создаст повторный артефакт. Эти helpers здесь являются протоколом, не обещанием готовой реализации очереди.

Проверяют очередь с задержкой: отозвать membership после 202, запустить job, убедиться в отсутствии чтения данных. Отдельно — отзыв во время `RUNNING` согласно разрешённому контракту и отказ скачивания после отзыва. Если бизнес требует немедленно прерывать уже начатый экспорт, модель меняется: нужны дополнительные проверки и отмена, одного контекста недостаточно.

</details>

<a id="12-after_commit-изменяет-entity-без-следующего-commit"></a>

### 12. Когда сохранятся отметка об оплате и запись аудита?

Контракт: после оплаты `order.paid=true` и запись локального аудита должны фиксироваться атомарно. Spring Framework 6.x, JPA, обычная thread-bound транзакция; event публикуется внутри неё. Никакой внешний HTTP-вызов в listener не нужен. Тест проверяет только состояние полученной entity, а не новую транзакцию чтения.

```java
@TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
public void paid(OrderPaid event) {
    Order order = repository.findById(event.orderId()).orElseThrow();
    order.setPaid(true);
    auditRepository.save(new Audit(event.orderId(), "PAID"));
}
```

Проверьте, что увидит независимый читатель после завершения платежа и при ошибке записи аудита. Как проверить это интеграционным тестом?

<details>
<summary>Разбор</summary>

В фазе `AFTER_COMMIT` исходная транзакция уже завершила commit. Ресурсы ещё могут оставаться привязанными к потоку, а обращение к данным выглядит участвующим в прежней транзакции. Однако следующего commit для этих изменений не будет. JPA dirty checking и наблюдение изменённого Java-объекта не доказывают устойчивую запись. Принудительный flush не возвращает уже завершённую транзакцию к жизни.

Контракт требует одной атомарной фиксации, поэтому обязательную запись проще выполнить в исходном transactional service:

```java
@Transactional
public void markPaid(UUID orderId) {
    Order order = repository.findById(orderId).orElseThrow();
    if (order.isPaid()) return; // допустимый повтор по условию
    order.setPaid(true);
    auditRepository.save(new Audit(orderId, "PAID"));
}
```

Это фрагмент метода Spring-бина, вызываемого через proxy. Для конкурентных оплат нужен отдельный механизм сериализации: `@Version`, блокировка строки либо условное обновление с проверкой количества строк; простая проверка `isPaid` не делает аудит идемпотентным при двух транзакциях. В схеме полезен UNIQUE для единственного факта оплаты, если таков бизнес-инвариант.

`BEFORE_COMMIT` listener может присоединить обязательную локальную запись к исходной транзакции, но делает важный путь менее явным. `REQUIRES_NEW` на отдельном бине обеспечивает новый commit после исходного, однако это уже две транзакции: первая может успешно завершиться, а аудит — упасть. Он подходит только при допустимом отдельном эффекте и наличии восстановления. Для обязательной последующей доставки сохраняют outbox в первой транзакции, не надеются на callback в памяти.

Проверка вызывает сервис без окружающей тестовой транзакции, затем читает order и audit из нового persistence context. Инъекция ошибки записи аудита должна оставить оба изменения неподтверждёнными. Повтор и два одновременных вызова проверяют отдельно. Если event публикуется вне транзакции, listener по умолчанию вообще не вызывается; `fallbackExecution=true` меняет поведение, но не создаёт исходную атомарность.

</details>

[← Методика код-ревью](../code-review.md) · [Задачи на рассуждение](../reasoning-questions.md)
