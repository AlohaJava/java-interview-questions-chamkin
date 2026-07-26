# Код-ревью на собеседовании

Отдельный живой формат собеседования: кандидату показывают кусок кода и просят найти в нём проблемы. Так проверяют не знание синтаксиса, а «насмотренность»: умеете ли вы читать чужой код, видите ли типовые ловушки с многопоточностью, ресурсами и деньгами, и можете ли внятно объяснить, почему это плохо и как исправить. В этом разделе — методика ответа, восемь тренировочных сниппетов с разбором и поведенческая часть про ревью в команде.

## Оглавление
1. [Как отвечать на «поревьюйте этот код»](#1-как-отвечать-на-поревьюйте-этот-код)
2. [Сниппет: многопоточный синглтон и счётчик](#2-сниппет-многопоточный-синглтон-и-счётчик)
3. [Сниппет: ресурсы и исключения](#3-сниппет-ресурсы-и-исключения)
4. [Сниппет: equals, hashCode и коллекции](#4-сниппет-equals-hashcode-и-коллекции)
5. [Сниппет: JPA и Spring](#5-сниппет-jpa-и-spring)
6. [Сниппет: REST API контроллер](#6-сниппет-rest-api-контроллер)
7. [Сниппет: BigDecimal и деньги](#7-сниппет-bigdecimal-и-деньги)
8. [Сниппет: строки и даты](#8-сниппет-строки-и-даты)
9. [Сниппет: Optional и Stream API](#9-сниппет-optional-и-stream-api)
10. [Как проводить код-ревью в команде](#10-как-проводить-код-ревью-в-команде)

---

### 1. Как отвечать на «поревьюйте этот код»

**Ответ:**

Главное, что проверяют в этом формате, — не найдёте ли вы все ошибки, а как вы работаете с незнакомым кодом. Методика, которая стабильно выглядит хорошо:

1. **Сначала прочитайте код целиком, молча.** 30–60 секунд на то, чтобы понять, что код вообще делает. Начинать комментировать с первой строчки, не дочитав, — частая ошибка: можно минуту разбирать нейминг переменной, а в конце окажется race condition, и приоритеты придётся переставлять.
2. **Проговаривайте вслух, что поняли.** «Это сервис, который пересчитывает баланс пользователя: читает заказы, суммирует и пишет в базу». Это показывает, что вы читаете код, а не ищете знакомые паттерны ошибок.
3. **Идите от серьёзного к косметическому.** Рабочий порядок приоритетов: корректность (баги, NPE, неправильная логика) → многопоточность → ресурсы (не закрыты, утечки) → обработка ошибок → дизайн (SOLID, связанность, слои) → производительность → стиль и нейминг. Если начать со стиля, интервьюер решит, что багов вы не видите.
4. **Для каждой находки называйте следствие и исправление.** Не «тут плохо», а «тут инкремент не атомарен, при двух потоках счётчик потеряет обновления; чинится AtomicInteger или synchronized». Формула: что → при каком условии сломается → как починить.
5. **Приоритизируйте в конце.** «Критично починить X и Y, остальное — на уровне предложений». Это то, как ревью работает в жизни: не все комментарии равнозначны.

Чек-лист, что искать в первую очередь (по нему удобно пробегать глазами любой сниппет):

1. NPE: вызовы методов на результатах, которые могут быть null; `equals` на переменной вместо константы; `optional.get()` без проверки.
2. Равенство: `==` для `String` и обёрток (`Integer`, `Long`), `equals` без `hashCode`, сравнение `double` на равенство.
3. Многопоточность: разделяемое мутабельное состояние, неатомарные check-then-act, broken double-checked locking, не-thread-safe объекты в `static`-полях (`SimpleDateFormat`).
4. Ресурсы: открытые `Connection`, `InputStream`, `ResultSet` без try-with-resources; файлы/пулы, которые утекают при исключении.
5. Исключения: пустой `catch`, `catch (Exception e)` с одним `printStackTrace()`, `throws Exception` в сигнатуре, глотание `InterruptedException`.
6. Деньги и числа: `double` для сумм, `new BigDecimal(double)`, деление без scale, переполнение `int` в арифметике.
7. Базы данных: SQL-инъекции через конкатенацию строк, N+1 запросы в цикле, транзакции, которые на самом деле не работают (`@Transactional` на private/self-invocation).
8. API: наружу торчат entity вместо DTO, нет валидации входа, внутренние ошибки и стектрейсы уходят клиенту, неправильные статус-коды.
9. Коллекции: изменение коллекции во время итерации, мутация объектов, лежащих в `HashSet`/ключах `HashMap`, `O(n²)` на больших данных там, где есть `HashMap`.
10. Стиль в последнюю очередь: нейминг, магические числа, мёртвый код, комментарии, которые врут.

Сеньорский приём: если видите ошибку, но не уверены на 100% — сформулируйте вопросом: «здесь `counter++`, при двух потоках это потеряет обновления, верно?». Это нормальный язык реального ревью, и он страхует от случая, когда вы неправильно поняли контекст.

---

### 2. Сниппет: многопоточный синглтон и счётчик

```java
public class RateLimiterService {

    private static RateLimiterService instance;

    private long totalRequests = 0;
    private final Map<String, Integer> requestsByClient = new HashMap<>();

    public static RateLimiterService getInstance() {
        if (instance == null) {
            synchronized (RateLimiterService.class) {
                if (instance == null) {
                    instance = new RateLimiterService();
                }
            }
        }
        return instance;
    }

    public void registerRequest(String clientId) {
        totalRequests++;
        Integer count = requestsByClient.get(clientId);
        if (count == null) {
            requestsByClient.put(clientId, 1);
        } else {
            requestsByClient.put(clientId, count + 1);
        }
    }

    public long getTotalRequests() {
        return totalRequests;
    }
}
```

Найдите проблемы.

<details>
<summary>Разбор</summary>

1. **Broken double-checked locking.** Паттерн написан правильно по форме, но поле `instance` не объявлено `volatile`. Без `volatile` второй поток может увидеть не-null ссылку на объект, чей конструктор ещё не отработал до конца (переупорядочивание инструкций: запись ссылки может произойти раньше инициализации полей объекта). Поток получит «полуинициализированный» синглтон. Лечится `private static volatile RateLimiterService instance`, а ещё лучше — вообще уйти от ручного DCL: enum-синглтон или holder-идиома (`private static class Holder { static final RateLimiterService INSTANCE = new RateLimiterService(); }`) — проще и безопаснее.

2. **Race condition на инкременте.** `totalRequests++` — это три операции (прочитать, прибавить, записать), и между ними вклинивается другой поток. При конкурентных вызовах счётчик теряет обновления. Классический пример неатомарного read-modify-write. Лечится `AtomicLong` и `incrementAndGet()`, либо синхронизацией всего метода.

3. **Непотокобезопасный `HashMap` под конкурентной записью.** `registerRequest` вызывается из многих потоков, а `HashMap` не thread-safe: конкурентные `put` могут не только терять записи, но и повредить внутреннюю структуру (на старых JDK — бесконечный цикл при resize). Плюс сам блок «get → проверка → put» — это check-then-act, тоже race. Лечится `ConcurrentHashMap` и `merge(clientId, 1, Integer::sum)` — атомарно и в одну строку.

4. Косметика: синглтон через статику — ещё и проблема тестируемости (общее состояние между тестами), но это уже разговор про дизайн, а не про баг.

На собеседовании достаточно первых трёх: volatile, неатомарный инкремент, `ConcurrentHashMap.merge`.

</details>

---

### 3. Сниппет: ресурсы и исключения

```java
public class ReportExporter {

    private final DataSource dataSource;

    public ReportExporter(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    public void exportUserReport(long userId, String outputPath) throws Exception {
        Connection connection = dataSource.getConnection();
        PreparedStatement statement = connection.prepareStatement(
                "SELECT id, name, email, created_at FROM users WHERE id = " + userId);
        ResultSet rs = statement.executeQuery();

        FileInputStream template = new FileInputStream("templates/report.html");
        byte[] content = template.readAllBytes();

        try (FileWriter writer = new FileWriter(outputPath)) {
            if (rs.next()) {
                String report = new String(content)
                        .replace("${name}", rs.getString("name"))
                        .replace("${email}", rs.getString("email"));
                writer.write(report);
            }
        } catch (IOException e) {
        }

        rs.close();
        statement.close();
        connection.close();
    }
}
```

Найдите проблемы.

<details>
<summary>Разбор</summary>

1. **Ресурсы утекают при исключении.** `Connection`, `PreparedStatement`, `ResultSet` и `FileInputStream` закрываются вручную в конце метода. Если любая строка выше бросит исключение (упал `executeQuery`, не нашёлся файл шаблона), до `close()` выполнение не дойдёт — соединение останется висеть в пуле, файловый дескриптор — открытым. При повторяющихся вызовах это исчерпает пул соединений или лимит файловых дескрипторов. Лечится try-with-resources на все четыре ресурса: они закроются в правильном порядке даже при исключении.

2. **Проглоченное исключение.** Пустой `catch (IOException e) {}` — худший вариант обработки: отчёт молча не запишется, вызывающий код решит, что всё хорошо, а в логах будет пусто. Как минимум — залогировать, как правило — пробросить дальше или завернуть в своё доменное исключение. «Молчаливый catch» на ревью — всегда первый кандидат в комментарии.

3. **`throws Exception` в сигнатуре.** Метод заявляет, что может бросить что угодно: вызывающий код не понимает, к чему готовиться, и вынужден либо ловить `Exception` (та же болезнь уровнем выше), либо тоже писать `throws Exception`, и зараза расползается по коду. Нужны конкретные типы: `SQLException`, `IOException` — или своё unchecked-исключение с понятным именем.

4. **SQL-инъекция как бонус.** `"... WHERE id = " + userId` — конкатенация вместо параметра. Здесь `userId` — `long`, поэтому реально проэксплуатировать не выйдет, но это повод на ревью потребовать привычку всегда писать `WHERE id = ?` и `statement.setLong(1, userId)`: завтра на это место кто-то поставит строку из запроса. Отдельно: `FileInputStream` на шаблоне читается в каждом вызове, хотя шаблон статичен — его можно закешировать.

</details>

---

### 4. Сниппет: equals, hashCode и коллекции

```java
public class Employee {
    private String department;
    private Long personnelNumber;

    public Employee(String department, Long personnelNumber) {
        this.department = department;
        this.personnelNumber = personnelNumber;
    }

    public void setDepartment(String department) {
        this.department = department;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Employee employee = (Employee) o;
        return department.equals(employee.department)
                && personnelNumber.equals(employee.personnelNumber);
    }
}

public class SalaryRegistry {
    private final Map<Employee, BigDecimal> salaries = new HashMap<>();

    public void assignSalary(Employee employee, BigDecimal salary) {
        salaries.put(employee, salary);
    }

    public BigDecimal findSalary(Employee employee) {
        return salaries.get(employee);
    }

    public boolean isCached(Long personnelNumber, Long cachedNumber) {
        return personnelNumber == cachedNumber;
    }
}
```

Найдите проблемы.

<details>
<summary>Разбор</summary>

1. **`equals` без `hashCode`.** Контракт нарушен: два равных по `equals` объекта получат разные `hashCode()` (унаследованный от `Object` — по адресу). `Employee` используется как ключ `HashMap`, поэтому `findSalary(new Employee("IT", 42L))` вернёт null, даже если точно такой же сотрудник уже лежит в мапе: `get` сначала ищет bucket по хешу, а тот не совпадёт. Правило простое: переопределяете `equals` — всегда переопределяйте `hashCode` (в идеале — `Objects.hash(department, personnelNumber)` или генерация IDE).

2. **Мутация ключа в мапе.** У `Employee` есть сеттер `setDepartment`, а `department` участвует в `equals` (и, после починки, в `hashCode`). Если сотрудник уже лежит ключом в `HashMap`, и кто-то переведёт его в другой отдел — хеш ключа изменится, а объект останется в старом bucket'е. Запись «потеряется»: `get` и даже `containsKey` перестанут её находить, а в `keySet()` она при этом будет видна. Классическая причина, почему ключи коллекций должны быть immutable: поля ключа — final, сеттеров нет.

3. **Сравнение `Long` через `==`.** В `isCached` сравниваются два `Long` оператором `==`, а он сравнивает ссылки, не значения. Для значений −128..127 повезёт (кеш `Long.valueOf`), для 1000 — уже нет: `false` при равных числах. Самое коварное, что на маленьких тестовых данных код «работает», а в проде с реальными номерами ломается. Лечится `Objects.equals(a, b)` или распаковкой в `long` с проверкой на null.

4. Мелочь: `findSalary` может вернуть null, и сигнатура `BigDecimal` это скрывает — `Optional<BigDecimal>` честнее. Но это уровень предложения, не бага.

</details>

---

### 5. Сниппет: JPA и Spring

```java
@Entity
public class Order {
    @Id
    private Long id;

    @ManyToOne(fetch = FetchType.EAGER)
    private Customer customer;

    @OneToMany(fetch = FetchType.EAGER)
    private List<OrderItem> items;

    @Override
    public String toString() {
        return "Order{id=" + id + ", customer=" + customer + ", items=" + items + "}";
    }
}

@Service
public class OrderService {

    @Autowired
    private OrderRepository orderRepository;

    public List<Order> getOrdersWithTotals() {
        List<Order> orders = orderRepository.findAll();
        for (Order order : orders) {
            Customer customer = order.getCustomer();
            order.setCustomerName(customer.getName());
            List<OrderItem> items = order.getItems();
            order.setTotal(items.stream().map(OrderItem::getPrice).reduce(BigDecimal.ZERO, BigDecimal::add));
        }
        return orders;
    }

    @Transactional
    private void recalculateOrder(Order order) {
        orderRepository.save(order);
    }

    public void recalculateAll() {
        List<Order> orders = orderRepository.findAll();
        for (Order order : orders) {
            recalculateOrder(order);
            log.info("Recalculated: {}", order);
        }
    }
}
```

Найдите проблемы.

<details>
<summary>Разбор</summary>

1. **N+1.** `findAll()` тянет все заказы, а потом в цикле для каждого заказа обращается к `customer` и `items`. Даже при EAGER (с ним Hibernate часто делает дополнительные select'ы вместо join'а), а при LAZY — гарантированно: один запрос на заказы + по запросу на каждого customer и каждую коллекцию items. Тысяча заказов — тысячи запросов к базе. Лечится `JOIN FETCH` в JPQL/`@EntityGraph` для конкретного запроса, либо выборкой сразу нужных полей в проекцию.

2. **`@Transactional` на private-методе не работает.** Spring создаёт транзакционный прокси, а прокси перехватывают только public-методы, вызванные извне. `recalculateOrder` private и вызывается из `recalculateAll` того же класса — это self-invocation, вызов идёт напрямую, минуя прокси. Аннотация молча игнорируется, транзакции нет. Лечится: вынести метод в отдельный бин и вызывать через него, или сделать public и вызывать извне, или весь `recalculateAll` пометить `@Transactional`.

3. **LAZY-коллекция в `toString` / вне транзакции.** Допустим, кто-то «починит» N+1, поставив LAZY (типичная первая реакция). Тогда `log.info("Recalculated: {}", order)` сработает `toString()`, который обращается к `items` — а сессия к этому моменту уже закрыта, потому что транзакции, напомним, нет. Получаем `LazyInitializationException`. Печатать entity в `toString` целиком с коллекциями — плохая привычка: логируйте id и скалярные поля, коллекции исключайте.

4. **EAGER везде.** `@ManyToOne(fetch = EAGER)` — дефолт в JPA, который надо явно переопределять в LAZY, а `@OneToMany(fetch = EAGER)` — просто минное поле: каждый заказ тянет все позиции всегда, даже когда нужен только номер и дата. Плюс EAGER на `toOne` игнорирует LAZY при запросах через `find` (join) и плодит select'ы при JPQL. Правило: везде LAZY, а нужные связи подгружать точечно в запросе через `JOIN FETCH`.

На собеседовании стоит проговорить именно связку: N+1 виден сразу, а `@Transactional` на private — вопрос «на засыпку», который отличает человека, писавшего Spring руками.

</details>

---

### 6. Сниппет: REST API контроллер

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    private final JdbcTemplate jdbcTemplate;
    private final UserRepository userRepository;

    public UserController(JdbcTemplate jdbcTemplate, UserRepository userRepository) {
        this.jdbcTemplate = jdbcTemplate;
        this.userRepository = userRepository;
    }

    @GetMapping("/search")
    public ResponseEntity<?> searchUsers(@RequestParam String name) {
        try {
            String sql = "SELECT * FROM users WHERE name LIKE '%" + name + "%'";
            List<User> users = jdbcTemplate.query(sql, new BeanPropertyRowMapper<>(User.class));
            if (users.isEmpty()) {
                return ResponseEntity.ok("Users not found");
            }
            return ResponseEntity.ok(users);
        } catch (Exception e) {
            return ResponseEntity.ok("Error: " + e.getMessage());
        }
    }

    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User saved = userRepository.save(user);
        return ResponseEntity.ok(saved);
    }
}
```

Найдите проблемы.

<details>
<summary>Разбор</summary>

1. **SQL-инъекция.** `name` из запроса конкатенируется прямо в SQL. Значение `' OR '1'='1` вернёт всю таблицу; на многих драйверах можно дальше — `'; DROP TABLE ...`. Это критическая уязвимость, первый комментарий на любом ревью. Лечится параметризованным запросом: `jdbcTemplate.query("SELECT * FROM users WHERE name LIKE ?", mapper, "%" + name + "%")`.

2. **200 OK на ошибку.** И «не найдено», и исключение возвращаются со статусом 200 и строкой в теле. Клиент не может отличить успех от сбоя без парсинга текста; мониторинг по статус-кодам показывает, что всё здорово, когда сервис лежит. Правильно: пустой результат — 200 с пустым списком (это не ошибка), исключение — 500 через `@ControllerAdvice`/`@ExceptionHandler`, битый запрос — 400. И в ответ клиенту не должен уходить `e.getMessage()` — там могут быть детали схемы БД и внутренних адресов.

3. **Наружу торчит entity.** `createUser` принимает и возвращает JPA-сущность `User` напрямую. Последствия: клиент может прислать любые поля, включая те, что не должен ставить сам (id, роль, флаги) — mass assignment; в ответе уезжают все поля entity, включая хеш пароля; схема API жёстко сшита со схемой БД. Нужны отдельные DTO на вход и выход с маппингом.

4. **Нет валидации входа.** `@RequestBody User user` без `@Valid` — сохранится и пользователь с пустым email, и с отрицательным возрастом, всё зависит от милости констрейнтов в базе. Минимум: `@Valid @RequestBody CreateUserRequest` с `@NotBlank`, `@Email` и обработчиком `MethodArgumentNotValidException`, который вернёт 400 со списком ошибок.

Отдельный повод для разговора: `BeanPropertyRowMapper` с `SELECT *` хрупок к переименованию колонок, а `LIKE '%...%'` не использует индекс — но это уже следующий уровень после безопасности и контракта.

</details>

---

### 7. Сниппет: BigDecimal и деньги

```java
public class PaymentCalculator {

    public double calculateTotalWithCommission(double amount, double commissionRate) {
        double commission = amount * commissionRate;
        double total = amount + commission;
        return Math.round(total * 100) / 100.0;
    }

    public boolean isPaymentComplete(double paid, double expected) {
        return paid == expected;
    }

    public BigDecimal convertToPoints(double amount) {
        BigDecimal points = new BigDecimal(amount);
        return points.multiply(new BigDecimal("10"));
    }

    public BigDecimal splitBill(double total, int people) {
        BigDecimal bill = new BigDecimal(total);
        return bill.divide(new BigDecimal(people));
    }
}
```

Найдите проблемы.

<details>
<summary>Разбор</summary>

1. **`double` для денег.** Весь класс считает суммы в `double`, а double — бинарная арифметика с погрешностью: `0.1 + 0.2 != 0.3`. На комиссиях и округлениях ошибка копится и периодически «всплывает» в копейку — сводки не сойдутся, платёж не пройдёт сверку. Деньги считаются в `BigDecimal` (или в минорных единицах — копейках — в `long`). `Math.round(total * 100) / 100.0` — попытка округлить double, которая сама страдает от той же погрешности и теряет само понятие scale.

2. **Сравнение `double` через `==`.** `paid == expected` почти никогда не сработает так, как ждут: `0.1 + 0.2 == 0.3` — это `false`. Два значения, вычисленные разными путями, практически никогда не совпадут бит в бит. Если уж сравнивать double — то с эпсилоном, но правильный ответ — `BigDecimal.compareTo` (не `equals`: у `equals` учитывается scale, `1.0` и `1.00` не равны) или сравнение в копейках.

3. **`new BigDecimal(double)`.** Конструктор от double берёт точное бинарное представление: `new BigDecimal(0.1)` — это `0.1000000000000000055511151231257827021181583404541015625`. Погрешность вносится на входе, и дальше BigDecimal честно её размножает. Правильно: `BigDecimal.valueOf(double)` (берёт каноническую строку) или лучше `new BigDecimal("0.1")` — от строки, когда значение известно на старте.

4. **Деление без scale и rounding mode.** `bill.divide(new BigDecimal(people))` для суммы, которая не делится нацело (100 / 3), бросит `ArithmeticException: Non-terminating decimal expansion`. В платёжном коде это 500-ка пользователю на обычном «разделить счёт». Нужно явно: `divide(divisor, 2, RoundingMode.HALF_EVEN)` — с scale результата и банковским округлением.

</details>

---

### 8. Сниппет: строки и даты

```java
public class CsvExportService {

    private static final SimpleDateFormat DATE_FORMAT = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

    public String buildCsv(List<Transaction> transactions) {
        String csv = "id;amount;date;status\n";
        for (Transaction tx : transactions) {
            csv += tx.getId() + ";"
                    + tx.getAmount() + ";"
                    + DATE_FORMAT.format(tx.getCreatedAt()) + ";"
                    + tx.getStatus() + "\n";
        }
        return csv;
    }

    public boolean isCompleted(Transaction tx) {
        return tx.getStatus().equals("COMPLETED");
    }

    public boolean sameReference(Transaction a, Transaction b) {
        return a.getReference() == b.getReference();
    }
}
```

Найдите проблемы.

<details>
<summary>Разбор</summary>

1. **`SimpleDateFormat` в static-поле.** `SimpleDateFormat` не thread-safe: внутри у него мутабельный `Calendar`, и конкурентные `format()`/`parse()` перемешивают его состояние. В многопоточном сервисе это даёт не исключение, а тихую порчу данных: в CSV периодически попадают даты с чужими годом или месяцем. Самая коварная ошибка в списке — тестами ловится плохо. Лечится `DateTimeFormatter` (immutable, thread-safe) + `java.time`, либо `ThreadLocal`, но это уже костыль прошлого.

2. **Конкатенация строк в цикле.** `csv += ...` на каждой итерации создаёт новый `String` и копирует всё накопленное содержимое: десять тысяч транзакций — квадратичная работа и куча мусора для GC. Компилятор оптимизирует конкатенацию внутри одного выражения, но не между итерациями цикла. Лечится `StringBuilder` (или запись в `Writer` без промежуточной строки, если файл большой).

3. **`equals` в неправильном порядке.** `tx.getStatus().equals("COMPLETED")` бросит NPE, если статус null — а для черновиков транзакций null-статус вполне реален. Константа слева (`"COMPLETED".equals(tx.getStatus())`) или `Objects.equals(...)` делает сравнение null-безопасным. На собеседовании стоит добавить: ещё лучше enum вместо строки-статуса, тогда вопрос снимается сам.

4. **`==` для строк.** `a.getReference() == b.getReference()` сравнивает ссылки. Иногда «работает» из-за interning и строковых литералов, поэтому баг любит прятаться в тестах, но две строки с одинаковым содержимым, пришедшие из разных источников (из БД и из запроса), дадут `false`. Лечится `Objects.equals(a.getReference(), b.getReference())`.

</details>

---

### 9. Сниппет: Optional и Stream API

```java
public class RecommendationService {

    private final ProductRepository productRepository;
    private final List<Product> blacklisted = new ArrayList<>();

    public String getDefaultCategoryName(long userId) {
        Optional<User> user = findUser(userId);
        return user.get().getPreferences().getDefaultCategory().getName();
    }

    public List<String> getTopProductNames(List<Long> ids, int limit) {
        List<String> names = new ArrayList<>();
        ids.stream()
                .map(productRepository::findById)
                .filter(Optional::isPresent)
                .map(Optional::get)
                .sorted((p1, p2) -> Double.compare(p2.getRating(), p1.getRating()))
                .limit(limit)
                .forEach(p -> {
                    names.add(p.getName());
                    if (p.getRating() < 3.0) {
                        blacklisted.add(p);
                    }
                });
        return names;
    }

    public List<Integer> getPrices(List<Product> products) {
        return products.stream()
                .map(Product::getPrice)
                .map(price -> price * 1.2)
                .boxed()
                .collect(Collectors.toList());
    }

    private Optional<User> findUser(long userId) {
        return Optional.empty();
    }
}
```

Найдите проблемы.

<details>
<summary>Разбор</summary>

1. **`optional.get()` без проверки.** Весь смысл `Optional` — заставить обработать отсутствие значения; `get()` без `isPresent()` отбрасывает эту защиту и бросает `NoSuchElementException` — фактически тот же NPE, только дороже читается. Здесь пользователь может не найтись, и метод упадёт. Нормальные варианты: `map(User::getPreferences).map(...).orElse("Без категории")`, `orElseThrow(() -> new UserNotFoundException(userId))`. Отдельно: цепочка `getPreferences().getDefaultCategory()` — ещё два кандидата на обычный NPE.

2. **Side-effect в `forEach`.** Стрим используется как цикл с побочными эффектами: наполняет внешний `names` и, что хуже, мутирует поле сервиса `blacklisted` — разделяемое состояние синглтон-бина. При параллельных запросах это race condition на `ArrayList` (а если кто-то добавит `.parallel()` — ещё и на самом стриме). Сборку результата надо делать через `collect(toList())`, а бизнес-эффект «добавить в чёрный список» — отдельным явным шагом после стрима, и не в поле сервиса, а в репозиторий. Плюс логика странная сама по себе: в «чёрный список» попадают только те товары, что прошли `limit`, — завтра лимит поменяют, и поведение молча изменится.

3. **Лишний `boxed()`.** `map(Product::getPrice)` предположительно даёт `Double`, тогда `.map(price -> price * 1.2)` остаётся `Stream<Double>`, и `boxed()` там нечего боксить — лишний вызов, который читается как «тут был примитивный стрим», запутывая читающего. Если же `getPrice` — `double`, то проблема глубже: деньги в double (см. сниппет про BigDecimal), и `boxed()` — последнее, о чём стоит думать. На ревью комментарий звучит так: либо уберите `boxed()` за ненадобностью, либо, если стрим примитивный, используйте `mapToObj`/коллектор соответствующего примитивного стрима.

4. Бонус для разговора: `filter(Optional::isPresent).map(Optional::get)` — идиома, которая на Java 9+ пишется как `.flatMap(Optional::stream)`.

</details>

---

### 10. Как проводить код-ревью в команде

**Ответ:**

Это поведенческий вопрос, и готовят его как рассказ о вашем реальном опыте: как вы ревьюите, что для вас блокер, а что нитпик. Структура хорошего ответа:

**Что комментировать в первую очередь.** Приоритеты те же, что и при разборе сниппета: корректность и баги → безопасность → архитектура и дизайн → читаемость → стиль. Стиль, покрытый линтером и форматтером, вообще не должен обсуждаться людьми: если за каждым MR спорят про отступы, проблема не в коде, а в отсутствии настроенного инструмента. Блокерами (request changes) должны быть только баги, дыры в безопасности и архитектурные решения, которые дорого откатывать. Всё остальное — комментарии, не блокирующие мерж.

**Как формулировать замечания.** Главное правило: комментируем код, а не автора. «Здесь соединение не закрывается при исключении» вместо «ты опять забыл закрыть ресурс». Формулировать лучше вопросами, а не приказами: «а что будет, если сюда придёт null?» вместо «добавь проверку на null» — вопрос оставляет автору пространство, иногда оказывается, что null там невозможен по контракту, и вы узнали что-то новое о коде. И всегда объясняйте «почему»: замечание без причины воспринимается как каприз.

**Нитпики и префиксы.** Удобная практика — префиксы в комментариях, чтобы автор сразу понимал вес замечания:

- `nit:` — мелочь, чинить необязательно («nit: название `data` ничего не говорит, может `activeUsers`?»);
- `question:` — не замечание, а попытка понять («question: почему тут EAGER, а не LAZY?»);
- `suggestion:` — как сделал бы я, но на усмотрение автора;
- `blocker:` / без префикса — то, без чего аппрува не будет.

**Скорость ревью.** Ревью — это чужая блокирующая работа: автор сидит и ждёт. Норма зрелых команд — первая реакция в течение рабочего дня, лучше пары часов. Если нет времени на полноценное ревью, лучше сказать «посмотрю к 16:00», чем молчать. Сам MR тоже влияет на скорость: ревьюить 1500 строк никто не будет, поэтому большие задачи режутся на маленькие MR по 200–400 строк.

**Чего не делать:**

- **Резиновый штамп «LGTM».** Аппрув без чтения обесценивает всю практику: когда настоящий баг пройдёт через трёх аппрувнувших, доверие к ревью в команде умрёт. Не успеваете — скажите прямо или попросите переназначить.
- **Переписывание под себя.** «Я бы сделал иначе» — не аргумент. Если код корректен, читаем и покрыт тестами, вкусовые различия — не повод для request changes. Иначе ревью превращается в навязывание личного стиля, и люди начинают писать код «под ревьюера».
- **Ревью личности.** Никаких «ну ты как всегда» и сарказма. После такого люди перестают показывать код заранее и ревью мертвеет как процесс.
- **Восемь итераций по кругу.** Если спор зашёл в третий раунд комментариев — созвонитесь на 10 минут, текстом договариваться дороже.

Сеньорский штрих в конце ответа: ревью — это не только контроль качества, но и способ делиться знанием о кодовой базе. Поэтому ревьюить должен не один «главный», а вся команда, включая джунов: даже комментарий «не понял, что делает этот метод» — ценный сигнал, что код надо упростить.

---

[← Вернуться на главную страницу](README.md)
