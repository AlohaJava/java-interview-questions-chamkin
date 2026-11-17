# Spring Framework и Spring Boot

<!-- learning-nav:start -->
[← Все разделы](README.md) · [Предыдущая тема](databases-and-sql.md) · [Следующая тема →](testing.md)
<!-- learning-nav:end -->

Материал идёт от контейнера и конфигурации к транзакционным границам, асинхронности и веб-слою. Основные примеры рассчитаны на Java 17–21, Spring Framework 6.2 и Spring Boot 3.x; особенности Spring 7/Boot 4 отмечены отдельно. Номера вопросов сохранены как стабильные идентификаторы: порядок чтения тематический.

## Оглавление

<!-- toc:start -->
<details>
<summary>Показать темы и вопросы</summary>

- [Контейнер, зависимости и конфигурация](#контейнер-зависимости-и-конфигурация)
  - [1. Инверсия контроля (IoC) и Внедрение зависимостей (DI)](#1-инверсия-контроля-ioc-и-внедрение-зависимостей-di)
  - [2. Spring Контейнеры: ApplicationContext и BeanFactory](#2-spring-контейнеры-applicationcontext-и-beanfactory)
  - [3. Spring Bean: Понятие, Области видимости (Scopes) и Жизненный цикл](#3-spring-bean-понятие-области-видимости-scopes-и-жизненный-цикл)
  - [48. Где хранить промежуточный результат, если контроллер обслуживает два запроса одновременно?](#48-где-хранить-промежуточный-результат-если-контроллер-обслуживает-два-запроса-одновременно)
  - [37. Почему prototype внутри singleton создаётся только один раз?](#37-почему-prototype-внутри-singleton-создаётся-только-один-раз)
  - [45. Почему бин с @Lazy всё равно создаётся при старте приложения?](#45-почему-бин-с-lazy-всё-равно-создаётся-при-старте-приложения)
  - [4. Ключевые аннотации конфигурации: @Bean, @Component, @Autowired, @Primary, @Qualifier](#4-ключевые-аннотации-конфигурации-bean-component-autowired-primary-qualifier)
  - [44. Как собрать список обработчиков и задать порядок проверок?](#44-как-собрать-список-обработчиков-и-задать-порядок-проверок)
  - [38. Что меняется при @Configuration(proxyBeanMethods = false)?](#38-что-меняется-при-configurationproxybeanmethods--false)
  - [7. Spring Boot: Преимущества, Автоконфигурация и Starters](#7-spring-boot-преимущества-автоконфигурация-и-starters)
  - [39. Как расследовать, почему условная конфигурация создала неправильный бин?](#39-как-расследовать-почему-условная-конфигурация-создала-неправильный-бин)
  - [15. Профили и внешняя конфигурация: @Profile и application.yml](#15-профили-и-внешняя-конфигурация-profile-и-applicationyml)
  - [43. Почему адрес сервиса заполнен локально, а на сервере приходит пустым?](#43-почему-адрес-сервиса-заполнен-локально-а-на-сервере-приходит-пустым)
  - [17. BeanPostProcessor и BeanFactoryPostProcessor](#17-beanpostprocessor-и-beanfactorypostprocessor)
  - [11. Циклические зависимости и их разрешение в Spring](#11-циклические-зависимости-и-их-разрешение-в-spring)
  - [36. Почему @PostConstruct не подходит для запуска транзакционного фонового процесса?](#36-почему-postconstruct-не-подходит-для-запуска-транзакционного-фонового-процесса)
- [Прокси, транзакции и доступ к данным](#прокси-транзакции-и-доступ-к-данным)
  - [5. Аспектно-Ориентированное Программирование (AOP) в Spring](#5-аспектно-ориентированное-программирование-aop-в-spring)
  - [12. Прокси в Spring: JDK Dynamic Proxy и CGLIB, проблема self-invocation](#12-прокси-в-spring-jdk-dynamic-proxy-и-cglib-проблема-self-invocation)
  - [8. Управление транзакциями и аннотация @Transactional](#8-управление-транзакциями-и-аннотация-transactional)
  - [29. На подумать: почему REQUIRES_NEW внутри того же сервиса не спас аудит?](#29-на-подумать-почему-requires_new-внутри-того-же-сервиса-не-спас-аудит)
  - [30. Почему после перехваченного исключения возникает UnexpectedRollbackException?](#30-почему-после-перехваченного-исключения-возникает-unexpectedrollbackexception)
  - [31. Как checked exceptions и rollbackFor влияют на результат транзакции?](#31-как-checked-exceptions-и-rollbackfor-влияют-на-результат-транзакции)
  - [32. На подумать: почему REQUIRES_NEW исчерпал пул соединений?](#32-на-подумать-почему-requires_new-исчерпал-пул-соединений)
  - [33. Чем NESTED отличается от REQUIRES_NEW при частичном откате?](#33-чем-nested-отличается-от-requires_new-при-частичном-откате)
  - [34. Почему внутренняя аннотация isolation или timeout может ничего не изменить?](#34-почему-внутренняя-аннотация-isolation-или-timeout-может-ничего-не-изменить)
  - [35. Когда TransactionTemplate делает границу операции понятнее аннотации?](#35-когда-transactiontemplate-делает-границу-операции-понятнее-аннотации)
  - [14. Spring Data JPA: derived queries, @Query, проекции и пагинация](#14-spring-data-jpa-derived-queries-query-проекции-и-пагинация)
  - [47. Как составить поиск с необязательными фильтрами и сохранить ограничение по организации?](#47-как-составить-поиск-с-необязательными-фильтрами-и-сохранить-ограничение-по-организации)
  - [26. Lombok: что генерирует и ловушки с JPA-сущностями](#26-lombok-что-генерирует-и-ловушки-с-jpa-сущностями)
- [Асинхронность, события и кэш](#асинхронность-события-и-кэш)
  - [13. Асинхронное и плановое выполнение: @Async и @Scheduled](#13-асинхронное-и-плановое-выполнение-async-и-scheduled)
  - [40. Что теряется на границе @Async: транзакция, MDC и SecurityContext?](#40-что-теряется-на-границе-async-транзакция-mdc-и-securitycontext)
  - [42. Как обеспечить единственный запуск @Scheduled в нескольких экземплярах приложения?](#42-как-обеспечить-единственный-запуск-scheduled-в-нескольких-экземплярах-приложения)
  - [18. События в Spring: ApplicationEvent и @EventListener](#18-события-в-spring-applicationevent-и-eventlistener)
  - [41. Почему AFTER_COMMIT ещё не гарантирует сохранение изменений и доставку события?](#41-почему-after_commit-ещё-не-гарантирует-сохранение-изменений-и-доставку-события)
  - [19. Spring WebFlux: реактивная модель, Mono и Flux](#19-spring-webflux-реактивная-модель-mono-и-flux)
  - [16. Кэширование: @Cacheable, @CacheEvict и @CachePut](#16-кэширование-cacheable-cacheevict-и-cacheput)
- [Веб-слой и HTTP-клиенты](#веб-слой-и-http-клиенты)
  - [6. Spring MVC и паттерн Front Controller (DispatcherServlet)](#6-spring-mvc-и-паттерн-front-controller-dispatcherservlet)
  - [10. Создание RESTful API: @RestController, обработка исключений и валидация](#10-создание-restful-api-restcontroller-обработка-исключений-и-валидация)
  - [46. Почему отсутствие идентификатора в пути даёт 404, а неверный тип значения может дать 400?](#46-почему-отсутствие-идентификатора-в-пути-даёт-404-а-неверный-тип-значения-может-дать-400)
  - [20. Валидация: Bean Validation, @Valid и @Validated](#20-валидация-bean-validation-valid-и-validated)
  - [23. HTTP-клиенты между сервисами: RestTemplate, WebClient и OpenFeign](#23-http-клиенты-между-сервисами-resttemplate-webclient-и-openfeign)
  - [24. Filter, HandlerInterceptor и AOP: что где перехватывает](#24-filter-handlerinterceptor-и-aop-что-где-перехватывает)
  - [25. Jackson в Spring: настройка ObjectMapper, аннотации и циклические ссылки](#25-jackson-в-spring-настройка-objectmapper-аннотации-и-циклические-ссылки)
- [Безопасность и аутентификация](#безопасность-и-аутентификация)
  - [9. Spring Security: Архитектура, Аутентификация и JWT](#9-spring-security-архитектура-аутентификация-и-jwt)
  - [27. Session-based и token-based (JWT) аутентификация, OAuth2 и OIDC](#27-session-based-и-token-based-jwt-аутентификация-oauth2-и-oidc)
  - [28. Хранение паролей и основы безопасности: BCrypt и OWASP Top-10](#28-хранение-паролей-и-основы-безопасности-bcrypt-и-owasp-top-10)
- [Эксплуатация и наблюдаемость](#эксплуатация-и-наблюдаемость)
  - [21. Spring Boot Actuator: health, info, metrics и интеграция с Prometheus](#21-spring-boot-actuator-health-info-metrics-и-интеграция-с-prometheus)
  - [22. Логирование: SLF4J, Logback и MDC](#22-логирование-slf4j-logback-и-mdc)

</details>
<!-- toc:end -->

## Контейнер, зависимости и конфигурация

### 1. Инверсия контроля (IoC) и Внедрение зависимостей (DI)

**Инверсия контроля (Inversion of Control, IoC)** — это архитектурный принцип, при котором управление потоком выполнения программы передается внешнему фреймворку, а не осуществляется самим пользовательским кодом. В традиционном подходе объекты сами создают свои зависимости, что приводит к сильной связности (tight coupling). При использовании IoC фреймворк берет на себя ответственность за создание объектов и управление их жизненным циклом.

**Внедрение зависимостей (Dependency Injection, DI)** — это конкретный паттерн реализации IoC. Суть DI заключается в том, что зависимости передаются объекту извне (контейнером), а не создаются внутри него самого.

В Spring Framework DI реализуется через IoC-контейнер, который внедряет зависимости тремя основными способами:

1. **Через конструктор (Constructor Injection)** — предпочтительный способ: зависимости можно объявить `final`, объект нельзя создать в неполном состоянии, а в юнит-тесте зависимости передаются явно, без рефлексии. Побочный плюс — циклическая зависимость через конструкторы валится сразу при старте приложения, а не в рантайме.
2. **Через сеттер (Setter Injection)** — используется для опциональных зависимостей, которые можно поменять после создания объекта.
3. **Через поле (Field Injection)** — осуществляется с помощью Reflection API. Не рекомендуется: зависимости скрыты (по сигнатуре класса не видно, что ему нужно), поле нельзя сделать `final`, а в тесте его придётся заполнять через `ReflectionTestUtils`.

Начиная со Spring 4.3, если у бина единственный конструктор, `@Autowired` на нём можно не ставить — контейнер подхватит его сам. Поэтому в современном коде конструкторная инъекция пишется вообще без аннотаций.

Пример внедрения через конструктор:
```java
import org.springframework.stereotype.Service;

@Service
public class OrderService {
    private final PaymentProcessor paymentProcessor;

    // Внедрение зависимости через конструктор
    public OrderService(PaymentProcessor paymentProcessor) {
        this.paymentProcessor = paymentProcessor;
    }

    public void processOrder() {
        paymentProcessor.process();
    }
}
```

---

### 2. Spring Контейнеры: ApplicationContext и BeanFactory

Spring предоставляет два основных интерфейса для работы с IoC-контейнером: `BeanFactory` и `ApplicationContext`.

**BeanFactory** — базовый контракт контейнера: получение объектов, разрешение зависимостей и scopes. Сам интерфейс не обещает определённую стратегию старта. Например, `DefaultListableBeanFactory` создаёт бин при обращении, но допускает и явный вызов `preInstantiateSingletons()`. Поэтому lazy/eager — поведение конфигурации и реализации, а не жёсткое свойство имени интерфейса.

**ApplicationContext** — это расширенный интерфейс, который наследуется от `BeanFactory` и предоставляет дополнительные возможности корпоративного уровня. В отличие от `BeanFactory`, `ApplicationContext` по умолчанию инициализирует все singleton-бины сразу при запуске приложения (Eager Initialization) — ошибки конфигурации всплывают на старте, а не при первом запросе.

Дополнительные возможности `ApplicationContext`:

- Поддержка интернационализации (i18n) через интерфейс `MessageSource`.
- Публикация и прослушивание событий приложения (ApplicationEventPublisher).
- Поддержка AOP.
- Специфичные контексты для веб-приложений (например, `WebApplicationContext`).

В большинстве приложений используют `ApplicationContext`: Spring Boot стартует именно его (`AnnotationConfigServletWebServerApplicationContext` для сервлетного стека, `AnnotationConfigReactiveWebServerApplicationContext` для WebFlux). Eager-инициализацию можно отключить глобально — `spring.main.lazy-initialization=true`: ускоряет старт, но ошибки в бинах вылезут при первом обращении, а не при запуске.

Пример инициализации контекста:
```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        OrderService orderService = context.getBean(OrderService.class);
        orderService.processOrder();
    }
}
```

---

### 3. Spring Bean: Понятие, Области видимости (Scopes) и Жизненный цикл

**Spring Bean** — это объект, который создается, настраивается и управляется IoC-контейнером Spring. Конфигурация бина содержит метаданные о том, как его создать, его зависимости и область видимости.

**Области видимости (Scopes):**

1. `singleton` (по умолчанию) — создается единственный экземпляр бина на весь ApplicationContext.
2. `prototype` — при каждом запросе создается новый экземпляр бина.
3. `request` — новый экземпляр бина создается на каждый HTTP-запрос (только для веб-приложений).
4. `session` — новый экземпляр бина создается на каждую HTTP-сессию.
5. `application` — бин живет в рамках жизненного цикла `ServletContext`.
6. `websocket` — бин связан с жизненным циклом WebSocket.

**Жизненный цикл бина:**

1. Инстанцирование (вызов конструктора).
2. Внедрение зависимостей (Dependency Injection).
3. Вызов методов `setBeanName`, `setBeanFactory`, `setApplicationContext` (если реализованы интерфейсы `Aware`).
4. Пре-инициализация: метод `postProcessBeforeInitialization` интерфейса `BeanPostProcessor`.
5. Инициализация: метод, аннотированный `@PostConstruct`, затем метод `afterPropertiesSet` интерфейса `InitializingBean`, затем пользовательский init-метод.
6. Пост-инициализация: метод `postProcessAfterInitialization` интерфейса `BeanPostProcessor` (здесь создаются прокси для AOP).
7. Использование бина приложением.
8. Уничтожение: метод, аннотированный `@PreDestroy`, затем метод `destroy` интерфейса `DisposableBean`, затем пользовательский destroy-метод.

```java
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;
import jakarta.annotation.PostConstruct;
import jakarta.annotation.PreDestroy;

@Component
@Scope("prototype")
public class TaskRunner {

    @PostConstruct
    public void init() {
        System.out.println("Инициализация ресурсов TaskRunner");
    }

    @PreDestroy
    public void cleanup() {
        System.out.println("Очистка ресурсов TaskRunner");
    }
}
```

Начиная со Spring Framework 6 / Spring Boot 3 эти аннотации живут в пакете `jakarta.annotation` — импорт `javax.annotation` в новом проекте просто не скомпилируется, это частый источник путаницы при миграции.

Два нюанса. Контейнер не вызывает destruction callbacks для выданных prototype-экземпляров: их ресурсы освобождает владелец. Для singleton callbacks вызываются при корректном закрытии контекста, а request/session scopes могут управлять уничтожением через свой lifecycle. Аварийное завершение процесса не гарантирует `@PreDestroy`. Кроме того, singleton-scope не означает потокобезопасность: общее изменяемое состояние требует синхронизации или иной модели владения. Пример выше специально показывает prototype: его `cleanup()` автоматически не вызовется. Как получать новый prototype для каждой операции, разобрано в вопросе 37.

---

### 48. Где хранить промежуточный результат, если контроллер обслуживает два запроса одновременно?

Пусть singleton-контроллер записывает сумму заказа в своё поле, а затем читает поле при построении ответа. Запрос A записал 100, запрос B успел записать 200, затем A прочитал 200. Разные HTTP-соединения не изолируют поля общего Java-объекта. Даже `volatile` допускает этот порядок действий.

Для результата одного вызова достаточно локальной переменной или созданного внутри метода объекта. Передавайте его дальше как аргумент. Если разные компоненты разделяют данные только в рамках одного HTTP-запроса, можно выделить request-scoped бин. Для общего счётчика запросов подойдёт инструмент для потокобезопасных метрик. При этом решите, нужен ли итог по процессу или по всем экземплярам сервиса.

Проверка для исходного кода: задержите A сразу после записи, позвольте B записать свой результат, затем разрешите A вернуть ответ. Так вы воспроизведёте ошибку без случайной нагрузки и таймеров.

---

### 37. Почему prototype внутри singleton создаётся только один раз?

Scope определяет поведение обращения к контейнеру, а не превращает Java-ссылку в фабрику. Когда Spring создаёт singleton-сервис и внедряет в конструктор prototype, он получает один экземпляр зависимости. Дальнейшие вызовы метода сервиса используют ту же ссылку; нового запроса к контейнеру не происходит.

```java
@Service
class ExportService {
    private final ObjectProvider<ExportSession> sessions;

    ExportService(ObjectProvider<ExportSession> sessions) {
        this.sessions = sessions;
    }

    void export() {
        ExportSession session = sessions.getObject();
        try {
            session.write();
        } finally {
            session.close();
        }
    }
}
```

Здесь `ExportSession` объявлен prototype, а `ObjectProvider` явно запрашивает его для каждой операции. Вне Spring ту же идею выражает обычная фабрика. Альтернатива — scoped proxy, но тогда получение целевого объекта скрыто внутри вызова, и нужно понимать границу scope. Для request-scope прокси позволяет внедрить зависимость в singleton, однако обращение вне активного запроса не создаёт HTTP-контекст автоматически.

Контейнер не вызывает destruction callbacks для выданных prototype-экземпляров, поэтому ресурсы в примере закрывает владелец операции. У request/session-scoped бинов соответствующий scope может выполнять destruction callbacks: правило «destroy только для singleton» слишком широкое.

Частая ошибка — выбрать prototype для изменяемого объекта и считать проблему конкурентности решённой, хотя объект был один раз внедрён в singleton. Надёжнее держать состояние операции в локальных переменных и создавать короткоживущие объекты явно, когда управление контейнера для них не требуется.

---

### 45. Почему бин с @Lazy всё равно создаётся при старте приложения?

Пусть обычный singleton `ReportService` напрямую получает через конструктор ленивый singleton `Renderer`. При создании сервиса контейнер обязан предоставить реальную зависимость и может создать `Renderer` уже при старте. Аннотация на определении бина отменяет его предварительное создание по инициативе контейнера, но допускает ранний запрос со стороны другого бина.

Если обработчику отчёта нужен рендерер только при вызове, внедрите `ObjectProvider<Renderer>` и вызовите `getObject()` в этом месте. Для проксируемого типа подойдёт и `@Lazy` в точке внедрения: сервис получит прокси, а цель понадобится при вызове метода. Чтобы сам целевой singleton не создался отдельно при обычной инициализации контекста, его определение также должно быть ленивым.

Проверьте отдельно старт и первый запрос отчёта. Цена отложенного создания — ошибки конфигурации и затраты на инициализацию могут впервые проявиться уже в запросе пользователя.

---

### 4. Ключевые аннотации конфигурации: @Bean, @Component, @Autowired, @Primary, @Qualifier

- `@Component`: базовая аннотация уровня класса. Указывает, что класс является кандидатом для автоматического сканирования (`@ComponentScan`) и регистрации в качестве бина. Имеет специализации: `@Service`, `@Repository`, `@Controller`.
- `@Bean`: аннотация уровня метода, применяемая в конфигурационных классах (`@Configuration`). Метод возвращает объект, который будет зарегистрирован как бин. Это позволяет регистрировать в контексте классы из сторонних библиотек.
- `@Autowired`: аннотация для внедрения зависимостей. Указывает контейнеру Spring на необходимость автоматического связывания (autowiring) зависимости по типу (byType).
- `@Qualifier`: сужает набор подходящих кандидатов по метке; имя бина может быть такой меткой, но qualifier не ограничивается поиском по имени.
- `@Primary`: указывает, что данный бин является приоритетным для внедрения, если есть несколько бинов одного типа.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.stereotype.Component;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;

interface NotificationSender { void send(); }

@Component
@Qualifier("email")
class EmailSender implements NotificationSender {
    public void send() { /* ... */ }
}

@Component
@Qualifier("sms")
class SmsSender implements NotificationSender {
    public void send() { /* ... */ }
}

@Component
class NotificationService {
    private final NotificationSender sender;

    @Autowired
    public NotificationService(@Qualifier("email") NotificationSender sender) {
        this.sender = sender;
    }
}

@Configuration
class AppConfig {
    @Bean
    public DataSource dataSource() {
        return new CustomDataSource();
    }
}
```

Частые follow-up:

- Что будет при нескольких кандидатах без однозначного выбора? При разрешении обязательной одиночной зависимости возникнет `NoUniqueBeanDefinitionException`. Возможны дополнительные правила, например совпадение имени параметра/поля с именем бина; для имён параметров в Spring 6.1+ важен флаг компиляции `-parameters`. Коллекционная инъекция может получить всех кандидатов.
- Чем `@Component` отличается от `@Bean`? `@Component` ставится на ваш класс, экземпляр создаёт контейнер. `@Bean` — на метод в `@Configuration`-классе, объект создаёте вы сами: это способ зарегистрировать класс из сторонней библиотеки или собрать объект с нетривиальной инициализацией.
- Зачем специализации `@Component`? `@Service` и `@Controller` обозначают роли компонентов, причём `@Controller` используется инфраструктурой MVC для обнаружения обработчиков. `@Repository` делает компонент кандидатом для трансляции исключений persistence в `DataAccessException`, если настроен `PersistenceExceptionTranslationPostProcessor`. Это не означает автоматическое преобразование любого checked `SQLException` из произвольного метода.
- `@Autowired` vs `@Resource` vs `@Inject`: `@Autowired` ищет по типу, `@Resource` (jakarta) — сначала по имени бина, `@Inject` — стандарт JSR-330, почти эквивалент `@Autowired`. В новых проектах используют `@Autowired` (точнее, просто конструктор).

---

### 44. Как собрать список обработчиков и задать порядок проверок?

Если у трёх бинов есть общий интерфейс `OrderCheck`, конструктор может принять `List<OrderCheck>`. Spring передаст подходящие реализации. Проверку заполненности адреса полезно выполнять до дорогого вызова службы доставки. Пометьте реализации или методы `@Bean` разными значениями `@Order`, например 10 и 20. В списке меньшее число идёт раньше.

`@Primary` помогает выбрать одну реализацию для одиночной зависимости. В список по умолчанию попадут все подходящие бины. `@Order` задаёт порядок элементов, но сам сервис должен пройти список и решить, прекращать ли проверки после первой ошибки. Порядок элементов списка не задаёт порядок создания singleton-бинов при запуске.

Проверьте результат на одном заказе: после ошибки «адрес пуст» внешняя служба не вызывалась. Такой тест по-прежнему проверит требование, даже если между двумя проверками появится третья.

---

### 38. Что меняется при @Configuration(proxyBeanMethods = false)?

В обычном `@Configuration` Spring может перехватывать вызовы `@Bean`-методов между собой и возвращать управляемый контейнером объект. При `proxyBeanMethods = false` такого перехвата нет: прямой Java-вызов метода создаёт новый объект по его телу. Сам метод по-прежнему регистрирует bean definition; отключено именно расширение конфигурационного класса.

```java
@Configuration(proxyBeanMethods = false)
class ClientConfiguration {
    @Bean
    Transport transport() {
        return new Transport();
    }

    @Bean
    ApiClient apiClient(Transport transport) {
        return new ApiClient(transport);
    }
}
```

Параметр `apiClient()` разрешается контейнером, поэтому клиент получает тот же singleton `Transport`, который доступен другим бинам. Если написать `new ApiClient(transport())`, это будет обычный вызов фабричного метода, создающий второй экземпляр. У него могут оказаться отдельный пул соединений, отсутствие ожидаемой постобработки и другое управление ресурсами.

Такой стиль удобен для независимых фабричных методов и широко используется в автоконфигурации. Он позволяет обходиться без CGLIB-расширения конфигурационного класса, но не отключает AOP-прокси у создаваемых сервисов. Это два разных механизма: один обеспечивает семантику межбиновых вызовов фабрик, другой добавляет поведение бизнес-методам.

Частая ошибка — включить `false` ради ускорения старта, не просмотрев внутренние вызовы `@Bean`-методов. Проверяют идентичность реально внедрённых ресурсов, а не только успешный старт. Передача зависимостей параметрами методов обычно делает конфигурацию понятнее и сохраняет нужную семантику независимо от выбранного режима.

---

### 7. Spring Boot: Преимущества, Автоконфигурация и Starters

**Spring Boot** — это надстройка над Spring Framework, предназначенная для упрощения процесса создания и настройки приложений, готовых к внедрению в производство (production-ready).

Ключевые преимущества:

1. **Автоконфигурация (Auto-Configuration)**: Механизм, который автоматически настраивает компоненты Spring на основе присутствующих в classpath библиотек. Реализован с помощью аннотации `@EnableAutoConfiguration`.
2. **Spring Boot Starters**: Набор удобных дескрипторов зависимостей. Например, `spring-boot-starter-web` в Boot 3.x добавляет Spring MVC, Tomcat и Jackson; Bean Validation подключается отдельно через `spring-boot-starter-validation`. Набор серверов и состав стартеров зависит от major-версии Boot.
3. **Встроенные серверы**: Приложения Spring Boot поставляются со встроенными серверами (Tomcat, Jetty или Undertow), что позволяет запускать приложение как самостоятельный Java-процесс (`java -jar`).
4. **Actuator**: Предоставляет готовые эндпоинты для мониторинга метрик, состояния (health check) и управления приложением в production.

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication // Включает @Configuration, @EnableAutoConfiguration и @ComponentScan
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

Как автоконфигурация устроена под капотом — обязательный follow-up. При старте Spring Boot читает файл `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports` из каждого jar в classpath (в старых версиях использовался `META-INF/spring.factories`; формат `AutoConfiguration.imports` появился в Boot 2.7) и регистрирует перечисленные там `@Configuration`-классы. Но срабатывают они не всегда: на каждом висят условия — `@ConditionalOnClass` (класс есть в classpath), `@ConditionalOnMissingBean` (пользователь не определил свой бин), `@ConditionalOnProperty` и другие. Поэтому ваш собственный `@Bean DataSource` отменяет автоконфигурацию DataSource — условие `OnMissingBean` перестаёт выполняться. Отключить конкретную автоконфигурацию можно через параметр `exclude` в `@SpringBootApplication` или свойство `spring.autoconfigure.exclude`.

---

### 39. Как расследовать, почему условная конфигурация создала неправильный бин?

Начните с условий регистрации, а не с `@Primary`. `@ConditionalOnClass` проверяет наличие класса, `@ConditionalOnMissingBean` — отсутствие подходящих bean definitions, `@ConditionalOnProperty` — значения свойств. Бин может вообще не попасть в контекст; тогда выбор при autowiring уже ни при чём.

```java
@Bean
@ConditionalOnProperty(prefix = "mail", name = "enabled",
        havingValue = "true", matchIfMissing = false)
MailSender mailSender() {
    return new RemoteMailSender();
}
```

Без `havingValue` поведение `@ConditionalOnProperty` шире простого «равно true»: по умолчанию подходит существующее значение, отличное от `false`. Поэтому строка `mail.enabled=disabled` может неожиданно включить конфигурацию. Отсутствие свойства отдельно контролируется `matchIfMissing`.

В Boot смотрят condition evaluation report (`--debug`), фактические property sources, активные профили и наличие пользовательского бина. `@ConditionalOnMissingBean` зависит от уже обработанных определений; его обычно используют в правильно упорядоченной автоконфигурации, а не как универсальное средство устранения конфликта двух произвольных конфигураций. Возвращаемый тип `@Bean` желательно объявлять достаточно конкретно, чтобы условие распознало доступный компонент до его создания.

Условия оцениваются при построении контекста. Изменение свойства в удалённой конфигурации не обязано удалять и создавать бины в живом приложении. Для динамического переключения нужен явный runtime-механизм. Проверки включённого, выключенного и пользовательского варианта удобно писать через `ApplicationContextRunner`: контекст небольшой, а причина выбора видна точнее, чем в полном старте сервиса.

---

### 15. Профили и внешняя конфигурация: @Profile и application.yml

Профили позволяют иметь разные бины и настройки для разных окружений (dev, test, prod). Активируются через `spring.profiles.active=prod` (в свойствах, переменной окружения или аргументе запуска).

```java
@Configuration
@Profile("prod")
public class ProdDataSourceConfig {
    // бины из этого класса создадутся только с активным профилем prod
}

@Component
@Profile("!prod") // логические выражения тоже поддерживаются: &, |, !
public class LocalStubEmailSender implements EmailSender { ... }
```

Настройки хранятся в `application.yml` / `application.properties`, а профильные переопределения — в `application-{profile}.yml`. Профильный файл имеет приоритет над базовым.

Порядок переопределения свойств (от высшего приоритета к низшему, сокращённо):

1. Аргументы командной строки (`--server.port=9090`).
2. `SPRING_APPLICATION_JSON`.
3. Параметры JNDI, системные свойства JVM (`-D...`).
4. Переменные окружения (`SERVER_PORT` маппится на `server.port` — relaxed binding).
5. Профильные `application-{profile}.properties` вне jar, затем внутри jar.
6. Базовые `application.properties` вне jar, затем внутри jar.
7. `@PropertySource` на конфигурационных классах.
8. Дефолты `SpringApplication.setDefaultProperties`.

Частый follow-up — чем `@ConfigurationProperties` лучше `@Value`:

```java
@Component
@ConfigurationProperties(prefix = "payment")
public class PaymentProperties {
    private String url;
    private Duration timeout; // Duration, списки, вложенные объекты биндятся из коробки
    // getters/setters
}
```

`@ConfigurationProperties` даёт типобезопасный биндинг целой группы свойств, relaxed binding (`payment.base-url` = `payment.baseUrl` = `PAYMENT_BASEURL`), валидацию через `@Validated` и метаданные для IDE. `@Value` — для разовых значений и SpEL, но не поддерживает relaxed binding и роняет приложение, если свойства нет (без дефолта `:`).

---

### 43. Почему адрес сервиса заполнен локально, а на сервере приходит пустым?

Убедитесь, что клиент получил зарегистрированный бин настроек. Объект, созданный клиентом через `new`, Spring Boot не заполняет. Проверьте активный профиль и наличие ключа под нужным префиксом в окружении самого процесса. Для ключа `billing.base-url` соответствующая переменная окружения Boot — `BILLING_BASEURL`. Проверьте также более приоритетные источники настроек.

Чтобы обнаружить проблему при запуске, объявите обязательные параметры явно:

```java
@Validated
@ConfigurationProperties("billing")
public record BillingProperties(@NotNull URI baseUrl,
                                @NotNull Duration timeout) {}
```

Зарегистрируйте тип через `@ConfigurationPropertiesScan` или `@EnableConfigurationProperties(BillingProperties.class)`. При наличии реализации Bean Validation отсутствующее обязательное значение остановит запуск. Если URL должен быть абсолютным, а таймаут положительным, добавьте отдельные проверки этих условий. Для диагностики достаточно имён свойств и их происхождения. Значения секретов в лог выводить не нужно.

---

### 17. BeanPostProcessor и BeanFactoryPostProcessor

Это две точки расширения контейнера, и путают их постоянно. Разница — в моменте срабатывания и в том, с чем они работают.

**BeanFactoryPostProcessor** срабатывает до обычной инициализации прикладных singleton-бинов, когда контейнер прочитал только метаданные (`BeanDefinition`). Он может менять определения: добавить свойство, заменить класс, поменять scope. Внутри Spring его классические представители:

- `ConfigurationClassPostProcessor` — обрабатывает `@Configuration`, `@Bean`, `@ComponentScan`. Собственно, вся аннотационная конфигурация работает благодаря ему.
- `PropertySourcesPlaceholderConfigurer` — подставляет значения в плейсхолдеры `${...}`.

**BeanPostProcessor** срабатывает для каждого уже созданного экземпляра бина — до и после инициализации (`postProcessBeforeInitialization` / `postProcessAfterInitialization`). Может вернуть другой объект вместо исходного — именно так рождаются прокси. Внутри Spring:

- `AutowiredAnnotationBeanPostProcessor` — обрабатывает `@Autowired`, `@Value`, `@Inject`.
- `CommonAnnotationBeanPostProcessor` — `@PostConstruct`, `@PreDestroy`, `@Resource`.
- `AnnotationAwareAspectJAutoProxyCreator` — создаёт AOP-прокси на этапе postProcessAfterInitialization.

Свой BeanPostProcessor пишется так:

```java
@Component
public class TimedBeanPostProcessor implements BeanPostProcessor {

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) {
        if (bean instanceof MetricsAware) {
            ((MetricsAware) bean).initMetrics();
        }
        return bean; // возвращаем исходный бин или осознанно созданную замену
    }
}
```

Практическое следствие: если ваш бин обрабатывается `BeanPostProcessor`'ом, который сам зависит от бинов, часть бинов может остаться без постобработки — контейнер честно напишет об этом в лог ("is not eligible for getting processed by all BeanPostProcessors"). Поэтому BPP обычно делают максимально независимыми.

---

### 11. Циклические зависимости и их разрешение в Spring

Циклическая зависимость возникает, когда создание A требует B, а создание B требует A. Некоторые singleton-циклы через поля/сеттеры контейнер способен разрешить ранними ссылками, если это разрешено конфигурацией. Это не гарантия для любых scopes, прокси и комбинаций зависимостей.

Механика такая. Контейнер хранит синглтоны в трёх кэшах: `singletonObjects` (готовые бины), `earlySingletonObjects` и `singletonFactories`. Когда создаётся бин A, он сначала инстанцируется конструктором, а затем сразу регистрируется в `singletonFactories` — то есть ссылка на «сырой», ещё не заполненный A становится доступна. Когда при заполнении B потребуется A, контейнер возьмёт эту раннюю ссылку, внедрит её в B, доведёт B до готовности и вернётся к заполнению A. Цикл разрешён.

С конструктором этот трюк не работает: чтобы создать A, нужен готовый B, а чтобы создать B — готовый A. Ни один объект не существует даже в «сыром» виде, выставлять раннюю ссылку нечего. Результат — `BeanCurrentlyInCreationException`.

Что изменилось в Spring Boot 2.6+: циклические зависимости запрещены по умолчанию, приложение просто не стартует, даже если цикл раньше разрешался через сеттеры. Вернуть старое поведение можно флагом:

```properties
spring.main.allow-circular-references=true
```

На практике цикл — симптом проблемного дизайна. Варианты лечения: вынести общую логику в третий бин, внедрить одну из зависимостей лениво (`@Lazy` — внедряется прокси, реальный бин создаётся при первом обращении) или передавать зависимость через явный provider там, где отложенное получение имеет смысл. Переход к сеттерам не исправляет архитектурный цикл и при запрете circular references в Boot не является самостоятельным решением.

---

### 36. Почему @PostConstruct не подходит для запуска транзакционного фонового процесса?

`@PostConstruct` вызывается во время инициализации исходного экземпляра, до публикации готового бина с его AOP-поведением. Аннотация `@Transactional` на этом же init-методе не превращает его в обычный внешний вызов через прокси. Запуск потока из конструктора ещё опаснее: объект может стать виден до внедрения зависимостей и завершения настройки.

Например, загрузчик справочника в `@PostConstruct` вызывает собственный `@Async load()`, а затем ждёт результат. Асинхронный interceptor обходится из-за self-invocation; возможны долгий старт и неожиданный синхронный доступ к БД. Ожидание других бинов в процессе singleton-инициализации дополнительно создаёт риск взаимной блокировки при старте.

Для локальной проверки конфигурации и подготовки небольших структур `@PostConstruct` подходит. Для работы после создания обычных singleton-бинов есть `SmartInitializingSingleton`; для жизненного цикла фонового потребителя — `SmartLifecycle` с управляемыми start/stop. В Boot одноразовую стартовую задачу можно оформить как `ApplicationRunner`, вызывающий отдельный транзакционный сервис. Событие `ApplicationReadyEvent` удобно, когда нужен момент готовности приложения, но тяжёлая работа в его синхронном слушателе всё равно задерживает выполнение слушателей.

Выбор определяется тем, должна ли задача блокировать readiness, как обрабатываются её ошибки и как она завершается. Не нужно молча переводить критичную миграцию в fire-and-forget: приложение объявит готовность до появления обязательных данных. На остановке следует прекратить приём новой работы и дождаться ограниченного завершения уже принятой.

## Прокси, транзакции и доступ к данным

### 5. Аспектно-Ориентированное Программирование (AOP) в Spring

**Аспектно-Ориентированное Программирование (AOP)** позволяет отделить сквозную логику (cross-cutting concerns), такую как логирование, управление транзакциями или безопасность, от основной бизнес-логики.

Ключевые термины AOP:

- **Aspect (Аспект)** — модуль, инкапсулирующий сквозную функциональность.
- **Join Point (Точка соединения)** — точка в выполнении программы (например, вызов метода), где может быть применен аспект.
- **Advice (Совет)** — действие, выполняемое аспектом в определенной точке соединения (Before, After, AfterReturning, AfterThrowing, Around).
- **Pointcut (Срез)** — выражение, определяющее набор точек соединения, к которым должен быть применен Advice.

Spring AOP реализуется на основе проксирования (Proxy-based). При инъекции бина Spring оборачивает его в прокси-объект (JDK Dynamic Proxy или CGLIB), который перехватывает вызовы методов и применяет логику аспекта.

```java
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class LoggingAspect {

    @Around("execution(* com.example.service.*.*(..))")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.nanoTime();

        Object proceed = joinPoint.proceed(); // Вызов реального метода

        long executionTime = (System.nanoTime() - start) / 1_000_000;
        System.out.println(joinPoint.getSignature() + " выполнился за " + executionTime + " мс");
        return proceed;
    }
}
```

Частый follow-up — чем Spring AOP отличается от AspectJ. Spring AOP работает в рантайме через прокси: перехватываются вызовы доступных для конкретного прокси методов бинов извне. JDK-прокси работает через методы интерфейса; class-based прокси может перехватывать и переопределяемые protected/package-visible методы. Поля, конструкторы и self-invocation обычному Spring AOP недоступны. AspectJ — полноценный AOP-фреймворк с weaving'ом на этапе компиляции или загрузки классов (LTW через javaagent): перехватывает любые join points, включая вызовы изнутри самого объекта. Цена — сложность настройки. На практике почти всегда достаточно Spring AOP.

---

### 12. Прокси в Spring: JDK Dynamic Proxy и CGLIB, проблема self-invocation

Spring AOP, транзакции, `@Async`, `@Cacheable` — всё это работает через прокси: контейнер возвращает не ваш объект, а обёртку, которая перехватывает вызов метода и добавляет поведение (открыть транзакцию, отправить в другой поток, проверить кэш).

Два механизма проксирования:

- **JDK Dynamic Proxy** — создаёт реализацию интерфейса в рантайме через `java.lang.reflect.Proxy`. Работает только если класс реализует интерфейс.
- **CGLIB** — генерирует подкласс целевого класса в байткоде. Нужен, когда интерфейса нет. Spring Boot 2.x/3.x по умолчанию предпочитает class-based AOP-прокси (`spring.aop.proxy-target-class=true`), если применяется соответствующая автоконфигурация. Это не меняет ограничения `final` и не означает, что сервис нужно наследовать вручную.

Ограничения CGLIB: нельзя проксировать `final`-классы и `final`-методы (подкласс их не переопределит).

Главная ловушка — **self-invocation**: вызов метода бина из другого метода того же бина идёт через `this`, минуя прокси. Аннотации на внутреннем методе молча игнорируются:

```java
@Service
public class ReportService {

    public void generateAll() {
        generateOne(); // this.generateOne() — прокси обойдён, транзакция НЕ откроется
    }

    @Transactional
    public void generateOne() { ... }
}
```

Частый follow-up: как починить. Варианты:

1. Вынести `generateOne()` в отдельный бин — самый чистый способ.
2. Внедрить бин сам в себя (self-injection) и вызывать через эту ссылку.
3. `AopContext.currentProxy()` с `exposeProxy=true` — работает, но привязывает код к инфраструктуре Spring.
4. Отказаться от прокси: `TransactionTemplate` для транзакций, ручной `Executor` для асинхронности.

---

### 8. Управление транзакциями и аннотация @Transactional

В Spring управление транзакциями абстрагировано с помощью интерфейса `PlatformTransactionManager`. Основной способ управления — декларативный подход с использованием аннотации `@Transactional`. В стандартном proxy-режиме interceptor перед вызовом выбирает транзакционную границу согласно propagation, а после вызова завершает её. При `REQUIRED` это может быть участие в уже открытой транзакции, а окончательный commit выполнит внешняя граница. Правила rollback, состояние ресурса и ошибки commit определяют итог; одного нормального возврата из тела метода недостаточно.

Важные атрибуты `@Transactional`:

- `propagation`: определяет поведение транзакции (`REQUIRED` — по умолчанию, использовать текущую или создать новую; `REQUIRES_NEW` — всегда новая, текущая приостанавливается; `NESTED` — savepoint внутри текущей; `SUPPORTS`, `NOT_SUPPORTED`, `MANDATORY` — требует существующую, иначе исключение; `NEVER`).
- `isolation`: определяет уровень изоляции базы данных (`READ_COMMITTED`, `REPEATABLE_READ`, и т.д.).
- `rollbackFor`: позволяет указать checked-исключения, при которых также должен происходить откат транзакции (по умолчанию откат только на `RuntimeException` и `Error`).
- `readOnly`: сигнал о режиме чтения. При интеграции с Hibernate Spring может менять flush mode и режим read-only сессии для снижения накладных расходов. Точное поведение зависит от менеджера и провайдера; переносимой гарантии запрета записи нет.
- `timeout`: максимальная длительность транзакции в секундах.

```java
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class TransferService {

    @Transactional(propagation = Propagation.REQUIRES_NEW, isolation = Isolation.SERIALIZABLE)
    public void transferMoney(String fromAccount, String toAccount, BigDecimal amount) {
        // Логика перевода. При выбросе исключения, изменения в БД будут отменены.
        accountRepository.debit(fromAccount, amount);
        accountRepository.credit(toAccount, amount);
    }
}
```

Ловушки, о которых спрашивают почти всегда:

- Прокси перехватывает только внешние вызовы. Self-invocation — вызов `@Transactional`-метода из другого метода того же бина — не применит настройки внутренней аннотации. Уже открытая внешняя транзакция продолжит действовать (подробно в вопросе 12).
- Аннотация на `private`-методе молча игнорируется.
- Если исключение перехвачено внутри метода, interceptor видит нормальный возврат и может попытаться выполнить commit. Однако ресурс или вложенный участник могли уже пометить транзакцию rollback-only. Симметричная проблема: вложенный `@Transactional`-участник пометил общую транзакцию rollback-only, вы поймали его исключение и пошли дальше — на коммите получите `UnexpectedRollbackException`.
- Для современных proxy-based конфигураций Spring 6.x аннотации интерфейса поддерживаются и с class-based прокси. Практичнее аннотировать конкретный класс/метод; AspectJ weaving не наследует аннотации интерфейсов как аннотации реализации. Начиная со Spring 6.0 class-based транзакционный прокси по умолчанию поддерживает также protected/package-visible методы; интерфейсные вызовы через JDK-прокси должны быть public.

---

### 29. На подумать: почему REQUIRES_NEW внутри того же сервиса не спас аудит?

Условие: внешний вызов `place()` проходит через Spring-прокси. Оба метода находятся в одном бине; после записи аудита `place()` выбрасывает исключение. Сохранится ли аудит?

```java
@Transactional
public void place() {
    orders.save(order);
    audit();
    throw new IllegalStateException("fail");
}

@Transactional(propagation = Propagation.REQUIRES_NEW)
public void audit() {
    auditRepository.save(entry);
}
```

<details>
<summary>Разбор</summary>

Аудит откатится вместе с заказом, если репозиторий использует тот же транзакционный ресурс. Вызов `audit()` идёт через `this`, поэтому транзакционный interceptor внутреннего метода не вызывается. Открыта одна физическая транзакция, начатая прокси для `place()`. Ошибочно говорить, что «внутри вообще нет транзакции»: существующая транзакция продолжает действовать, игнорируется именно настройка внутренней границы.

Чтобы аудит фиксировался независимо, выделяют `AuditService` и вызывают его внедрённый прокси либо используют отдельный `TransactionTemplate` с нужным propagation. После исправления аудит может пережить откат заказа. Его содержание должно отражать попытку операции, а не ложно утверждать, что заказ создан. Проверку делают интеграционным тестом: вызывают внешний бин, ловят ошибку за транзакционной границей, читают обе таблицы в новой транзакции.

Изменение propagation не решает все проблемы. Независимая транзакция обычно требует ещё одно соединение, а запись со ссылкой на незакоммиченный заказ может блокироваться или нарушать внешний ключ. Не следует лечить self-invocation включением circular dependencies и self-injection по всему приложению: явная граница операции понятнее и проще проверяется.

</details>

---

### 30. Почему после перехваченного исключения возникает UnexpectedRollbackException?

Перехват Java-исключения не снимает признак `rollback-only` у уже испорченной транзакции. При `REQUIRED` несколько вызовов через разные прокси образуют логические границы, но могут использовать одну физическую транзакцию. Внутренний interceptor видит `RuntimeException` и помечает общую транзакцию для отката. Внешний метод ловит ошибку, нормально возвращается, однако подтвердить изменения уже нельзя.

```java
@Transactional
public void checkout() {
    orders.save(order);
    try {
        inventory.reserve(); // другой бин, REQUIRED
    } catch (OutOfStockException ex) {
        log.info("Резерв не получен");
    }
}
```

На завершении внешней границы вызывающая сторона получает `UnexpectedRollbackException`, чтобы не принять откат за успешный commit. Сама СУБД или JPA-провайдер тоже могут перевести транзакцию в состояние, в котором продолжать запись нельзя. Поэтому утверждение «поймали исключение — всё обязательно закоммитится» неверно.

Выбор исправления зависит от смысла операции. Если заказ без резерва недопустим, ошибку возвращают вызывающей стороне и откатывают всё. Если отказ резерва — ожидаемый бизнес-результат, его можно выразить значением результата до аварийной записи; `noRollbackFor` уместен только при действительно допустимом commit и работоспособном ресурсе. Для независимой операции рассматривают отдельную транзакцию, но принимают её последствия для атомарности. После ошибки constraint не стоит выполнять несколько новых SQL «на удачу»: сначала нужно завершить неуспешную транзакцию.

---

### 31. Как checked exceptions и rollbackFor влияют на результат транзакции?

В стандартной конфигурации Spring откатывает декларативную транзакцию при вышедших через границу `RuntimeException` и `Error`. Проверяемое исключение само по себе означает commit, если ресурс не потребовал откат. Тип исключения здесь — техническое правило, которое не всегда совпадает с бизнес-смыслом отказа.

```java
@Transactional(rollbackFor = IOException.class)
public void importPayments(Path file) throws IOException {
    payments.save(header);
    parser.readRemaining(file); // IOException должна отменить header
}
```

Без `rollbackFor` запись заголовка могла бы сохраниться, хотя импорт не завершён. Правило нужно проверять у внешнего вызывающего кода: исключение из тела метода и итог транзакции — разные наблюдения. Если метод перехватил `IOException` и вернул статус, interceptor вообще не увидит ошибку; правило `rollbackFor` не анализирует лог-файлы. Тогда либо возвращают исключение через границу, либо явно управляют результатом транзакции.

В Spring Framework 6.2 появился глобальный выбор `@EnableTransactionManagement(rollbackOn = RollbackOn.ALL_EXCEPTIONS)`. Это полезная согласованная политика, но её изменение в существующем сервисе меняет поведение checked exceptions и требует проверки сценариев, ранее сохранявших частичный результат. Явные правила конкретной транзакции учитываются поверх выбранного значения по умолчанию.

Предпочитайте типы в `rollbackFor`/`noRollbackFor`, а не строковые шаблоны имён исключений: слишком широкий шаблон может случайно совпасть с другим классом. Откат БД не отменяет уже отправленный HTTP-запрос или письмо — для таких эффектов нужны отдельные гарантии доставки и компенсации.

---

### 32. На подумать: почему REQUIRES_NEW исчерпал пул соединений?

Условие: десять рабочих потоков уже открыли внешние JDBC-транзакции и удерживают по соединению. Размер пула — десять. Затем каждый поток синхронно вызывает другой бин с `REQUIRES_NEW`. База почти не загружена, но запросы зависают. Где узкое место?

<details>
<summary>Разбор</summary>

Приостановка внешней транзакции не возвращает её соединение в пул. Внутренней независимой транзакции требуется своё соединение, а свободных нет. Все десять потоков ждут ресурс, который сами освободят только после завершения внутренних вызовов. Обычно ожидание заканчивается timeout получения соединения; отсутствие высокой загрузки CPU или БД не опровергает проблему.

Для этого конкретного сценария один дополнительный слот позволяет внутренним операциям проходить последовательно, если нет других блокировок. Это минимум для прогресса при заданных условиях, а не универсальная формула производительности. При глубокой вложенности, других потребителях пула и требованиях к задержке нужен иной расчёт. Увеличение пула до сотен соединений без проверки возможностей БД тоже опасно.

Диагностика сопоставляет active/pending connections, время ожидания пула и thread dump. Нагрузочный тест должен одновременно доводить потоки до внутреннего вызова, иначе одиночный тест не воспроизведёт насыщение. Исправления: убрать ненужную независимую границу, сократить внешнюю транзакцию, ограничить конкурентность либо обоснованно увеличить пул. Если внутренний аудит ждёт блокировку, удерживаемую внешней транзакцией, даже свободное соединение не поможет. Поэтому сначала рисуют границы ресурсов и блокировок, а уже затем меняют численные настройки.

</details>

---

### 33. Чем NESTED отличается от REQUIRES_NEW при частичном откате?

`NESTED` обычно означает savepoint в той же физической JDBC-транзакции. Откат внутреннего участка возвращает состояние БД к savepoint; успешный участок ещё не становится долговечным. Если внешняя транзакция позже откатится, исчезнут и его изменения. `REQUIRES_NEW` использует независимую физическую транзакцию, чей commit может пережить откат внешней.

Например, импорт содержит заголовок и необязательную строку. В JDBC-сервисе внутренний вызов через другой бин с `NESTED` может откатить невалидную строку, а внешний — сохранить заголовок и остальные строки. Для аудита, который должен сохраниться даже после общего отказа импорта, такой savepoint не подходит: независимость даёт другое поведение, а не просто другое имя аннотации.

Поддержка зависит от менеджера транзакций, драйвера и используемого ресурса. `DataSourceTransactionManager` работает с JDBC savepoints. Нельзя автоматически переносить это ожидание на JPA: savepoint базы не обязан откатывать состояние уже загруженных сущностей в persistence context. Получится расхождение между объектами в памяти и таблицами. Возможности `JpaTransactionManager` следует проверять для конкретного пути доступа, а не считать вложенные JPA-транзакции гарантированной функцией.

Частая ошибка — назвать `NESTED` «новой независимой транзакцией с меньшими накладными расходами». Важнее спросить, какие данные должны пережить внешний rollback. Если требуется продолжать обработку после отдельных ошибок, иногда проще сделать каждую запись отдельной верхнеуровневой транзакцией и хранить прогресс импорта явно.

---

### 34. Почему внутренняя аннотация isolation или timeout может ничего не изменить?

При `REQUIRED` метод, присоединившийся к существующей физической транзакции, обычно наследует её характеристики. Объявленный у него `SERIALIZABLE` не превращает уже открытую `READ_COMMITTED`-транзакцию в более строгую. Аналогично локальный timeout не обязан стать новым таймером для всего вызова.

```java
@Transactional(isolation = Isolation.READ_COMMITTED)
public void reserve() {
    stock.checkAndUpdate(); // другой бин, REQUIRED + SERIALIZABLE
}
```

Если автор `stock` рассчитывал на сериализацию, операция может нарушить инвариант под конкуренцией. Сначала находят фактическую внешнюю границу и проверяют SQL, блокировки и ограничения базы. В менеджерах, наследующих `AbstractPlatformTransactionManager`, настройка `setValidateExistingTransaction(true)` помогает выявить несовместимые объявления, вместо молчаливого присоединения. Она не повышает уровень автоматически и не заменяет тест с конкурирующими транзакциями.

`readOnly` также не является переносимым запретом SQL-записи. Это сигнал менеджеру и ресурсу для оптимизации; фактическая защита зависит от драйвера и настроек СУБД. Для запрета нужны проверенные гарантии самой базы или права доступа.

Отдельная ловушка: transaction timeout не равен общему deadline HTTP-запроса. Он применяется конкретным менеджером и ресурсами, может влиять на SQL statements, но не обязан прерывать произвольный Java-цикл или сетевой вызов. Таймауты клиента, ожидания пула и БД согласуют по бюджету операции; транзакцию не держат открытой, пока медленный внешний сервис думает над ответом.

---

### 35. Когда TransactionTemplate делает границу операции понятнее аннотации?

`TransactionTemplate` полезен, когда транзакция занимает только небольшой фрагмент метода либо её результат нужно обработать после фактического commit. Он выполняет callback через `PlatformTransactionManager`; вызов через Spring AOP-прокси для этого не нужен. Это позволяет убрать проблему self-invocation, но не избавляет от выбора propagation и ресурсов.

```java
public Receipt process(Command command) {
    RemoteData data = client.fetch(command.id());
    return transactionTemplate.execute(status -> {
        Order order = repository.save(toOrder(data));
        return new Receipt(order.getId());
    });
}
```

Сеть здесь вызывается до открытия транзакции, поэтому соединение не удерживается во время ожидания HTTP. При стандартном `REQUIRED` и отсутствии внешней транзакции callback и commit завершаются до возврата `execute()`. Ошибку commit можно обработать вокруг этого вызова. Если внешняя транзакция уже существует, шаблон может присоединиться к ней, и окончательный commit случится позже — это принципиальное ограничение примера.

У `TransactionTemplate` общая конфигурация, поэтому её задают при создании, а не меняют propagation в singleton-объекте перед каждым вызовом: конкурентные запросы могут повлиять друг на друга. Для другой политики создают отдельный настроенный экземпляр.

Частая ошибка — выполнить внешнее списание внутри callback и считать его атомарным с БД. При rollback удалится строка, но не сетевой эффект. Для согласования независимых систем нужны идемпотентность, outbox или компенсации. Для реактивного менеджера применяется `TransactionalOperator`, а не блокирующий шаблон поверх `Mono`.

---

### 14. Spring Data JPA: derived queries, @Query, проекции и пагинация

Spring Data JPA генерирует реализацию репозиториев сама: вы объявляете интерфейс, наследующий `JpaRepository`, а запросы строятся из имён методов или аннотаций.

**Derived query methods** — запрос парсится из имени метода:

```java
public interface UserRepository extends JpaRepository<User, Long> {

    List<User> findByStatusAndCreatedAtAfter(Status status, LocalDateTime date);

    Optional<User> findFirstByEmailOrderByCreatedAtDesc(String email);

    long countByStatus(Status status);
}
```

**@Query** — когда имя метода стало нечитаемым или нужен join/fetch. JPQL по умолчанию, `nativeQuery = true` для чистого SQL:

```java
@Query("select u from User u join fetch u.orders where u.status = :status")
List<User> findActiveWithOrders(@Param("status") Status status);

@Query(value = "select * from users where age > ?1", nativeQuery = true)
List<User> findAdults(int minAge);

@Modifying
@Query("update User u set u.status = :status where u.id = :id")
int updateStatus(@Param("id") Long id, @Param("status") Status status);
```

`@Modifying`-запросы требуют транзакции и по умолчанию не синхронизируют persistence context — после них контекст может содержать устаревшие сущности (лечится `clearAutomatically = true`).

**Проекции** — способ не тянуть всю сущность. Interface-based проекция: объявляете интерфейс с геттерами, Spring создаёт прокси, Hibernate сам подставит нужные колонки в SELECT. DTO-проекция — через `new` в JPQL (`select new com.example.UserDto(u.name, u.email) ...`).

**Пагинация** — через `Pageable`:

```java
Page<User> findByStatus(Status status, Pageable pageable);

// вызов
Page<User> page = userRepository.findByStatus(
        Status.ACTIVE, PageRequest.of(0, 20, Sort.by("createdAt").descending()));
```

`Page` предоставляет общее число элементов и может потребовать дополнительный count-запрос; некоторые случаи Spring Data оптимизирует без него. На больших таблицах такой count бывает дорогим. Если нужна только кнопка «далее», берите `Slice` — он читает на одну запись больше лимита и не делает count.

---

### 47. Как составить поиск с необязательными фильтрами и сохранить ограничение по организации?

В Spring Data JPA `Specification<T>` описывает условие Criteria-запроса. Для поиска заказов сформируйте обязательный фильтр по организации пользователя и добавляйте к нему выбранные ограничения. Репозиторий для такого вызова реализует `JpaSpecificationExecutor<Order>`.

```java
Specification<Order> spec = (root, query, cb) ->
        cb.equal(root.get("tenantId"), currentTenantId);
if (status != null) {
    spec = spec.and((root, query, cb) -> cb.equal(root.get("status"), status));
}
return orders.findAll(spec, pageable);
```

Когда поиск допускает «открытый заказ или заказ с пометкой», группируйте условия как `tenant AND (open OR flagged)`. Выражение `(tenant AND open) OR flagged` вернёт помеченные заказы других организаций. Проверьте результат на двух организациях с одинаковыми статусами и флагами. Идентификатор организации берите из проверенного контекста доступа.

---

### 26. Lombok: что генерирует и ловушки с JPA-сущностями

Lombok — это annotation processor: на этапе компиляции он дописывает код прямо в AST, и в байткод попадает обычный Java-код, как будто вы написали его руками. В рантайме Lombok не участвует. Побочное следствие — IDE без плагина не видит сгенерированные методы и подчёркивает код красным.

Основные аннотации:

- `@Data` — комбо: геттеры и сеттеры на все поля, `toString()`, `equals()`/`hashCode()` по всем полям, конструктор для `final`-полей.
- `@Value` — immutable-вариант: класс и поля становятся `final`, генерируются только геттеры, конструктор на всё, `equals`/`hashCode`/`toString`. Удобен для DTO.
- `@Builder` — паттерн «строитель»: `UserDto.builder().name("a").build()`. На пару с `@Value` даёт удобный конструктор для immutable-объектов.
- `@NoArgsConstructor`, `@AllArgsConstructor`, `@RequiredArgsConstructor` — конструкторы; `@Slf4j` — поле `log`.

На DTO это всё безобидно и экономит кучу кода. Проблемы начинаются, когда `@Data` вешают на JPA-сущность — на собеседовании это вопрос с подвохом, и ждут, что вы назовёте обе ловушки.

**Ловушка 1: `equals()`/`hashCode()` по всем полям.** Во-первых, в «все поля» попадают ленивые коллекции и связи — вызов `equals()` за пределами транзакции ловит `LazyInitializationException`, а внутри транзакции молча грузит полбазы. Во-вторых, если сравнение основано только на nullable id, две новые сущности с `id = null` могут ошибочно оказаться равны. У `@Data` сравниваются и другие включённые поля, поэтому одного совпадения null-id ещё недостаточно для такого вывода. А после сохранения `id` появляется, `hashCode()` меняется — и сущность «теряется» в `HashSet`, куда её положили до сохранения. Рабочее правило: `equals`/`hashCode` по бизнес-ключу (email, номер договора), либо по `id` с константным `hashCode()` (чтобы он не менялся), либо вообще не переопределять.

**Ловушка 2: `toString()` по всем полям.** Тот же механизм: `toString()` трогает ленивые коллекции → `LazyInitializationException` вне транзакции или каскад запросов и N+1 внутри. А при двунаправленных связях `toString()` ещё и зацикливается: `Order` печатает `Customer`, тот печатает заказы. Лечится `@ToString.Exclude` на связях и коллекциях, но проще не создавать проблему.

Плюс `@Data` генерирует сеттеры на всё подряд, что ломает инкапсуляцию сущности — менять состояние должны бизнес-методы, а не `setStatus()` откуда попало.

Практический итог, который обычно озвучивают на собеседовании: на сущностях — минимальный набор (`@Getter`, `@NoArgsConstructor`, при необходимости `@Setter` на отдельные поля), `equals`/`hashCode`/`toString` писать осознанно; `@Data`/`@Value`/`@Builder` — для DTO, где этих проблем нет.

## Асинхронность, события и кэш

### 13. Асинхронное и плановое выполнение: @Async и @Scheduled

`@Async` передаёт выполнение исполнителю, `@Scheduled` регистрирует запуск по расписанию. Поддержку включают `@EnableAsync` и `@EnableScheduling`. В стандартном режиме `@Async` работает через AOP-прокси, поэтому self-invocation его обходит. `@Scheduled` обнаруживается отдельным bean post-processor и регистрируется в scheduler: это не тот же механизм проксирования. Прямой вызов scheduled-метода просто выполняет обычный метод, а не планирует новый запуск. Для обычного scheduled-метода нужны отсутствие аргументов и, как правило, игнорируемый результат; public не является универсальным требованием.

```java
@Service
public class MailService {

    @Async
    public CompletableFuture<Void> sendReport(String email) {
        // выполнится в потоке из пула
        return CompletableFuture.completedFuture(null);
    }

    @Scheduled(fixedDelay = 60_000) // каждые 60 секунд после завершения предыдущего запуска
    public void cleanupTempFiles() { ... }

    @Scheduled(cron = "0 0 3 * * *") // каждый день в 03:00
    public void nightlyJob() { ... }
}
```

Пулы потоков — здесь кроется основной подвох:

- В типичной автоконфигурации Boot 3.x без virtual threads `@Async` использует `ThreadPoolTaskExecutor` (`applicationTaskExecutor`): core pool 8 потоков, очередь по умолчанию неограниченная. Пользовательский executor или `AsyncConfigurer` меняет выбор. При Java 21+ и `spring.threads.virtual.enabled=true` Boot 3.2+ использует другую стратегию исполнения на virtual threads. Это значит, что при нагрузке задачи копятся в очереди, а потоки сверх восьми не создаются — можно получить часовую задержку вместо параллелизма.
- Если Spring не нашёл настроенный executor (уникальный `TaskExecutor` либо `Executor` с именем `taskExecutor`), fallback — `SimpleAsyncTaskExecutor`. В обычном режиме он создаёт поток на задачу, а не ограниченный пул. При высокой конкурентности без ограничения это может исчерпать ресурсы.
- Без пользовательского scheduler обычная конфигурация использует один scheduler-поток: долгий запуск задерживает другие задачи на нём. Стратегия меняется при пользовательском scheduler и virtual threads, поэтому фактическую конфигурацию проверяют по версии Boot и настройкам.

На практике обычно объявляют свой пул явно:

```java
@Bean(name = "mailExecutor")
public Executor mailExecutor() {
    ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
    executor.setCorePoolSize(4);
    executor.setMaxPoolSize(8);
    executor.setQueueCapacity(100);
    executor.initialize();
    return executor;
}

@Async("mailExecutor")
public CompletableFuture<Void> sendReport(String email) { ... }
```

Для `@Scheduled` размер пула задаётся через `spring.task.scheduling.pool.size`. Ещё одно ограничение: исключение из `@Async`-метода, возвращающего `void`, нельзя получить вызывающей стороне как обычное исключение; по умолчанию оно логируется — для осмысленной обработки нужен `AsyncUncaughtExceptionHandler` или возврат `Future`.

---

### 40. Что теряется на границе @Async: транзакция, MDC и SecurityContext?

У обычного императивного Spring-транзакционного менеджера транзакция привязана к потоку. Передача задачи исполнителю не переносит в новый поток соединение, persistence context и незавершённую транзакцию. Если асинхронному обработчику нужна БД, он начинает собственную транзакционную операцию через отдельный сервис. Передавать ему управляемую JPA-сущность с lazy-связями рискованно; лучше передать id или неизменяемый снимок данных.

Например, `create()` сохраняет заказ и сразу вызывает другой бин `@Async notify(id)`. Задача может начать чтение до commit и не найти строку. Решение — запуск после успешного commit, а при требовании надёжной доставки — запись задания/outbox в той же транзакции. Один executor не даёт долговечности: процесс может завершиться между commit и выполнением задачи.

MDC и `SecurityContext` требуют отдельного решения. Для MDC применяют `TaskDecorator`, который захватывает нужные значения при отправке и восстанавливает предыдущее состояние рабочего потока в `finally`. Для безопасности существуют `DelegatingSecurityContextAsyncTaskExecutor` и родственные обёртки Spring Security. Простое наследование ThreadLocal не решает переиспользование потоков пула.

Переносить нужно только необходимый контекст, понимая его срок жизни. Идентификатор пользователя как данные команды и полномочия выполнения — разные вещи; права на отложенную операцию могут измениться. Ошибки результата `CompletableFuture` необходимо наблюдать, а для `void` требуется обработчик ошибок. Ошибка фоновой задачи уже не может откатить ранее подтверждённый HTTP-запрос.

---

### 42. Как обеспечить единственный запуск @Scheduled в нескольких экземплярах приложения?

`@Scheduled` планирует выполнение внутри каждого ApplicationContext. Два экземпляра приложения обычно запустят одну и ту же задачу независимо. Увеличение или уменьшение пула scheduler не создаёт координацию между процессами. Даже один экземпляр может повторить работу после перезапуска, если результат предыдущего запуска неизвестен.

Например, три pod каждую минуту выбирают просроченные платежи и отправляют уведомления. Проверка `if (!sent)` перед отправкой не защищает от гонки: несколько исполнителей прочитают одно состояние одновременно. Рабочие варианты — атомарный захват записей в БД с lease, распределённая блокировка с ограничением срока, либо внешний scheduler, создающий устойчивые задания. После захвата каждое действие всё равно проектируют идемпотентным по ключу платежа и типу уведомления.

При lease важны время истечения, продление, аварийное завершение и защита от старого владельца, который продолжил работу после потери аренды. Одного обещания «блокировку держим пять минут» недостаточно, если задача может работать десять. Для критичных операций применяют проверку актуального владельца/версии на стороне ресурса.

`fixedDelay` задаёт задержку между завершением и новым запуском данного расписания, а cron — календарные моменты; часовой пояс указывают явно через `zone`, если он относится к бизнес-правилу. На собеседовании стоит разделять две гарантии: один текущий исполнитель и один бизнес-эффект. Первая сама по себе не даёт вторую при сбоях и повторах.

---

### 18. События в Spring: ApplicationEvent и @EventListener

События — способ слабо связать компоненты: один бин публикует факт («заказ создан»), другие реагируют, не зная об издателе.

```java
public record OrderCreatedEvent(Long orderId) {}

@Service
public class OrderService {
    private final ApplicationEventPublisher publisher;

    public OrderService(ApplicationEventPublisher publisher) {
        this.publisher = publisher;
    }

    public void create() {
        publisher.publishEvent(new OrderCreatedEvent(42L));
    }
}

@Component
public class NotificationListener {

    @EventListener
    public void onOrderCreated(OrderCreatedEvent event) {
        // отправить уведомление
    }
}
```

По умолчанию слушатель выполняется синхронно в потоке издателя, и исключение в слушателе улетит обратно в издателя. `@Async` на слушателе делает обработку асинхронной, `@Order` управляет порядком нескольких слушателей. Также можно вернуть из слушателя новое событие — контейнер опубликует его (цепочка событий).

Классическая проблема — событие внутри транзакции. `@EventListener` сработает сразу при публикации, то есть до коммита: слушатель может уйти в другой сервис за данными, которых в БД ещё нет, или отправить письмо о заказе, который потом откатится. Решение — `@TransactionalEventListener` с фазами:

```java
@TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
public void onOrderCreated(OrderCreatedEvent event) { ... }
```

Фазы: `AFTER_COMMIT` (по умолчанию — только если транзакция успешно закоммичена), `AFTER_ROLLBACK`, `AFTER_COMPLETION` (в любом случае, аналог finally), `BEFORE_COMMIT` (ещё можно дозаписать в ту же транзакцию). Если транзакции не было вообще, слушатель по умолчанию не выполнится — изменить это можно флагом `fallbackExecution = true`.

---

### 41. Почему AFTER_COMMIT ещё не гарантирует сохранение изменений и доставку события?

`@TransactionalEventListener(AFTER_COMMIT)` означает, что слушатель вызывается после успешного commit. Это устраняет преждевременное уведомление о ещё неподтверждённых данных, но не делает обработку отдельной надёжной транзакцией. Ресурсы завершённой транзакции могут ещё быть доступны; запись, выполненная через них без новой границы, не получит нового commit автоматически.

```java
@TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
public void on(OrderCreated event) {
    deliveryService.createTask(event.orderId());
}

// В отдельном бине:
@Transactional(propagation = Propagation.REQUIRES_NEW)
public void createTask(long orderId) {
    tasks.save(new DeliveryTask(orderId));
}
```

Новая транзакция делает запись задания самостоятельной, однако остаётся окно отказа: процесс может упасть после commit заказа и до записи задания. Для обязательной доставки заказ и outbox-запись сохраняют атомарно в исходной БД-транзакции, а отдельный worker повторяет публикацию. Потребитель должен быть идемпотентным, потому что повтор после сбоя может дать дубль.

Если активной транзакции нет, такой слушатель по умолчанию не вызывается; `fallbackExecution = true` меняет это поведение и требует осознанной семантики. Ошибка слушателя после commit уже не отменяет заказ. В стандартном императивном пути AFTER_COMMIT обрабатывается через callback afterCompletion: его ошибка логируется и не является надёжным каналом отказа вызывающему коду. Нужны отдельное наблюдение за обработкой и политика повторов, а при требовании доставки — устойчивое задание.

Частая ошибка — считать события `ApplicationEventPublisher` очередью сообщений. Внутрипроцессное событие не имеет журнала и подтверждения доставки. Выбор между обычным listener, after-commit listener и outbox зависит от допустимой потери, а не от количества аннотаций.

---

### 19. Spring WebFlux: реактивная модель, Mono и Flux

WebFlux — реактивный веб-стек Spring на Project Reactor. Вместо модели «поток на запрос» (Tomcat, пул из ~200 потоков, поток блокируется на каждом обращении к БД или внешнему API) здесь event loop: небольшое число потоков обслуживает тысячи соединений, а ожидание I/O не занимает поток. Сервер по умолчанию — Netty.

Два базовых типа Reactor:

- `Mono<T>` — асинхронный результат из 0..1 элемента.
- `Flux<T>` — поток из 0..N элементов.

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/{id}")
    public Mono<UserDto> getById(@PathVariable Long id) {
        return userRepository.findById(id)
                .map(UserDto::from)
                .switchIfEmpty(Mono.error(new UserNotFoundException(id)));
    }

    @GetMapping(produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public Flux<UserDto> streamAll() {
        return userRepository.findAll(); // Flux отдаёт элементы по мере готовности
    }
}
```

Ключевой момент для обычной холодной цепочки Reactor: работа запускается при подписке (`subscribe()`). Hot-источники могут генерировать данные независимо, а код, выполненный до сборки цепочки, уже мог произвести эффект. В WebFlux подписку делает сам фреймворк, когда контроллер вернул `Mono`/`Flux`. Вызов `block()` внутри реактивного конвейера убивает весь смысл модели.

**Backpressure** — обратное давление: подписчик сообщает издателю, сколько элементов готов принять (`request(n)`), и издатель не выдаёт больше. Это позволяет согласовывать спрос, если все участники поддерживают протокол. `limitRate` меняет размер запросов, а `onBackpressureBuffer/Drop/Latest` задают поведение при избытке. Неограниченный buffer может сам исчерпать память; backpressure не делает произвольный источник автоматически безопасным.

Когда WebFlux оправдан: высокая конкурентность на I/O-bound задачах (прокси, gateway, агрегаторы внешних API), стриминг (SSE, WebSocket), тысячи одновременных соединений при ограниченной памяти. Когда нет: обычный CRUD поверх JPA/JDBC — драйверы блокирующие, R2DBC имеет менее зрелую экосистему; CPU-bound логика реактивности не ускоряет; команда без опыта получит сложную отладку (стектрейсы реактивных цепочек — боль). Реактивность масштабирует количество одновременных запросов, но не делает один запрос быстрее.

---

### 16. Кэширование: @Cacheable, @CacheEvict и @CachePut

Абстракция кэша включается `@EnableCaching` и работает через прокси (да, self-invocation снова не сработает). Три основные аннотации:

- `@Cacheable` — сначала ищет в кэше по ключу; если есть — возвращает из кэша, метод не выполняется; если нет — выполняет и кладёт результат.
- `@CachePut` — метод выполняется всегда, результат обновляет кэш. Аннотация сама по себе не делает кэш атомарным с БД: при rollback или конкурирующих обновлениях согласованность нужно проектировать отдельно.
- `@CacheEvict` — удаляет запись из кэша. `allEntries = true` чистит весь кэш, `beforeInvocation = true` удаляет до выполнения метода (полезно, если метод может упасть и запись надо инвалидировать в любом случае).

```java
@Service
public class ProductService {

    @Cacheable(value = "products", key = "#id", condition = "#id > 0",
               unless = "#result == null")
    public Product getById(Long id) { ... }

    @CachePut(value = "products", key = "#product.id")
    public Product update(Product product) { ... }

    @CacheEvict(value = "products", allEntries = true)
    public void evictAll() { ... }
}
```

Ключ строится по параметрам (по умолчанию — все аргументы через `SimpleKey`), в `key` доступен SpEL: `#id`, `#product.id`, `#result` (после выполнения, например в `unless`, ключах `CachePut` и `CacheEvict` при удалении после вызова). `condition` решает, кэшировать ли вызов вообще; `unless` — фильтрует уже полученный результат (не класть null — типичное применение).

Провайдеры:

- Простой in-memory провайдер (`ConcurrentMapCacheManager`) — без автоматического TTL и ограничения размера. Подходит там, где жизненный цикл и объём данных контролируются; для произвольного потока ключей опасен неограниченным ростом.
- **Caffeine** — in-memory кэш с TTL, лимитом размера и статистикой. Стандарт для одноинстансовых приложений.
- **Redis** — общий кэш для нескольких экземпляров. Он нужен, когда требуется общее состояние кэша; локальный Caffeine на каждом экземпляре тоже допустим при подходящей политике устаревания и инвалидации. Настраивается через `RedisCacheManager`, TTL — на уровне кэша:

```java
@Bean
public RedisCacheManager cacheManager(RedisConnectionFactory factory) {
    RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
            .entryTtl(Duration.ofMinutes(10));
    return RedisCacheManager.builder(factory).cacheDefaults(config).build();
}
```

Ловушка распределённого кэша: данные в Redis не умирают вместе с приложением, поэтому после рефакторинга класса старые сериализованные записи могут не десериализоваться — версионируйте ключи.

## Веб-слой и HTTP-клиенты

### 6. Spring MVC и паттерн Front Controller (DispatcherServlet)

Spring MVC построен вокруг паттерна **Front Controller**. Роль Front Controller выполняет класс `DispatcherServlet`. Он является единой точкой входа для всех HTTP-запросов и управляет потоком их обработки.

**Архитектура обработки запроса:**

1. `DispatcherServlet` получает входящий HTTP-запрос.
2. Обращается к `HandlerMapping`, чтобы определить, какой `Controller` должен обработать данный запрос.
3. `HandlerAdapter` вызывает соответствующий метод контроллера.
4. Контроллер выполняет бизнес-логику и возвращает объект `ModelAndView` (содержащий логическое имя представления и данные).
5. `DispatcherServlet` передает логическое имя `ViewResolver`, который определяет конкретное представление (например, JSP, Thymeleaf).
6. Представление рендерится и отправляется клиенту.

В современных RESTful-приложениях этапы 4, 5 и 6 заменяются аннотацией `@ResponseBody` (входящей в состав `@RestController`), при которой данные сразу конвертируются в JSON с помощью `HttpMessageConverter` (например, Jackson).

Ещё один уровень детализации, который иногда спрашивают: привязку параметров метода контроллера (`@PathVariable`, `@RequestParam`, `@RequestBody`) выполняют `HandlerMethodArgumentResolver`'ы, а конвертацию тела запроса и ответа — `HttpMessageConverter`'ы (`MappingJackson2HttpMessageConverter` для JSON). Оба механизма расширяемы: можно написать свой resolver для кастомной аннотации.

---

### 10. Создание RESTful API: @RestController, обработка исключений и валидация

В Spring Boot создание REST API реализуется с помощью аннотации `@RestController`, которая объединяет `@Controller` и `@ResponseBody`.

**Обработка исключений:**
Глобальная обработка ошибок осуществляется с помощью механизма Controller Advice (аннотация `@RestControllerAdvice`). Это позволяет перехватывать исключения во всем приложении и возвращать стандартизированные ответы (например, объекты `ApiError`).

**Валидация:**
Для валидации входящих данных используется спецификация Bean Validation (Hibernate Validator) и аннотация `@Valid`.

```java
import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.*;
import jakarta.validation.Valid;

@RestController
@RequestMapping("/api/users")
public class UserController {

    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    public UserDto createUser(@Valid @RequestBody UserRegistrationRequest request) {
        return userService.registerUser(request);
    }
}

// Глобальный обработчик исключений
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(UserNotFoundException.class)
    @ResponseStatus(HttpStatus.NOT_FOUND)
    public ApiError handleUserNotFound(UserNotFoundException ex) {
        return new ApiError(404, ex.getMessage());
    }
}
```

Частые follow-up:

- Разница аннотаций параметров: `@PathVariable` — переменная из пути (`/users/{id}`), `@RequestParam` — query-параметр (`?page=2`), `@RequestBody` — тело запроса, `@RequestHeader` — заголовок.
- Для управления статусом и заголовками ответа вместо `@ResponseStatus` возвращают `ResponseEntity<T>`.
- Начиная со Spring 6 / Boot 3 есть встроенная поддержка Problem Details for HTTP APIs (RFC 9457, заменивший RFC 7807): `ProblemDetail` и интерфейс `ErrorResponse` дают стандартизированный формат ошибок `application/problem+json`, свой `ApiError` городить необязательно.

---

### 46. Почему отсутствие идентификатора в пути даёт 404, а неверный тип значения может дать 400?

Сначала Spring MVC выбирает маршрут, затем связывает входные значения с аргументами метода. Пусть единственный маршрут к заказу — `GET /orders/{id}`, а аргументы контроллера — `@PathVariable("id") long id` и `@RequestParam(name = "verbose", defaultValue = "false") boolean verbose`.

У `GET /orders` нет сегмента для этого маршрута. Если другого обработчика нет, результат — 404. Запрос `GET /orders/abc` подходит под шаблон, но строку `abc` нельзя преобразовать в `long`. При стандартной обработке это 400. Запрос `GET /orders/7` дойдёт до метода с `id = 7` и `verbose = false`. Если заказ 7 отсутствует, вернуть 404 по результату поиска должен уже сам API.

`required = false` у аргумента не добавляет новый маршрут. Для списка заказов и карточки заказа обычно удобнее два отдельных метода. В MVC-тестах проверьте отсутствие маршрута, ошибку преобразования и отсутствие объекта по отдельности.

---

### 20. Валидация: Bean Validation, @Valid и @Validated

Валидация входных данных в Spring строится на спецификации Jakarta Bean Validation, эталонная реализация — Hibernate Validator (приезжает со `spring-boot-starter-validation`).

Стандартные констрейнты ставятся на поля DTO: `@NotNull`, `@NotBlank`, `@Size`, `@Min/@Max`, `@Email`, `@Pattern`, `@Positive`. Чтобы они сработали, аргумент контроллера помечается `@Valid`:

```java
public record CreateUserRequest(
        @NotBlank @Size(max = 100) String name,
        @Email String email,
        @Valid @NotNull AddressDto address // @Valid каскадирует проверку во вложенный объект
) {}

@PostMapping
public UserDto create(@Valid @RequestBody CreateUserRequest request) { ... }
```

При ошибке бросается `MethodArgumentNotValidException`, из которого в `@RestControllerAdvice` собирают список ошибок по полям.

Разница `@Valid` и `@Validated` — частый вопрос. `@Valid` — аннотация спецификации (jakarta), работает в контроллерах и каскадирует вложенные объекты. `@Validated` — спринговая: включает method-level validation (проверка параметров любых методов бинов, не только контроллеров) и поддерживает группы валидации.

Кастомный констрейнт — аннотация плюс `ConstraintValidator`:

```java
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = PhoneValidator.class)
public @interface Phone {
    String message() default "Некорректный номер телефона";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}

public class PhoneValidator implements ConstraintValidator<Phone, String> {
    private static final Pattern PATTERN = Pattern.compile("^\\+7\\d{10}$");

    @Override
    public boolean isValid(String value, ConstraintValidatorContext context) {
        return value == null || PATTERN.matcher(value).matches();
        // null пропускаем: за обязательность отвечает @NotNull
    }
}
```

**Группы валидации** — маркерные интерфейсы, позволяющие применять разные наборы правил для разных сценариев (например, при создании id должен быть null, при обновлении — обязателен):

```java
public interface OnCreate {}
public interface OnUpdate {}

public record UserDto(
        @Null(groups = OnCreate.class) @NotNull(groups = OnUpdate.class) Long id,
        @NotBlank String name
) {}

@PutMapping
public UserDto update(@Validated(OnUpdate.class) @RequestBody UserDto dto) { ... }
```

Ловушка: если у поля указана хотя бы одна группа, без групп оно валидироваться не будет — `@Validated` без аргументов проверяет только констрейнты группы Default.

---

### 23. HTTP-клиенты между сервисами: RestTemplate, WebClient и OpenFeign

Для HTTP-вызовов нужно выбрать модель исполнения и способ описания контракта. Кроме исторических RestTemplate/WebClient/OpenFeign, в Spring Framework 6.1+ есть синхронный `RestClient`, а в 6.0+ — интерфейсы HTTP Service Client с `@HttpExchange`. Синхронный код не обязан становиться реактивным ради нового клиента.

**RestTemplate** — синхронный блокирующий клиент, живёт со Spring 3. Основные методы: `getForObject()` возвращает только тело, `getForEntity()` — `ResponseEntity` со статусом и заголовками (нужен, когда важен код ответа или хедеры), `exchange()` — универсальный метод для любых HTTP-методов и кастомных заголовков.

```java
UserDto user = restTemplate.getForObject("/users/{id}", UserDto.class, 42);
ResponseEntity<UserDto> entity = restTemplate.getForEntity("/users/{id}", UserDto.class, 42);
```

Версионный статус: в Spring Framework 6.2 `RestTemplate` ещё не deprecated; для нового синхронного кода уже доступен `RestClient` (с 6.1). Начиная с Spring Framework 7.0 `RestTemplate` deprecated в пользу `RestClient`. Поэтому ответ зависит от major-версии, а миграция на WebClient не является обязательным условием синхронных HTTP-вызовов.

Клиенту задают конечные timeout и согласованный бюджет ожидания. Пример для Java 17+ и Spring Framework 6.1+ с JDK HTTP client:

```java
var httpClient = java.net.http.HttpClient.newBuilder()
        .connectTimeout(Duration.ofSeconds(2))
        .build();
var factory = new JdkClientHttpRequestFactory(httpClient);
factory.setReadTimeout(Duration.ofSeconds(5));
RestClient client = RestClient.builder()
        .baseUrl("https://users.example")
        .requestFactory(factory)
        .build();
UserDto user = client.get().uri("/users/{id}", 42)
        .retrieve().body(UserDto.class);
```

Connect timeout, ожидание соединения из пула и ожидание ответа — разные ограничения; набор настроек зависит от выбранного HTTP-движка. Код Apache HttpClient 4 нельзя без изменений переносить в Spring 6, использующий HttpComponents 5.

**WebClient** — клиент из `spring-webflux`, реактивный и неблокирующий: запросы выполняются на маленьком пуле event-loop потоков, а не «поток на запрос». При этом ничто не мешает использовать его в обычном сервлетном приложении и сразу блокироваться на результате — для синхронного кода это прямая замена RestTemplate:

```java
UserDto user = webClient.get()
        .uri("/users/{id}", 42)
        .retrieve()
        .bodyToMono(UserDto.class)
        .timeout(Duration.ofSeconds(5))
        .block(); // в реактивном коде block() делать нельзя
```

`retrieve()` сам бросает исключение на 4xx/5xx, `exchangeToMono()` даёт полный контроль над ответом. Нюанс: чтобы WebClient заработал, в classpath должен быть Reactor и реактивный HTTP-клиент (Reactor Netty по умолчанию).

**OpenFeign** — декларативный клиент: объявляете интерфейс, а реализацию генерирует Spring Cloud. Код выглядит как вызов локального метода, HTTP скрыт полностью:

```java
@FeignClient(name = "user-service", url = "${clients.user-service.url}")
public interface UserClient {

    @GetMapping("/users/{id}")
    UserDto getUser(@PathVariable long id);
}
```

Плюс интеграция с service discovery и client-side load balancing (`name = "user-service"` без url — Feign сам найдёт инстансы через Eureka/Kubernetes), ретраи через `Retryer`, таймауты в современных Spring Cloud OpenFeign 4.x через `spring.cloud.openfeign.client.config.<client>.connectTimeout` / `readTimeout`. Для выбора инстанса нужны соответствующая интеграция discovery и Spring Cloud LoadBalancer; одного имени в аннотации недостаточно.

Про ретраи (это следующий follow-up после таймаутов): повторять можно только идемпотентные операции — GET обычно допускает повтор по семантике, но учитываются общий deadline и нагрузка; POST с созданием платежа нельзя без ключа идемпотентности, иначе снимете деньги дважды. Ретраи делают с экспоненциальной задержкой и ограничением числа попыток (Resilience4j, Spring Retry), а для защиты от умирающего соседнего сервиса добавляют circuit breaker — после серии ошибок запросы «рубятся» сразу, не дожидаясь таймаута.

---

### 24. Filter, HandlerInterceptor и AOP: что где перехватывает

Все три механизма позволяют вклиниться в обработку запроса, но работают на разных уровнях, и весь вопрос — уметь объяснить, кто где стоит.

**Servlet Filter** — уровень сервлет-контейнера, до `DispatcherServlet`. Видит только «сырые» `HttpServletRequest`/`HttpServletResponse` и не знает ничего о контроллере, который обработает запрос. Зато срабатывает на всё: статика, ошибки, любые сервлеты. Типовые задачи: correlation id и MDC, логирование запроса/ответа, CORS, gzip, аутентификация (весь Spring Security — это цепочка фильтров).

**HandlerInterceptor** — уровень Spring MVC, после `DispatcherServlet`. Знает, какой handler будет вызван: в `preHandle()` приходит `HandlerMethod`, из которого можно достать класс контроллера и аннотации на методе — это основное отличие от фильтра. Три метода: `preHandle()` (до контроллера, `false` обрывает обработку), `postHandle()` (после контроллера, есть доступ к модели), `afterCompletion()` (после рендера ответа, вызывается даже при исключении — подходит для замера времени). Задачи: локаль, измерения и контекст конкретного handler. Для границы безопасности предпочтителен Spring Security в filter chain: правила interceptor могут расходиться с маршрутизацией контроллеров.

**AOP** — вообще не про HTTP: перехватывает вызовы методов любых бинов. Контроллер — частный случай, но чаще аспекты вешают на сервисный слой: `@Transactional`, кэш, аудит, замеры. Если сквозная логика относится к бизнес-методам, а не к веб-слою — это AOP.

Порядок в цепочке на запросе:

```text
Filter (doFilter → chain.doFilter)
  → DispatcherServlet
    → Interceptor.preHandle
      → AOP-советы на контроллере/сервисах → метод контроллера
    → Interceptor.postHandle
  → рендер ответа
    → Interceptor.afterCompletion
← Filter (код после chain.doFilter)
```

То есть фильтры «самые внешние», дальше интерцепторы, внутри всего — AOP на бинах.

`OncePerRequestFilter` использует атрибут запроса для контроля повторной обработки, но обещание «ровно один раз за весь жизненный цикл запроса» слишком сильное. Важны тип dispatch, регистрация фильтра и методы `shouldNotFilterAsyncDispatch()` / `shouldNotFilterErrorDispatch()`. Async и error dispatch могут происходить в других потоках; для них правила задают явно. По умолчанию фильтр пропускает последующие async/error dispatch. Проверять повторное логирование нужно с фактической конфигурацией servlet-контейнера, а не считать все `FORWARD`/`ERROR` включёнными автоматически.

---

### 25. Jackson в Spring: настройка ObjectMapper, аннотации и циклические ссылки

Spring Boot сам создаёт и настраивает `ObjectMapper` через `JacksonAutoConfiguration` — именно он сериализует возвращаемые из `@RestController` объекты и парсит `@RequestBody`. Из коробки уже зарегистрирован модуль для Java 8 Time (`jsr310`), а `WRITE_DATES_AS_TIMESTAMPS` выключен, поэтому `LocalDate` уезжает как `"2026-07-26"`, а не как массив чисел.

Простые вещи настраиваются свойствами, без кода:

```yaml
spring:
  jackson:
    date-format: yyyy-MM-dd HH:mm:ss
    default-property-inclusion: non_null   # null-поля не сериализуются
```

Для кода есть два пути. Правильный — `Jackson2ObjectMapperBuilderCustomizer`: вы дополняете автоконфигурацию. Если объявить собственный `@Bean ObjectMapper`, Boot отступит от создания стандартного mapper. Это не выключает всю Jackson-автоконфигурацию, но настройки нового `ObjectMapper()` не совпадут с настроенным builder; модули и параметры придётся учитывать явно. Можно построить собственный mapper через предоставленный Boot builder. Эти примеры относятся к Boot 3.x/Jackson 2; в Boot 4 стандартный стек Jackson изменён, поэтому API настройки проверяют по версии.

Основные аннотации на полях:

```java
public class UserDto {
    @JsonProperty("user_name")          // имя поля в JSON
    private String name;

    @JsonIgnore                          // поле не ходит в JSON ни туда, ни обратно
    private String password;

    @JsonFormat(pattern = "dd.MM.yyyy")  // формат конкретной даты
    private LocalDate birthDate;
}
```

Ещё полезны `@JsonInclude(Include.NON_NULL)` на уровне класса и `@JsonIgnoreProperties(ignoreUnknown = true)` — не падать на неизвестных полях при десериализации (спасает при эволюции контрактов между сервисами).

**Циклические ссылки.** Классический сценарий — двунаправленная связь JPA: `Order` ссылается на `Customer`, `Customer` содержит список заказов. При сериализации Jackson бежит по графу бесконечно и падает со `StackOverflowError` (точнее, `JsonMappingException` о бесконечной рекурсии). Способы лечения:

- `@JsonManagedReference` на «родительской» стороне + `@JsonBackReference` на «дочерней»: managed сериализуется, back — нет, цикл разорван.
- `@JsonIdentityInfo` — сериализация по id: первая встреча объекта отдаётся целиком, повторные — только идентификатором.
- Самое правильное решение — DTO: сущности вообще не отдавать наружу, а маппить их в отдельные классы без циклов и лишних полей. Заодно решается проблема ленивых коллекций, которые при сериализации дёргают базу за пределами транзакции.

**Кастомный сериализатор** нужен, когда аннотациями формат не выразить — например, деньги как строка с двумя знаками:

```java
public class MoneySerializer extends JsonSerializer<BigDecimal> {
    @Override
    public void serialize(BigDecimal value, JsonGenerator gen, SerializerProvider serializers)
            throws IOException {
        gen.writeString(value.setScale(2, RoundingMode.HALF_UP).toPlainString());
    }
}
```

Применяется либо точечно — `@JsonSerialize(using = MoneySerializer.class)` на поле, либо глобально — регистрацией в `SimpleModule` (через тот же `Jackson2ObjectMapperBuilderCustomizer`).

## Безопасность и аутентификация

### 9. Spring Security: Архитектура, Аутентификация и JWT

**Spring Security** — фреймворк для обеспечения аутентификации, авторизации и защиты от атак (CSRF, XSS и др.).
Он базируется на цепочке фильтров сервлетов (`SecurityFilterChain`), которые перехватывают каждый входящий запрос до его попадания в `DispatcherServlet`.

Ключевые компоненты:

- `SecurityContextHolder`: хранит информацию о текущем авторизованном пользователе (объект `Authentication`).
- `AuthenticationManager`: основной интерфейс для проверки аутентификации. Делегирует работу `AuthenticationProvider`.
- `UserDetailsService`: интерфейс для загрузки пользовательских данных (часто реализуется для чтения из БД).

Для JWT API обычно используют готовую поддержку OAuth2 Resource Server (`spring-boot-starter-oauth2-resource-server`): она извлекает bearer token, декодирует и валидирует JWT, создаёт `Authentication`. Помимо подписи и срока действия проверяют нужные `issuer` и `audience` согласно модели доверия. Самописный `OncePerRequestFilter` нужен только при требованиях, которые штатная инфраструктура не покрывает; пример ниже показывает место такого фильтра, а не обязательный способ работы с JWT.

Несколько моментов, которые спрашивают следом:

- Разница аутентификации и авторизации: аутентификация — кто вы (проверка логина/пароля или токена), авторизация — что вам можно (роли и права).
- Пароли хранятся через `PasswordEncoder`, стандарт — `BCryptPasswordEncoder` (адаптивный хеш с солью). Plain-text в БД — провал на любом собеседовании.
- Авторизация на уровне методов включается `@EnableMethodSecurity`, дальше `@PreAuthorize("hasRole('ADMIN')")` или SpEL с параметрами метода (`@PostAuthorize`, `@PreFilter`). Работает через прокси — применима к любым бинам, не только контроллерам.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    private final JwtAuthenticationFilter jwtAuthFilter;

    public SecurityConfig(JwtAuthenticationFilter jwtAuthFilter) {
        this.jwtAuthFilter = jwtAuthFilter;
    }

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable()) // Допустимо для API с bearer token в Authorization, без cookie-аутентификации
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**").permitAll()
                .anyRequest().authenticated()
            )
            .addFilterBefore(jwtAuthFilter, UsernamePasswordAuthenticationFilter.class);
        return http.build();
    }
}
```

Для JWT-API сессию принято делать stateless — `sessionManagement(s -> s.sessionCreationPolicy(SessionCreationPolicy.STATELESS))`, это запрещает Spring Security использовать HTTP-сессию для получения SecurityContext. Сам факт наличия JWT не определяет session policy всего приложения: возможны несколько цепочек для разных маршрутов. Отключение CSRF оправдано именно потому, что токен передаётся заголовком, а не cookie: CSRF-атака эксплуатирует автоматическую отправку cookie браузером.

---

### 27. Session-based и token-based (JWT) аутентификация, OAuth2 и OIDC

Разница между подходами — в одном вопросе: хранит ли сервер состояние о залогиненном пользователе.

**Session-based.** После логина сервер создаёт сессию, отдаёт её id в cookie, а данные хранит у себя (в памяти или Redis). На каждом запросе браузер присылает cookie, сервер идёт в хранилище. Это stateful-подход. Плюсы: мгновенная инвалидация — удалил сессию, пользователь разлогинен; в cookie ходит только бессмысленный id. Минусы: горизонтальное масштабирование требует либо sticky sessions на балансировщике, либо общего хранилища сессий (Spring Session + Redis — стандартное решение).

**Token-based (JWT).** Сервер при логине выдаёт подписанный токен, внутри которого уже есть всё нужное: `header.payload.signature` — кто пользователь, роли, срок жизни (`exp`). На запросах сервер только проверяет подпись своим ключом и ничего не хранит — stateless. Плюсы: идеальное масштабирование и естественный fit для микросервисов — любой сервис валидирует токен сам, без похода в общее хранилище. Минусы, которые обязательно надо назвать:

- при чисто локальной проверке JWT без серверного состояния токен нельзя индивидуально отозвать до истечения `exp` — пользователь «разлогинен», а токен ещё 15 минут валиден. Варианты: короткий TTL, проверка denylist/версии сессии или другой серверный механизм; это меняет стоимость проверки и модель состояния;
- payload — это base64, а не шифрование: содержимое читается кем угодно, секретов туда не кладут;
- токен тяжелее session id и ходит в каждом запросе.

Отсюда схема с **refresh token**: access token живёт 5–15 минут, refresh — неделями, хранится на сервере и потому отзываем. Access протух — клиент молча обменивает refresh на новую пару, не трогая пользователя паролем.

**Где хранить токен на клиенте** — классический follow-up. `localStorage` удобен, но любой XSS (внедрённый скрипт) читает его и угоняет токен. `httpOnly` cookie из JavaScript недоступна, но тогда запросы снова «браузерные» — нужна защита от CSRF. Компромисс на практике: access token в памяти SPA + refresh в httpOnly cookie с `SameSite`.

**OAuth2 и OpenID Connect.** OAuth2 — протокол делегированной авторизации: приложение получает ограниченный доступ к ресурсам пользователя без его пароля. Роли: Resource Owner (пользователь), Client (наше приложение), Authorization Server (выдаёт токены — например, Keycloak), Resource Server (API, которое проверяет токены). Основные flow:

- **Authorization Code** (с PKCE для SPA и мобильных) — для случаев «в приложение входит человек»: пользователь логинится на стороне authorization server'а, браузер возвращается с кодом, бэкенд меняет код на токены по защищённому каналу. Сам код в браузере бесполезен без секрета/verifier'а — поэтому перехват редиректа не страшен.
- **Client Credentials** — межсервисное общение (machine-to-machine): пользователя нет, сервис аутентифицируется своими client id/secret и получает токен от своего имени.

OIDC — тонкий слой поверх OAuth2: добавляет `id_token` (JWT с данными о пользователе) и стандартный эндпоинт `/userinfo`. OAuth2 отвечает на вопрос «что можно», OIDC — «кто ты». Keycloak — готовый authorization server с поддержкой обоих, в Spring всё сводится к `spring-boot-starter-oauth2-resource-server` и свойству `spring.security.oauth2.resourceserver.jwt.issuer-uri`: приложение получает метаданные/JWK и валидирует JWT по настройке. По умолчанию scopes преобразуются в authorities с префиксом `SCOPE_`; роли из нестандартных claims Keycloak могут требовать собственного converter. Проверку audience тоже настраивают под конкретный API.

---

### 28. Хранение паролей и основы безопасности: BCrypt и OWASP Top-10

Пароли хешируют, а не шифруют. Обратимое шифрование не подходит по двум причинам: где-то лежит ключ, и его утечка расшифровывает все пароли разом; плюс администратор системы вообще не должен иметь технической возможности узнать пароль пользователя. Хеш необратим — при логине хешируют введённый пароль и сравнивают хеши.

Подходят медленные адаптивные функции: **BCrypt** (стандарт в Spring Security, работ-фактор настраивается, соль встроена в сам хеш) и **Argon2** (современнее, memory-hard — затрудняет перебор на GPU и ASIC). Не подходят MD5, SHA-1 и даже одиночный SHA-256: они слишком быстрые, на GPU перебираются миллиардами в секунду, плюс на общие пароли давно есть rainbow tables. **Соль** — случайная строка, добавляемая к паролю перед хешированием: убивает rainbow tables и заставляет перебирать каждый пароль каждого пользователя отдельно, даже если пароли совпадают. Соль не секретна и хранится рядом с хешем (BCrypt кладёт её прямо в результирующую строку).

В Spring Security пароль проходит через `PasswordEncoder`:

```java
@Bean
PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder(); // или PasswordEncoderFactories.createDelegatingPasswordEncoder()
}
```

`DelegatingPasswordEncoder` хранит хеш с префиксом `{bcrypt}...` — это позволяет мигрировать алгоритмы: старые хеши проверяются старым энкодером, новые пишутся новым.

Дальше — бегло по OWASP Top-10, что обычно и просят:

**SQL-инъекции.** Причина — конкатенация пользовательского ввода в запрос. Лечение — параметризованные запросы: `PreparedStatement` в JDBC, named parameters в JPQL (`@Query("select u from User u where u.email = :email")`). Два слепых пятна: native queries, где разработчик снова склеивает строки, и `ORDER BY` — направление и поле сортировки параметром не подставить, только whitelist допустимых значений.

**XSS (межсайтовый скриптинг).** Пользовательский ввод попадает в HTML и исполняется как скрипт — угон сессий и токенов (см. вопрос 27). Лечение — экранирование вывода: Thymeleaf экранирует по умолчанию, а вот `th:utext` отдаёт HTML как есть — частая дыра. Плюс CSP-заголовки как вторая линия обороны.

**CSRF (межсайтовая подделка запроса).** Атака на доверие браузера к cookie: злонамеренная страница отправляет форму на ваш сайт, браузер сам прикрепляет сессионную cookie — и запрос выполняется от имени залогиненного пользователя. Spring Security включает защиту по умолчанию: для изменяющих запросов требуется CSRF-токен (скрытое поле формы или заголовок), который внешний сайт прочитать не может. Когда CSRF законно отключают (`csrf.disable()`): для stateless REST API, где аутентификация идёт через заголовок `Authorization: Bearer ...` — браузер не прикрепляет такой заголовок самостоятельно, значит атаковать нечем. А вот при session-based аутентификации через cookie отключать нельзя — это как раз уязвимый случай (частично помогает `SameSite=Lax/Strict` на cookie).

Из того же списка на собеседовании могут спросить: не логировать пароли, токены и персональные данные; не отдавать stack trace наружу (глобальный `@ControllerAdvice` вместо дефолтной страницы ошибки); security-заголовки из коробки в Spring Security; и общий принцип наименьших привилегий — у приложения минимальные права на базу, у пользователя — на данные.

## Эксплуатация и наблюдаемость

### 21. Spring Boot Actuator: health, info, metrics и интеграция с Prometheus

Actuator — стартер, добавляющий production-эндпоинты для наблюдения за приложением: `/actuator/health`, `/actuator/info`, `/actuator/metrics`, `/actuator/env`, `/actuator/heapdump` и другие.

Из коробки через HTTP открыт только `/health` (и то в сокращённом виде). Остальное включается явно:

```yaml
management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics,prometheus
  endpoint:
    health:
      show-details: when-authorized
```

`/health` агрегирует health-индикаторы: база, диск, Redis, Kafka. Свой индикатор добавляется бином:

```java
@Component
public class PaymentGatewayHealthIndicator implements HealthIndicator {
    @Override
    public Health health() {
        if (gateway.ping()) return Health.up().build();
        return Health.down().withDetail("reason", "gateway unreachable").build();
    }
}
```

В Kubernetes health-группы `liveness` и `readiness` (`/actuator/health/liveness`, `/actuator/health/readiness`) маппятся на пробы напрямую — liveness отражает жизнеспособность приложения, readiness — готовность принимать трафик. Внешние зависимости вроде БД не стоит включать в liveness: общий сбой базы вызовет бессмысленные рестарты. Boot не включает все внешние проверки в readiness автоматически; состав группы выбирают осознанно, чтобы общий отказ зависимости не вывел из балансировки все экземпляры.

**Metrics и Prometheus.** Метрики собираются через Micrometer: счётчики, таймеры, gauge на пулы потоков, JVM, HTTP-запросы. Для Prometheus добавляется зависимость `micrometer-registry-prometheus`, появляется эндпоинт `/actuator/prometheus`, который Prometheus скрейпит по расписанию. Своя метрика — через `MeterRegistry`:

```java
Counter counter = Counter.builder("orders.created")
        .tag("channel", "web")
        .register(meterRegistry);
counter.increment();
```

**Защита эндпоинтов** — обязательная часть вопроса на собеседовании. `/env` светит все свойства (включая замаскированные, но всё же), `/heapdump` отдаёт дамп памяти с паролями в открытом виде, `/shutdown` (если включить) гасит приложение. Минимум мер:

- не открывать лишнее через `exposure.include`;
- вынести actuator на отдельный порт, недоступный снаружи: `management.server.port=9091` (+ firewall / network policy);
- закрыть Spring Security'ем (`requestMatchers("/actuator/**").hasRole("ADMIN")`);
- включать shutdown только при обоснованном сценарии и строго ограниченном доступе; по умолчанию оставлять отключённым.

---

### 22. Логирование: SLF4J, Logback и MDC

Стандартная связка в Spring Boot: фасад **SLF4J** + реализация **Logback** (приезжает с любым `spring-boot-starter`). Код пишется против SLF4J, реализацию можно заменить (Log4j2, JUL) без правок кода.

Уровни по возрастанию серьёзности: `TRACE < DEBUG < INFO < WARN < ERROR`. Установленный уровень отсекает всё ниже. Управляются из конфигурации без пересборки:

```yaml
logging:
  level:
    root: info
    com.example.payment: debug
    org.springframework.web: warn
  pattern:
    console: "%d{yyyy-MM-dd HH:mm:ss} %-5level [%thread] [%X{correlationId}] %logger{36} - %msg%n"
```

Для сложных сценариев ( rolling-файлы, отдельные аппендеры, разные конфиги по профилям) используется `logback-spring.xml` — именно `-spring` версия, потому что она понимает тег `<springProfile>` и `${}` из Environment.

**MDC (Mapped Diagnostic Context)** — ThreadLocal-карта, содержимое которой Logback подставляет в каждую строку лога через `%X{...}`. Это основной способ сквозной трассировки: кладёте correlation id в начале запроса — и все логи запроса (включая вложенные вызовы сервисов в том же потоке) помечены им.

```java
@Component
public class CorrelationIdFilter extends OncePerRequestFilter {

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response,
                                    FilterChain chain) throws ServletException, IOException {
        String correlationId = Optional.ofNullable(request.getHeader("X-Correlation-Id"))
                .orElse(UUID.randomUUID().toString());
        MDC.put("correlationId", correlationId);
        response.setHeader("X-Correlation-Id", correlationId);
        try {
            chain.doFilter(request, response);
        } finally {
            MDC.clear(); // обязательно: поток вернётся в пул Tomcat с чужим id
        }
    }
}
```

Две практические оговорки. Первая: `MDC.clear()` в `finally` обязателен — потоки в пуле переиспользуются, и без очистки следующий запрос залогируется с чужим id. Вторая: MDC живёт в потоке, поэтому при `@Async` или реактивных цепочках контекст теряется — для `@Async` пробрасывают через `TaskDecorator`, в WebFlux используют Reactor Context. На границе сервисов id передаётся заголовком, и следующий сервис кладёт его в свой MDC — так цепочка логов собирается по одному идентификатору во всех системах.

[← Вернуться на главную страницу](README.md)
