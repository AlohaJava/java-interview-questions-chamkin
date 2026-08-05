# Java-собеседование: вопросы и ответы для подготовки

![Java](https://img.shields.io/badge/Java-17--21-orange)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-brightgreen)
![Вопросов в базе](https://img.shields.io/badge/%D0%B2%D0%BE%D0%BF%D1%80%D0%BE%D1%81%D0%BE%D0%B2-301-blue)
![Язык](https://img.shields.io/badge/%D1%8F%D0%B7%D1%8B%D0%BA-%D1%80%D1%83%D1%81%D1%81%D0%BA%D0%B8%D0%B9-lightgrey)
![License](https://img.shields.io/badge/license-MIT-green)

**Ключевые слова:** java собеседование вопросы и ответы, java interview questions, подготовка к собеседованию java, вопросы на собеседовании java junior middle senior, hashmap, jvm, многопоточность java, spring boot вопросы, hibernate jpa, sql собеседование, микросервисы kafka, system design на русском, лайвкодинг java, мок собеседование

Привет! Это база вопросов для подготовки к собеседованию на Java-разработчика — от Junior до Senior. Я разобрал записи сотен реальных собеседований (русскоязычных и англоязычных), классические списки вопросов и книги, выкинул дубли и воду и написал на каждый вопрос честный ответ: суть, нюансы, примеры кода и те самые уточняющие вопросы, которыми интервьюеры копают глубже.

Меня зовут **Сергей Чамкин**, я и собрал этот репозиторий. Провожу мок-собеседования по Java: прогоняю как настоящий интервьюер, разбираю ответы и помогаю закрыть пробелы перед реальным собеседованием. Пишите в Telegram — **[@jaba_javi](https://t.me/jaba_javi)**.

Смотрите также смежный репозиторий: [system-design-interview-chamkin](https://github.com/AlohaJava/system-design-interview-chamkin) — как пройти System Design собеседование: шаги, паттерны и разборы задач, собранные из 500+ транскриптов реальных моков и разборов.

## Разделы

| Файл | Тема | Вопросов |
| :--- | :--- | ---: |
| [oop-and-core.md](oop-and-core.md) | ООП и Java Core | 33 |
| [collections.md](collections.md) | Java Collections Framework | 20 |
| [jvm-and-memory.md](jvm-and-memory.md) | JVM, Память и Сборщик Мусора (GC) | 16 |
| [multithreading.md](multithreading.md) | Многопоточность и Concurrency | 31 |
| [spring-framework.md](spring-framework.md) | Spring Framework и Spring Boot | 28 |
| [databases-and-sql.md](databases-and-sql.md) | Базы Данных, SQL и Hibernate / JPA | 38 |
| [kafka.md](kafka.md) | Apache Kafka | 17 |
| [microservices.md](microservices.md) | Микросервисы: архитектура, REST и паттерны | 19 |
| [system-design-and-patterns.md](system-design-and-patterns.md) | System Design, SOLID и Паттерны проектирования | 24 |
| [testing.md](testing.md) | Тестирование и Качество Кода | 18 |
| [livecoding-algorithms.md](livecoding-algorithms.md) | Лайвкодинг и алгоритмы | 23 |
| [code-review.md](code-review.md) | Код-ревью на собеседовании | 13 |
| [behavioral-questions.md](behavioral-questions.md) | Поведенческие вопросы и soft skills | 14 |
| [dev-tools-and-processes.md](dev-tools-and-processes.md) | Инструменты и процессы разработки | 7 |

## Как готовиться

- Junior — начинайте с ООП и Java Core, Коллекций и JVM.
- Middle — добавьте Многопоточность, Spring и Базы данных.
- Senior — Микросервисы, System Design и лайвкодинг.
- Внутри раздела вопросы идут от самых частых к более редким: если времени мало, читайте сверху вниз.
- Не зубрите — проговаривайте ответ вслух. На собеседовании ценят не цитату, а понимание.

---

## Полное оглавление вопросов

### 1. ООП и Java Core (`oop-and-core.md`)

- [1.1. Контракт между методами equals() и hashCode()](oop-and-core.md#1-контракт-между-методами-equals-и-hashcode)
- [1.2. Фундаментальные принципы ООП](oop-and-core.md#2-фундаментальные-принципы-ооп)
- [1.3. Класс String: Пул строк и неизменяемость (Immutability)](oop-and-core.md#3-класс-string-пул-строк-и-неизменяемость-immutability)
- [1.4. Иерархия исключений: Checked и Unchecked](oop-and-core.md#4-иерархия-исключений-checked-и-unchecked)
- [1.5. Механизм передачи параметров: по значению или по ссылке](oop-and-core.md#5-механизм-передачи-параметров-по-значению-или-по-ссылке)
- [1.6. Абстрактные классы и интерфейсы: ключевые отличия](oop-and-core.md#6-абстрактные-классы-и-интерфейсы-ключевые-отличия)
- [1.7. Методы класса Object: toString, clone, finalize, wait и notify](oop-and-core.md#7-методы-класса-object-tostring-clone-finalize-wait-и-notify)
- [1.8. Ключевые слова final, finally и finalize](oop-and-core.md#8-ключевые-слова-final-finally-и-finalize)
- [1.9. Ключевое слово static: поля, методы, блоки и вложенные классы](oop-and-core.md#9-ключевое-слово-static-поля-методы-блоки-и-вложенные-классы)
- [1.10. Как создать неизменяемый (Immutable) класс](oop-and-core.md#10-как-создать-неизменяемый-immutable-класс)
- [1.11. Autoboxing и Unboxing: классы-обертки и их ловушки](oop-and-core.md#11-autoboxing-и-unboxing-классы-обертки-и-их-ловушки)
- [1.12. Generics: стирание типов и Wildcards (PECS)](oop-and-core.md#12-generics-стирание-типов-и-wildcards-pecs)
- [1.13. Функциональные интерфейсы и лямбда-выражения](oop-and-core.md#13-функциональные-интерфейсы-и-лямбда-выражения)
- [1.14. Stream API: операции, ленивость и Collectors](oop-and-core.md#14-stream-api-операции-ленивость-и-collectors)
- [1.15. Класс Optional: назначение и паттерны использования](oop-and-core.md#15-класс-optional-назначение-и-паттерны-использования)
- [1.16. Загрузка классов и иерархия ClassLoader](oop-and-core.md#16-загрузка-классов-и-иерархия-classloader)
- [1.17. Reflection API: назначение, применение и цена](oop-and-core.md#17-reflection-api-назначение-применение-и-цена)
- [1.18. Сериализация: Serializable, transient, serialVersionUID и Externalizable](oop-and-core.md#18-сериализация-serializable-transient-serialversionuid-и-externalizable)
- [1.19. Вложенные классы: static, inner, локальные и анонимные](oop-and-core.md#19-вложенные-классы-static-inner-локальные-и-анонимные)
- [1.20. Records и sealed-классы](oop-and-core.md#20-records-и-sealed-классы)
- [1.21. Comparable и Comparator](oop-and-core.md#21-comparable-и-comparator)
- [1.22. try-with-resources и AutoCloseable](oop-and-core.md#22-try-with-resources-и-autocloseable)
- [1.23. Перегрузка (Overload) и переопределение (Override) методов](oop-and-core.md#23-перегрузка-overload-и-переопределение-override-методов)
- [1.24. Обзор новшеств Java 9–21](oop-and-core.md#24-обзор-новшеств-java-921)
- [1.25. StringBuilder, StringBuffer и конкатенация строк](oop-and-core.md#25-stringbuilder-stringbuffer-и-конкатенация-строк)
- [1.26. Примитивные типы: размеры, widening и narrowing, переполнение и потеря точности](oop-and-core.md#26-примитивные-типы-размеры-widening-и-narrowing-переполнение-и-потеря-точности)
- [1.27. Модификаторы доступа: private, package-private, protected и public](oop-and-core.md#27-модификаторы-доступа-private-package-private-protected-и-public)
- [1.28. Порядок инициализации класса и объекта в иерархии наследования](oop-and-core.md#28-порядок-инициализации-класса-и-объекта-в-иерархии-наследования)
- [1.29. Кастомные исключения: checked и unchecked, когда оправданы](oop-and-core.md#29-кастомные-исключения-checked-и-unchecked-когда-оправданы)
- [1.30. Deep copy и Shallow copy: Cloneable, copy-конструктор, сериализация](oop-and-core.md#30-deep-copy-и-shallow-copy-cloneable-copy-конструктор-сериализация)
- [1.31. Default- и static-методы интерфейсов и diamond problem](oop-and-core.md#31-default--и-static-методы-интерфейсов-и-diamond-problem)
- [1.32. Кастомные аннотации: @Target, @Retention и обработка через Reflection](oop-and-core.md#32-кастомные-аннотации-target-retention-и-обработка-через-reflection)
- [1.33. Date/Time API (java.time): LocalDateTime, Instant, Duration и Period](oop-and-core.md#33-datetime-api-javatime-localdatetime-instant-duration-и-period)
---

### 2. Java Collections Framework (`collections.md`)

- [2.1. Как устроена HashMap: хэширование, бакеты, коллизии, деревья и resize](collections.md#1-как-устроена-hashmap-хэширование-бакеты-коллизии-деревья-и-resize)
- [2.2. В чем разница между ArrayList и LinkedList и какова сложность их основных операций (Big O)?](collections.md#2-в-чем-разница-между-arraylist-и-linkedlist-и-какова-сложность-их-основных-операций-big-o)
- [2.3. Как устроена ConcurrentHashMap и почему она эффективнее Collections.synchronizedMap()?](collections.md#3-как-устроена-concurrenthashmap-и-почему-она-эффективнее-collectionssynchronizedmap)
- [2.4. Какова иерархия интерфейсов в Java Collections Framework?](collections.md#4-какова-иерархия-интерфейсов-в-java-collections-framework)
- [2.5. Чем HashMap отличается от TreeMap и LinkedHashMap?](collections.md#5-чем-hashmap-отличается-от-treemap-и-linkedhashmap)
- [2.6. В чем разница между Fail-Fast и Fail-Safe итераторами?](collections.md#6-в-чем-разница-между-fail-fast-и-fail-safe-итераторами)
- [2.7. Как устроен HashSet и почему внутри него обычная HashMap?](collections.md#7-как-устроен-hashset-и-почему-внутри-него-обычная-hashmap)
- [2.8. PriorityQueue и пары методов Queue/Deque: offer/poll/peek против add/remove/element](collections.md#8-priorityqueue-и-пары-методов-queuedeque-offerpollpeek-против-addremoveelement)
- [2.9. Iterator и Iterable: зачем нужны, как работает remove() итератора и что скрывается под капотом for-each?](collections.md#9-iterator-и-iterable-зачем-нужны-как-работает-remove-итератора-и-что-скрывается-под-капотом-for-each)
- [2.10. Специализированные реализации Map: WeakHashMap, EnumMap и IdentityHashMap](collections.md#10-специализированные-реализации-map-weakhashmap-enummap-и-identityhashmap)
- [2.11. Какие требования предъявляются к объектам в роли ключей HashMap?](collections.md#11-какие-требования-предъявляются-к-объектам-в-роли-ключей-hashmap)
- [2.12. Неизменяемые коллекции: List.of/Set.of/Map.of против Arrays.asList и Collections.unmodifiableList](collections.md#12-неизменяемые-коллекции-listofsetofmapof-против-arraysaslist-и-collectionsunmodifiablelist)
- [2.13. Vector, Hashtable и Stack: почему они считаются legacy и что использовать вместо них?](collections.md#13-vector-hashtable-и-stack-почему-они-считаются-legacy-и-что-использовать-вместо-них)
- [2.14. Как сортируются коллекции: Collections.sort, TimSort, стабильность и сложность сортировки?](collections.md#14-как-сортируются-коллекции-collectionssort-timsort-стабильность-и-сложность-сортировки)
- [2.15. LinkedHashMap: порядок вставки, порядок доступа и LRU-кэш из коробки](collections.md#15-linkedhashmap-порядок-вставки-порядок-доступа-и-lru-кэш-из-коробки)
- [2.16. TreeMap и NavigableMap: красно-черное дерево и методы навигации](collections.md#16-treemap-и-navigablemap-красно-черное-дерево-и-методы-навигации)
- [2.17. CopyOnWriteArrayList: устройство и когда она лучше синхронизированного списка](collections.md#17-copyonwritearraylist-устройство-и-когда-она-лучше-синхронизированного-списка)
- [2.18. ArrayDeque: почему для стека и очереди берут его, а не Stack и LinkedList](collections.md#18-arraydeque-почему-для-стека-и-очереди-берут-его-а-не-stack-и-linkedlist)
- [2.19. Блокирующие очереди: ArrayBlockingQueue, LinkedBlockingQueue, SynchronousQueue](collections.md#19-блокирующие-очереди-arrayblockingqueue-linkedblockingqueue-synchronousqueue)
- [2.20. Как выбрать коллекцию под задачу: практическая шпаргалка](collections.md#20-как-выбрать-коллекцию-под-задачу-практическая-шпаргалка)
---

### 3. JVM, Память и Сборщик Мусора (GC) (`jvm-and-memory.md`)

- [3.1. Как устроена память в JVM: в чем разница между Heap (кучей) и Stack (стеком)?](jvm-and-memory.md#1-как-устроена-память-в-jvm-в-чем-разница-между-heap-кучей-и-stack-стеком)
- [3.2. Какие бывают сборщики мусора в JVM (Serial, Parallel, G1, ZGC, Shenandoah, Epsilon)?](jvm-and-memory.md#2-какие-бывают-сборщики-мусора-в-jvm-serial-parallel-g1-zgc-shenandoah-epsilon)
- [3.3. Чем различаются JDK, JRE и JVM?](jvm-and-memory.md#3-чем-различаются-jdk-jre-и-jvm)
- [3.4. Как устроены поколения в Heap (Young Generation, Old Generation) и процесс сборки мусора?](jvm-and-memory.md#4-как-устроены-поколения-в-heap-young-generation-old-generation-и-процесс-сборки-мусора)
- [3.5. Что такое OutOfMemoryError (OOM) и StackOverflowError и как их отлаживать?](jvm-and-memory.md#5-что-такое-outofmemoryerror-oom-и-stackoverflowerror-и-как-их-отлаживать)
- [3.6. Что делает метод System.gc() и гарантирует ли он немедленную сборку мусора?](jvm-and-memory.md#6-что-делает-метод-systemgc-и-гарантирует-ли-он-немедленную-сборку-мусора)
- [3.7. Как устроена иерархия ClassLoader (Bootstrap, Platform, Application) и что такое делегирование родителю?](jvm-and-memory.md#7-как-устроена-иерархия-classloader-bootstrap-platform-application-и-что-такое-делегирование-родителю)
- [3.8. Как JVM исполняет код: интерпретатор, JIT-компиляторы C1/C2 и tiered compilation?](jvm-and-memory.md#8-как-jvm-исполняет-код-интерпретатор-jit-компиляторы-c1c2-и-tiered-compilation)
- [3.9. Какие бывают типы ссылок в Java (Strong, Soft, Weak, Phantom) и для чего нужна ReferenceQueue?](jvm-and-memory.md#9-какие-бывают-типы-ссылок-в-java-strong-soft-weak-phantom-и-для-чего-нужна-referencequeue)
- [3.10. Какие основные флаги JVM нужно знать (-Xms/-Xmx, выбор GC, логи GC, HeapDumpOnOutOfMemoryError)?](jvm-and-memory.md#10-какие-основные-флаги-jvm-нужно-знать--xms-xmx-выбор-gc-логи-gc-heapdumponoutofmemoryerror)
- [3.11. Что такое Metaspace, чем он заменил PermGen и как регулируется его размер?](jvm-and-memory.md#11-что-такое-metaspace-чем-он-заменил-permgen-и-как-регулируется-его-размер)
- [3.12. Какие оптимизации применяет JVM к коду и данным (Compact Strings, String Deduplication, Escape Analysis)?](jvm-and-memory.md#12-какие-оптимизации-применяет-jvm-к-коду-и-данным-compact-strings-string-deduplication-escape-analysis)
- [3.13. Какие инструменты JDK помогают диагностировать проблемы JVM (jps, jstack, jmap, jstat, jcmd, JFR)?](jvm-and-memory.md#13-какие-инструменты-jdk-помогают-диагностировать-проблемы-jvm-jps-jstack-jmap-jstat-jcmd-jfr)
- [3.14. Что такое off-heap память и DirectByteBuffer и где это используется на практике?](jvm-and-memory.md#14-что-такое-off-heap-память-и-directbytebuffer-и-где-это-используется-на-практике)
- [3.15. Утечки памяти в Java: типичные причины (статические коллекции, ресурсы, слушатели, ThreadLocal, кэши) и как их диагностировать?](jvm-and-memory.md#15-утечки-памяти-в-java-типичные-причины-статические-коллекции-ресурсы-слушатели-threadlocal-кэши-и-как-их-диагностировать)
- [3.16. Практика тюнинга JVM/GC: что отвечать на «приходилось ли тюнить» и какие флаги реально крутят?](jvm-and-memory.md#16-практика-тюнинга-jvmgc-что-отвечать-на-приходилось-ли-тюнить-и-какие-флаги-реально-крутят)
---

### 4. Многопоточность и Concurrency (`multithreading.md`)

- [4.1. Что такое процесс и поток? Чем они отличаются?](multithreading.md#1-что-такое-процесс-и-поток-чем-они-отличаются)
- [4.2. Какие существуют способы создания потока в Java? В чем разница между Runnable, Callable и Thread?](multithreading.md#2-какие-существуют-способы-создания-потока-в-java-в-чем-разница-между-runnable-callable-и-thread)
- [4.3. Расскажите о жизненном цикле потока (состояния потока в Java).](multithreading.md#3-расскажите-о-жизненном-цикле-потока-состояния-потока-в-java)
- [4.4. В чем разница между методами start() и run()?](multithreading.md#4-в-чем-разница-между-методами-start-и-run)
- [4.5. Для чего нужны методы sleep(), yield() и join()?](multithreading.md#5-для-чего-нужны-методы-sleep-yield-и-join)
- [4.6. Как работает механизм прерывания потоков (interrupt, isInterrupted, interrupted)?](multithreading.md#6-как-работает-механизм-прерывания-потоков-interrupt-isinterrupted-interrupted)
- [4.7. Что такое потоки-демоны (Daemon threads)?](multithreading.md#7-что-такое-потоки-демоны-daemon-threads)
- [4.8. Что такое состояние гонки (Race Condition) и критическая секция?](multithreading.md#8-что-такое-состояние-гонки-race-condition-и-критическая-секция)
- [4.9. Что такое Deadlock, Livelock и Starvation? Как избежать Deadlock?](multithreading.md#9-что-такое-deadlock-livelock-и-starvation-как-избежать-deadlock)
- [4.10. Что такое Java Memory Model (JMM)? Концепция Happens-before.](multithreading.md#10-что-такое-java-memory-model-jmm-концепция-happens-before)
- [4.11. Для чего используется ключевое слово volatile? Чем оно отличается от атомарных классов?](multithreading.md#11-для-чего-используется-ключевое-слово-volatile-чем-оно-отличается-от-атомарных-классов)
- [4.12. Что такое монитор и мьютекс? Как работает ключевое слово synchronized?](multithreading.md#12-что-такое-монитор-и-мьютекс-как-работает-ключевое-слово-synchronized)
- [4.13. Как взаимодействуют потоки через wait(), notify() и notifyAll()? Почему они вызываются только в synchronized-блоке?](multithreading.md#13-как-взаимодействуют-потоки-через-wait-notify-и-notifyall-почему-они-вызываются-только-в-synchronized-блоке)
- [4.14. Что такое ThreadLocal и для чего он применяется?](multithreading.md#14-что-такое-threadlocal-и-для-чего-он-применяется)
- [4.15. Что такое ThreadPool? Архитектура Executor, ExecutorService, ThreadPoolExecutor.](multithreading.md#15-что-такое-threadpool-архитектура-executor-executorservice-threadpoolexecutor)
- [4.16. Расскажите про интерфейсы Future и CompletableFuture.](multithreading.md#16-расскажите-про-интерфейсы-future-и-completablefuture)
- [4.17. В чем отличие Lock API (ReentrantLock) от synchronized? Что такое ReadWriteLock?](multithreading.md#17-в-чем-отличие-lock-api-reentrantlock-от-synchronized-что-такое-readwritelock)
- [4.18. Какие классы-синхронизаторы есть в java.util.concurrent (CountDownLatch, CyclicBarrier, Semaphore)?](multithreading.md#18-какие-классы-синхронизаторы-есть-в-javautilconcurrent-countdownlatch-cyclicbarrier-semaphore)
- [4.19. Как работают потокобезопасные коллекции (ConcurrentHashMap, CopyOnWriteArrayList)?](multithreading.md#19-как-работают-потокобезопасные-коллекции-concurrenthashmap-copyonwritearraylist)
- [4.20. Как правильно реализовать потокобезопасный Singleton в Java?](multithreading.md#20-как-правильно-реализовать-потокобезопасный-singleton-в-java)
- [4.21. CAS (Compare-And-Swap) и атомарные классы: AtomicInteger, AtomicReference, LongAdder](multithreading.md#21-cas-compare-and-swap-и-атомарные-классы-atomicinteger-atomicreference-longadder)
- [4.22. AbstractQueuedSynchronizer (AQS): на чем построены ReentrantLock, Semaphore и CountDownLatch](multithreading.md#22-abstractqueuedsynchronizer-aqs-на-чем-построены-reentrantlock-semaphore-и-countdownlatch)
- [4.23. Fork/Join Framework и Work Stealing: RecursiveTask и RecursiveAction](multithreading.md#23-forkjoin-framework-и-work-stealing-recursivetask-и-recursiveaction)
- [4.24. Виртуальные потоки (Project Loom, Java 21): чем отличаются от платформенных и где выигрыш](multithreading.md#24-виртуальные-потоки-project-loom-java-21-чем-отличаются-от-платформенных-и-где-выигрыш)
- [4.25. StampedLock и оптимистичное чтение (Optimistic Read)](multithreading.md#25-stampedlock-и-оптимистичное-чтение-optimistic-read)
- [4.26. ScheduledExecutorService против Timer: почему Timer использовать нельзя](multithreading.md#26-scheduledexecutorservice-против-timer-почему-timer-использовать-нельзя)
- [4.27. ThreadLocal на практике: утечки памяти, remove() и наследование контекста](multithreading.md#27-threadlocal-на-практике-утечки-памяти-remove-и-наследование-контекста)
- [4.28. Почему многопоточность не всегда ускоряет: закон Амдала, false sharing и @Contended](multithreading.md#28-почему-многопоточность-не-всегда-ускоряет-закон-амдала-false-sharing-и-contended)
- [4.29. Структурированная конкурентность: StructuredTaskScope и ScopedValue](multithreading.md#29-структурированная-конкурентность-structuredtaskscope-и-scopedvalue)
- [4.30. Как подобрать размер пула потоков: CPU-bound против IO-bound задач, формула Брайана Гетца](multithreading.md#30-как-подобрать-размер-пула-потоков-cpu-bound-против-io-bound-задач-формула-брайана-гетца)
- [4.31. parallelStream и ForkJoinPool: зачем параллельные стримы и когда они вредят](multithreading.md#31-parallelstream-и-forkjoinpool-зачем-параллельные-стримы-и-когда-они-вредят)
---

### 5. Spring Framework и Spring Boot (`spring-framework.md`)

- [5.1. Инверсия контроля (IoC) и Внедрение зависимостей (DI)](spring-framework.md#1-инверсия-контроля-ioc-и-внедрение-зависимостей-di)
- [5.2. Spring Контейнеры: ApplicationContext и BeanFactory](spring-framework.md#2-spring-контейнеры-applicationcontext-и-beanfactory)
- [5.3. Spring Bean: Понятие, Области видимости (Scopes) и Жизненный цикл](spring-framework.md#3-spring-bean-понятие-области-видимости-scopes-и-жизненный-цикл)
- [5.4. Ключевые аннотации конфигурации: @Bean, @Component, @Autowired, @Primary, @Qualifier](spring-framework.md#4-ключевые-аннотации-конфигурации-bean-component-autowired-primary-qualifier)
- [5.5. Аспектно-Ориентированное Программирование (AOP) в Spring](spring-framework.md#5-аспектно-ориентированное-программирование-aop-в-spring)
- [5.6. Spring MVC и паттерн Front Controller (DispatcherServlet)](spring-framework.md#6-spring-mvc-и-паттерн-front-controller-dispatcherservlet)
- [5.7. Spring Boot: Преимущества, Автоконфигурация и Starters](spring-framework.md#7-spring-boot-преимущества-автоконфигурация-и-starters)
- [5.8. Управление транзакциями и аннотация @Transactional](spring-framework.md#8-управление-транзакциями-и-аннотация-transactional)
- [5.9. Spring Security: Архитектура, Аутентификация и JWT](spring-framework.md#9-spring-security-архитектура-аутентификация-и-jwt)
- [5.10. Создание RESTful API: @RestController, обработка исключений и валидация](spring-framework.md#10-создание-restful-api-restcontroller-обработка-исключений-и-валидация)
- [5.11. Циклические зависимости и их разрешение в Spring](spring-framework.md#11-циклические-зависимости-и-их-разрешение-в-spring)
- [5.12. Прокси в Spring: JDK Dynamic Proxy и CGLIB, проблема self-invocation](spring-framework.md#12-прокси-в-spring-jdk-dynamic-proxy-и-cglib-проблема-self-invocation)
- [5.13. Асинхронное и плановое выполнение: @Async и @Scheduled](spring-framework.md#13-асинхронное-и-плановое-выполнение-async-и-scheduled)
- [5.14. Spring Data JPA: derived queries, @Query, проекции и пагинация](spring-framework.md#14-spring-data-jpa-derived-queries-query-проекции-и-пагинация)
- [5.15. Профили и внешняя конфигурация: @Profile и application.yml](spring-framework.md#15-профили-и-внешняя-конфигурация-profile-и-applicationyml)
- [5.16. Кэширование: @Cacheable, @CacheEvict и @CachePut](spring-framework.md#16-кэширование-cacheable-cacheevict-и-cacheput)
- [5.17. BeanPostProcessor и BeanFactoryPostProcessor](spring-framework.md#17-beanpostprocessor-и-beanfactorypostprocessor)
- [5.18. События в Spring: ApplicationEvent и @EventListener](spring-framework.md#18-события-в-spring-applicationevent-и-eventlistener)
- [5.19. Spring WebFlux: реактивная модель, Mono и Flux](spring-framework.md#19-spring-webflux-реактивная-модель-mono-и-flux)
- [5.20. Валидация: Bean Validation, @Valid и @Validated](spring-framework.md#20-валидация-bean-validation-valid-и-validated)
- [5.21. Spring Boot Actuator: health, info, metrics и интеграция с Prometheus](spring-framework.md#21-spring-boot-actuator-health-info-metrics-и-интеграция-с-prometheus)
- [5.22. Логирование: SLF4J, Logback и MDC](spring-framework.md#22-логирование-slf4j-logback-и-mdc)
- [5.23. HTTP-клиенты между сервисами: RestTemplate, WebClient и OpenFeign](spring-framework.md#23-http-клиенты-между-сервисами-resttemplate-webclient-и-openfeign)
- [5.24. Filter, HandlerInterceptor и AOP: что где перехватывает](spring-framework.md#24-filter-handlerinterceptor-и-aop-что-где-перехватывает)
- [5.25. Jackson в Spring: настройка ObjectMapper, аннотации и циклические ссылки](spring-framework.md#25-jackson-в-spring-настройка-objectmapper-аннотации-и-циклические-ссылки)
- [5.26. Lombok: что генерирует и ловушки с JPA-сущностями](spring-framework.md#26-lombok-что-генерирует-и-ловушки-с-jpa-сущностями)
- [5.27. Session-based и token-based (JWT) аутентификация, OAuth2 и OIDC](spring-framework.md#27-session-based-и-token-based-jwt-аутентификация-oauth2-и-oidc)
- [5.28. Хранение паролей и основы безопасности: BCrypt и OWASP Top-10](spring-framework.md#28-хранение-паролей-и-основы-безопасности-bcrypt-и-owasp-top-10)
---

### 6. Базы Данных, SQL и Hibernate / JPA (`databases-and-sql.md`)

- [6.1. Что такое SQL? Группы операторов: DDL, DML, DCL, TCL](databases-and-sql.md#1-что-такое-sql-группы-операторов-ddl-dml-dcl-tcl)
- [6.2. Виды JOIN: INNER, LEFT, RIGHT, FULL OUTER, CROSS](databases-and-sql.md#2-виды-join-inner-left-right-full-outer-cross)
- [6.3. GROUP BY и HAVING: агрегация и фильтрация групп](databases-and-sql.md#3-group-by-и-having-агрегация-и-фильтрация-групп)
- [6.4. UNION, UNION ALL, INTERSECT, EXCEPT: различия](databases-and-sql.md#4-union-union-all-intersect-except-различия)
- [6.5. Подзапросы и CTE (WITH)](databases-and-sql.md#5-подзапросы-и-cte-with)
- [6.6. Оконные функции: ROW_NUMBER, RANK, DENSE_RANK, LAG/LEAD](databases-and-sql.md#6-оконные-функции-row_number-rank-dense_rank-laglead)
- [6.7. Нюансы работы с NULL в SQL](databases-and-sql.md#7-нюансы-работы-с-null-в-sql)
- [6.8. Ограничения целостности: PRIMARY KEY vs UNIQUE vs FOREIGN KEY](databases-and-sql.md#8-ограничения-целостности-primary-key-vs-unique-vs-foreign-key)
- [6.9. Индексы: B-Tree, Hash, составные и покрывающие индексы](databases-and-sql.md#9-индексы-b-tree-hash-составные-и-покрывающие-индексы)
- [6.10. SELECT * против явного списка колонок; count(*) против count(column)](databases-and-sql.md#10-select--против-явного-списка-колонок-count-против-countcolumn)
- [6.11. Нормализация (1NF, 2NF, 3NF) и денормализация](databases-and-sql.md#11-нормализация-1nf-2nf-3nf-и-денормализация)
- [6.12. Хранимые процедуры и триггеры](databases-and-sql.md#12-хранимые-процедуры-и-триггеры)
- [6.13. EXPLAIN и план выполнения запроса](databases-and-sql.md#13-explain-и-план-выполнения-запроса)
- [6.14. Принципы ACID](databases-and-sql.md#14-принципы-acid)
- [6.15. Уровни изоляции транзакций и аномалии чтения](databases-and-sql.md#15-уровни-изоляции-транзакций-и-аномалии-чтения)
- [6.16. Блокировки в БД: оптимистичная и пессимистичная модели, deadlock](databases-and-sql.md#16-блокировки-в-бд-оптимистичная-и-пессимистичная-модели-deadlock)
- [6.17. MVCC: многоверсионный контроль конкурентного доступа](databases-and-sql.md#17-mvcc-многоверсионный-контроль-конкурентного-доступа)
- [6.18. ORM и JPA: Entity, EntityManager, Persistence Context](databases-and-sql.md#18-orm-и-jpa-entity-entitymanager-persistence-context)
- [6.19. Проблема N+1 и способы её решения](databases-and-sql.md#19-проблема-n1-и-способы-её-решения)
- [6.20. Стратегии загрузки LAZY vs EAGER и LazyInitializationException](databases-and-sql.md#20-стратегии-загрузки-lazy-vs-eager-и-lazyinitializationexception)
- [6.21. Кэши Hibernate: первый уровень (L1), второй уровень (L2), кэш запросов](databases-and-sql.md#21-кэши-hibernate-первый-уровень-l1-второй-уровень-l2-кэш-запросов)
- [6.22. Связи сущностей: @OneToMany, @ManyToOne, @ManyToMany, mappedBy, CascadeType](databases-and-sql.md#22-связи-сущностей-onetomany-manytoone-manytomany-mappedby-cascadetype)
- [6.23. Оптимистичная блокировка через @Version в JPA](databases-and-sql.md#23-оптимистичная-блокировка-через-version-в-jpa)
- [6.24. @Transactional в Spring: propagation и типичные ловушки](databases-and-sql.md#24-transactional-в-spring-propagation-и-типичные-ловушки)
- [6.25. Миграции БД: Flyway и Liquibase](databases-and-sql.md#25-миграции-бд-flyway-и-liquibase)
- [6.26. Пагинация: LIMIT/OFFSET против keyset (cursor) pagination](databases-and-sql.md#26-пагинация-limitoffset-против-keyset-cursor-pagination)
- [6.27. Партиционирование таблиц: range, list и hash в PostgreSQL](databases-and-sql.md#27-партиционирование-таблиц-range-list-и-hash-в-postgresql)
- [6.28. Репликация: синхронная и асинхронная, read replicas и replication lag](databases-and-sql.md#28-репликация-синхронная-и-асинхронная-read-replicas-и-replication-lag)
- [6.29. Шардинг: отличие от репликации и выбор ключа шардирования](databases-and-sql.md#29-шардинг-отличие-от-репликации-и-выбор-ключа-шардирования)
- [6.30. Пул соединений: HikariCP и выбор размера пула](databases-and-sql.md#30-пул-соединений-hikaricp-и-выбор-размера-пула)
- [6.31. equals и hashCode в JPA-сущностях](databases-and-sql.md#31-equals-и-hashcode-в-jpa-сущностях)
- [6.32. Пакетная вставка и обновление в Hibernate (batch)](databases-and-sql.md#32-пакетная-вставка-и-обновление-в-hibernate-batch)
- [6.33. Частые антипаттерны в работе с БД](databases-and-sql.md#33-частые-антипаттерны-в-работе-с-бд)
- [6.34. JDBC под капотом ORM: DriverManager/DataSource, Statement vs PreparedStatement, CallableStatement, ResultSet](databases-and-sql.md#34-jdbc-под-капотом-orm-drivermanagerdatasource-statement-vs-preparedstatement-callablestatement-resultset)
- [6.35. DELETE vs TRUNCATE vs DROP: механика, откат, identity-счётчики](databases-and-sql.md#35-delete-vs-truncate-vs-drop-механика-откат-identity-счётчики)
- [6.36. Стратегии генерации первичных ключей в JPA: IDENTITY / SEQUENCE / TABLE / UUID](databases-and-sql.md#36-стратегии-генерации-первичных-ключей-в-jpa-identity--sequence--table--uuid)
- [6.37. persist vs merge vs saveOrUpdate (и getById vs getReference)](databases-and-sql.md#37-persist-vs-merge-vs-saveorupdate-и-getbyid-vs-getreference)
- [6.38. HikariCP на собеседовании: глубокие вопросы по конфигурации и эксплуатации](databases-and-sql.md#38-hikaricp-на-собеседовании-глубокие-вопросы-по-конфигурации-и-эксплуатации)
---

### 7. Apache Kafka (`kafka.md`)

- [7.1. Архитектура и основные компоненты Apache Kafka](kafka.md#1-архитектура-и-основные-компоненты-apache-kafka)
- [7.2. Принципы работы Partition и Consumer Group в Kafka](kafka.md#2-принципы-работы-partition-и-consumer-group-в-kafka)
- [7.3. Гарантии доставки сообщений (Acks, Exactly-once) и управление Offset](kafka.md#3-гарантии-доставки-сообщений-acks-exactly-once-и-управление-offset)
- [7.4. Хранение данных в Kafka: Retention Policy и Log Compaction](kafka.md#4-хранение-данных-в-kafka-retention-policy-и-log-compaction)
- [7.5. Сравнение Apache Kafka и RabbitMQ](kafka.md#5-сравнение-apache-kafka-и-rabbitmq)
- [7.6. Идемпотентность консьюмера Kafka: дедупликация и idempotent producer](kafka.md#6-идемпотентность-консьюмера-kafka-дедупликация-и-idempotent-producer)
- [7.7. Rebalance консьюмер-групп в Kafka: причины, последствия, static membership](kafka.md#7-rebalance-консьюмер-групп-в-kafka-причины-последствия-static-membership)
- [7.8. Ключ партиционирования и гарантии порядка в Kafka](kafka.md#8-ключ-партиционирования-и-гарантии-порядка-в-kafka)
- [7.9. Сериализаторы и десериализаторы: встроенные, JSON, кастомные](kafka.md#9-сериализаторы-и-десериализаторы-встроенные-json-кастомные)
- [7.10. Schema Registry и Avro: эволюция схем и совместимость](kafka.md#10-schema-registry-и-avro-эволюция-схем-и-совместимость)
- [7.11. Продюсер под капотом: батчи, сжатие и порядок отправки](kafka.md#11-продюсер-под-капотом-батчи-сжатие-и-порядок-отправки)
- [7.12. Стратегии назначения партиций: range, round-robin, sticky и cooperative-sticky](kafka.md#12-стратегии-назначения-партиций-range-round-robin-sticky-и-cooperative-sticky)
- [7.13. ISR и min.insync.replicas: репликация без иллюзий](kafka.md#13-isr-и-mininsyncreplicas-репликация-без-иллюзий)
- [7.14. Consumer Lag: мониторинг и способы борьбы](kafka.md#14-consumer-lag-мониторинг-и-способы-борьбы)
- [7.15. Kafka Streams и ksqlDB: потоковая обработка внутри Kafka](kafka.md#15-kafka-streams-и-ksqldb-потоковая-обработка-внутри-kafka)
- [7.16. Dead Letter Queue в Kafka: retry-топики и разбор ошибок](kafka.md#16-dead-letter-queue-в-kafka-retry-топики-и-разбор-ошибок)
- [7.17. KRaft вместо Zookeeper: метаданные без внешней зависимости](kafka.md#17-kraft-вместо-zookeeper-метаданные-без-внешней-зависимости)
---

### 8. Микросервисы: архитектура, REST и паттерны (`microservices.md`)

- [8.1. Архитектурный стиль REST и принципы RESTful API](microservices.md#1-архитектурный-стиль-rest-и-принципы-restful-api)
- [8.2. HTTP-методы, идемпотентность и безопасность](microservices.md#2-http-методы-идемпотентность-и-безопасность)
- [8.3. Коды ответов HTTP в REST API](microservices.md#3-коды-ответов-http-в-rest-api)
- [8.4. Сравнение REST и SOAP](microservices.md#4-сравнение-rest-и-soap)
- [8.5. Сравнение REST и gRPC](microservices.md#5-сравнение-rest-и-grpc)
- [8.6. Микросервисная архитектура: преимущества, недостатки и отличие от монолита](microservices.md#6-микросервисная-архитектура-преимущества-недостатки-и-отличие-от-монолита)
- [8.7. Паттерны взаимодействия микросервисов (API Gateway, Saga, Circuit Breaker)](microservices.md#7-паттерны-взаимодействия-микросервисов-api-gateway-saga-circuit-breaker)
- [8.8. Резилентность микросервисов: Retry, Timeout, Rate Limiter и Bulkhead (Resilience4j)](microservices.md#8-резилентность-микросервисов-retry-timeout-rate-limiter-и-bulkhead-resilience4j)
- [8.9. Service Discovery: Eureka, Consul, client-side и server-side discovery](microservices.md#9-service-discovery-eureka-consul-client-side-и-server-side-discovery)
- [8.10. Централизованная конфигурация: Spring Cloud Config, Consul и Vault](microservices.md#10-централизованная-конфигурация-spring-cloud-config-consul-и-vault)
- [8.11. Паттерн Transactional Outbox и Inbox](microservices.md#11-паттерн-transactional-outbox-и-inbox)
- [8.12. Распределенная трассировка: traceId, spanId, Micrometer Tracing, Jaeger и Zipkin](microservices.md#12-распределенная-трассировка-traceid-spanid-micrometer-tracing-jaeger-и-zipkin)
- [8.13. Версионирование API и Consumer-Driven Contracts](microservices.md#13-версионирование-api-и-consumer-driven-contracts)
- [8.14. Event-Driven против Request-Response: когда события, когда синхронный вызов](microservices.md#14-event-driven-против-request-response-когда-события-когда-синхронный-вызов)
- [8.15. CQRS и Event Sourcing: разделение команд и запросов](microservices.md#15-cqrs-и-event-sourcing-разделение-команд-и-запросов)
- [8.16. Graceful Degradation и fallback-стратегии](microservices.md#16-graceful-degradation-и-fallback-стратегии)
- [8.17. Распил монолита: Strangler Fig, database-per-service и Anti-Corruption Layer](microservices.md#17-распил-монолита-strangler-fig-database-per-service-и-anti-corruption-layer)
- [8.18. Распределенные транзакции: 2PC против Saga](microservices.md#18-распределенные-транзакции-2pc-против-saga)
- [8.19. Централизованное логирование: ELK, correlation id и структурированные логи](microservices.md#19-централизованное-логирование-elk-correlation-id-и-структурированные-логи)
---

### 9. System Design, SOLID и Паттерны проектирования (`system-design-and-patterns.md`)

- [9.1. Что такое принципы SOLID? Подробный разбор каждого принципа.](system-design-and-patterns.md#1-что-такое-принципы-solid-подробный-разбор-каждого-принципа)
- [9.2. К чему приводит нарушение принципов SOLID?](system-design-and-patterns.md#2-к-чему-приводит-нарушение-принципов-solid)
- [9.3. Что такое паттерны проектирования и антипаттерны? Классификация паттернов.](system-design-and-patterns.md#3-что-такое-паттерны-проектирования-и-антипаттерны-классификация-паттернов)
- [9.4. Паттерн Singleton (Одиночка) и его правильная реализация в Java.](system-design-and-patterns.md#4-паттерн-singleton-одиночка-и-его-правильная-реализация-в-java)
- [9.5. Паттерн Builder (Строитель).](system-design-and-patterns.md#5-паттерн-builder-строитель)
- [9.6. Паттерны Factory Method (Фабричный метод) и Abstract Factory (Абстрактная фабрика).](system-design-and-patterns.md#6-паттерны-factory-method-фабричный-метод-и-abstract-factory-абстрактная-фабрика)
- [9.7. Паттерны Adapter (Адаптер) и Decorator (Декоратор).](system-design-and-patterns.md#7-паттерны-adapter-адаптер-и-decorator-декоратор)
- [9.8. Паттерны Proxy (Заместитель) и Facade (Фасад).](system-design-and-patterns.md#8-паттерны-proxy-заместитель-и-facade-фасад)
- [9.9. Паттерн Observer (Наблюдатель).](system-design-and-patterns.md#9-паттерн-observer-наблюдатель)
- [9.10. Паттерн Strategy (Стратегия).](system-design-and-patterns.md#10-паттерн-strategy-стратегия)
- [9.11. Паттерны Template Method (Шаблонный метод) и Chain of Responsibility (Цепочка обязанностей).](system-design-and-patterns.md#11-паттерны-template-method-шаблонный-метод-и-chain-of-responsibility-цепочка-обязанностей)
- [9.12. Основные концепции System Design (Масштабирование, Балансировка нагрузки, Кэширование).](system-design-and-patterns.md#12-основные-концепции-system-design-масштабирование-балансировка-нагрузки-кэширование)
- [9.13. CAP-теорема и выбор базы данных (SQL vs NoSQL).](system-design-and-patterns.md#13-cap-теорема-и-выбор-базы-данных-sql-vs-nosql)
- [9.14. Монолитная и Микросервисная архитектуры.](system-design-and-patterns.md#14-монолитная-и-микросервисная-архитектуры)
- [9.15. Redis: основные структуры данных и типовые сценарии (кэш, сессии, rate limiter, pub/sub).](system-design-and-patterns.md#15-redis-основные-структуры-данных-и-типовые-сценарии-кэш-сессии-rate-limiter-pubsub)
- [9.16. Паттерны кэширования: Cache-Aside, Read-Through, Write-Through и Write-Behind.](system-design-and-patterns.md#16-паттерны-кэширования-cache-aside-read-through-write-through-и-write-behind)
- [9.17. Docker: образ, контейнер, слои и кэш Dockerfile. Best practices.](system-design-and-patterns.md#17-docker-образ-контейнер-слои-и-кэш-dockerfile-best-practices)
- [9.18. Kubernetes для собеседования: Pod, Deployment, Service, Ingress, ConfigMap и Secret.](system-design-and-patterns.md#18-kubernetes-для-собеседования-pod-deployment-service-ingress-configmap-и-secret)
- [9.19. CI/CD и стратегии деплоя: rolling, blue-green и canary.](system-design-and-patterns.md#19-cicd-и-стратегии-деплоя-rolling-blue-green-и-canary)
- [9.20. Rate limiting: алгоритмы token bucket, leaky bucket, fixed window и sliding window.](system-design-and-patterns.md#20-rate-limiting-алгоритмы-token-bucket-leaky-bucket-fixed-window-и-sliding-window)
- [9.21. Задача System Design: спроектируй URL shortener.](system-design-and-patterns.md#21-задача-system-design-спроектируй-url-shortener)
- [9.22. Как подходить к задаче System Design на собеседовании.](system-design-and-patterns.md#22-как-подходить-к-задаче-system-design-на-собеседовании)
- [9.23. Распределённые механики на пальцах: Consistent Hashing, Leader Election, генерация уникальных ID и фильтр Блума.](system-design-and-patterns.md#23-распределённые-механики-на-пальцах-consistent-hashing-leader-election-генерация-уникальных-id-и-фильтр-блума)
- [9.24. Сети для бэкенд-разработчика: TCP vs UDP, TLS handshake, HTTP/1.1 vs HTTP/2 vs HTTP/3, keep-alive и DNS.](system-design-and-patterns.md#24-сети-для-бэкенд-разработчика-tcp-vs-udp-tls-handshake-http11-vs-http2-vs-http3-keep-alive-и-dns)
---

### 10. Тестирование и Качество Кода (`testing.md`)

- [10.1. Что такое модульное (юнит) тестирование и чем оно отличается от интеграционного и компонентного?](testing.md#1-что-такое-модульное-юнит-тестирование-и-чем-оно-отличается-от-интеграционного-и-компонентного)
- [10.2. Пирамида тестирования и основные виды тестов (включая микросервисы)](testing.md#2-пирамида-тестирования-и-основные-виды-тестов-включая-микросервисы)
- [10.3. Виды тестовых объектов (Test Doubles): Dummy, Fake, Stub, Spy, Mock](testing.md#3-виды-тестовых-объектов-test-doubles-dummy-fake-stub-spy-mock)
- [10.4. Фреймворк JUnit 5: жизненный цикл тестов, фикстуры и Assertions](testing.md#4-фреймворк-junit-5-жизненный-цикл-тестов-фикстуры-и-assertions)
- [10.5. Использование Mockito: создание моков, заглушек (stubs) и верификация поведения](testing.md#5-использование-mockito-создание-моков-заглушек-stubs-и-верификация-поведения)
- [10.6. Тестирование Spring-приложений: контекст, WebTestClient и Testcontainers](testing.md#6-тестирование-spring-приложений-контекст-webtestclient-и-testcontainers)
- [10.7. Параметризованные тесты в JUnit 5: @ParameterizedTest, @ValueSource, @CsvSource и @MethodSource](testing.md#7-параметризованные-тесты-в-junit-5-parameterizedtest-valuesource-csvsource-и-methodsource)
- [10.8. AssertJ и Hamcrest: fluent-assertions против стандартных assertEquals](testing.md#8-assertj-и-hamcrest-fluent-assertions-против-стандартных-assertequals)
- [10.9. WireMock: тестирование HTTP-клиентов без реальных сетевых вызовов](testing.md#9-wiremock-тестирование-http-клиентов-без-реальных-сетевых-вызовов)
- [10.10. Что можно и нельзя мокать в Mockito: final-классы и static-методы (mockito-inline и MockMaker)](testing.md#10-что-можно-и-нельзя-мокать-в-mockito-final-классы-и-static-методы-mockito-inline-и-mockmaker)
- [10.11. Покрытие кода: JaCoCo, виды coverage (line и branch) и mutation testing с PIT](testing.md#11-покрытие-кода-jacoco-виды-coverage-line-и-branch-и-mutation-testing-с-pit)
- [10.12. @SpringBootTest против слайс-тестов: @WebMvcTest, @DataJpaTest, @JsonTest](testing.md#12-springboottest-против-слайс-тестов-webmvctest-datajpatest-jsontest)
- [10.13. Принципы хороших тестов: F.I.R.S.T, независимость и одна проверка на тест](testing.md#13-принципы-хороших-тестов-first-независимость-и-одна-проверка-на-тест)
- [10.14. TDD и BDD: red-green-refactor и Given-When-Then](testing.md#14-tdd-и-bdd-red-green-refactor-и-given-when-then)
- [10.15. Тестирование асинхронного кода: Awaitility](testing.md#15-тестирование-асинхронного-кода-awaitility)
- [10.16. @Transactional, @Sql и @DirtiesContext в Spring-тестах](testing.md#16-transactional-sql-и-dirtiescontext-в-spring-тестах)
- [10.17. Тестирование Kafka: Embedded Kafka, Testcontainers и моки консьюмеров](testing.md#17-тестирование-kafka-embedded-kafka-testcontainers-и-моки-консьюмеров)
- [10.18. Нагрузочное тестирование: метрики (RPS, latency, перцентили, error rate), инструменты и виды тестов](testing.md#18-нагрузочное-тестирование-метрики-rps-latency-перцентили-error-rate-инструменты-и-виды-тестов)
---

### 11. Лайвкодинг и алгоритмы (`livecoding-algorithms.md`)

- [11.1. Big O: как оценивать сложность алгоритма](livecoding-algorithms.md#1-big-o-как-оценивать-сложность-алгоритма)
- [11.2. Развернуть строку и проверить палиндром](livecoding-algorithms.md#2-развернуть-строку-и-проверить-палиндром)
- [11.3. Two Sum: найти пару чисел с заданной суммой](livecoding-algorithms.md#3-two-sum-найти-пару-чисел-с-заданной-суммой)
- [11.4. Проверить сбалансированность скобок в строке](livecoding-algorithms.md#4-проверить-сбалансированность-скобок-в-строке)
- [11.5. Найти дубликаты в массиве](livecoding-algorithms.md#5-найти-дубликаты-в-массиве)
- [11.6. Бинарный поиск: итеративно и рекурсивно](livecoding-algorithms.md#6-бинарный-поиск-итеративно-и-рекурсивно)
- [11.7. Развернуть односвязный список](livecoding-algorithms.md#7-развернуть-односвязный-список)
- [11.8. Найти N-й элемент с конца односвязного списка](livecoding-algorithms.md#8-найти-n-й-элемент-с-конца-односвязного-списка)
- [11.9. Частотный анализ: посчитать слова и вывести топ-N](livecoding-algorithms.md#9-частотный-анализ-посчитать-слова-и-вывести-топ-n)
- [11.10. Типовые задачи на Stream API](livecoding-algorithms.md#10-типовые-задачи-на-stream-api)
- [11.11. Числа Фибоначчи: рекурсия, мемоизация, итерация](livecoding-algorithms.md#11-числа-фибоначчи-рекурсия-мемоизация-итерация)
- [11.12. FizzBuzz и что он на самом деле проверяет](livecoding-algorithms.md#12-fizzbuzz-и-что-он-на-самом-деле-проверяет)
- [11.13. Потокобезопасный счётчик и producer-consumer](livecoding-algorithms.md#13-потокобезопасный-счётчик-и-producer-consumer)
- [11.14. LRU Cache на LinkedHashMap](livecoding-algorithms.md#14-lru-cache-на-linkedhashmap)
- [11.15. Как вести себя на лайвкодинге](livecoding-algorithms.md#15-как-вести-себя-на-лайвкодинге)
- [11.16. Проверить цикл в односвязном списке: алгоритм Флойда](livecoding-algorithms.md#16-проверить-цикл-в-односвязном-списке-алгоритм-флойда)
- [11.17. Бинарное дерево: обходы и проверка на BST](livecoding-algorithms.md#17-бинарное-дерево-обходы-и-проверка-на-bst)
- [11.18. LCA: наименьший общий предок и сумма пути](livecoding-algorithms.md#18-lca-наименьший-общий-предок-и-сумма-пути)
- [11.19. SQL-задачи: вторая зарплата, top-N в группе, кумулятивная сумма](livecoding-algorithms.md#19-sql-задачи-вторая-зарплата-top-n-в-группе-кумулятивная-сумма)
- [11.20. Анаграммы и пропавшее число в массиве](livecoding-algorithms.md#20-анаграммы-и-пропавшее-число-в-массиве)
- [11.21. SQL: работа с дублями и самосоединения](livecoding-algorithms.md#21-sql-работа-с-дублями-и-самосоединения)
- [11.22. SQL: gaps and islands и последние записи](livecoding-algorithms.md#22-sql-gaps-and-islands-и-последние-записи)
- [11.23. SQL: условная агрегация и трансформация вывода](livecoding-algorithms.md#23-sql-условная-агрегация-и-трансформация-вывода)
---

### 12. Код-ревью на собеседовании (`code-review.md`)

- [12.1. Как отвечать на «поревьюйте этот код»](code-review.md#1-как-отвечать-на-поревьюйте-этот-код)
- [12.2. Сниппет: многопоточный синглтон и счётчик](code-review.md#2-сниппет-многопоточный-синглтон-и-счётчик)
- [12.3. Сниппет: ресурсы и исключения](code-review.md#3-сниппет-ресурсы-и-исключения)
- [12.4. Сниппет: equals, hashCode и коллекции](code-review.md#4-сниппет-equals-hashcode-и-коллекции)
- [12.5. Сниппет: JPA и Spring](code-review.md#5-сниппет-jpa-и-spring)
- [12.6. Сниппет: REST API контроллер](code-review.md#6-сниппет-rest-api-контроллер)
- [12.7. Сниппет: BigDecimal и деньги](code-review.md#7-сниппет-bigdecimal-и-деньги)
- [12.8. Сниппет: строки и даты](code-review.md#8-сниппет-строки-и-даты)
- [12.9. Сниппет: Optional и Stream API](code-review.md#9-сниппет-optional-и-stream-api)
- [12.10. Как проводить код-ревью в команде](code-review.md#10-как-проводить-код-ревью-в-команде)
- [12.11. Сниппет: внешние вызовы внутри @Transactional](code-review.md#11-сниппет-внешние-вызовы-внутри-transactional)
- [12.12. Задача: перевод денег между счетами (дедлок)](code-review.md#12-задача-перевод-денег-между-счетами-дедлок)
- [12.13. Задача: банкомат (ATM)](code-review.md#13-задача-банкомат-atm)
---

### 13. Поведенческие вопросы и soft skills (`behavioral-questions.md`)

- [13.1. Расскажите о себе](behavioral-questions.md#1-расскажите-о-себе)
- [13.2. Почему вы уходите с текущего места работы?](behavioral-questions.md#2-почему-вы-уходите-с-текущего-места-работы)
- [13.3. Расскажите о самом сложном проекте или задаче](behavioral-questions.md#3-расскажите-о-самом-сложном-проекте-или-задаче)
- [13.4. Расскажите о конфликте в команде и как вы его решили](behavioral-questions.md#4-расскажите-о-конфликте-в-команде-и-как-вы-его-решили)
- [13.5. Расскажите о вашей ошибке или провале](behavioral-questions.md#5-расскажите-о-вашей-ошибке-или-провале)
- [13.6. Приходилось ли работать с легаси и чужим кодом?](behavioral-questions.md#6-приходилось-ли-работать-с-легаси-и-чужим-кодом)
- [13.7. Как вы оцениваете задачи и что делаете, если не укладываетесь в срок?](behavioral-questions.md#7-как-вы-оцениваете-задачи-и-что-делаете-если-не-укладываетесь-в-срок)
- [13.8. Как вы относитесь к код-ревью?](behavioral-questions.md#8-как-вы-относитесь-к-код-ревью)
- [13.9. Куда вы хотите развиваться в ближайшие 2-3 года?](behavioral-questions.md#9-куда-вы-хотите-развиваться-в-ближайшие-2-3-года)
- [13.10. Ваши сильные и слабые стороны](behavioral-questions.md#10-ваши-сильные-и-слабые-стороны)
- [13.11. Ожидания по зарплате: как обсуждать деньги](behavioral-questions.md#11-ожидания-по-зарплате-как-обсуждать-деньги)
- [13.12. Какие вопросы задать работодателю](behavioral-questions.md#12-какие-вопросы-задать-работодателю)
- [13.13. Почему именно наша компания? Что вы о нас знаете?](behavioral-questions.md#13-почему-именно-наша-компания-что-вы-о-нас-знаете)
- [13.14. Инцидент в проде: что будете делать?](behavioral-questions.md#14-инцидент-в-проде-что-будете-делать)
---

### 14. Инструменты и процессы разработки (`dev-tools-and-processes.md`)

- [14.1. Git: merge против rebase, cherry-pick, revert и reset, стратегии ветвления](dev-tools-and-processes.md#1-git-merge-против-rebase-cherry-pick-revert-и-reset-стратегии-ветвления)
- [14.2. Maven: жизненный цикл, scopes зависимостей, dependencyManagement и конфликты версий](dev-tools-and-processes.md#2-maven-жизненный-цикл-scopes-зависимостей-dependencymanagement-и-конфликты-версий)
- [14.3. Linux для бэкенд-разработчика: диагностика на сервере](dev-tools-and-processes.md#3-linux-для-бэкенд-разработчика-диагностика-на-сервере)
- [14.4. Scrum и Kanban: роли, церемонии, оценка задач](dev-tools-and-processes.md#4-scrum-и-kanban-роли-церемонии-оценка-задач)
- [14.5. Мониторинг и наблюдаемость: метрики, алертинг, SLI/SLO/SLA](dev-tools-and-processes.md#5-мониторинг-и-наблюдаемость-метрики-алертинг-slislosla)
- [14.6. Профилировщики Java: JFR, async-profiler, VisualVM и поиск узких мест](dev-tools-and-processes.md#6-профилировщики-java-jfr-async-profiler-visualvm-и-поиск-узких-мест)
- [14.7. CI/CD: как устроен пайплайн и что спрашивают следом](dev-tools-and-processes.md#7-cicd-как-устроен-пайплайн-и-что-спрашивают-следом)

---

**Всего вопросов в базе: 301**

---

**Собрал — Сергей Чамкин.** Если база помогла — поделитесь с тем, кто тоже готовится. А если хотите прогнать собеседование вживую перед настоящим — приходите на мок, Telegram: **[@jaba_javi](https://t.me/jaba_javi)**.
