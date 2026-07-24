# Spring Framework и Spring Boot

## Оглавление
1. [Инверсия контроля (IoC) и Внедрение зависимостей (DI)](#1-инверсия-контроля-ioc-и-внедрение-зависимостей-di)
2. [Spring Контейнеры: ApplicationContext и BeanFactory](#2-spring-контейнеры-applicationcontext-и-beanfactory)
3. [Spring Bean: Понятие, Области видимости (Scopes) и Жизненный цикл](#3-spring-bean-понятие-области-видимости-scopes-и-жизненный-цикл)
4. [Ключевые аннотации конфигурации: @Bean, @Component, @Autowired, @Primary, @Qualifier](#4-ключевые-аннотации-конфигурации-bean-component-autowired-primary-qualifier)
5. [Аспектно-Ориентированное Программирование (AOP) в Spring](#5-аспектно-ориентированное-программирование-aop-в-spring)
6. [Spring MVC и паттерн Front Controller (DispatcherServlet)](#6-spring-mvc-и-паттерн-front-controller-dispatcherservlet)
7. [Spring Boot: Преимущества, Автоконфигурация и Starters](#7-spring-boot-преимущества-автоконфигурация-и-starters)
8. [Управление транзакциями и аннотация @Transactional](#8-управление-транзакциями-и-аннотация-transactional)
9. [Spring Security: Архитектура, Аутентификация и JWT](#9-spring-security-архитектура-аутентификация-и-jwt)
10. [Создание RESTful API: @RestController, обработка исключений и валидация](#10-создание-restful-api-restcontroller-обработка-исключений-и-валидация)
11. [Циклические зависимости и их разрешение в Spring](#11-циклические-зависимости-и-их-разрешение-в-spring)
12. [Прокси в Spring: JDK Dynamic Proxy и CGLIB, проблема self-invocation](#12-прокси-в-spring-jdk-dynamic-proxy-и-cglib-проблема-self-invocation)
13. [Асинхронное и плановое выполнение: @Async и @Scheduled](#13-асинхронное-и-плановое-выполнение-async-и-scheduled)
14. [Spring Data JPA: derived queries, @Query, проекции и пагинация](#14-spring-data-jpa-derived-queries-query-проекции-и-пагинация)
15. [Профили и внешняя конфигурация: @Profile и application.yml](#15-профили-и-внешняя-конфигурация-profile-и-applicationyml)
16. [Кэширование: @Cacheable, @CacheEvict и @CachePut](#16-кэширование-cacheable-cacheevict-и-cacheput)
17. [BeanPostProcessor и BeanFactoryPostProcessor](#17-beanpostprocessor-и-beanfactorypostprocessor)
18. [События в Spring: ApplicationEvent и @EventListener](#18-события-в-spring-applicationevent-и-eventlistener)
19. [Spring WebFlux: реактивная модель, Mono и Flux](#19-spring-webflux-реактивная-модель-mono-и-flux)
20. [Валидация: Bean Validation, @Valid и @Validated](#20-валидация-bean-validation-valid-и-validated)
21. [Spring Boot Actuator: health, info, metrics и интеграция с Prometheus](#21-spring-boot-actuator-health-info-metrics-и-интеграция-с-prometheus)
22. [Логирование: SLF4J, Logback и MDC](#22-логирование-slf4j-logback-и-mdc)

---

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

**BeanFactory** — это базовый интерфейс IoC-контейнера. Он реализует паттерн «Фабрика», осуществляет отложенную инициализацию бинов (Lazy Initialization) — бины создаются только при первом обращении к ним (вызове метода `getBean()`). `BeanFactory` потребляет минимальное количество ресурсов, поэтому он может быть полезен в условиях ограниченной памяти, однако в современных enterprise-приложениях он практически не используется напрямую.

**ApplicationContext** — это расширенный интерфейс, который наследуется от `BeanFactory` и предоставляет дополнительные возможности корпоративного уровня. В отличие от `BeanFactory`, `ApplicationContext` по умолчанию инициализирует все singleton-бины сразу при запуске приложения (Eager Initialization) — ошибки конфигурации всплывают на старте, а не при первом запросе.

Дополнительные возможности `ApplicationContext`:
- Поддержка интернационализации (i18n) через интерфейс `MessageSource`.
- Публикация и прослушивание событий приложения (ApplicationEventPublisher).
- Поддержка AOP.
- Специфичные контексты для веб-приложений (например, `WebApplicationContext`).

На практике используется всегда `ApplicationContext`: Spring Boot стартует именно его (`AnnotationConfigServletWebServerApplicationContext` для сервлетного стека, `AnnotationConfigReactiveWebServerApplicationContext` для WebFlux). Eager-инициализацию можно отключить глобально — `spring.main.lazy-initialization=true`: ускоряет старт, но ошибки в бинах вылезут при первом обращении, а не при запуске.

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

Два нюанса, которые любят спрашивать следом. Первый: `@PreDestroy` и destroy-методы вообще вызываются только для singleton-бинов. Жизненным циклом prototype после создания контейнер не управляет — ресурсы такого бина освобождаете вы сами. Второй: singleton-scope не означает потокобезопасность. Экземпляр один на все потоки, поэтому изменяемого состояния в нём быть не должно — синхронизация на вашей совести.

---

### 4. Ключевые аннотации конфигурации: @Bean, @Component, @Autowired, @Primary, @Qualifier

- `@Component`: базовая аннотация уровня класса. Указывает, что класс является кандидатом для автоматического сканирования (`@ComponentScan`) и регистрации в качестве бина. Имеет специализации: `@Service`, `@Repository`, `@Controller`.
- `@Bean`: аннотация уровня метода, применяемая в конфигурационных классах (`@Configuration`). Метод возвращает объект, который будет зарегистрирован как бин. Это позволяет регистрировать в контексте классы из сторонних библиотек.
- `@Autowired`: аннотация для внедрения зависимостей. Указывает контейнеру Spring на необходимость автоматического связывания (autowiring) зависимости по типу (byType).
- `@Qualifier`: используется совместно с `@Autowired` для точного указания имени бина, если в контексте существует несколько бинов одного типа.
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

- Что будет, если два бина одного типа и нет ни `@Primary`, ни `@Qualifier`? `NoUniqueBeanDefinitionException` при старте приложения.
- Чем `@Component` отличается от `@Bean`? `@Component` ставится на ваш класс, экземпляр создаёт контейнер. `@Bean` — на метод в `@Configuration`-классе, объект создаёте вы сами: это способ зарегистрировать класс из сторонней библиотеки или собрать объект с нетривиальной инициализацией.
- Зачем специализации `@Component`? `@Service` и `@Controller` — семантические маркеры (плюс точки для AOP-срезов), а `@Repository` дополнительно включает трансляцию исключений персистентности: `SQLException` и прочие заворачиваются в иерархию `DataAccessException`.
- `@Autowired` vs `@Resource` vs `@Inject`: `@Autowired` ищет по типу, `@Resource` (jakarta) — сначала по имени бина, `@Inject` — стандарт JSR-330, почти эквивалент `@Autowired`. В новых проектах используют `@Autowired` (точнее, просто конструктор).

---

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
        long start = System.currentTimeMillis();
        
        Object proceed = joinPoint.proceed(); // Вызов реального метода
        
        long executionTime = System.currentTimeMillis() - start;
        System.out.println(joinPoint.getSignature() + " выполнился за " + executionTime + " мс");
        return proceed;
    }
}
```

Частый follow-up — чем Spring AOP отличается от AspectJ. Spring AOP работает в рантайме через прокси: перехватываются только вызовы public-методов бинов извне (обращения к полям, конструкторы и self-invocation недоступны). AspectJ — полноценный AOP-фреймворк с weaving'ом на этапе компиляции или загрузки классов (LTW через javaagent): перехватывает любые join points, включая вызовы изнутри самого объекта. Цена — сложность настройки. На практике почти всегда достаточно Spring AOP.

---

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

### 7. Spring Boot: Преимущества, Автоконфигурация и Starters

**Spring Boot** — это надстройка над Spring Framework, предназначенная для упрощения процесса создания и настройки приложений, готовых к внедрению в производство (production-ready).

Ключевые преимущества:
1. **Автоконфигурация (Auto-Configuration)**: Механизм, который автоматически настраивает компоненты Spring на основе присутствующих в classpath библиотек. Реализован с помощью аннотации `@EnableAutoConfiguration`.
2. **Spring Boot Starters**: Набор удобных дескрипторов зависимостей. Например, `spring-boot-starter-web` автоматически добавит Spring MVC, Tomcat, Jackson и валидацию.
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

Как автоконфигурация устроена под капотом — обязательный follow-up. При старте Spring Boot читает файл `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports` из каждого jar в classpath (в Boot 2.x использовался `META-INF/spring.factories`) и регистрирует перечисленные там `@Configuration`-классы. Но срабатывают они не всегда: на каждом висят условия — `@ConditionalOnClass` (класс есть в classpath), `@ConditionalOnMissingBean` (пользователь не определил свой бин), `@ConditionalOnProperty` и другие. Поэтому ваш собственный `@Bean DataSource` отменяет автоконфигурацию DataSource — условие `OnMissingBean` перестаёт выполняться. Отключить конкретную автоконфигурацию можно через параметр `exclude` в `@SpringBootApplication` или свойство `spring.autoconfigure.exclude`.

---

### 8. Управление транзакциями и аннотация @Transactional

В Spring управление транзакциями абстрагировано с помощью интерфейса `PlatformTransactionManager`. Основной способ управления — декларативный подход с использованием аннотации `@Transactional`. Эта аннотация использует AOP для создания прокси, который открывает транзакцию перед выполнением метода, фиксирует ее (commit) при успешном завершении или откатывает (rollback) при выбросе исключения `RuntimeException`.

Важные атрибуты `@Transactional`:
- `propagation`: определяет поведение транзакции (`REQUIRED` — по умолчанию, использовать текущую или создать новую; `REQUIRES_NEW` — всегда новая, текущая приостанавливается; `NESTED` — savepoint внутри текущей; `SUPPORTS`, `NOT_SUPPORTED`, `MANDATORY` — требует существующую, иначе исключение; `NEVER`).
- `isolation`: определяет уровень изоляции базы данных (`READ_COMMITTED`, `REPEATABLE_READ`, и т.д.).
- `rollbackFor`: позволяет указать checked-исключения, при которых также должен происходить откат транзакции (по умолчанию откат только на `RuntimeException` и `Error`).
- `readOnly`: хинт, что транзакция только читает. С Hibernate переводит persistence context в `FlushMode.MANUAL` — сущности не проверяются на dirty checking, чтение реально ускоряется. Гарантии запрета записи на уровне БД это не даёт.
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

- Прокси перехватывает только внешние вызовы. Self-invocation — вызов `@Transactional`-метода из другого метода того же бина — транзакцию не откроет (подробно в вопросе 12).
- Аннотация на `private`-методе молча игнорируется.
- Если поймать исключение внутри `@Transactional`-метода и проглотить его, транзакция успешно закоммитится — откат случается только когда исключение покидает метод. Симметричная проблема: вложенный `@Transactional`-участник пометил общую транзакцию rollback-only, вы поймали его исключение и пошли дальше — на коммите получите `UnexpectedRollbackException`.
- Аннотация на интерфейсе работает только с JDK-прокси; ставьте её на класс реализации — так надёжнее.

---

### 9. Spring Security: Архитектура, Аутентификация и JWT

**Spring Security** — фреймворк для обеспечения аутентификации, авторизации и защиты от атак (CSRF, XSS и др.).
Он базируется на цепочке фильтров сервлетов (`SecurityFilterChain`), которые перехватывают каждый входящий запрос до его попадания в `DispatcherServlet`.

Ключевые компоненты:
- `SecurityContextHolder`: хранит информацию о текущем авторизованном пользователе (объект `Authentication`).
- `AuthenticationManager`: основной интерфейс для проверки аутентификации. Делегирует работу `AuthenticationProvider`.
- `UserDetailsService`: интерфейс для загрузки пользовательских данных (часто реализуется для чтения из БД).

Для работы с **JWT (JSON Web Token)** в современных API создается кастомный фильтр (наследующий `OncePerRequestFilter`), который извлекает токен из заголовка `Authorization: Bearer <token>`, валидирует его и помещает объект аутентификации в `SecurityContextHolder`. Альтернатива самописному фильтру — готовый Resource Server (`spring-boot-starter-oauth2-resource-server`), который сам валидирует JWT по подписи и exp.

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
            .csrf(csrf -> csrf.disable()) // Отключаем CSRF для REST API
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**").permitAll()
                .anyRequest().authenticated()
            )
            .addFilterBefore(jwtAuthFilter, UsernamePasswordAuthenticationFilter.class);
        return http.build();
    }
}
```

Для JWT-API сессию принято делать stateless — `sessionManagement(s -> s.sessionCreationPolicy(SessionCreationPolicy.STATELESS))`, иначе Spring Security создаст `HttpSession` и будет хранить контекст на сервере, что противоречит идее токена. Отключение CSRF оправдано именно потому, что токен передаётся заголовком, а не cookie: CSRF-атака эксплуатирует автоматическую отправку cookie браузером.

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
- Начиная со Spring 6 / Boot 3 есть встроенная поддержка RFC 7807 (Problem Details for HTTP APIs): `ProblemDetail` и интерфейс `ErrorResponse` дают стандартизированный формат ошибок `application/problem+json`, свой `ApiError` городить необязательно.

---

### 11. Циклические зависимости и их разрешение в Spring

Циклическая зависимость — это когда бин A зависит от бина B, а бин B — от бина A. Spring умеет разрешать такой цикл, но только при внедрении через сеттер или поле.

Механика такая. Контейнер хранит синглтоны в трёх кэшах: `singletonObjects` (готовые бины), `earlySingletonObjects` и `singletonFactories`. Когда создаётся бин A, он сначала инстанцируется конструктором, а затем сразу регистрируется в `singletonFactories` — то есть ссылка на «сырой», ещё не заполненный A становится доступна. Когда при заполнении B потребуется A, контейнер возьмёт эту раннюю ссылку, внедрит её в B, доведёт B до готовности и вернётся к заполнению A. Цикл разрешён.

С конструктором этот трюк не работает: чтобы создать A, нужен готовый B, а чтобы создать B — готовый A. Ни один объект не существует даже в «сыром» виде, выставлять раннюю ссылку нечего. Результат — `BeanCurrentlyInCreationException`.

Что изменилось в Spring Boot 2.6+: циклические зависимости запрещены по умолчанию, приложение просто не стартует, даже если цикл раньше разрешался через сеттеры. Вернуть старое поведение можно флагом:

```properties
spring.main.allow-circular-references=true
```

На практике цикл — симптом проблемного дизайна. Варианты лечения: вынести общую логику в третий бин, внедрить одну из зависимостей лениво (`@Lazy` — внедряется прокси, реальный бин создаётся при первом обращении) или оставить сеттер для одной стороны. `@Lazy` — рабочий, но это скорее костыль, чем решение.

---

### 12. Прокси в Spring: JDK Dynamic Proxy и CGLIB, проблема self-invocation

Spring AOP, транзакции, `@Async`, `@Cacheable` — всё это работает через прокси: контейнер возвращает не ваш объект, а обёртку, которая перехватывает вызов метода и добавляет поведение (открыть транзакцию, отправить в другой поток, проверить кэш).

Два механизма проксирования:
- **JDK Dynamic Proxy** — создаёт реализацию интерфейса в рантайме через `java.lang.reflect.Proxy`. Работает только если класс реализует интерфейс.
- **CGLIB** — генерирует подкласс целевого класса в байткоде. Нужен, когда интерфейса нет. В Spring Boot 2.x CGLIB используется по умолчанию всегда (`proxyTargetClass=true`), поэтому наследоваться от класса сервиса можно независимо от интерфейсов.

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

### 13. Асинхронное и плановое выполнение: @Async и @Scheduled

`@Async` выполняет метод в отдельном потоке, `@Scheduled` — по расписанию. Обе включаются аннотациями `@EnableAsync` и `@EnableScheduling` на конфигурационном классе и обе работают через прокси (значит, self-invocation не сработает, метод должен быть public).

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
- В Spring Boot `@Async` по умолчанию использует `ThreadPoolTaskExecutor` (бин `applicationTaskExecutor`): core pool 8 потоков, очередь **неограниченная**. Это значит, что при нагрузке задачи копятся в очереди, а потоки сверх восьми не создаются — можно получить часовую задержку вместо параллелизма.
- В «голом» Spring без Boot дефолт — `SimpleAsyncTaskExecutor`, который вообще создаёт **новый поток на каждый вызов**. На проде это почти гарантированный `OutOfMemoryError` под нагрузкой.
- `@Scheduled` по умолчанию работает на однопоточном планировщике: одна зависшая задача остановит все остальные.

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

Для `@Scheduled` размер пула задаётся через `spring.task.scheduling.pool.size`. Ещё одно ограничение: исключение из `@Async`-метода, возвращающего `void`, просто теряется в логах пула — для осмысленной обработки нужен `AsyncUncaughtExceptionHandler` или возврат `Future`.

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

`Page` дополнительно выполняет count-запрос для общего числа элементов — на больших таблицах он дорогой. Если нужна только кнопка «далее», берите `Slice` — он читает на одну запись больше лимита и не делает count.

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

### 16. Кэширование: @Cacheable, @CacheEvict и @CachePut

Абстракция кэша включается `@EnableCaching` и работает через прокси (да, self-invocation снова не сработает). Три основные аннотации:

- `@Cacheable` — сначала ищет в кэше по ключу; если есть — возвращает из кэша, метод не выполняется; если нет — выполняет и кладёт результат.
- `@CachePut` — метод выполняется всегда, результат обновляет кэш. Нужен для операций записи, чтобы кэш не рассинхронизировался с БД.
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

Ключ строится по параметрам (по умолчанию — все аргументы через `SimpleKey`), в `key` доступен SpEL: `#id`, `#product.id`, `#result` (только в `unless` и `CachePut`). `condition` решает, кэшировать ли вызов вообще; `unless` — фильтрует уже полученный результат (не класть null — типичное применение).

Провайдеры:
- Без дополнительных зависимостей — `ConcurrentHashMap` в памяти, без TTL и eviction-политик. Годится только для тестов.
- **Caffeine** — in-memory кэш с TTL, лимитом размера и статистикой. Стандарт для одноинстансовых приложений.
- **Redis** — распределённый кэш, обязателен при нескольких инстансах. Настраивается через `RedisCacheManager`, TTL — на уровне кэша:

```java
@Bean
public RedisCacheManager cacheManager(RedisConnectionFactory factory) {
    RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
            .entryTtl(Duration.ofMinutes(10));
    return RedisCacheManager.builder(factory).cacheDefaults(config).build();
}
```

Ловушка распределённого кэша: данные в Redis не умирают вместе с приложением, поэтому после рефакторинга класса старые сериализованные записи могут не десериализоваться — версионируйте ключи.

---

### 17. BeanPostProcessor и BeanFactoryPostProcessor

Это две точки расширения контейнера, и путают их постоянно. Разница — в моменте срабатывания и в том, с чем они работают.

**BeanFactoryPostProcessor** срабатывает до создания любого бина, когда контейнер прочитал только метаданные (`BeanDefinition`). Он может менять определения: добавить свойство, заменить класс, поменять scope. Внутри Spring его классические представители:
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
        return bean; // важно вернуть бин (или его замену), иначе контекст сломается
    }
}
```

Практическое следствие: если ваш бин обрабатывается `BeanPostProcessor`'ом, который сам зависит от бинов, часть бинов может остаться без постобработки — контейнер честно напишет об этом в лог ("is not eligible for getting processed by all BeanPostProcessors"). Поэтому BPP обычно делают максимально независимыми.

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

Ключевой момент: ничего не выполняется, пока нет подписки (`subscribe()`). В WebFlux подписку делает сам фреймворк, когда контроллер вернул `Mono`/`Flux`. Вызов `block()` внутри реактивного конвейера убивает весь смысл модели.

**Backpressure** — обратное давление: подписчик сообщает издателю, сколько элементов готов принять (`request(n)`), и издатель не выдаёт больше. Это защищает медленного потребителя от переполнения. Проявляется в операторах `limitRate`, `onBackpressureBuffer/Drop/Latest`.

Когда WebFlux оправдан: высокая конкурентность на I/O-bound задачах (прокси, gateway, агрегаторы внешних API), стриминг (SSE, WebSocket), тысячи одновременных соединений при ограниченной памяти. Когда нет: обычный CRUD поверх JPA/JDBC — драйверы блокирующие, R2DBC имеет менее зрелую экосистему; CPU-bound логика реактивности не ускоряет; команда без опыта получит сложную отладку (стектрейсы реактивных цепочек — боль). Реактивность масштабирует количество одновременных запросов, но не делает один запрос быстрее.

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

В Kubernetes health-группы `liveness` и `readiness` (`/actuator/health/liveness`, `/actuator/health/readiness`) маппятся на пробы напрямую — liveness проверяет, что процесс жив, readiness — что приложение готово принимать трафик (БД поднялась и т.п.).

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
- никогда не включать `management.endpoint.shutdown.enabled=true` на проде.

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

---

[← Вернуться на главную страницу](README.md)
