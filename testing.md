# Тестирование и Качество Кода

В данном разделе собраны наиболее часто задаваемые на собеседованиях вопросы по тестированию в Java (JUnit 5, Mockito, Testcontainers), прошедшие строгую логическую дедупликацию.

## Оглавление
1. [Что такое модульное (юнит) тестирование и чем оно отличается от интеграционного и компонентного?](#1-что-такое-модульное-юнит-тестирование-и-чем-оно-отличается-от-интеграционного-и-компонентного)
2. [Пирамида тестирования и основные виды тестов (включая микросервисы)](#2-пирамида-тестирования-и-основные-виды-тестов-включая-микросервисы)
3. [Виды тестовых объектов (Test Doubles): Dummy, Fake, Stub, Spy, Mock](#3-виды-тестовых-объектов-test-doubles-dummy-fake-stub-spy-mock)
4. [Фреймворк JUnit 5: жизненный цикл тестов, фикстуры и Assertions](#4-фреймворк-junit-5-жизненный-цикл-тестов-фикстуры-и-assertions)
5. [Использование Mockito: создание моков, заглушек (stubs) и верификация поведения](#5-использование-mockito-создание-моков-заглушек-stubs-и-верификация-поведения)
6. [Тестирование Spring-приложений: контекст, WebTestClient и Testcontainers](#6-тестирование-spring-приложений-контекст-webtestclient-и-testcontainers)
7. [Параметризованные тесты в JUnit 5: @ParameterizedTest, @ValueSource, @CsvSource и @MethodSource](#7-параметризованные-тесты-в-junit-5-parameterizedtest-valuesource-csvsource-и-methodsource)
8. [AssertJ и Hamcrest: fluent-assertions против стандартных assertEquals](#8-assertj-и-hamcrest-fluent-assertions-против-стандартных-assertequals)
9. [WireMock: тестирование HTTP-клиентов без реальных сетевых вызовов](#9-wiremock-тестирование-http-клиентов-без-реальных-сетевых-вызовов)
10. [Что можно и нельзя мокать в Mockito: final-классы и static-методы (mockito-inline и MockMaker)](#10-что-можно-и-нельзя-мокать-в-mockito-final-классы-и-static-методы-mockito-inline-и-mockmaker)
11. [Покрытие кода: JaCoCo, виды coverage (line и branch) и mutation testing с PIT](#11-покрытие-кода-jacoco-виды-coverage-line-и-branch-и-mutation-testing-с-pit)
12. [@SpringBootTest против слайс-тестов: @WebMvcTest, @DataJpaTest, @JsonTest](#12-springboottest-против-слайс-тестов-webmvctest-datajpatest-jsontest)
13. [Принципы хороших тестов: F.I.R.S.T, независимость и одна проверка на тест](#13-принципы-хороших-тестов-first-независимость-и-одна-проверка-на-тест)
14. [TDD и BDD: red-green-refactor и Given-When-Then](#14-tdd-и-bdd-red-green-refactor-и-given-when-then)
15. [Тестирование асинхронного кода: Awaitility](#15-тестирование-асинхронного-кода-awaitility)
16. [@Transactional, @Sql и @DirtiesContext в Spring-тестах](#16-transactional-sql-и-dirtiescontext-в-spring-тестах)
17. [Тестирование Kafka: Embedded Kafka, Testcontainers и моки консьюмеров](#17-тестирование-kafka-embedded-kafka-testcontainers-и-моки-консьюмеров)

---

### 1. Что такое модульное (юнит) тестирование и чем оно отличается от интеграционного и компонентного?

**Ответ:**

Тестирование программного обеспечения делится на различные уровни в зависимости от масштаба проверяемых компонентов.

*   **Модульное тестирование (Unit Testing)** — это тестирование наименьших логических единиц исходного кода (в Java это класс или отдельный метод) в полной изоляции от внешних зависимостей. Цель юнит-тестов — доказать, что конкретный алгоритм работает корректно при заданных входных данных. Для изоляции от базы данных, файловой системы или сети используются "дублеры" (Test Doubles, например моки и стабы).
*   **Компонентное тестирование (Component Testing)** — это тестирование более крупной части системы (например, целого сервиса со всей его бизнес-логикой), но всё ещё с изоляцией от тяжеловесных внешних систем. Грань между юнит- и компонентным тестированием часто размыта, но компонентный тест обычно затрагивает сразу несколько реальных классов, работающих в связке, мокая только I/O (БД, API).
*   **Интеграционное тестирование (Integration Testing)** — проверяет корректность взаимодействия между несколькими компонентами системы или между приложением и внешней средой (базами данных, брокерами сообщений, внешними API). Здесь используются реальные или максимально приближенные к реальным сервисы (например, поднимается база данных в Testcontainers).

**Ключевое отличие:** Юнит-тесты проверяют *логику* в изоляции и работают мгновенно. Интеграционные тесты проверяют *контракты и связь* компонентов, они требуют поднятия контекста (например, Spring Context) или внешних ресурсов, из-за чего выполняются медленнее.

Частый follow-up: «что именно считать юнитом?» Здесь есть две школы:

*   **Одиночные тесты (solitary, «лондонская» школа)** — изолируется класс: все его зависимости-коллабораторы заменяются моками. Тесты проверяют в том числе взаимодействие с зависимостями (`verify(...)`). Плюс — точная локализация причины падения; минус — тесты жёстко привязаны к внутренней реализации и ломаются при рефакторинге.
*   **Социальные тесты (sociable, «детройтская»/классическая школа)** — юнитом считается поведение, а не класс: связка реальных объектов работает вместе, мокаются только тяжёлые I/O-зависимости (БД, сеть). Тесты устойчивее к рефакторингу, но при падении причину искать сложнее.

На собеседовании не ждут «правильной» школы — ждут, что вы знаете об обеих и сознательно выбираете. По той же причине граница «юнит/интеграционный» в разных командах проходит по-разному: тест репозитория против H2 кто-то называет интеграционным, кто-то компонентным. Спорить о термине бессмысленно; важно понимать свойства теста: скорость, изоляция, хрупкость.

Ещё один практический признак: если тест может упасть из-за чего-то вне кода (сеть, время, занятый порт, порядок запуска) — это уже не юнит-тест, и его надо отделять (в Maven, например, `surefire` для юнит-тестов и `failsafe` для интеграционных, чтобы `mvn test` оставался быстрым).

---

### 2. Пирамида тестирования и основные виды тестов (включая микросервисы)

**Ответ:**

**Пирамида тестирования (по Майку Кону)** — это концептуальная модель, определяющая оптимальное соотношение различных типов тестов в проекте. Она рекомендует:
1.  **Unit Tests (Основание пирамиды):** Должны составлять большинство (около 70%). Они быстрые, дешевые в написании и поддержке.
2.  **Integration Tests (Средний уровень):** Проверяют связки компонентов (около 20%). Более медленные и хрупкие.
3.  **E2E (End-to-End) / UI Tests (Вершина):** Тестирование всей системы целиком через пользовательский интерфейс или внешнее API (около 10%). Самые медленные, дорогие и подверженные ложным срабатываниям (flaky tests).

**Тестирование в микросервисной архитектуре** дополнительно включает:
*   **Contract Testing (Контрактное тестирование):** Проверка того, что микросервис (Provider) соблюдает согласованный формат API, ожидаемый клиентом (Consumer). Популярный инструмент: *Spring Cloud Contract* или *Pact*.
*   **Chaos Engineering:** Умышленное внедрение сбоев (например, отключение сервисов) для проверки устойчивости системы.
*   **Load/Performance Testing:** Нагрузочное тестирование (Gatling, JMeter) для выявления узких мест при высоком RPS.

Две оговорки, которые ценят на собеседовании:

1.  Пропорции 70/20/10 — ориентир, а не догма. Соотношение зависит от проекта: в библиотеке почти всё может быть юнит-тестами, в тонком orchestration-сервисе почти вся ценность в интеграционных.
2.  Классический анти-паттерн — «рожок мороженого» (ice cream cone): мало юнит-тестов, много ручного тестирования и тяжёлых E2E. Сборка длится час, тесты flaky, разработчики им не доверяют и игнорируют падения — метрика «процент зелёных прогонов» становится бессмысленной. В микросервисах этот анти-паттерн особенно дорог: E2E, поднимающий пять сервисов вместе, держат в минимальном количестве, а уверенность на стыках получают через contract-тесты.

---

### 3. Виды тестовых объектов (Test Doubles): Dummy, Fake, Stub, Spy, Mock

**Ответ:**

При написании модульных тестов необходимо изолировать тестируемый класс (System Under Test — SUT) от его зависимостей. Для этого применяются "тестовые дублеры" (Test Doubles). Классификация на 5 типов — из книги Джерарда Мезароса «xUnit Test Patterns», популяризировал её Мартин Фаулер в статье «Mocks Aren't Stubs»:

1.  **Dummy (Пустышка):** Объекты, которые передаются в методы, но никогда не используются. Нужны только для компиляции и удовлетворения сигнатуры метода (например, `null` или пустая строка).
2.  **Fake (Фальшивка):** Работающая реализация интерфейса, но с использованием "шорткатов", непригодных для продакшена (например, `InMemoryRepository`, использующий `HashMap` вместо реальной базы данных).
3.  **Stub (Заглушка):** Объект, возвращающий жестко заданный ответ (hardcoded data) на вызовы методов. Он не содержит логики и не запоминает вызовы. Используется для имитации ответов внешних систем.
4.  **Spy (Шпион):** Обертка над реальным объектом (или стабом), которая запоминает, какие методы были вызваны, сколько раз и с какими аргументами.
5.  **Mock (Мок):** Интеллектуальный объект, который настраивается на ожидание определенных вызовов (с проверкой аргументов). Если вызов не происходит или аргументы не совпадают, мок провалит тест.

*Пример концепций в Mockito:* `Mockito.mock()` по умолчанию создает микс из Stub и Mock, так как позволяет задавать поведение (`when().thenReturn()`) и верифицировать вызовы (`verify()`).

Частый follow-up: «мок или стаб — что выбрать?» По сути это выбор между проверкой *состояния* (state verification — проверяем результат работы) и проверкой *поведения* (behavior verification — проверяем, что SUT вызвал зависимость определённым образом). Проверка поведения уместна там, где сам вызов и есть ценность (отправка события в Kafka, списание денег). А вот верифицировать каждый геттер и каждый промежуточный вызов — вредно: тест оказывается «переспецифицированным» (overspecified) и ломается при любом безобидном рефакторинге, хотя поведение системы не изменилось. На собеседовании это ловушка: кандидат, который верифицирует всё подряд, демонстрирует, что не понимает цену хрупких тестов.

---

### 4. Фреймворк JUnit 5: жизненный цикл тестов, фикстуры и Assertions

**Ответ:**

**JUnit 5** — стандартный фреймворк для тестирования в Java. Он состоит из трех модулей: *JUnit Platform* (запуск тестов на JVM), *JUnit Jupiter* (API для написания тестов) и *JUnit Vintage* (поддержка старых тестов JUnit 4).

**Фикстуры (Fixtures)** — это подготовка состояния среды, необходимого для выполнения теста (подготовка данных, инициализация объектов).
В JUnit 5 используются следующие аннотации жизненного цикла:
*   `@BeforeAll` (замена `@BeforeClass` из JUnit 4) — выполняется один раз перед всеми тестами в классе (метод должен быть `static`).
*   `@BeforeEach` (замена `@Before`) — выполняется перед *каждым* тестовым методом. Идеально для сброса состояния (например, очистки моков).
*   `@AfterEach` (замена `@After`) — выполняется после каждого теста.
*   `@AfterAll` (замена `@AfterClass`) — выполняется один раз после всех тестов.
*   `@Disabled` (замена `@Ignore`) — отключает выполнение теста.

Почему `@BeforeAll` должен быть `static`: по умолчанию JUnit 5 создаёт **новый экземпляр тестового класса на каждый тестовый метод** (`@TestInstance(Lifecycle.PER_METHOD)`). Это сделано ради изоляции: поля класса не утекают между тестами. Но раз экземпляра «до всех тестов» ещё нет, «всеобщий» коллбэк может быть только статическим. Аннотация `@TestInstance(Lifecycle.PER_CLASS)` снимает это требование — тогда `@BeforeAll` может быть нестатическим, но платой будет общее состояние экземпляра между тестами.

Ещё два инструмента структурирования, о которых спрашивают:
*   `@Nested` — вложенные тестовые классы, группирующие тесты по сценариям. Внутренний класс видит фикстуру внешнего, и у каждого уровня вложенности могут быть свои `@BeforeEach` (выполняются снаружи внутрь). Удобно для стиля «один метод — один вложенный класс с группой сценариев».
*   `@RepeatedTest(n)` и `@Tag` — повторный запуск и метки для фильтрации (например, тег `slow`, чтобы гонять быстрые тесты локально, а полный набор — в CI).

**Assertions (Утверждения):** В отличие от ключевого слова `assert` в Java (которое можно отключить в JVM через флаг `-da`), методы из `org.junit.jupiter.api.Assertions` всегда выполняются. Кроме привычных `assertEquals` / `assertTrue` / `assertThrows` есть три, которые регулярно забывают:

*   `assertAll("user", () -> ..., () -> ...)` — групповая проверка: выполняет все лямбды и отчитывается по каждой упавшей, а не падает на первой (аналог SoftAssertions из AssertJ, см. вопрос 8).
*   `assertTimeout(ofMillis(100), () -> ...)` — падает, если код выполняется дольше лимита (вариант `assertTimeoutPreemptively` прерывает выполнение, но код при этом продолжает жить в другом потоке — осторожно с общими ресурсами).
*   `assumeTrue(...)` / `assumingThat(...)` — это *assumptions*, а не assertions: при невыполненном условии тест не падает, а помечается прерванным (aborted). Используют для тестов, осмысленных только в определённой среде (например, только на Linux или только с доступной БД). Ловушка: путать assumption с `assertTrue` — в первом случае тест молча «не считается», и так можно спрятать реально неработающий код.

```java
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

class CalculatorTest {

    private Calculator calculator;

    @BeforeEach
    void setUp() {
        calculator = new Calculator(); // Фикстура
    }

    @Test
    @DisplayName("Сложение двух положительных чисел")
    void testAddition() {
        int result = calculator.add(2, 3);
        assertEquals(5, result, "2 + 3 должно равняться 5");
    }

    @Test
    @DisplayName("Ожидание исключения при делении на ноль")
    void testDivisionByZero() {
        assertThrows(IllegalArgumentException.class, () -> calculator.divide(10, 0));
    }
}
```

---

### 5. Использование Mockito: создание моков, заглушек (stubs) и верификация поведения

**Ответ:**

**Mockito** — популярный фреймворк для создания Test Doubles (моков и шпионов) в Java.

Основные концепции:
1.  **Создание моков:** Осуществляется через `Mockito.mock()` или аннотацию `@Mock` (требуется `@ExtendWith(MockitoExtension.class)` в JUnit 5).
2.  **Задание поведения (Stubbing):** Осуществляется с помощью `when(...).thenReturn(...)` или `given(...).willReturn(...)` (в BDDMockito).
3.  **Верификация (Verification):** Проверка того, что метод был вызван с нужными параметрами через `verify(...)`.

```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;
import java.util.Optional;

import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

@ExtendWith(MockitoExtension.class)
class UserServiceTest {

    @Mock
    private UserRepository userRepository;

    @InjectMocks // Mockito автоматически внедрит userRepository в UserService
    private UserService userService;

    @Test
    void testFindUser_Success() {
        // Arrange: настраиваем стаб (заглушку)
        User mockUser = new User(1L, "Ivan");
        when(userRepository.findById(1L)).thenReturn(Optional.of(mockUser));

        // Act: вызываем тестируемый метод
        String userName = userService.getUserName(1L);

        // Assert: проверяем результат
        assertEquals("Ivan", userName);

        // Верификация: убеждаемся, что метод findById был вызван ровно 1 раз (Interaction testing)
        verify(userRepository, times(1)).findById(1L);
    }
}
```

Набор follow-up вопросов, на которых здесь обычно спотыкаются:

*   **Argument matchers и правило «всё или ничего».** Если для одного аргумента использован матчер (`any()`, `eq()`, `argThat()`), то *все* аргументы этого вызова должны быть матчерами. Смесь `when(repo.find(anyLong(), "Ivan"))` падает с `InvalidUseOfMatchersException` — лечится `eq("Ivan")`.
*   **ArgumentCaptor** — когда нужно проверить не факт вызова, а содержимое аргумента, который SUT собрал сам (например, объект события):

```java
ArgumentCaptor<UserEvent> captor = ArgumentCaptor.forClass(UserEvent.class);
verify(eventPublisher).publish(captor.capture());
assertEquals("USER_CREATED", captor.getValue().getType());
```

*   **Strict stubbing.** Начиная с Mockito 2, `MockitoExtension` по умолчанию работает в строгом режиме: стаб, который ни разу не использовался, роняет тест с `UnnecessaryStubbingException`. Это фича, а не баг — она вычищает мёртвые заглушки после рефакторинга. Отключить точечно можно через `lenient().when(...)`, но частое использование `lenient` — признак того, что фикстура раздута.
*   **`when(...).thenReturn(...)` против `doReturn(...).when(...)` для шпионов.** У `@Spy` запись `when(spy.get(0)).thenReturn(x)` при стаббинге вызывает *настоящий* метод `get(0)` — с реальными побочными эффектами или исключением. Для шпионов правильный синтаксис — `doReturn(x).when(spy).get(0)`. Кстати, `@Spy` в целом — инструмент для легаси: частичный мок класса, который вы же и тестируете, обычно означает, что класс стоит разделить.
*   **`@InjectMocks` молчит при ошибках.** Mockito пытается внедрить моки через конструктор, затем через сеттеры, затем через поля; если подходящего мока нет — поле останется `null` без всякого предупреждения, и вы получите NPE в середине теста. В новом коде надёжнее создавать SUT явно: `userService = new UserService(userRepository)` — тогда тест ещё и документирует реальные зависимости класса.

---

### 6. Тестирование Spring-приложений: контекст, WebTestClient и Testcontainers

**Ответ:**

Для интеграционного тестирования в Spring Boot используется аннотация `@SpringBootTest`, которая поднимает ApplicationContext.

**Важные инструменты:**
*   **`@MockBean`**: Заменяет реальный бин в Spring-контексте на Mockito-мок. Начиная со Spring Boot 3.4 аннотация объявлена deprecated в пользу **`@MockitoBean`** (`org.springframework.test.context.bean.override.mockito.MockitoBean`) — поведение то же, но механизм переехал в spring-test; если на проекте Boot 3.4+, в новых тестах пишут `@MockitoBean`. Аналогично `@SpyBean` → `@MockitoSpyBean`.
*   **`MockMvc`**: Позволяет тестировать контроллеры (Spring MVC), не поднимая полноценный HTTP-сервер (Tomcat), эмулируя запросы. Учтите: через `MockMvc` запросы не проходят реальный сетевой стек — не тестируются ни сериализация на уровне контейнера, ни, например, сжатие ответа.
*   **`WebTestClient`**: Реактивный неблокирующий клиент для тестирования WebFlux приложений, а также полноценного E2E тестирования (с поднятым сервером на случайном порту: `@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)`). Удобная деталь: `WebTestClient` умеет работать и поверх `MockMvc` (`bindToApplicationContext()`), так что его fluent-API доступен и без запуска сервера.

По умолчанию `@SpringBootTest` использует `webEnvironment = MOCK` — сервер не поднимается вовсе, и это самый частый режим. `RANDOM_PORT` / `DEFINED_PORT` нужны, только когда тестируете через настоящий HTTP (например, с `WebTestClient` или `TestRestTemplate`).

Ещё одна вещь, из-за которой тесты «вдруг» замедляются: Spring **кеширует ApplicationContext** между тестовыми классами с одинаковой конфигурацией. Каждый уникальный набор `@MockBean`, `@TestPropertySource` или `@ActiveProfiles` создаёт отдельный кешированный контекст — десять классов с разными моками = десять поднятых контекстов. Отсюда практическое правило: держать конфигурацию тестов однородной (общие абстрактные классы с одинаковым набором моков).

**Testcontainers:** Фреймворк, позволяющий поднимать Docker-контейнеры (PostgreSQL, Kafka, Redis) прямо из Java-кода. Это решает проблему "на моем компьютере тесты работают", обеспечивая идентичное окружение для интеграционных тестов везде.

```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.DynamicPropertyRegistry;
import org.springframework.test.context.DynamicPropertySource;
import org.testcontainers.containers.PostgreSQLContainer;
import org.testcontainers.junit.jupiter.Container;
import org.testcontainers.junit.jupiter.Testcontainers;
import static org.junit.jupiter.api.Assertions.assertTrue;

@SpringBootTest
@Testcontainers
class UserRepositoryIntegrationTest {

    // Поднимаем реальную базу данных в Docker-контейнере перед запуском тестов
    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:15-alpine");

    // Динамически передаем настройки подключения в Spring Context
    @DynamicPropertySource
    static void configureProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }

    @Autowired
    private UserRepository userRepository;

    @Test
    void testSaveAndFind() {
        User user = new User("Anna");
        userRepository.save(user);

        assertTrue(userRepository.findById(user.getId()).isPresent());
    }
}
```

---

### 7. Параметризованные тесты в JUnit 5: @ParameterizedTest, @ValueSource, @CsvSource и @MethodSource

**Ответ:**

Когда одну и ту же логику нужно проверить на десятке наборов входных данных, писать десять почти одинаковых тестов — плохая идея. Для этого в JUnit 5 есть `@ParameterizedTest`: один тест-шаблон, который фреймворк запускает отдельно для каждого набора аргументов. Каждый запуск — полноценный тест в отчёте, поэтому падение видно точечно.

Основные источники аргументов:
*   `@ValueSource` — простые значения одного типа (строки, числа).
*   `@CsvSource` — пары/тройки значений в CSV-формате, JUnit сам приводит типы.
*   `@MethodSource` — метод, возвращающий `Stream<Arguments>`. Единственный вариант, когда аргументы — сложные объекты.
*   Есть ещё `@EnumSource`, `@NullSource`, `@EmptySource` и `@ArgumentsSource` (свой провайдер).

```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.*;
import java.util.stream.Stream;
import static org.junit.jupiter.api.Assertions.*;

class PriceCalculatorTest {

    @ParameterizedTest(name = "{index}: скидка {0}% от {1} = {2}")
    @CsvSource({
        "10, 1000, 900",
        "0,  1000, 1000",
        "50, 200,  100"
    })
    void calculatesDiscount(int percent, int price, int expected) {
        assertEquals(expected, PriceCalculator.applyDiscount(percent, price));
    }

    @ParameterizedTest
    @ValueSource(strings = {"", "  ", "\t"})
    void rejectsBlankEmail(String email) {
        assertFalse(EmailValidator.isValid(email));
    }

    @ParameterizedTest
    @MethodSource("invalidUsers")
    void rejectsInvalidUsers(User user, String reason) {
        var ex = assertThrows(ValidationException.class, () -> service.register(user));
        assertTrue(ex.getMessage().contains(reason));
    }

    static Stream<Arguments> invalidUsers() {
        return Stream.of(
            Arguments.of(new User(null, 25), "name"),
            Arguments.of(new User("Ivan", -1), "age")
        );
    }
}
```

Частый follow-up: почему метод в `@MethodSource` должен быть `static`. Потому что он вызывается до создания экземпляра тестового класса. Если хочется нестатический метод — ставят `@TestInstance(TestInstance.Lifecycle.PER_CLASS)`, но тогда состояние класса живёт между тестами, что ломает изоляцию. Ловушка в том, что JUnit молча не подскажет: просто получите ошибку про невозможность вызвать нестатический метод.

---

### 8. AssertJ и Hamcrest: fluent-assertions против стандартных assertEquals

**Ответ:**

`assertEquals(expected, actual)` из JUnit работает, но на этом его достоинства заканчиваются. AssertJ и Hamcrest — библиотеки более выразительных проверок. Они обе входят в `spring-boot-starter-test`, так что в Spring-проекте они обычно уже есть из коробки.

Проблемы голых JUnit-ассертов, которые они решают:
*   Порядок аргументов `assertEquals(expected, actual)` — вечный источник путаницы: перепутал местами — и сообщение об ошибке вводит в заблуждение.
*   Для коллекций и объектов `assertEquals` говорит только «не совпало». AssertJ покажет, какие именно элементы лишние и каких не хватает.
*   Цепочки условий (`result.size() == 3 && result.get(0).isActive() && ...`) читаются плохо и падают без деталей.

```java
import static org.assertj.core.api.Assertions.*;

// AssertJ: читается как предложение, начинается с фактического значения
assertThat(users)
    .hasSize(2)
    .extracting(User::getName)
    .containsExactlyInAnyOrder("Ivan", "Anna");

assertThatThrownBy(() -> service.register(null))
    .isInstanceOf(ValidationException.class)
    .hasMessageContaining("name");

// Soft assertions: собирает ВСЕ нарушения, а не падает на первом
SoftAssertions.assertSoftly(softly -> {
    softly.assertThat(user.getName()).startsWith("Iv");
    softly.assertThat(user.getAge()).isPositive();
});
```

Hamcrest — старше и строится на матчерах: `assertThat(actual, is(expected))`, `hasItem(...)`, `containsString(...)`. Сам по себе как основной ассерт-фреймворк его сейчас используют редко, но он живёт внутри MockMvc: `jsonPath("$.name", is("Ivan"))` — это Hamcrest-матчеры.

На практике обычно берут AssertJ как основной инструмент, а Hamcrest встречается «по наследству» — в MockMvc и старых тестах. Аргумент за AssertJ на собеседовании: автодополнение в IDE после `assertThat(x)` само подсказывает релевантные для типа проверки, а текст ошибки читается без отладчика.

---

### 9. WireMock: тестирование HTTP-клиентов без реальных сетевых вызовов

**Ответ:**

Если сервис ходит во внешний HTTP API, в тестах нельзя ни звонить по-настоящему (медленно, нестабильно, нет сети в CI), ни мокать сам `RestTemplate` через Mockito — так вы тестируете Mockito, а не свой код. Сериализация JSON, заголовки, обработка 4xx/5xx и таймаутов при таком моке остаются за кадром. WireMock решает это иначе: он поднимает локальный HTTP-сервер, который отвечает заранее настроенными заглушками. Клиент работает как с настоящим API, просто по адресу `localhost`.

```java
import com.github.tomakehurst.wiremock.junit5.WireMockExtension;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.RegisterExtension;
import static com.github.tomakehurst.wiremock.client.WireMock.*;
import static com.github.tomakehurst.wiremock.core.WireMockConfiguration.wireMockConfig;
import static org.junit.jupiter.api.Assertions.*;

class PaymentClientTest {

    @RegisterExtension
    static WireMockExtension wireMock = WireMockExtension.newInstance()
            .options(wireMockConfig().dynamicPort())
            .build();

    @Test
    void returnsPaymentStatus() {
        wireMock.stubFor(get(urlEqualTo("/payments/42"))
                .willReturn(okJson("{\"status\": \"APPROVED\"}")));

        PaymentClient client = new PaymentClient(wireMock.baseUrl());

        assertEquals("APPROVED", client.getStatus(42L));
        wireMock.verify(getRequestedFor(urlEqualTo("/payments/42"))
                .withHeader("Authorization", matching("Bearer .*")));
    }
}
```

Главная ценность WireMock — негативные сценарии, которые с реальным API не воспроизведёшь по щелчку пальцев: `serverError()`, `fault(Fault.CONNECTION_RESET_BY_PEER)`, задержки через `withFixedDelay(5000)` для проверки таймаутов. Ещё он умеет записывать заглушки с реального сервиса (record & playback), но записанные стабы быстро протухают, на практике их лучше писать руками.

Для RestTemplate/WebClient есть и более лёгкая альтернатива без отдельного сервера — `MockRestServiceServer` (в паре с `@RestClientTest`). Она перехватывает запросы на уровне клиентского бина, но сетевые сценарии вроде обрыва соединения так не смоделируешь.

---

### 10. Что можно и нельзя мокать в Mockito: final-классы и static-методы (mockito-inline и MockMaker)

**Ответ:**

Из коробки Mockito создаёт моки через динамическое наследование (subclass MockMaker на Byte Buddy): подкласс переопределяет методы и подставляет нужное поведение. Отсюда классические ограничения — нельзя мокать то, что нельзя переопределить:
*   `final`-классы и `final`-методы;
*   `static`-методы (они принадлежат классу, а не экземпляру);
*   `private`-методы и конструкторы.

Эти ограничения снимает **inline MockMaker**: вместо наследования он через инструментацию (Java agent) подменяет байт-код самого класса. Раньше для этого подключали отдельный артефакт `mockito-inline`; начиная с Mockito 5 inline-режим стал поведением по умолчанию, и `final`-классы мокаются без дополнительных телодвижений.

Статика мокается отдельным API, и это единственный правильный способ:

```java
import org.mockito.MockedStatic;
import static org.mockito.Mockito.*;

@Test
void usesCurrentTime() {
    Instant fixed = Instant.parse("2024-01-01T00:00:00Z");

    // mockStatic работает только внутри try-with-resources
    try (MockedStatic<Instant> mocked = mockStatic(Instant.class)) {
        mocked.when(Instant::now).thenReturn(fixed);

        assertEquals(fixed, service.currentTime());
    }
}
```

Обратите внимание на try-with-resources: статический мок действует глобально на класс, и без явного закрытия он протечёт в другие тесты — классический источник «магических» падений, когда тесты зелёные по отдельности и красные пачкой.

Теперь главное для собеседования: потребность мокать статику — почти всегда запах дизайна. Статический вызов — это скрытая зависимость, которую не видно в сигнатуре и нельзя подменить через конструктор. Правильное лечение — завернуть вызов в интерфейс (`Clock` вместо `Instant.now()`, `IdGenerator` вместо `UUID.randomUUID()`) и инжектить его. `mockStatic` — инструмент для легаси, а не целевое состояние кода. Кстати, для новых объектов внутри метода есть `mockConstruction(...)`, но и он лечит симптом, а не причину.

---

### 11. Покрытие кода: JaCoCo, виды coverage (line и branch) и mutation testing с PIT

**Ответ:**

Покрытие кода (code coverage) — это метрика того, какая доля кода выполнилась при прогоне тестов. Стандартный инструмент в Java — **JaCoCo**: он инструментирует байт-код во время тестов и строит отчёт (HTML/XML) по классам и строкам. В CI его отчёт обычно уходит в SonarQube, где можно выставить quality gate — сборка падает, если покрытие ниже порога.

JaCoCo считает несколько метрик:
*   **Line coverage** — какие строки кода выполнились хотя бы раз.
*   **Branch coverage** — какие ветки условий (`if/else`, `switch`, тернарники) пройдены в обе стороны. Метрика строже: одна строка `if (a && b)` — это две ветки.
*   Ещё есть instruction (байт-код инструкции) и method coverage, но в разговоре обычно имеют в виду line и branch.

Почему 100% покрытие — плохая цель:
1.  Покрытие измеряет только *выполнение*, а не *проверку*. Тест без единого assert'а покрывает код идеально. Команды под давлением метрики быстро учатся писать такие тесты.
2.  Последние проценты стоят дорого: геттеры, `toString()`, обработчики маловероятных ошибок. Цена теста перестаёт окупаться.
3.  100% покрытия не говорит ничего о корректности: код `if (balance >= amount)` полностью покрыт тестами, даже если должен быть `>` — граничное условие никто не проверил.

Разумная цель на практике — 70–80% branch coverage плюс обязательное покрытие критичной бизнес-логики.

Чтобы проверить качество самих тестов, есть **mutation testing** с фреймворком **PIT (Pitest)**. Идея: PIT автоматически вносит в байт-код мутации (меняет `>` на `>=`, убирает вызов метода, инвертирует условие) и прогоняет тесты. Если после мутации все тесты зелёные — мутант «выжил», значит, тесты эту ошибку не ловят. Метрика называется mutation score, и она куда честнее coverage. Цена — скорость: PIT прогоняет тесты десятки раз, поэтому обычно его гоняют ночью или на изменённых классах.

---

### 12. @SpringBootTest против слайс-тестов: @WebMvcTest, @DataJpaTest, @JsonTest

**Ответ:**

`@SpringBootTest` поднимает полный ApplicationContext — все бины, автоконфигурации, инфраструктуру. Это даёт максимальную уверенность, но стоит секунды на каждый тестовый класс, а если контекст по какой-то причине не переиспользуется (разные `@MockBean`, разные properties — Spring кеширует контексты по конфигурации) — то и десятки секунд. На сотне тестовых классов сборка превращается в кофе-брейк.

Слайс-тесты режут эту проблему: каждая аннотация поднимает только «срез» контекста, нужный для одного слоя:

*   **`@WebMvcTest`** — только инфраструктура Spring MVC: контроллеры, `@ControllerAdvice`, фильтры, конвертеры. Сервисы и репозитории в контекст не попадают — их мокают через `@MockBean`. HTTP-сервер не поднимается, запросы идут через `MockMvc`.
*   **`@DataJpaTest`** — только JPA-слой: репозитории, сущности, `TestEntityManager`. По умолчанию подменяет DataSource на встроенную H2; против настоящей PostgreSQL — `@AutoConfigureTestDatabase(replace = NONE)` плюс Testcontainers.
*   **`@JsonTest`** — только Jackson: проверка сериализации/десериализации DTO (форматы дат, `@JsonProperty`, игнорируемые поля).

```java
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.beans.factory.annotation.Autowired;
import org.junit.jupiter.api.Test;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@WebMvcTest(UserController.class) // только MVC-слой и этот контроллер
class UserControllerTest {

    @Autowired MockMvc mockMvc;

    @MockBean UserService userService; // сервиса в контексте нет — мокаем (в Boot 3.4+ пишут @MockitoBean, см. вопрос 6)

    @Test
    void returnsUser() throws Exception {
        when(userService.getById(1L)).thenReturn(new User(1L, "Ivan"));

        mockMvc.perform(get("/users/1"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.name").value("Ivan"));
    }
}
```

Компромисс понятен: скорость против полноты. Слайс быстрый, но не поймает проблемы на стыке слоёв — неправильный бин, конфликт автоконфигураций, ошибку в маппинге транзакций. Здоровый набор на практике: масса юнит-тестов, слайсы для контроллеров и репозиториев, и несколько `@SpringBootTest` на ключевые end-to-end сценарии внутри сервиса (то, что иногда называют smoke-тестами контекста).

---

### 13. Принципы хороших тестов: F.I.R.S.T, независимость и одна проверка на тест

**Ответ:**

Тест — это тоже код, и к нему применимы те же требования, что и к продакшен-коду. Базовый чек-лист — акроним **F.I.R.S.T**:

*   **Fast** — быстрые. Юнит-тест должен занимать миллисекунды. Медленные тесты перестают запускать, и они теряют смысл.
*   **Independent** — независимые. Тест не зависит от других тестов и от порядка выполнения: никаких общих изменяемых состояний, статических счётчиков, «сначала создай пользователя в тесте А». Каждый тест готовит свой контекст сам.
*   **Repeatable** — воспроизводимые. Одинаковый результат в любой среде и в любое время. Источники невоспроизводимости: зависимость от текущего времени (`Instant.now()` вместо `Clock`), случайности, сети, порядка итерации по `HashMap`.
*   **Self-validating** — самопроверяемые. Результат бинарный: зелёный/красный. Если для понимания результата надо читать логи или консоль — это не тест.
*   **Timely** — своевременные. Пишутся вместе с кодом (или до него), а не «когда-нибудь потом». Потом не наступает.

Про «один assert на тест» — частый вопрос и частое заблуждение. Корректная формулировка: **одна проверка — один смысл** (one concept per test). Три `assertThat` подряд, проверяющие три поля одного результата, — нормально (и SoftAssertions для этого придуман). Плохо, когда тест проверяет создание заказа, отправку письма и списание со склада — при падении непонятно, что сломалось, и название теста врёт.

Про тестовые данные. Копипаста `new User("Ivan", 30, "ivan@mail.com", ...)` в каждом тесте быстро превращается в ад при добавлении поля в конструктор. Два рабочих паттерна:
*   **Object Mother / фабричные методы**: `TestUsers.ivan()` — готовый валидный объект с осмысленными дефолтами.
*   **Test Data Builder**: `aUser().withBlocked(true).build()` — дефолты валидные, меняется только то, что важно для конкретного теста. Это ключевой момент: читатель теста сразу видит, какое поле здесь существенно, а какие — шум.

Отдельно: не переиспользуйте изменяемые фикстуры между тестами. Общий `@BeforeEach`, создающий новые объекты, — хорошо; общий `static` список, который тесты мутируют, — источник flaky-тестов.

---

### 14. TDD и BDD: red-green-refactor и Given-When-Then

**Ответ:**

**TDD (Test-Driven Development)** — техника разработки, в которой тест пишется до кода. Цикл классический:

1.  **Red** — пишем тест на ещё не существующее поведение. Он должен упасть (и важно убедиться, что падает именно по той причине, которую мы ожидаем).
2.  **Green** — пишем минимальный код, чтобы тест прошёл. Без «заодно» и «на будущее».
3.  **Refactor** — чистим код и тесты, пока всё зелёное. Тесты здесь — страховка, позволяющая рефакторить без страха.

Ценность TDD не в «тестах раньше», а в том, что API класса проектируется с точки зрения потребителя: сначала вы пишете, как кодом *удобно пользоваться*, и только потом реализацию. Побочный эффект — код получается тестируемым по построению, с инжекцией зависимостей и без статики.

**BDD (Behavior-Driven Development)** — развитие TDD с акцентом на поведение и общий язык с бизнесом. Сценарии формулируются как **Given-When-Then**: *Given* (дано) — начальное состояние, *When* (когда) — действие, *Then* (тогда) — ожидаемый результат. Даже без инструментов этот шаблон полезен как структура теста — блоки `// Arrange / // Act / // Assert` в примерах выше это и есть. Как инструмент BDD обычно означает Cucumber: сценарии на языке Gherkin лежат в `.feature`-файлах, читаемы аналитиком, а Java-код реализует шаги.

```gherkin
Feature: Снятие денег со счёта

  Scenario: Недостаточно средств
    Given на счёте клиента 100 рублей
    When клиент пытается снять 150 рублей
    Then операция отклоняется с ошибкой "Недостаточно средств"
    And баланс остаётся 100 рублей
```

Где помогает:
*   TDD — в сложной бизнес-логике с множеством ветвлений, при исправлении багов (сначала воспроизводящий тест), в библиотечном коде с чётким контрактом.
*   BDD — там, где требования обсуждаются с не-разработчиками: сценарий на Gherkin становится живой спецификацией, которую бизнес реально читает.

Где мешает:
*   В исследовательском коде и прототипах, когда интерфейс ещё непонятен — писать тесты вперёд на то, что будет выброшено, дорого.
*   В тривиальном CRUD — церемония без выигрыша.
*   Cucumber без вовлечённого бизнеса — худший вариант: двойная поддержка (feature-файлы плюс step definitions), которую никто не читает. Если аналитик не смотрит в `.feature`-файлы, честнее писать обычные тесты с говорящими именами.

Частый follow-up: «TDD замедляет разработку?» На короткой дистанции — да, на дистанции месяцев обычно нет: меньше отладки и регрессий. Но доктринёрское «всё только через TDD» на собеседовании звучит подозрительно — интервьюер ждёт, что вы понимаете границы применимости.

---

### 15. Тестирование асинхронного кода: Awaitility

Асинхронная логика (обработчик очереди, `@Async`, реактивная цепочка, колбэк) не завершается к моменту проверки в тесте. Два плохих решения: `Thread.sleep(2000)` (медленно и флаки — на нагруженном CI двух секунд не хватит) и игнорирование результата.

**Awaitility** — библиотека, которая опрашивает условие, пока оно не выполнится или не истечёт таймаут:

```java
@Test
void orderIsProcessed() {
    orderProducer.send(new Order(42));

    await().atMost(Duration.ofSeconds(5))
           .pollInterval(Duration.ofMillis(100))
           .untilAsserted(() ->
               assertEquals(OrderStatus.PROCESSED, orderRepository.findById(42).getStatus())
           );
}
```

Тест завершается, как только условие выполнилось (не ждёт все 5 секунд), а при таймауте падает с понятным сообщением. `untilAsserted` удобен тем, что перевыбрасывает ошибку из последней неудачной проверки.

Альтернативы и дополнения:

- В реактивном коде — `StepVerifier` из reactor-test: `StepVerifier.create(flux).expectNext(...).verifyComplete()` — синхронная верификация асинхронного потока без ожиданий.
- Для `CompletableFuture` в юнит-тестах обычно достаточно `future.get(1, TimeUnit.SECONDS)` — но только если вы контролируете экзекутор; иначе это тот же sleep с другим лицом.
- Ловушка: если условие так и не наступило, Awaitility подождёт весь таймаут — ставьте разумные 2–5 секунд, а не 30.

---

### 16. @Transactional, @Sql и @DirtiesContext в Spring-тестах

Три инструмента управления состоянием между тестами, у каждого своя цена.

**`@Transactional` на тестовом классе** (из `spring-test`, не из бизнес-кода): каждый тест выполняется в транзакции, которая **откатывается** в конце — данные не утекают между тестами, `@DataJpaTest` ведёт себя так по умолчанию. Нюансы:

- Откат маскирует проблемы, которые проявляются только на коммите: нарушение constraint, ошибка на flush, триггеры. Тест зелёный, а в проде падает.
- В тесте с `TestEntityManager` или вложенных транзакциях поведение отличается от реального — ленивая загрузка вне сессии, кэш первого уровня общий на весь тест.
- Для E2E с реальным коммитом транзакцию на тесте отключают (`@Transactional(propagation = NOT_SUPPORTED)` или `@Commit`).

**`@Sql`** — выполнить SQL-скрипт до теста (или после): подготовка фикстур в БД.

```java
@Sql("/testdata/users.sql")
@Sql(scripts = "/testdata/cleanup.sql", executionPhase = Sql.ExecutionPhase.AFTER_TEST_METHOD)
```

**`@DirtiesContext`** — пометить ApplicationContext «грязным»: Spring закроет его и создаст заново для следующего теста. Нужен, когда тест меняет глобальное состояние контекста (порт, бин-синглтон, кэш). Цена огромная: подъём контекста — секунды, поэтому `@DirtiesContext` на каждом тестовом классе превращает прогон в вечность. До него стоит пробовать более дешёвые варианты: `@DynamicPropertySource` с Testcontainers, сброс состояния в `@AfterEach`.

Практический вывод: изоляцию данных лучше всего даёт свежая БД на класс/метод через Testcontainers (каждый тест-класс — свой контейнер или очистка таблиц), а `@Transactional`-откат — быстрый компромисс для большинства `@DataJpaTest`.

---

### 17. Тестирование Kafka: Embedded Kafka, Testcontainers и моки консьюмеров

Три подхода, от быстрого к честному.

**Моки (юнит-уровень).** У `kafka-clients` есть `MockProducer` и `MockConsumer`: продюсер-тест проверяет отправленные записи без брокера, консьюмер-тест скармливает записи руками. Подходит для логики сериализации/маршрутизации, не проверяет интеграцию.

**Embedded Kafka** (`@EmbeddedKafka` из spring-kafka-test): брокер поднимается в той же JVM. Быстро, не нужен Docker.

```java
@EmbeddedKafka(partitions = 1, topics = "orders")
class OrderListenerTest {

    @Autowired KafkaTemplate<String, String> template;

    @Test
    void consumes() {
        template.send("orders", "payload");
        // дальше Awaitility: ждём, пока листенер обработает
    }
}
```

Минусы: embedded-брокер — не настоящий Kafka (упрощённая семантика), конфликты портов на CI, долгий старт контекста.

**Testcontainers** — настоящий брокер в Docker-контейнере:

```java
@Testcontainers
class KafkaIT {

    @Container
    @ServiceConnection  // Spring Boot 3.1+: свойства подставятся сами
    static KafkaContainer kafka = new KafkaContainer(
        DockerImageName.parse("confluentinc/cp-kafka:7.6.0"));
}
```

Честная интеграция (реальные партиции, оффсеты, retry), но медленнее и требует Docker на CI.

Общие приёмы независимо от подхода: уникальное имя топика/`group.id` на тестовый класс (иначе тесты читают чужие сообщения), ожидание обработки через Awaitility, а не `Thread.sleep`, и проверка негативных сценариев — poison pill и отправка в DLQ (см. раздел по микросервисам).

---

[← Вернуться на главную страницу](README.md)
