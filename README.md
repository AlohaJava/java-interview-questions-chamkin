# Java-собеседование: вопросы и ответы для подготовки

![Java](https://img.shields.io/badge/Java-17--21-orange)
![Вопросов](https://img.shields.io/badge/вопросов-594-blue)
![Язык](https://img.shields.io/badge/язык-русский-lightgrey)
![License](https://img.shields.io/badge/license-MIT-green)

В пособии **19 глав** с вопросами и разобранными задачами. Всего вопросов и задач — **594**. Здесь можно изучить механизм, проверить его на примере и понять, где решение перестаёт работать. Материал идёт от языка и коллекций к транзакциям, многопоточности и работе сервиса под нагрузкой.

Меня зовут **Сергей Чамкин**. Провожу мок-собеседования по Java и System Design: разбираем ответы, ход решения и пробелы перед реальным интервью. Telegram — **[@jaba_javi](https://t.me/jaba_javi)**.

Для проектирования целых систем есть отдельное пособие: [System Design: методика, вопросы и разборы задач](https://github.com/AlohaJava/system-design-interview-chamkin).

## Как готовиться

1. **Постройте основу.** Язык → коллекции → JVM → ввод-вывод. После каждого ответа объясните механизм своими словами и придумайте контрпример.
2. **Соберите backend.** Многопоточность → SQL/JPA → Spring → тестирование → безопасность. Разбирайте границы транзакций, ресурсы и поведение при ошибках.
3. **Перейдите к распределённой работе.** Kafka → микросервисы → проектирование → инструменты. Для каждого решения называйте гарантию, цену и сценарий отказа.
4. **Потренируйте интервью.** Лайвкодинг → код-ревью → вопросы на подумать → рассказ об опыте. Сначала решайте без разбора, затем сравнивайте рассуждение.

Для первого собеседования начните с первых двух этапов, уделяя больше времени основам. Для Middle добавьте диагностику, конкурентный доступ и интеграции. Для Senior проходите те же темы через инварианты, альтернативы и последствия отказов. Это маршруты подготовки, а не универсальная шкала грейдов.

**Рабочий цикл на один вопрос:** ответьте вслух → прочитайте объяснение → воспроизведите пример → измените одно условие. Возвращайтесь к вопросам, где можете назвать термин, но не можете предсказать результат.

Для первой устной тренировки включите [фрагмент интервью про `hashCode` с 5:39](https://www.youtube.com/watch?v=RrK5fWunrsY&t=339s). После вопроса поставьте видео на паузу. Могут ли два разных ключа иметь одинаковый хеш? Предскажите, как `HashMap.get` найдёт нужное значение, и сверьтесь с [разбором контракта в главе о Java](oop-and-core.md#1-контракт-между-методами-equals-и-hashcode).

Для практики ревью начните с [базовых задач](code-review.md), затем выберите [конкурентность и данные](review/concurrency-and-data.md) или [web и интеграции](review/web-and-integration.md). Прочитайте условие, запишите замечание к коду и придумайте тест, который покажет ошибку. После этого откройте разбор.

## Разделы

Номера внутри старых глав — устойчивые идентификаторы вопросов; после тематической перестановки они могут идти не подряд. Для чтения следуйте порядку в оглавлении. Базовые примеры ориентированы на Java 17–21; возможности других версий и preview-функции отмечены отдельно.

| Порядок | Раздел | Вопросов |
| ---: | --- | ---: |
| 1 | [Язык и ООП](oop-and-core.md) | 54 |
| 2 | [Коллекции](collections.md) | 36 |
| 3 | [JVM и память](jvm-and-memory.md) | 32 |
| 4 | [Ввод-вывод и сети](io-and-networking.md) | 16 |
| 5 | [Многопоточность](multithreading.md) | 49 |
| 6 | [SQL, транзакции и JPA](databases-and-sql.md) | 66 |
| 7 | [Spring и Spring Boot](spring-framework.md) | 48 |
| 8 | [Тестирование](testing.md) | 30 |
| 9 | [Безопасность приложений](application-security.md) | 16 |
| 10 | [Kafka](kafka.md) | 29 |
| 11 | [Микросервисы и API](microservices.md) | 31 |
| 12 | [Проектирование и паттерны](system-design-and-patterns.md) | 30 |
| 13 | [Инструменты и процессы](dev-tools-and-processes.md) | 20 |
| 14 | [Алгоритмы и лайвкодинг](livecoding-algorithms.md) | 33 |
| 15 | [Код-ревью: базовый практикум](code-review.md) | 24 |
| 16 | [Ревью: конкурентность и данные](review/concurrency-and-data.md) | 15 |
| 17 | [Ревью: web, интеграции и безопасность](review/web-and-integration.md) | 12 |
| 18 | [Вопросы на подумать](reasoning-questions.md) | 28 |
| 19 | [Опыт и командная работа](behavioral-questions.md) | 25 |

## Полное оглавление вопросов

Раскройте нужную главу, чтобы перейти сразу к вопросу.

<a id="1-ооп-и-java-core-oop-and-coremd"></a>

<details>
<summary>Язык и ООП — 54 вопроса</summary>

### 1. Язык и ООП

- [2. Фундаментальные принципы ООП](oop-and-core.md#2-фундаментальные-принципы-ооп)
- [6. Абстрактные классы и интерфейсы: ключевые отличия](oop-and-core.md#6-абстрактные-классы-и-интерфейсы-ключевые-отличия)
- [31. Default- и static-методы интерфейсов и diamond problem](oop-and-core.md#31-default--и-static-методы-интерфейсов-и-diamond-problem)
- [27. Модификаторы доступа: private, package-private, protected и public](oop-and-core.md#27-модификаторы-доступа-private-package-private-protected-и-public)
- [5. Механизм передачи параметров: по значению или по ссылке](oop-and-core.md#5-механизм-передачи-параметров-по-значению-или-по-ссылке)
- [49. Почему поле и метод одного объекта дают разные ответы после приведения типа?](oop-and-core.md#49-почему-поле-и-метод-одного-объекта-дают-разные-ответы-после-приведения-типа)
- [1. Контракт между методами equals() и hashCode()](oop-and-core.md#1-контракт-между-методами-equals-и-hashcode)
- [10. Как создать неизменяемый (Immutable) класс](oop-and-core.md#10-как-создать-неизменяемый-immutable-класс)
- [30. Deep copy и Shallow copy: Cloneable, copy-конструктор, сериализация](oop-and-core.md#30-deep-copy-и-shallow-copy-cloneable-copy-конструктор-сериализация)
- [20. Records и sealed-классы](oop-and-core.md#20-records-и-sealed-классы)
- [48. Почему record с массивом не получает равенство по содержимому и как защитить его состояние?](oop-and-core.md#48-почему-record-с-массивом-не-получает-равенство-по-содержимому-и-как-защитить-его-состояние)
- [8. Ключевые слова final, finally и finalize](oop-and-core.md#8-ключевые-слова-final-finally-и-finalize)
- [9. Ключевое слово static: поля, методы, блоки и вложенные классы](oop-and-core.md#9-ключевое-слово-static-поля-методы-блоки-и-вложенные-классы)
- [28. Порядок инициализации класса и объекта в иерархии наследования](oop-and-core.md#28-порядок-инициализации-класса-и-объекта-в-иерархии-наследования)
- [23. Перегрузка (Overload) и переопределение (Override) методов](oop-and-core.md#23-перегрузка-overload-и-переопределение-override-методов)
- [34. Как компилятор выбирает между widening, boxing и varargs при перегрузке?](oop-and-core.md#34-как-компилятор-выбирает-между-widening-boxing-и-varargs-при-перегрузке)
- [35. На подумать: почему null вызывает неоднозначную перегрузку и неожиданный unboxing?](oop-and-core.md#35-на-подумать-почему-null-вызывает-неоднозначную-перегрузку-и-неожиданный-unboxing)
- [26. Примитивные типы: размеры, widening и narrowing, переполнение и потеря точности](oop-and-core.md#26-примитивные-типы-размеры-widening-и-narrowing-переполнение-и-потеря-точности)
- [50. Куда пишет выражение с постинкрементом индекса?](oop-and-core.md#50-куда-пишет-выражение-с-постинкрементом-индекса)
- [36. Как правильно считать деньги через BigDecimal: scale, precision и округление?](oop-and-core.md#36-как-правильно-считать-деньги-через-bigdecimal-scale-precision-и-округление)
- [11. Autoboxing и Unboxing: классы-обертки и их ловушки](oop-and-core.md#11-autoboxing-и-unboxing-классы-обертки-и-их-ловушки)
- [3. Класс String: Пул строк и неизменяемость (Immutability)](oop-and-core.md#3-класс-string-пул-строк-и-неизменяемость-immutability)
- [25. StringBuilder, StringBuffer и конкатенация строк](oop-and-core.md#25-stringbuilder-stringbuffer-и-конкатенация-строк)
- [37. Почему String.length() не всегда равно числу символов на экране?](oop-and-core.md#37-почему-stringlength-не-всегда-равно-числу-символов-на-экране)
- [21. Comparable и Comparator](oop-and-core.md#21-comparable-и-comparator)
- [33. Date/Time API (java.time): LocalDateTime, Instant, Duration и Period](oop-and-core.md#33-datetime-api-javatime-localdatetime-instant-duration-и-period)
- [51. Можно ли сдвинуть элементы в том же массиве через System.arraycopy?](oop-and-core.md#51-можно-ли-сдвинуть-элементы-в-том-же-массиве-через-systemarraycopy)
- [12. Generics: стирание типов и Wildcards (PECS)](oop-and-core.md#12-generics-стирание-типов-и-wildcards-pecs)
- [38. Когда нужен generic-метод, а когда wildcard, и что такое capture conversion?](oop-and-core.md#38-когда-нужен-generic-метод-а-когда-wildcard-и-что-такое-capture-conversion)
- [39. Что такое heap pollution и почему raw List откладывает ошибку до чтения?](oop-and-core.md#39-что-такое-heap-pollution-и-почему-raw-list-откладывает-ошибку-до-чтения)
- [40. Почему нельзя перегрузить методы List&lt;String&gt; и List&lt;Integer&gt;, и зачем bridge-методы?](oop-and-core.md#40-почему-нельзя-перегрузить-методы-liststring-и-listinteger-и-зачем-bridge-методы)
- [41. Почему generic varargs вызывает warning и когда допустим @SafeVarargs?](oop-and-core.md#41-почему-generic-varargs-вызывает-warning-и-когда-допустим-safevarargs)
- [4. Иерархия исключений: Checked и Unchecked](oop-and-core.md#4-иерархия-исключений-checked-и-unchecked)
- [29. Кастомные исключения: checked и unchecked, когда оправданы](oop-and-core.md#29-кастомные-исключения-checked-и-unchecked-когда-оправданы)
- [22. try-with-resources и AutoCloseable](oop-and-core.md#22-try-with-resources-и-autocloseable)
- [7. Методы класса Object: toString, clone, finalize, wait и notify](oop-and-core.md#7-методы-класса-object-tostring-clone-finalize-wait-и-notify)
- [13. Функциональные интерфейсы и лямбда-выражения](oop-and-core.md#13-функциональные-интерфейсы-и-лямбда-выражения)
- [52. Когда вычисляется получатель связанной ссылки на метод?](oop-and-core.md#52-когда-вычисляется-получатель-связанной-ссылки-на-метод)
- [15. Класс Optional: назначение и паттерны использования](oop-and-core.md#15-класс-optional-назначение-и-паттерны-использования)
- [53. Как получить email из Optional, если сам поиск адреса тоже возвращает Optional?](oop-and-core.md#53-как-получить-email-из-optional-если-сам-поиск-адреса-тоже-возвращает-optional)
- [14. Stream API: операции, ленивость и Collectors](oop-and-core.md#14-stream-api-операции-ленивость-и-collectors)
- [42. Как собрать Stream в Map, если ключи повторяются и важен порядок?](oop-and-core.md#42-как-собрать-stream-в-map-если-ключи-повторяются-и-важен-порядок)
- [43. Как downstream-коллекторы меняют результат groupingBy?](oop-and-core.md#43-как-downstream-коллекторы-меняют-результат-groupingby)
- [44. На подумать: почему parallel reduce с identity даёт неверную сумму?](oop-and-core.md#44-на-подумать-почему-parallel-reduce-с-identity-даёт-неверную-сумму)
- [45. Почему peek может не выполниться и нельзя строить на нём обязательную обработку?](oop-and-core.md#45-почему-peek-может-не-выполниться-и-нельзя-строить-на-нём-обязательную-обработку)
- [46. Чем потребление Stream отличается от close и как повторить обработку файла?](oop-and-core.md#46-чем-потребление-stream-отличается-от-close-и-как-повторить-обработку-файла)
- [47. Как Stream работает с null и почему allMatch пустого потока возвращает true?](oop-and-core.md#47-как-stream-работает-с-null-и-почему-allmatch-пустого-потока-возвращает-true)
- [16. Загрузка классов и иерархия ClassLoader](oop-and-core.md#16-загрузка-классов-и-иерархия-classloader)
- [17. Reflection API: назначение, применение и цена](oop-and-core.md#17-reflection-api-назначение-применение-и-цена)
- [18. Сериализация: Serializable, transient, serialVersionUID и Externalizable](oop-and-core.md#18-сериализация-serializable-transient-serialversionuid-и-externalizable)
- [19. Вложенные классы: static, inner, локальные и анонимные](oop-and-core.md#19-вложенные-классы-static-inner-локальные-и-анонимные)
- [32. Кастомные аннотации: @Target, @Retention и обработка через Reflection](oop-and-core.md#32-кастомные-аннотации-target-retention-и-обработка-через-reflection)
- [54. Почему enum.ordinal() опасно сохранять в сообщении или строке БД?](oop-and-core.md#54-почему-enumordinal-опасно-сохранять-в-сообщении-или-строке-бд)
- [24. Обзор новшеств Java 9–21](oop-and-core.md#24-обзор-новшеств-java-921)

</details>

<a id="2-java-collections-framework-collectionsmd"></a>

<details>
<summary>Коллекции — 36 вопросов</summary>

### 2. Коллекции

- [4. Какова иерархия интерфейсов в Java Collections Framework?](collections.md#4-какова-иерархия-интерфейсов-в-java-collections-framework)
- [20. Как выбрать коллекцию под задачу: практическая шпаргалка](collections.md#20-как-выбрать-коллекцию-под-задачу-практическая-шпаргалка)
- [25. Как устроены equals и hashCode у List, Set и Map?](collections.md#25-как-устроены-equals-и-hashcode-у-list-set-и-map)
- [31. Как вычесть одну коллекцию из другой, если значения повторяются?](collections.md#31-как-вычесть-одну-коллекцию-из-другой-если-значения-повторяются)
- [12. Неизменяемые коллекции: List.of/Set.of/Map.of против Arrays.asList и Collections.unmodifiableList](collections.md#12-неизменяемые-коллекции-listofsetofmapof-против-arraysaslist-и-collectionsunmodifiablelist)
- [21. Что возвращает subList и почему изменение исходного списка ломает представление?](collections.md#21-что-возвращает-sublist-и-почему-изменение-исходного-списка-ломает-представление)
- [22. Можно ли менять Map через keySet, values и entrySet?](collections.md#22-можно-ли-менять-map-через-keyset-values-и-entryset)
- [26. Какие ловушки есть у Arrays.asList с примитивами и у Collection.toArray?](collections.md#26-какие-ловушки-есть-у-arraysaslist-с-примитивами-и-у-collectiontoarray)
- [29. Что добавили SequencedCollection, SequencedSet и SequencedMap в Java 21?](collections.md#29-что-добавили-sequencedcollection-sequencedset-и-sequencedmap-в-java-21)
- [2. В чем разница между ArrayList и LinkedList и какова сложность их основных операций (Big O)?](collections.md#2-в-чем-разница-между-arraylist-и-linkedlist-и-какова-сложность-их-основных-операций-big-o)
- [24. Чем size отличается от capacity и почему new ArrayList&lt;&gt;(100) не содержит сто элементов?](collections.md#24-чем-size-отличается-от-capacity-и-почему-new-arraylist100-не-содержит-сто-элементов)
- [23. Почему List&lt;Integer&gt;.remove(1) удаляет второй элемент, а не число 1?](collections.md#23-почему-listintegerremove1-удаляет-второй-элемент-а-не-число-1)
- [9. Iterator и Iterable: зачем нужны, как работает remove() итератора и что скрывается под капотом for-each?](collections.md#9-iterator-и-iterable-зачем-нужны-как-работает-remove-итератора-и-что-скрывается-под-капотом-for-each)
- [6. В чем разница между Fail-Fast и Fail-Safe итераторами?](collections.md#6-в-чем-разница-между-fail-fast-и-fail-safe-итераторами)
- [32. Как вставить элемент во время обхода списка и какой элемент вернёт previous()?](collections.md#32-как-вставить-элемент-во-время-обхода-списка-и-какой-элемент-вернёт-previous)
- [14. Как сортируются коллекции: Collections.sort, TimSort, стабильность и сложность сортировки?](collections.md#14-как-сортируются-коллекции-collectionssort-timsort-стабильность-и-сложность-сортировки)
- [28. Как binarySearch кодирует отсутствие элемента и какие требования предъявляет к сортировке?](collections.md#28-как-binarysearch-кодирует-отсутствие-элемента-и-какие-требования-предъявляет-к-сортировке)
- [1. Как устроена HashMap: хэширование, бакеты, коллизии, деревья и resize](collections.md#1-как-устроена-hashmap-хэширование-бакеты-коллизии-деревья-и-resize)
- [33. Почему полный обход двух HashMap одинакового размера может стоить по-разному?](collections.md#33-почему-полный-обход-двух-hashmap-одинакового-размера-может-стоить-по-разному)
- [11. Какие требования предъявляются к объектам в роли ключей HashMap?](collections.md#11-какие-требования-предъявляются-к-объектам-в-роли-ключей-hashmap)
- [7. Как устроен HashSet и почему внутри него обычная HashMap?](collections.md#7-как-устроен-hashset-и-почему-внутри-него-обычная-hashmap)
- [34. Как вывести повторяющиеся значения по одному и в нужном порядке?](collections.md#34-как-вывести-повторяющиеся-значения-по-одному-и-в-нужном-порядке)
- [5. Чем HashMap отличается от TreeMap и LinkedHashMap?](collections.md#5-чем-hashmap-отличается-от-treemap-и-linkedhashmap)
- [35. Как описать набор разрешений через EnumSet, чтобы после обновления не появилось лишнее право?](collections.md#35-как-описать-набор-разрешений-через-enumset-чтобы-после-обновления-не-появилось-лишнее-право)
- [15. LinkedHashMap: порядок вставки, порядок доступа и LRU-кэш из коробки](collections.md#15-linkedhashmap-порядок-вставки-порядок-доступа-и-lru-кэш-из-коробки)
- [16. TreeMap и NavigableMap: красно-черное дерево и методы навигации](collections.md#16-treemap-и-navigablemap-красно-черное-дерево-и-методы-навигации)
- [10. Специализированные реализации Map: WeakHashMap, EnumMap и IdentityHashMap](collections.md#10-специализированные-реализации-map-weakhashmap-enummap-и-identityhashmap)
- [27. Чем отличаются getOrDefault, putIfAbsent, computeIfAbsent и merge при null-значениях?](collections.md#27-чем-отличаются-getordefault-putifabsent-computeifabsent-и-merge-при-null-значениях)
- [8. PriorityQueue и пары методов Queue/Deque: offer/poll/peek против add/remove/element](collections.md#8-priorityqueue-и-пары-методов-queuedeque-offerpollpeek-против-addremoveelement)
- [18. ArrayDeque: почему для стека и очереди берут его, а не Stack и LinkedList](collections.md#18-arraydeque-почему-для-стека-и-очереди-берут-его-а-не-stack-и-linkedlist)
- [36. Как обработать строку с Backspace и сохранить порядок ввода?](collections.md#36-как-обработать-строку-с-backspace-и-сохранить-порядок-ввода)
- [30. На подумать: как найти top-K событий и почему изменение приоритета ломает PriorityQueue?](collections.md#30-на-подумать-как-найти-top-k-событий-и-почему-изменение-приоритета-ломает-priorityqueue)
- [3. Как устроена ConcurrentHashMap и почему она эффективнее Collections.synchronizedMap()?](collections.md#3-как-устроена-concurrenthashmap-и-почему-она-эффективнее-collectionssynchronizedmap)
- [17. CopyOnWriteArrayList: устройство и когда она лучше синхронизированного списка](collections.md#17-copyonwritearraylist-устройство-и-когда-она-лучше-синхронизированного-списка)
- [19. Блокирующие очереди: ArrayBlockingQueue, LinkedBlockingQueue, SynchronousQueue](collections.md#19-блокирующие-очереди-arrayblockingqueue-linkedblockingqueue-synchronousqueue)
- [13. Vector, Hashtable и Stack: почему они считаются legacy и что использовать вместо них?](collections.md#13-vector-hashtable-и-stack-почему-они-считаются-legacy-и-что-использовать-вместо-них)

</details>

<a id="3-jvm-память-и-сборщик-мусора-gc-jvm-and-memorymd"></a>

<details>
<summary>JVM и память — 32 вопроса</summary>

### 3. JVM и память

- [3. Чем различаются JDK, JRE и JVM?](jvm-and-memory.md#3-чем-различаются-jdk-jre-и-jvm)
- [1. Как устроена память в JVM: в чем разница между Heap (кучей) и Stack (стеком)?](jvm-and-memory.md#1-как-устроена-память-в-jvm-в-чем-разница-между-heap-кучей-и-stack-стеком)
- [27. Почему по полям класса нельзя точно назвать размер одного объекта?](jvm-and-memory.md#27-почему-по-полям-класса-нельзя-точно-назвать-размер-одного-объекта)
- [7. Как устроена иерархия ClassLoader (Bootstrap, Platform, Application) и что такое делегирование родителю?](jvm-and-memory.md#7-как-устроена-иерархия-classloader-bootstrap-platform-application-и-что-такое-делегирование-родителю)
- [17. Когда инициализация класса блокирует другие потоки и что произойдёт после ошибки в static-блоке?](jvm-and-memory.md#17-когда-инициализация-класса-блокирует-другие-потоки-и-что-произойдёт-после-ошибки-в-static-блоке)
- [18. Почему одинаковое имя класса не гарантирует совместимость типов и как это связано с утечкой ClassLoader?](jvm-and-memory.md#18-почему-одинаковое-имя-класса-не-гарантирует-совместимость-типов-и-как-это-связано-с-утечкой-classloader)
- [28. Почему после обновления библиотеки старый клиент по-прежнему видит прежнее значение константы?](jvm-and-memory.md#28-почему-после-обновления-библиотеки-старый-клиент-по-прежнему-видит-прежнее-значение-константы)
- [29. Библиотеку собрали на новом JDK. Как проверить, запустится ли она на старой JVM?](jvm-and-memory.md#29-библиотеку-собрали-на-новом-jdk-как-проверить-запустится-ли-она-на-старой-jvm)
- [8. Как JVM исполняет код: интерпретатор, JIT-компиляторы C1/C2 и tiered compilation?](jvm-and-memory.md#8-как-jvm-исполняет-код-интерпретатор-jit-компиляторы-c1c2-и-tiered-compilation)
- [30. Что можно узнать об invokedynamic из байт-кода обычной лямбды?](jvm-and-memory.md#30-что-можно-узнать-об-invokedynamic-из-байт-кода-обычной-лямбды)
- [12. Какие оптимизации применяет JVM к коду и данным (Compact Strings, String Deduplication, Escape Analysis)?](jvm-and-memory.md#12-какие-оптимизации-применяет-jvm-к-коду-и-данным-compact-strings-string-deduplication-escape-analysis)
- [25. Почему простой замер nanoTime вокруг метода может обмануть и чем помогает JMH?](jvm-and-memory.md#25-почему-простой-замер-nanotime-вокруг-метода-может-обмануть-и-чем-помогает-jmh)
- [4. Как устроены поколения в Heap (Young Generation, Old Generation) и процесс сборки мусора?](jvm-and-memory.md#4-как-устроены-поколения-в-heap-young-generation-old-generation-и-процесс-сборки-мусора)
- [2. Какие бывают сборщики мусора в JVM (Serial, Parallel, G1, ZGC, Shenandoah, Epsilon)?](jvm-and-memory.md#2-какие-бывают-сборщики-мусора-в-jvm-serial-parallel-g1-zgc-shenandoah-epsilon)
- [6. Что делает метод System.gc() и гарантирует ли он немедленную сборку мусора?](jvm-and-memory.md#6-что-делает-метод-systemgc-и-гарантирует-ли-он-немедленную-сборку-мусора)
- [9. Какие бывают типы ссылок в Java (Strong, Soft, Weak, Phantom) и для чего нужна ReferenceQueue?](jvm-and-memory.md#9-какие-бывают-типы-ссылок-в-java-strong-soft-weak-phantom-и-для-чего-нужна-referencequeue)
- [31. Heap почти пуст, а файловые дескрипторы закончились. Поможет ли Cleaner вместо close?](jvm-and-memory.md#31-heap-почти-пуст-а-файловые-дескрипторы-закончились-поможет-ли-cleaner-вместо-close)
- [23. Почему G1 может страдать от humongous-объектов даже при свободной памяти?](jvm-and-memory.md#23-почему-g1-может-страдать-от-humongous-объектов-даже-при-свободной-памяти)
- [24. Как корректно сравнить G1 и ZGC и что меняется между JDK 21, 23 и 24?](jvm-and-memory.md#24-как-корректно-сравнить-g1-и-zgc-и-что-меняется-между-jdk-21-23-и-24)
- [5. Что такое OutOfMemoryError (OOM) и StackOverflowError и как их отлаживать?](jvm-and-memory.md#5-что-такое-outofmemoryerror-oom-и-stackoverflowerror-и-как-их-отлаживать)
- [32. JVM внезапно завершилась. Как отличить фатальный сбой HotSpot от OutOfMemoryError и внешнего убийства процесса?](jvm-and-memory.md#32-jvm-внезапно-завершилась-как-отличить-фатальный-сбой-hotspot-от-outofmemoryerror-и-внешнего-убийства-процесса)
- [10. Какие основные флаги JVM нужно знать (-Xms/-Xmx, выбор GC, логи GC, HeapDumpOnOutOfMemoryError)?](jvm-and-memory.md#10-какие-основные-флаги-jvm-нужно-знать--xms-xmx-выбор-gc-логи-gc-heapdumponoutofmemoryerror)
- [11. Что такое Metaspace, чем он заменил PermGen и как регулируется его размер?](jvm-and-memory.md#11-что-такое-metaspace-чем-он-заменил-permgen-и-как-регулируется-его-размер)
- [13. Какие инструменты JDK помогают диагностировать проблемы JVM (jps, jstack, jmap, jstat, jcmd, JFR)?](jvm-and-memory.md#13-какие-инструменты-jdk-помогают-диагностировать-проблемы-jvm-jps-jstack-jmap-jstat-jcmd-jfr)
- [14. Что такое off-heap память и DirectByteBuffer и где это используется на практике?](jvm-and-memory.md#14-что-такое-off-heap-память-и-directbytebuffer-и-где-это-используется-на-практике)
- [15. Утечки памяти в Java: типичные причины (статические коллекции, ресурсы, слушатели, ThreadLocal, кэши) и как их диагностировать?](jvm-and-memory.md#15-утечки-памяти-в-java-типичные-причины-статические-коллекции-ресурсы-слушатели-threadlocal-кэши-и-как-их-диагностировать)
- [16. Практика тюнинга JVM/GC: что отвечать на «приходилось ли тюнить» и какие флаги реально крутят?](jvm-and-memory.md#16-практика-тюнинга-jvmgc-что-отвечать-на-приходилось-ли-тюнить-и-какие-флаги-реально-крутят)
- [19. Как отличить утечку heap от нормального роста живых данных и временного всплеска аллокаций?](jvm-and-memory.md#19-как-отличить-утечку-heap-от-нормального-роста-живых-данных-и-временного-всплеска-аллокаций)
- [20. Heap свободен, но контейнер OOMKilled: где искать память и как использовать Native Memory Tracking?](jvm-and-memory.md#20-heap-свободен-но-контейнер-oomkilled-где-искать-память-и-как-использовать-native-memory-tracking)
- [21. Как построить расследование задержек с JFR, GC-логами и метриками приложения?](jvm-and-memory.md#21-как-построить-расследование-задержек-с-jfr-gc-логами-и-метриками-приложения)
- [22. Что означают shallow size, retained size и dominator tree в heap dump? Помогают ли циклические ссылки?](jvm-and-memory.md#22-что-означают-shallow-size-retained-size-и-dominator-tree-в-heap-dump-помогают-ли-циклические-ссылки)
- [26. Почему приложение замирает, хотя в GC-логах нет длинной сборки: что такое safepoint?](jvm-and-memory.md#26-почему-приложение-замирает-хотя-в-gc-логах-нет-длинной-сборки-что-такое-safepoint)

</details>

<details>
<summary>Ввод-вывод и сети — 16 вопросов</summary>

### 4. Ввод-вывод и сети

- [1. Чем отличаются InputStream, Reader и Stream API?](io-and-networking.md#1-чем-отличаются-inputstream-reader-и-stream-api)
- [2. Почему текст портится при чтении и как явно управлять кодировкой?](io-and-networking.md#2-почему-текст-портится-при-чтении-и-как-явно-управлять-кодировкой)
- [3. Почему одного read(buffer) недостаточно и что означает available()?](io-and-networking.md#3-почему-одного-readbuffer-недостаточно-и-что-означает-available)
- [4. Чем различаются buffering, flush и close и кто закрывает ресурс?](io-and-networking.md#4-чем-различаются-buffering-flush-и-close-и-кто-закрывает-ресурс)
- [5. Как выбрать между readString, lines, walk и потоковым чтением большого файла?](io-and-networking.md#5-как-выбрать-между-readstring-lines-walk-и-потоковым-чтением-большого-файла)
- [6. Что гарантируют Path.normalize(), toRealPath() и проверка startsWith()?](io-and-networking.md#6-что-гарантируют-pathnormalize-torealpath-и-проверка-startswith)
- [7. Как заменить файл без частично записанного содержимого и что значит durability?](io-and-networking.md#7-как-заменить-файл-без-частично-записанного-содержимого-и-что-значит-durability)
- [8. Правда ли, что NIO всегда неблокирующий и быстрее java.io?](io-and-networking.md#8-правда-ли-что-nio-всегда-неблокирующий-и-быстрее-javaio)
- [9. Как работают position, limit, flip, clear и compact у ByteBuffer?](io-and-networking.md#9-как-работают-position-limit-flip-clear-и-compact-у-bytebuffer)
- [10. Почему Channel.read/write могут обработать только часть сообщения?](io-and-networking.md#10-почему-channelreadwrite-могут-обработать-только-часть-сообщения)
- [11. Как Selector связан с TCP и почему готовность не означает готовое сообщение?](io-and-networking.md#11-как-selector-связан-с-tcp-и-почему-готовность-не-означает-готовое-сообщение)
- [12. Когда нужны direct buffer, memory-mapped file и transferTo?](io-and-networking.md#12-когда-нужны-direct-buffer-memory-mapped-file-и-transferto)
- [13. Как использовать HttpClient и чем send отличается от sendAsync?](io-and-networking.md#13-как-использовать-httpclient-и-чем-send-отличается-от-sendasync)
- [14. Чем connect timeout отличается от request timeout и общего deadline?](io-and-networking.md#14-чем-connect-timeout-отличается-от-request-timeout-и-общего-deadline)
- [15. Что Java-клиент проверяет при TLS и чем truststore отличается от keystore?](io-and-networking.md#15-что-java-клиент-проверяет-при-tls-и-чем-truststore-отличается-от-keystore)
- [16. Почему после смены DNS Java-приложение продолжает обращаться к старому адресу?](io-and-networking.md#16-почему-после-смены-dns-java-приложение-продолжает-обращаться-к-старому-адресу)

</details>

<a id="4-многопоточность-и-concurrency-multithreadingmd"></a>

<details>
<summary>Многопоточность — 49 вопросов</summary>

### 5. Многопоточность

- [1. Что такое процесс и поток? Чем они отличаются?](multithreading.md#1-что-такое-процесс-и-поток-чем-они-отличаются)
- [2. Какие существуют способы создания потока в Java? В чем разница между Runnable, Callable и Thread?](multithreading.md#2-какие-существуют-способы-создания-потока-в-java-в-чем-разница-между-runnable-callable-и-thread)
- [3. Расскажите о жизненном цикле потока (состояния потока в Java).](multithreading.md#3-расскажите-о-жизненном-цикле-потока-состояния-потока-в-java)
- [4. В чем разница между методами start() и run()?](multithreading.md#4-в-чем-разница-между-методами-start-и-run)
- [5. Для чего нужны методы sleep(), yield() и join()?](multithreading.md#5-для-чего-нужны-методы-sleep-yield-и-join)
- [7. Что такое потоки-демоны (Daemon threads)?](multithreading.md#7-что-такое-потоки-демоны-daemon-threads)
- [8. Что такое состояние гонки (Race Condition) и критическая секция?](multithreading.md#8-что-такое-состояние-гонки-race-condition-и-критическая-секция)
- [44. Может ли сервис продать последний билет дважды, если все обращения к счётчику атомарны?](multithreading.md#44-может-ли-сервис-продать-последний-билет-дважды-если-все-обращения-к-счётчику-атомарны)
- [10. Что такое Java Memory Model (JMM)? Концепция Happens-before.](multithreading.md#10-что-такое-java-memory-model-jmm-концепция-happens-before)
- [11. Для чего используется ключевое слово volatile? Чем оно отличается от атомарных классов?](multithreading.md#11-для-чего-используется-ключевое-слово-volatile-чем-оно-отличается-от-атомарных-классов)
- [32. Как безопасно опубликовать объект и почему final не спасает от утечки this из конструктора?](multithreading.md#32-как-безопасно-опубликовать-объект-и-почему-final-не-спасает-от-утечки-this-из-конструктора)
- [33. Два volatile-поля образуют инвариант: почему читатель всё равно может получить несогласованные данные?](multithreading.md#33-два-volatile-поля-образуют-инвариант-почему-читатель-всё-равно-может-получить-несогласованные-данные)
- [20. Как правильно реализовать потокобезопасный Singleton в Java?](multithreading.md#20-как-правильно-реализовать-потокобезопасный-singleton-в-java)
- [21. CAS (Compare-And-Swap) и атомарные классы: AtomicInteger, AtomicReference, LongAdder](multithreading.md#21-cas-compare-and-swap-и-атомарные-классы-atomicinteger-atomicreference-longadder)
- [45. Почему функция внутри updateAndGet может отработать дважды для одного вызова?](multithreading.md#45-почему-функция-внутри-updateandget-может-отработать-дважды-для-одного-вызова)
- [12. Что такое монитор и мьютекс? Как работает ключевое слово synchronized?](multithreading.md#12-что-такое-монитор-и-мьютекс-как-работает-ключевое-слово-synchronized)
- [13. Как взаимодействуют потоки через wait(), notify() и notifyAll()? Почему они вызываются только в synchronized-блоке?](multithreading.md#13-как-взаимодействуют-потоки-через-wait-notify-и-notifyall-почему-они-вызываются-только-в-synchronized-блоке)
- [17. В чем отличие Lock API (ReentrantLock) от synchronized? Что такое ReadWriteLock?](multithreading.md#17-в-чем-отличие-lock-api-reentrantlock-от-synchronized-что-такое-readwritelock)
- [46. Перевод между двумя счетами ограничили по времени. Как взять две блокировки и освободить первую при таймауте?](multithreading.md#46-перевод-между-двумя-счетами-ограничили-по-времени-как-взять-две-блокировки-и-освободить-первую-при-таймауте)
- [18. Какие классы-синхронизаторы есть в java.util.concurrent (CountDownLatch, CyclicBarrier, Semaphore)?](multithreading.md#18-какие-классы-синхронизаторы-есть-в-javautilconcurrent-countdownlatch-cyclicbarrier-semaphore)
- [47. В Phaser число задач меняется. Когда регистрировать задачу, чтобы стадия не завершилась раньше времени?](multithreading.md#47-в-phaser-число-задач-меняется-когда-регистрировать-задачу-чтобы-стадия-не-завершилась-раньше-времени)
- [19. Как работают потокобезопасные коллекции (ConcurrentHashMap, CopyOnWriteArrayList)?](multithreading.md#19-как-работают-потокобезопасные-коллекции-concurrenthashmap-copyonwritearraylist)
- [48. Два потока обмениваются буферами через Exchanger. Когда можно снова изменять переданный буфер?](multithreading.md#48-два-потока-обмениваются-буферами-через-exchanger-когда-можно-снова-изменять-переданный-буфер)
- [22. AbstractQueuedSynchronizer (AQS): на чем построены ReentrantLock, Semaphore и CountDownLatch](multithreading.md#22-abstractqueuedsynchronizer-aqs-на-чем-построены-reentrantlock-semaphore-и-countdownlatch)
- [25. StampedLock и оптимистичное чтение (Optimistic Read)](multithreading.md#25-stampedlock-и-оптимистичное-чтение-optimistic-read)
- [14. Что такое ThreadLocal и для чего он применяется?](multithreading.md#14-что-такое-threadlocal-и-для-чего-он-применяется)
- [27. ThreadLocal на практике: утечки памяти, remove() и наследование контекста](multithreading.md#27-threadlocal-на-практике-утечки-памяти-remove-и-наследование-контекста)
- [9. Что такое Deadlock, Livelock и Starvation? Как избежать Deadlock?](multithreading.md#9-что-такое-deadlock-livelock-и-starvation-как-избежать-deadlock)
- [15. Что такое ThreadPool? Архитектура Executor, ExecutorService, ThreadPoolExecutor.](multithreading.md#15-что-такое-threadpool-архитектура-executor-executorservice-threadpoolexecutor)
- [30. Как подобрать размер пула потоков: CPU-bound против IO-bound задач, формула Брайана Гетца](multithreading.md#30-как-подобрать-размер-пула-потоков-cpu-bound-против-io-bound-задач-формула-брайана-гетца)
- [39. Как ограничить перегрузку executor и почему CallerRunsPolicy или DiscardPolicy могут сломать контракт?](multithreading.md#39-как-ограничить-перегрузку-executor-и-почему-callerrunspolicy-или-discardpolicy-могут-сломать-контракт)
- [40. Как возникает thread starvation deadlock, если родитель и его подзадачи используют один пул?](multithreading.md#40-как-возникает-thread-starvation-deadlock-если-родитель-и-его-подзадачи-используют-один-пул)
- [16. Расскажите про интерфейсы Future и CompletableFuture.](multithreading.md#16-расскажите-про-интерфейсы-future-и-completablefuture)
- [49. Сто запросов отправлены в пул. Как обработать готовые результаты, если первый запрос застрял?](multithreading.md#49-сто-запросов-отправлены-в-пул-как-обработать-готовые-результаты-если-первый-запрос-застрял)
- [36. Куда исчезает исключение задачи при execute и submit и кто обязан его наблюдать?](multithreading.md#36-куда-исчезает-исключение-задачи-при-execute-и-submit-и-кто-обязан-его-наблюдать)
- [37. Как выбрать thenApply, thenCompose, handle и whenComplete, чтобы не проглотить ошибку?](multithreading.md#37-как-выбрать-thenapply-thencompose-handle-и-whencomplete-чтобы-не-проглотить-ошибку)
- [38. allOf и anyOf: что они делают при ошибке и почему первый завершившийся не равен первому успешному?](multithreading.md#38-allof-и-anyof-что-они-делают-при-ошибке-и-почему-первый-завершившийся-не-равен-первому-успешному)
- [6. Как работает механизм прерывания потоков (interrupt, isInterrupted, interrupted)?](multithreading.md#6-как-работает-механизм-прерывания-потоков-interrupt-isinterrupted-interrupted)
- [34. Что делать с InterruptedException на разных уровнях приложения и почему restore-and-continue опасен?](multithreading.md#34-что-делать-с-interruptedexception-на-разных-уровнях-приложения-и-почему-restore-and-continue-опасен)
- [35. Чем таймаут ожидания отличается от отмены работы и как передавать общий deadline?](multithreading.md#35-чем-таймаут-ожидания-отличается-от-отмены-работы-и-как-передавать-общий-deadline)
- [41. Как корректно завершить ExecutorService в пределах времени остановки приложения?](multithreading.md#41-как-корректно-завершить-executorservice-в-пределах-времени-остановки-приложения)
- [26. ScheduledExecutorService против Timer: почему Timer использовать нельзя](multithreading.md#26-scheduledexecutorservice-против-timer-почему-timer-использовать-нельзя)
- [23. Fork/Join Framework и Work Stealing: RecursiveTask и RecursiveAction](multithreading.md#23-forkjoin-framework-и-work-stealing-recursivetask-и-recursiveaction)
- [31. parallelStream и ForkJoinPool: зачем параллельные стримы и когда они вредят](multithreading.md#31-parallelstream-и-forkjoinpool-зачем-параллельные-стримы-и-когда-они-вредят)
- [24. Виртуальные потоки (Project Loom, Java 21): чем отличаются от платформенных и где выигрыш](multithreading.md#24-виртуальные-потоки-project-loom-java-21-чем-отличаются-от-платформенных-и-где-выигрыш)
- [42. Виртуальных потоков много, DB connections мало: где ставить лимит и почему одного Semaphore недостаточно?](multithreading.md#42-виртуальных-потоков-много-db-connections-мало-где-ставить-лимит-и-почему-одного-semaphore-недостаточно)
- [29. Структурированная конкурентность: StructuredTaskScope и ScopedValue](multithreading.md#29-структурированная-конкурентность-structuredtaskscope-и-scopedvalue)
- [28. Почему многопоточность не всегда ускоряет: закон Амдала, false sharing и @Contended](multithreading.md#28-почему-многопоточность-не-всегда-ускоряет-закон-амдала-false-sharing-и-contended)
- [43. Как по thread dump и JFR отличить CPU-цикл, ожидание lock и исчерпание пула?](multithreading.md#43-как-по-thread-dump-и-jfr-отличить-cpu-цикл-ожидание-lock-и-исчерпание-пула)

</details>

<a id="6-базы-данных-sql-и-hibernate--jpa-databases-and-sqlmd"></a>

<details>
<summary>SQL, транзакции и JPA — 66 вопросов</summary>

### 6. SQL, транзакции и JPA

- [1. Что такое SQL? Группы операторов: DDL, DML, DCL, TCL](databases-and-sql.md#1-что-такое-sql-группы-операторов-ddl-dml-dcl-tcl)
- [2. Виды JOIN: INNER, LEFT, RIGHT, FULL OUTER, CROSS](databases-and-sql.md#2-виды-join-inner-left-right-full-outer-cross)
- [64. Как вывести сотрудников и их непосредственных руководителей из одной таблицы?](databases-and-sql.md#64-как-вывести-сотрудников-и-их-непосредственных-руководителей-из-одной-таблицы)
- [39. Почему JOIN двух коллекций завышает суммы и как сохранить кардинальность?](databases-and-sql.md#39-почему-join-двух-коллекций-завышает-суммы-и-как-сохранить-кардинальность)
- [3. GROUP BY и HAVING: агрегация и фильтрация групп](databases-and-sql.md#3-group-by-и-having-агрегация-и-фильтрация-групп)
- [4. UNION, UNION ALL, INTERSECT, EXCEPT: различия](databases-and-sql.md#4-union-union-all-intersect-except-различия)
- [5. Подзапросы и CTE (WITH)](databases-and-sql.md#5-подзапросы-и-cte-with)
- [40. Когда EXISTS лучше JOIN и почему NOT EXISTS отличается от NOT IN?](databases-and-sql.md#40-когда-exists-лучше-join-и-почему-not-exists-отличается-от-not-in)
- [6. Оконные функции: ROW_NUMBER, RANK, DENSE_RANK, LAG/LEAD](databases-and-sql.md#6-оконные-функции-row_number-rank-dense_rank-laglead)
- [66. Как вывести всех пользователей с повторяющимся именем, сохранив их идентификаторы?](databases-and-sql.md#66-как-вывести-всех-пользователей-с-повторяющимся-именем-сохранив-их-идентификаторы)
- [41. Чем ROWS отличается от RANGE и почему LAST_VALUE возвращает текущую строку?](databases-and-sql.md#41-чем-rows-отличается-от-range-и-почему-last_value-возвращает-текущую-строку)
- [7. Нюансы работы с NULL в SQL](databases-and-sql.md#7-нюансы-работы-с-null-в-sql)
- [8. Ограничения целостности: PRIMARY KEY vs UNIQUE vs FOREIGN KEY](databases-and-sql.md#8-ограничения-целостности-primary-key-vs-unique-vs-foreign-key)
- [42. Почему CHECK (amount &gt; 0) допускает NULL и где проверять инварианты?](databases-and-sql.md#42-почему-check-amount--0-допускает-null-и-где-проверять-инварианты)
- [10. SELECT * против явного списка колонок; count(*) против count(column)](databases-and-sql.md#10-select--против-явного-списка-колонок-count-против-countcolumn)
- [11. Нормализация (1NF, 2NF, 3NF) и денормализация](databases-and-sql.md#11-нормализация-1nf-2nf-3nf-и-денормализация)
- [61. Отчёт по продажам нужен каждую минуту. Когда пригодится материализованное представление?](databases-and-sql.md#61-отчёт-по-продажам-нужен-каждую-минуту-когда-пригодится-материализованное-представление)
- [12. Хранимые процедуры и триггеры](databases-and-sql.md#12-хранимые-процедуры-и-триггеры)
- [35. DELETE vs TRUNCATE vs DROP: механика, откат, identity-счётчики](databases-and-sql.md#35-delete-vs-truncate-vs-drop-механика-откат-identity-счётчики)
- [9. Индексы: B-Tree, Hash, составные и покрывающие индексы](databases-and-sql.md#9-индексы-b-tree-hash-составные-и-покрывающие-индексы)
- [43. Как выбрать порядок колонок индекса под фильтр, сортировку и LIMIT?](databases-and-sql.md#43-как-выбрать-порядок-колонок-индекса-под-фильтр-сортировку-и-limit)
- [65. Как найти сотрудников, принятых за календарную неделю, если время хранится с часовым поясом?](databases-and-sql.md#65-как-найти-сотрудников-принятых-за-календарную-неделю-если-время-хранится-с-часовым-поясом)
- [44. Когда нужны частичный и функциональный индексы и почему параметр мешает плану?](databases-and-sql.md#44-когда-нужны-частичный-и-функциональный-индексы-и-почему-параметр-мешает-плану)
- [13. EXPLAIN и план выполнения запроса](databases-and-sql.md#13-explain-и-план-выполнения-запроса)
- [45. На подумать: в плане actual rows = 2, loops = 100000. Почему запрос медленный?](databases-and-sql.md#45-на-подумать-в-плане-actual-rows--2-loops--100000-почему-запрос-медленный)
- [46. Почему ANALYZE не всегда исправляет ошибочную оценку числа строк?](databases-and-sql.md#46-почему-analyze-не-всегда-исправляет-ошибочную-оценку-числа-строк)
- [26. Пагинация: LIMIT/OFFSET против keyset (cursor) pagination](databases-and-sql.md#26-пагинация-limitoffset-против-keyset-cursor-pagination)
- [47. На подумать: как сделать курсор для ORDER BY score DESC, id ASC?](databases-and-sql.md#47-на-подумать-как-сделать-курсор-для-order-by-score-desc-id-asc)
- [14. Принципы ACID](databases-and-sql.md#14-принципы-acid)
- [15. Уровни изоляции транзакций и аномалии чтения](databases-and-sql.md#15-уровни-изоляции-транзакций-и-аномалии-чтения)
- [48. На подумать: почему REPEATABLE READ не спасает двух дежурных врачей?](databases-and-sql.md#48-на-подумать-почему-repeatable-read-не-спасает-двух-дежурных-врачей)
- [16. Блокировки в БД: оптимистичная и пессимистичная модели, deadlock](databases-and-sql.md#16-блокировки-в-бд-оптимистичная-и-пессимистичная-модели-deadlock)
- [49. Как списать остаток без lost update и отрицательного баланса?](databases-and-sql.md#49-как-списать-остаток-без-lost-update-и-отрицательного-баланса)
- [50. Как правильно повторять транзакцию после deadlock или serialization failure?](databases-and-sql.md#50-как-правильно-повторять-транзакцию-после-deadlock-или-serialization-failure)
- [51. Почему проверка existsByEmail перед INSERT не гарантирует уникальность?](databases-and-sql.md#51-почему-проверка-existsbyemail-перед-insert-не-гарантирует-уникальность)
- [52. Как таблица идемпотентности защищает от повторного запроса после таймаута?](databases-and-sql.md#52-как-таблица-идемпотентности-защищает-от-повторного-запроса-после-таймаута)
- [17. MVCC: многоверсионный контроль конкурентного доступа](databases-and-sql.md#17-mvcc-многоверсионный-контроль-конкурентного-доступа)
- [53. Почему длинная читающая транзакция мешает VACUUM, хотя ничего не блокирует FOR UPDATE?](databases-and-sql.md#53-почему-длинная-читающая-транзакция-мешает-vacuum-хотя-ничего-не-блокирует-for-update)
- [25. Миграции БД: Flyway и Liquibase](databases-and-sql.md#25-миграции-бд-flyway-и-liquibase)
- [54. Как изменить обязательную колонку при rolling deployment без остановки записи?](databases-and-sql.md#54-как-изменить-обязательную-колонку-при-rolling-deployment-без-остановки-записи)
- [55. Чем CREATE INDEX CONCURRENTLY отличается от обычного создания индекса?](databases-and-sql.md#55-чем-create-index-concurrently-отличается-от-обычного-создания-индекса)
- [27. Партиционирование таблиц: range, list и hash в PostgreSQL](databases-and-sql.md#27-партиционирование-таблиц-range-list-и-hash-в-postgresql)
- [28. Репликация: синхронная и асинхронная, read replicas и replication lag](databases-and-sql.md#28-репликация-синхронная-и-асинхронная-read-replicas-и-replication-lag)
- [29. Шардинг: отличие от репликации и выбор ключа шардирования](databases-and-sql.md#29-шардинг-отличие-от-репликации-и-выбор-ключа-шардирования)
- [30. Пул соединений: HikariCP и выбор размера пула](databases-and-sql.md#30-пул-соединений-hikaricp-и-выбор-размера-пула)
- [38. HikariCP на собеседовании: глубокие вопросы по конфигурации и эксплуатации](databases-and-sql.md#38-hikaricp-на-собеседовании-глубокие-вопросы-по-конфигурации-и-эксплуатации)
- [34. JDBC под капотом ORM: DriverManager/DataSource, Statement vs PreparedStatement, CallableStatement, ResultSet](databases-and-sql.md#34-jdbc-под-капотом-orm-drivermanagerdatasource-statement-vs-preparedstatement-callablestatement-resultset)
- [60. Как читать миллионы строк через JDBC, не загрузив весь результат в память?](databases-and-sql.md#60-как-читать-миллионы-строк-через-jdbc-не-загрузив-весь-результат-в-память)
- [18. ORM и JPA: Entity, EntityManager, Persistence Context](databases-and-sql.md#18-orm-и-jpa-entity-entitymanager-persistence-context)
- [36. Стратегии генерации первичных ключей в JPA: IDENTITY / SEQUENCE / TABLE / UUID](databases-and-sql.md#36-стратегии-генерации-первичных-ключей-в-jpa-identity--sequence--table--uuid)
- [62. Как выбор стратегии наследования в JPA изменит запрос к истории платежей?](databases-and-sql.md#62-как-выбор-стратегии-наследования-в-jpa-изменит-запрос-к-истории-платежей)
- [37. persist vs merge vs saveOrUpdate (и getById vs getReference)](databases-and-sql.md#37-persist-vs-merge-vs-saveorupdate-и-getbyid-vs-getreference)
- [56. На подумать: UPDATE виден в SQL-логе после flush. Почему запись не сохранилась?](databases-and-sql.md#56-на-подумать-update-виден-в-sql-логе-после-flush-почему-запись-не-сохранилась)
- [22. Связи сущностей: @OneToMany, @ManyToOne, @ManyToMany, mappedBy, CascadeType](databases-and-sql.md#22-связи-сущностей-onetomany-manytoone-manytomany-mappedby-cascadetype)
- [63. Как описать составной ключ строки заказа и проверить его в JPA?](databases-and-sql.md#63-как-описать-составной-ключ-строки-заказа-и-проверить-его-в-jpa)
- [31. equals и hashCode в JPA-сущностях](databases-and-sql.md#31-equals-и-hashcode-в-jpa-сущностях)
- [23. Оптимистичная блокировка через @Version в JPA](databases-and-sql.md#23-оптимистичная-блокировка-через-version-в-jpa)
- [24. @Transactional в Spring: propagation и типичные ловушки](databases-and-sql.md#24-transactional-в-spring-propagation-и-типичные-ловушки)
- [20. Стратегии загрузки LAZY vs EAGER и LazyInitializationException](databases-and-sql.md#20-стратегии-загрузки-lazy-vs-eager-и-lazyinitializationexception)
- [19. Проблема N+1 и способы её решения](databases-and-sql.md#19-проблема-n1-и-способы-её-решения)
- [58. Как загрузить страницу родителей с коллекциями без пагинации в памяти?](databases-and-sql.md#58-как-загрузить-страницу-родителей-с-коллекциями-без-пагинации-в-памяти)
- [21. Кэши Hibernate: первый уровень (L1), второй уровень (L2), кэш запросов](databases-and-sql.md#21-кэши-hibernate-первый-уровень-l1-второй-уровень-l2-кэш-запросов)
- [32. Пакетная вставка и обновление в Hibernate (batch)](databases-and-sql.md#32-пакетная-вставка-и-обновление-в-hibernate-batch)
- [59. Чем batch fetching отличается от JDBC batching и размера транзакции?](databases-and-sql.md#59-чем-batch-fetching-отличается-от-jdbc-batching-и-размера-транзакции)
- [57. Почему bulk UPDATE через JPQL оставляет старые значения в EntityManager?](databases-and-sql.md#57-почему-bulk-update-через-jpql-оставляет-старые-значения-в-entitymanager)
- [33. Частые антипаттерны в работе с БД](databases-and-sql.md#33-частые-антипаттерны-в-работе-с-бд)

</details>

<a id="5-spring-framework-и-spring-boot-spring-frameworkmd"></a>

<details>
<summary>Spring и Spring Boot — 48 вопросов</summary>

### 7. Spring и Spring Boot

- [1. Инверсия контроля (IoC) и Внедрение зависимостей (DI)](spring-framework.md#1-инверсия-контроля-ioc-и-внедрение-зависимостей-di)
- [2. Spring Контейнеры: ApplicationContext и BeanFactory](spring-framework.md#2-spring-контейнеры-applicationcontext-и-beanfactory)
- [3. Spring Bean: Понятие, Области видимости (Scopes) и Жизненный цикл](spring-framework.md#3-spring-bean-понятие-области-видимости-scopes-и-жизненный-цикл)
- [48. Где хранить промежуточный результат, если контроллер обслуживает два запроса одновременно?](spring-framework.md#48-где-хранить-промежуточный-результат-если-контроллер-обслуживает-два-запроса-одновременно)
- [37. Почему prototype внутри singleton создаётся только один раз?](spring-framework.md#37-почему-prototype-внутри-singleton-создаётся-только-один-раз)
- [45. Почему бин с @Lazy всё равно создаётся при старте приложения?](spring-framework.md#45-почему-бин-с-lazy-всё-равно-создаётся-при-старте-приложения)
- [4. Ключевые аннотации конфигурации: @Bean, @Component, @Autowired, @Primary, @Qualifier](spring-framework.md#4-ключевые-аннотации-конфигурации-bean-component-autowired-primary-qualifier)
- [44. Как собрать список обработчиков и задать порядок проверок?](spring-framework.md#44-как-собрать-список-обработчиков-и-задать-порядок-проверок)
- [38. Что меняется при @Configuration(proxyBeanMethods = false)?](spring-framework.md#38-что-меняется-при-configurationproxybeanmethods--false)
- [7. Spring Boot: Преимущества, Автоконфигурация и Starters](spring-framework.md#7-spring-boot-преимущества-автоконфигурация-и-starters)
- [39. Как расследовать, почему условная конфигурация создала неправильный бин?](spring-framework.md#39-как-расследовать-почему-условная-конфигурация-создала-неправильный-бин)
- [15. Профили и внешняя конфигурация: @Profile и application.yml](spring-framework.md#15-профили-и-внешняя-конфигурация-profile-и-applicationyml)
- [43. Почему адрес сервиса заполнен локально, а на сервере приходит пустым?](spring-framework.md#43-почему-адрес-сервиса-заполнен-локально-а-на-сервере-приходит-пустым)
- [17. BeanPostProcessor и BeanFactoryPostProcessor](spring-framework.md#17-beanpostprocessor-и-beanfactorypostprocessor)
- [11. Циклические зависимости и их разрешение в Spring](spring-framework.md#11-циклические-зависимости-и-их-разрешение-в-spring)
- [36. Почему @PostConstruct не подходит для запуска транзакционного фонового процесса?](spring-framework.md#36-почему-postconstruct-не-подходит-для-запуска-транзакционного-фонового-процесса)
- [5. Аспектно-Ориентированное Программирование (AOP) в Spring](spring-framework.md#5-аспектно-ориентированное-программирование-aop-в-spring)
- [12. Прокси в Spring: JDK Dynamic Proxy и CGLIB, проблема self-invocation](spring-framework.md#12-прокси-в-spring-jdk-dynamic-proxy-и-cglib-проблема-self-invocation)
- [8. Управление транзакциями и аннотация @Transactional](spring-framework.md#8-управление-транзакциями-и-аннотация-transactional)
- [29. На подумать: почему REQUIRES_NEW внутри того же сервиса не спас аудит?](spring-framework.md#29-на-подумать-почему-requires_new-внутри-того-же-сервиса-не-спас-аудит)
- [30. Почему после перехваченного исключения возникает UnexpectedRollbackException?](spring-framework.md#30-почему-после-перехваченного-исключения-возникает-unexpectedrollbackexception)
- [31. Как checked exceptions и rollbackFor влияют на результат транзакции?](spring-framework.md#31-как-checked-exceptions-и-rollbackfor-влияют-на-результат-транзакции)
- [32. На подумать: почему REQUIRES_NEW исчерпал пул соединений?](spring-framework.md#32-на-подумать-почему-requires_new-исчерпал-пул-соединений)
- [33. Чем NESTED отличается от REQUIRES_NEW при частичном откате?](spring-framework.md#33-чем-nested-отличается-от-requires_new-при-частичном-откате)
- [34. Почему внутренняя аннотация isolation или timeout может ничего не изменить?](spring-framework.md#34-почему-внутренняя-аннотация-isolation-или-timeout-может-ничего-не-изменить)
- [35. Когда TransactionTemplate делает границу операции понятнее аннотации?](spring-framework.md#35-когда-transactiontemplate-делает-границу-операции-понятнее-аннотации)
- [14. Spring Data JPA: derived queries, @Query, проекции и пагинация](spring-framework.md#14-spring-data-jpa-derived-queries-query-проекции-и-пагинация)
- [47. Как составить поиск с необязательными фильтрами и сохранить ограничение по организации?](spring-framework.md#47-как-составить-поиск-с-необязательными-фильтрами-и-сохранить-ограничение-по-организации)
- [26. Lombok: что генерирует и ловушки с JPA-сущностями](spring-framework.md#26-lombok-что-генерирует-и-ловушки-с-jpa-сущностями)
- [13. Асинхронное и плановое выполнение: @Async и @Scheduled](spring-framework.md#13-асинхронное-и-плановое-выполнение-async-и-scheduled)
- [40. Что теряется на границе @Async: транзакция, MDC и SecurityContext?](spring-framework.md#40-что-теряется-на-границе-async-транзакция-mdc-и-securitycontext)
- [42. Как обеспечить единственный запуск @Scheduled в нескольких экземплярах приложения?](spring-framework.md#42-как-обеспечить-единственный-запуск-scheduled-в-нескольких-экземплярах-приложения)
- [18. События в Spring: ApplicationEvent и @EventListener](spring-framework.md#18-события-в-spring-applicationevent-и-eventlistener)
- [41. Почему AFTER_COMMIT ещё не гарантирует сохранение изменений и доставку события?](spring-framework.md#41-почему-after_commit-ещё-не-гарантирует-сохранение-изменений-и-доставку-события)
- [19. Spring WebFlux: реактивная модель, Mono и Flux](spring-framework.md#19-spring-webflux-реактивная-модель-mono-и-flux)
- [16. Кэширование: @Cacheable, @CacheEvict и @CachePut](spring-framework.md#16-кэширование-cacheable-cacheevict-и-cacheput)
- [6. Spring MVC и паттерн Front Controller (DispatcherServlet)](spring-framework.md#6-spring-mvc-и-паттерн-front-controller-dispatcherservlet)
- [10. Создание RESTful API: @RestController, обработка исключений и валидация](spring-framework.md#10-создание-restful-api-restcontroller-обработка-исключений-и-валидация)
- [46. Почему отсутствие идентификатора в пути даёт 404, а неверный тип значения может дать 400?](spring-framework.md#46-почему-отсутствие-идентификатора-в-пути-даёт-404-а-неверный-тип-значения-может-дать-400)
- [20. Валидация: Bean Validation, @Valid и @Validated](spring-framework.md#20-валидация-bean-validation-valid-и-validated)
- [23. HTTP-клиенты между сервисами: RestTemplate, WebClient и OpenFeign](spring-framework.md#23-http-клиенты-между-сервисами-resttemplate-webclient-и-openfeign)
- [24. Filter, HandlerInterceptor и AOP: что где перехватывает](spring-framework.md#24-filter-handlerinterceptor-и-aop-что-где-перехватывает)
- [25. Jackson в Spring: настройка ObjectMapper, аннотации и циклические ссылки](spring-framework.md#25-jackson-в-spring-настройка-objectmapper-аннотации-и-циклические-ссылки)
- [9. Spring Security: Архитектура, Аутентификация и JWT](spring-framework.md#9-spring-security-архитектура-аутентификация-и-jwt)
- [27. Session-based и token-based (JWT) аутентификация, OAuth2 и OIDC](spring-framework.md#27-session-based-и-token-based-jwt-аутентификация-oauth2-и-oidc)
- [28. Хранение паролей и основы безопасности: BCrypt и OWASP Top-10](spring-framework.md#28-хранение-паролей-и-основы-безопасности-bcrypt-и-owasp-top-10)
- [21. Spring Boot Actuator: health, info, metrics и интеграция с Prometheus](spring-framework.md#21-spring-boot-actuator-health-info-metrics-и-интеграция-с-prometheus)
- [22. Логирование: SLF4J, Logback и MDC](spring-framework.md#22-логирование-slf4j-logback-и-mdc)

</details>

<a id="10-тестирование-и-качество-кода-testingmd"></a>

<details>
<summary>Тестирование — 30 вопросов</summary>

### 8. Тестирование

- [1. Что такое модульное (юнит) тестирование и чем оно отличается от интеграционного и компонентного?](testing.md#1-что-такое-модульное-юнит-тестирование-и-чем-оно-отличается-от-интеграционного-и-компонентного)
- [2. Пирамида тестирования и основные виды тестов (включая микросервисы)](testing.md#2-пирамида-тестирования-и-основные-виды-тестов-включая-микросервисы)
- [26. На подумать: какие тесты выбрать для изменения правила платежа?](testing.md#26-на-подумать-какие-тесты-выбрать-для-изменения-правила-платежа)
- [3. Виды тестовых объектов (Test Doubles): Dummy, Fake, Stub, Spy, Mock](testing.md#3-виды-тестовых-объектов-test-doubles-dummy-fake-stub-spy-mock)
- [13. Принципы хороших тестов: F.I.R.S.T, независимость и одна проверка на тест](testing.md#13-принципы-хороших-тестов-first-независимость-и-одна-проверка-на-тест)
- [14. TDD и BDD: red-green-refactor и Given-When-Then](testing.md#14-tdd-и-bdd-red-green-refactor-и-given-when-then)
- [11. Покрытие кода: JaCoCo, виды coverage (line и branch) и mutation testing с PIT](testing.md#11-покрытие-кода-jacoco-виды-coverage-line-и-branch-и-mutation-testing-с-pit)
- [4. Фреймворк JUnit 5: жизненный цикл тестов, фикстуры и Assertions](testing.md#4-фреймворк-junit-5-жизненный-цикл-тестов-фикстуры-и-assertions)
- [7. Параметризованные тесты в JUnit 5: @ParameterizedTest, @ValueSource, @CsvSource и @MethodSource](testing.md#7-параметризованные-тесты-в-junit-5-parameterizedtest-valuesource-csvsource-и-methodsource)
- [8. AssertJ и Hamcrest: fluent-assertions против стандартных assertEquals](testing.md#8-assertj-и-hamcrest-fluent-assertions-против-стандартных-assertequals)
- [5. Использование Mockito: создание моков, заглушек (stubs) и верификация поведения](testing.md#5-использование-mockito-создание-моков-заглушек-stubs-и-верификация-поведения)
- [10. Что можно и нельзя мокать в Mockito: final-классы и static-методы (mockito-inline и MockMaker)](testing.md#10-что-можно-и-нельзя-мокать-в-mockito-final-классы-и-static-методы-mockito-inline-и-mockmaker)
- [19. Как тестировать срок действия и календарные границы через Clock?](testing.md#19-как-тестировать-срок-действия-и-календарные-границы-через-clock)
- [20. Что даёт property-based testing сверх параметризованных примеров?](testing.md#20-что-даёт-property-based-testing-сверх-параметризованных-примеров)
- [25. Как архитектурный тест защищает границы модулей?](testing.md#25-как-архитектурный-тест-защищает-границы-модулей)
- [6. Тестирование Spring-приложений: контекст, WebTestClient и Testcontainers](testing.md#6-тестирование-spring-приложений-контекст-webtestclient-и-testcontainers)
- [12. @SpringBootTest против слайс-тестов: @WebMvcTest, @DataJpaTest, @JsonTest](testing.md#12-springboottest-против-слайс-тестов-webmvctest-datajpatest-jsontest)
- [16. @Transactional, @Sql и @DirtiesContext в Spring-тестах](testing.md#16-transactional-sql-и-dirtiescontext-в-spring-тестах)
- [21. На подумать: почему JPA-тест зелёный, хотя в production запись нарушает constraint?](testing.md#21-на-подумать-почему-jpa-тест-зелёный-хотя-в-production-запись-нарушает-constraint)
- [22. Почему @Transactional не очищает БД после RANDOM_PORT или фонового обработчика?](testing.md#22-почему-transactional-не-очищает-бд-после-random_port-или-фонового-обработчика)
- [24. Когда Testcontainers полезнее мока, а когда только замедляет тест?](testing.md#24-когда-testcontainers-полезнее-мока-а-когда-только-замедляет-тест)
- [27. Как совместить кэш Spring-контекста и жизненный цикл контейнера?](testing.md#27-как-совместить-кэш-spring-контекста-и-жизненный-цикл-контейнера)
- [30. Как подготовить тесты к параллельному запуску без общей очистки таблиц?](testing.md#30-как-подготовить-тесты-к-параллельному-запуску-без-общей-очистки-таблиц)
- [9. WireMock: тестирование HTTP-клиентов без реальных сетевых вызовов](testing.md#9-wiremock-тестирование-http-клиентов-без-реальных-сетевых-вызовов)
- [29. Почему зелёный WireMock-тест не доказывает совместимость с поставщиком API?](testing.md#29-почему-зелёный-wiremock-тест-не-доказывает-совместимость-с-поставщиком-api)
- [17. Тестирование Kafka: Embedded Kafka, Testcontainers и моки консьюмеров](testing.md#17-тестирование-kafka-embedded-kafka-testcontainers-и-моки-консьюмеров)
- [15. Тестирование асинхронного кода: Awaitility](testing.md#15-тестирование-асинхронного-кода-awaitility)
- [23. Как расследовать flaky-тест вместо бесконечного увеличения timeout?](testing.md#23-как-расследовать-flaky-тест-вместо-бесконечного-увеличения-timeout)
- [28. Как проверить гонку двух транзакций без надежды на случайный sleep?](testing.md#28-как-проверить-гонку-двух-транзакций-без-надежды-на-случайный-sleep)
- [18. Нагрузочное тестирование: метрики (RPS, latency, перцентили, error rate), инструменты и виды тестов](testing.md#18-нагрузочное-тестирование-метрики-rps-latency-перцентили-error-rate-инструменты-и-виды-тестов)

</details>

<details>
<summary>Безопасность приложений — 16 вопросов</summary>

### 9. Безопасность приложений

- [1. Чем аутентификация отличается от авторизации и почему роли USER недостаточно?](application-security.md#1-чем-аутентификация-отличается-от-авторизации-и-почему-роли-user-недостаточно)
- [2. Как хранить и проверять пароли и почему SHA-256 недостаточно?](application-security.md#2-как-хранить-и-проверять-пароли-и-почему-sha-256-недостаточно)
- [3. Как сделать вход и восстановление пароля устойчивыми к злоупотреблению?](application-security.md#3-как-сделать-вход-и-восстановление-пароля-устойчивыми-к-злоупотреблению)
- [4. Как устроена безопасная серверная сессия и что означает «выйти на всех устройствах»?](application-security.md#4-как-устроена-безопасная-серверная-сессия-и-что-означает-выйти-на-всех-устройствах)
- [5. Какие проверки нужны JWT помимо успешного Base64-декодирования?](application-security.md#5-какие-проверки-нужны-jwt-помимо-успешного-base64-декодирования)
- [6. Чем различаются OAuth 2.0, OpenID Connect, access token и ID token?](application-security.md#6-чем-различаются-oauth-20-openid-connect-access-token-и-id-token)
- [7. Почему PreparedStatement защищает от SQL injection, а ORM — не всегда?](application-security.md#7-почему-preparedstatement-защищает-от-sql-injection-а-orm--не-всегда)
- [8. Как SSRF возникает в Java HTTP-клиенте и почему проверки строки URL недостаточно?](application-security.md#8-как-ssrf-возникает-в-java-http-клиенте-и-почему-проверки-строки-url-недостаточно)
- [9. Когда API нужна защита от CSRF и при чём здесь способ передачи JWT?](application-security.md#9-когда-api-нужна-защита-от-csrf-и-при-чём-здесь-способ-передачи-jwt)
- [10. Что реально делает CORS и почему он не защищает API от curl?](application-security.md#10-что-реально-делает-cors-и-почему-он-не-защищает-api-от-curl)
- [11. Почему Java backend должен учитывать XSS, даже если возвращает JSON?](application-security.md#11-почему-java-backend-должен-учитывать-xss-даже-если-возвращает-json)
- [12. Как хранить секреты и что делать, если ключ уже попал в Git?](application-security.md#12-как-хранить-секреты-и-что-делать-если-ключ-уже-попал-в-git)
- [13. Как писать полезные логи без утечки паролей, токенов и персональных данных?](application-security.md#13-как-писать-полезные-логи-без-утечки-паролей-токенов-и-персональных-данных)
- [14. Как безопасно принимать файлы и почему multipart max-size недостаточно?](application-security.md#14-как-безопасно-принимать-файлы-и-почему-multipart-max-size-недостаточно)
- [15. Как оценивать риск уязвимой зависимости и почему одного номера CVE недостаточно?](application-security.md#15-как-оценивать-риск-уязвимой-зависимости-и-почему-одного-номера-cve-недостаточно)
- [16. Чем опасна Java-десериализация недоверенных данных и спасает ли Serializable?](application-security.md#16-чем-опасна-java-десериализация-недоверенных-данных-и-спасает-ли-serializable)

</details>

<a id="7-apache-kafka-kafkamd"></a>

<details>
<summary>Kafka — 29 вопросов</summary>

### 10. Kafka

- [1. Архитектура и основные компоненты Apache Kafka](kafka.md#1-архитектура-и-основные-компоненты-apache-kafka)
- [2. Принципы работы Partition и Consumer Group в Kafka](kafka.md#2-принципы-работы-partition-и-consumer-group-в-kafka)
- [8. Ключ партиционирования и гарантии порядка в Kafka](kafka.md#8-ключ-партиционирования-и-гарантии-порядка-в-kafka)
- [4. Хранение данных в Kafka: Retention Policy и Log Compaction](kafka.md#4-хранение-данных-в-kafka-retention-policy-и-log-compaction)
- [17. KRaft вместо Zookeeper: метаданные без внешней зависимости](kafka.md#17-kraft-вместо-zookeeper-метаданные-без-внешней-зависимости)
- [5. Сравнение Apache Kafka и RabbitMQ](kafka.md#5-сравнение-apache-kafka-и-rabbitmq)
- [9. Сериализаторы и десериализаторы: встроенные, JSON, кастомные](kafka.md#9-сериализаторы-и-десериализаторы-встроенные-json-кастомные)
- [10. Schema Registry и Avro: эволюция схем и совместимость](kafka.md#10-schema-registry-и-avro-эволюция-схем-и-совместимость)
- [24. На подумать: совместимость схем прошла, но replay старой истории падает — как это возможно?](kafka.md#24-на-подумать-совместимость-схем-прошла-но-replay-старой-истории-падает--как-это-возможно)
- [11. Продюсер под капотом: батчи, сжатие и порядок отправки](kafka.md#11-продюсер-под-капотом-батчи-сжатие-и-порядок-отправки)
- [13. ISR и min.insync.replicas: репликация без иллюзий](kafka.md#13-isr-и-mininsyncreplicas-репликация-без-иллюзий)
- [3. Гарантии доставки сообщений (Acks, Exactly-once) и управление Offset](kafka.md#3-гарантии-доставки-сообщений-acks-exactly-once-и-управление-offset)
- [6. Идемпотентность консьюмера Kafka: дедупликация и idempotent producer](kafka.md#6-идемпотентность-консьюмера-kafka-дедупликация-и-idempotent-producer)
- [28. Почему таймаут producer не доказывает, что сообщение отсутствует в Kafka?](kafka.md#28-почему-таймаут-producer-не-доказывает-что-сообщение-отсутствует-в-kafka)
- [27. Как влияют открытые транзакции на read_committed и видимый lag?](kafka.md#27-как-влияют-открытые-транзакции-на-read_committed-и-видимый-lag)
- [15. Kafka Streams и ksqlDB: потоковая обработка внутри Kafka](kafka.md#15-kafka-streams-и-ksqldb-потоковая-обработка-внутри-kafka)
- [7. Rebalance консьюмер-групп в Kafka: причины, последствия, static membership](kafka.md#7-rebalance-консьюмер-групп-в-kafka-причины-последствия-static-membership)
- [12. Стратегии назначения партиций: range, round-robin, sticky и cooperative-sticky](kafka.md#12-стратегии-назначения-партиций-range-round-robin-sticky-и-cooperative-sticky)
- [18. На подумать: БД обновлена, а offset не закоммичен — что произойдет после падения?](kafka.md#18-на-подумать-бд-обновлена-а-offset-не-закоммичен--что-произойдет-после-падения)
- [19. Как коммитить offsets при параллельной обработке одного poll-батча?](kafka.md#19-как-коммитить-offsets-при-параллельной-обработке-одного-poll-батча)
- [20. На подумать: heartbeat проходит, но группу постоянно ребалансирует — почему?](kafka.md#20-на-подумать-heartbeat-проходит-но-группу-постоянно-ребалансирует--почему)
- [14. Consumer Lag: мониторинг и способы борьбы](kafka.md#14-consumer-lag-мониторинг-и-способы-борьбы)
- [25. Как отличить consumer lag от задержки бизнес-обработки и оценить время догона?](kafka.md#25-как-отличить-consumer-lag-от-задержки-бизнес-обработки-и-оценить-время-догона)
- [26. Что произойдет, если committed offset уже удален retention, и как делать reset?](kafka.md#26-что-произойдет-если-committed-offset-уже-удален-retention-и-как-делать-reset)
- [16. Dead Letter Queue в Kafka: retry-топики и разбор ошибок](kafka.md#16-dead-letter-queue-в-kafka-retry-топики-и-разбор-ошибок)
- [21. Как обработать poison pill, если listener вообще не вызывается?](kafka.md#21-как-обработать-poison-pill-если-listener-вообще-не-вызывается)
- [22. Когда retry-топики нарушают бизнес-порядок и как выбрать альтернативу?](kafka.md#22-когда-retry-топики-нарушают-бизнес-порядок-и-как-выбрать-альтернативу)
- [23. Как безопасно переиграть DLT после исправления ошибки?](kafka.md#23-как-безопасно-переиграть-dlt-после-исправления-ошибки)
- [29. Как проверить гарантии Kafka-интеграции аварийными сценариями?](kafka.md#29-как-проверить-гарантии-kafka-интеграции-аварийными-сценариями)

</details>

<a id="8-микросервисы-архитектура-rest-и-паттерны-microservicesmd"></a>

<details>
<summary>Микросервисы и API — 31 вопрос</summary>

### 11. Микросервисы и API

- [6. Микросервисная архитектура: преимущества, недостатки и отличие от монолита](microservices.md#6-микросервисная-архитектура-преимущества-недостатки-и-отличие-от-монолита)
- [17. Распил монолита: Strangler Fig, database-per-service и Anti-Corruption Layer](microservices.md#17-распил-монолита-strangler-fig-database-per-service-и-anti-corruption-layer)
- [14. Event-Driven против Request-Response: когда события, когда синхронный вызов](microservices.md#14-event-driven-против-request-response-когда-события-когда-синхронный-вызов)
- [15. CQRS и Event Sourcing: разделение команд и запросов](microservices.md#15-cqrs-и-event-sourcing-разделение-команд-и-запросов)
- [9. Service Discovery: Eureka, Consul, client-side и server-side discovery](microservices.md#9-service-discovery-eureka-consul-client-side-и-server-side-discovery)
- [10. Централизованная конфигурация: Spring Cloud Config, Consul и Vault](microservices.md#10-централизованная-конфигурация-spring-cloud-config-consul-и-vault)
- [1. Архитектурный стиль REST и принципы RESTful API](microservices.md#1-архитектурный-стиль-rest-и-принципы-restful-api)
- [2. HTTP-методы, идемпотентность и безопасность](microservices.md#2-http-методы-идемпотентность-и-безопасность)
- [3. Коды ответов HTTP в REST API](microservices.md#3-коды-ответов-http-в-rest-api)
- [4. Сравнение REST и SOAP](microservices.md#4-сравнение-rest-и-soap)
- [5. Сравнение REST и gRPC](microservices.md#5-сравнение-rest-и-grpc)
- [13. Версионирование API и Consumer-Driven Contracts](microservices.md#13-версионирование-api-и-consumer-driven-contracts)
- [30. Какие изменения HTTP API ломают клиентов, хотя JSON по-прежнему десериализуется?](microservices.md#30-какие-изменения-http-api-ломают-клиентов-хотя-json-по-прежнему-десериализуется)
- [20. Как сделать идемпотентный POST безопасным при одновременных повторах?](microservices.md#20-как-сделать-идемпотентный-post-безопасным-при-одновременных-повторах)
- [8. Резилентность микросервисов: Retry, Timeout, Rate Limiter и Bulkhead (Resilience4j)](microservices.md#8-резилентность-микросервисов-retry-timeout-rate-limiter-и-bulkhead-resilience4j)
- [21. Как распределить deadline и retry budget по цепочке HTTP-вызовов?](microservices.md#21-как-распределить-deadline-и-retry-budget-по-цепочке-http-вызовов)
- [22. На подумать: клиент получил timeout, но платеж прошел — повторять ли запрос?](microservices.md#22-на-подумать-клиент-получил-timeout-но-платеж-прошел--повторять-ли-запрос)
- [23. Почему отмена Future или HTTP-запроса не гарантирует остановку работы?](microservices.md#23-почему-отмена-future-или-http-запроса-не-гарантирует-остановку-работы)
- [24. Как выбрать Bulkhead, если используются virtual threads или неблокирующий клиент?](microservices.md#24-как-выбрать-bulkhead-если-используются-virtual-threads-или-неблокирующий-клиент)
- [25. Как связаны retries, Circuit Breaker и fallback, и почему важен порядок оберток?](microservices.md#25-как-связаны-retries-circuit-breaker-и-fallback-и-почему-важен-порядок-оберток)
- [16. Graceful Degradation и fallback-стратегии](microservices.md#16-graceful-degradation-и-fallback-стратегии)
- [7. Паттерны взаимодействия микросервисов (API Gateway, Saga, Circuit Breaker)](microservices.md#7-паттерны-взаимодействия-микросервисов-api-gateway-saga-circuit-breaker)
- [18. Распределенные транзакции: 2PC против Saga](microservices.md#18-распределенные-транзакции-2pc-против-saga)
- [11. Паттерн Transactional Outbox и Inbox](microservices.md#11-паттерн-transactional-outbox-и-inbox)
- [26. На подумать: outbox relay отправил событие и упал до отметки sent — что делать?](microservices.md#26-на-подумать-outbox-relay-отправил-событие-и-упал-до-отметки-sent--что-делать)
- [27. Почему outbox с несколькими relay может поменять порядок событий агрегата?](microservices.md#27-почему-outbox-с-несколькими-relay-может-поменять-порядок-событий-агрегата)
- [31. На подумать: старый шаг Saga завершился после компенсации — как не воскресить заказ?](microservices.md#31-на-подумать-старый-шаг-saga-завершился-после-компенсации--как-не-воскресить-заказ)
- [12. Распределенная трассировка: traceId, spanId, Micrometer Tracing, Jaeger и Zipkin](microservices.md#12-распределенная-трассировка-traceid-spanid-micrometer-tracing-jaeger-и-zipkin)
- [19. Централизованное логирование: ELK, correlation id и структурированные логи](microservices.md#19-централизованное-логирование-elk-correlation-id-и-структурированные-логи)
- [28. Как сохранить trace context через HTTP, executor и сообщения Kafka?](microservices.md#28-как-сохранить-trace-context-через-http-executor-и-сообщения-kafka)
- [29. Как заметить потерю бизнес-прогресса, если HTTP 200 и технические метрики зеленые?](microservices.md#29-как-заметить-потерю-бизнес-прогресса-если-http-200-и-технические-метрики-зеленые)

</details>

<a id="9-system-design-solid-и-паттерны-проектирования-system-design-and-patternsmd"></a>

<details>
<summary>Проектирование и паттерны — 30 вопросов</summary>

### 12. Проектирование и паттерны

- [1. Что такое принципы SOLID? Подробный разбор каждого принципа.](system-design-and-patterns.md#1-что-такое-принципы-solid-подробный-разбор-каждого-принципа)
- [2. К чему приводит нарушение принципов SOLID?](system-design-and-patterns.md#2-к-чему-приводит-нарушение-принципов-solid)
- [3. Что такое паттерны проектирования и антипаттерны? Классификация паттернов.](system-design-and-patterns.md#3-что-такое-паттерны-проектирования-и-антипаттерны-классификация-паттернов)
- [4. Паттерн Singleton (Одиночка) и его правильная реализация в Java.](system-design-and-patterns.md#4-паттерн-singleton-одиночка-и-его-правильная-реализация-в-java)
- [5. Паттерн Builder (Строитель).](system-design-and-patterns.md#5-паттерн-builder-строитель)
- [6. Паттерны Factory Method (Фабричный метод) и Abstract Factory (Абстрактная фабрика).](system-design-and-patterns.md#6-паттерны-factory-method-фабричный-метод-и-abstract-factory-абстрактная-фабрика)
- [7. Паттерны Adapter (Адаптер) и Decorator (Декоратор).](system-design-and-patterns.md#7-паттерны-adapter-адаптер-и-decorator-декоратор)
- [8. Паттерны Proxy (Заместитель) и Facade (Фасад).](system-design-and-patterns.md#8-паттерны-proxy-заместитель-и-facade-фасад)
- [9. Паттерн Observer (Наблюдатель).](system-design-and-patterns.md#9-паттерн-observer-наблюдатель)
- [10. Паттерн Strategy (Стратегия).](system-design-and-patterns.md#10-паттерн-strategy-стратегия)
- [11. Паттерны Template Method (Шаблонный метод) и Chain of Responsibility (Цепочка обязанностей).](system-design-and-patterns.md#11-паттерны-template-method-шаблонный-метод-и-chain-of-responsibility-цепочка-обязанностей)
- [12. Основные концепции System Design (Масштабирование, Балансировка нагрузки, Кэширование).](system-design-and-patterns.md#12-основные-концепции-system-design-масштабирование-балансировка-нагрузки-кэширование)
- [13. CAP-теорема и выбор базы данных (SQL vs NoSQL).](system-design-and-patterns.md#13-cap-теорема-и-выбор-базы-данных-sql-vs-nosql)
- [14. Монолитная и Микросервисная архитектуры.](system-design-and-patterns.md#14-монолитная-и-микросервисная-архитектуры)
- [15. Redis: основные структуры данных и типовые сценарии (кэш, сессии, rate limiter, pub/sub).](system-design-and-patterns.md#15-redis-основные-структуры-данных-и-типовые-сценарии-кэш-сессии-rate-limiter-pubsub)
- [16. Паттерны кэширования: Cache-Aside, Read-Through, Write-Through и Write-Behind.](system-design-and-patterns.md#16-паттерны-кэширования-cache-aside-read-through-write-through-и-write-behind)
- [17. Docker: образ, контейнер, слои и кэш Dockerfile. Best practices.](system-design-and-patterns.md#17-docker-образ-контейнер-слои-и-кэш-dockerfile-best-practices)
- [18. Kubernetes для собеседования: Pod, Deployment, Service, Ingress, ConfigMap и Secret.](system-design-and-patterns.md#18-kubernetes-для-собеседования-pod-deployment-service-ingress-configmap-и-secret)
- [19. CI/CD и стратегии деплоя: rolling, blue-green и canary.](system-design-and-patterns.md#19-cicd-и-стратегии-деплоя-rolling-blue-green-и-canary)
- [20. Rate limiting: алгоритмы token bucket, leaky bucket, fixed window и sliding window.](system-design-and-patterns.md#20-rate-limiting-алгоритмы-token-bucket-leaky-bucket-fixed-window-и-sliding-window)
- [21. Задача System Design: спроектируй URL shortener.](system-design-and-patterns.md#21-задача-system-design-спроектируй-url-shortener)
- [22. Как подходить к задаче System Design на собеседовании.](system-design-and-patterns.md#22-как-подходить-к-задаче-system-design-на-собеседовании)
- [23. Распределённые механики на пальцах: Consistent Hashing, Leader Election, генерация уникальных ID и фильтр Блума.](system-design-and-patterns.md#23-распределённые-механики-на-пальцах-consistent-hashing-leader-election-генерация-уникальных-id-и-фильтр-блума)
- [24. Сети для бэкенд-разработчика: TCP vs UDP, TLS handshake, HTTP/1.1 vs HTTP/2 vs HTTP/3, keep-alive и DNS.](system-design-and-patterns.md#24-сети-для-бэкенд-разработчика-tcp-vs-udp-tls-handshake-http11-vs-http2-vs-http3-keep-alive-и-dns)
- [25. Чем гексагональная архитектура отличается от внедрения зависимостей через Spring?](system-design-and-patterns.md#25-чем-гексагональная-архитектура-отличается-от-внедрения-зависимостей-через-spring)
- [26. Как выбрать границы модулей в Java-монолите, чтобы их можно было менять независимо?](system-design-and-patterns.md#26-как-выбрать-границы-модулей-в-java-монолите-чтобы-их-можно-было-менять-независимо)
- [27. Когда нужен интерфейс и Strategy, а когда достаточно метода или switch?](system-design-and-patterns.md#27-когда-нужен-интерфейс-и-strategy-а-когда-достаточно-метода-или-switch)
- [28. Обеспечивает ли локальный Observer или Spring event надёжную отправку после commit?](system-design-and-patterns.md#28-обеспечивает-ли-локальный-observer-или-spring-event-надёжную-отправку-после-commit)
- [29. Почему JPA entity, DTO HTTP API и доменная модель не всегда должны быть одним классом?](system-design-and-patterns.md#29-почему-jpa-entity-dto-http-api-и-доменная-модель-не-всегда-должны-быть-одним-классом)
- [30. На подумать: платёжный HTTP-вызов выполняется внутри транзакции заказа. Где провести границу?](system-design-and-patterns.md#30-на-подумать-платёжный-http-вызов-выполняется-внутри-транзакции-заказа-где-провести-границу)

</details>

<a id="14-инструменты-и-процессы-разработки-dev-tools-and-processesmd"></a>

<details>
<summary>Инструменты и процессы — 20 вопросов</summary>

### 13. Инструменты и процессы

- [1. Git: merge против rebase, cherry-pick, revert и reset, стратегии ветвления](dev-tools-and-processes.md#1-git-merge-против-rebase-cherry-pick-revert-и-reset-стратегии-ветвления)
- [2. Maven: жизненный цикл, scopes зависимостей, dependencyManagement и конфликты версий](dev-tools-and-processes.md#2-maven-жизненный-цикл-scopes-зависимостей-dependencymanagement-и-конфликты-версий)
- [3. Linux для бэкенд-разработчика: диагностика на сервере](dev-tools-and-processes.md#3-linux-для-бэкенд-разработчика-диагностика-на-сервере)
- [4. Scrum и Kanban: роли, церемонии, оценка задач](dev-tools-and-processes.md#4-scrum-и-kanban-роли-церемонии-оценка-задач)
- [17. Как превратить повторяющуюся жалобу на ретроспективе в проверяемое улучшение?](dev-tools-and-processes.md#17-как-превратить-повторяющуюся-жалобу-на-ретроспективе-в-проверяемое-улучшение)
- [5. Мониторинг и наблюдаемость: метрики, алертинг, SLI/SLO/SLA](dev-tools-and-processes.md#5-мониторинг-и-наблюдаемость-метрики-алертинг-slislosla)
- [6. Профилировщики Java: JFR, async-profiler, VisualVM и поиск узких мест](dev-tools-and-processes.md#6-профилировщики-java-jfr-async-profiler-visualvm-и-поиск-узких-мест)
- [7. CI/CD: как устроен пайплайн и что спрашивают следом](dev-tools-and-processes.md#7-cicd-как-устроен-пайплайн-и-что-спрашивают-следом)
- [18. Контракт API регулярно отстаёт от сервиса. Что проверять перед выпуском?](dev-tools-and-processes.md#18-контракт-api-регулярно-отстаёт-от-сервиса-что-проверять-перед-выпуском)
- [8. Как найти коммит, после которого появилась регрессия, с помощью git bisect?](dev-tools-and-processes.md#8-как-найти-коммит-после-которого-появилась-регрессия-с-помощью-git-bisect)
- [9. После разрешения merge-конфликта сборка зелёная, но поведение сломалось. Что проверять?](dev-tools-and-processes.md#9-после-разрешения-merge-конфликта-сборка-зелёная-но-поведение-сломалось-что-проверять)
- [10. Maven не скачивает зависимость. Как отличить проблему координат, репозитория и доступа?](dev-tools-and-processes.md#10-maven-не-скачивает-зависимость-как-отличить-проблему-координат-репозитория-и-доступа)
- [11. Почему возникает NoSuchMethodError, хотя проект успешно скомпилировался?](dev-tools-and-processes.md#11-почему-возникает-nosuchmethoderror-хотя-проект-успешно-скомпилировался)
- [12. Чем parent POM отличается от aggregator и как собрать один модуль вместе с зависимостями?](dev-tools-and-processes.md#12-чем-parent-pom-отличается-от-aggregator-и-как-собрать-один-модуль-вместе-с-зависимостями)
- [13. Как расследовать конфликт транзитивных версий в Gradle и закрепить решение?](dev-tools-and-processes.md#13-как-расследовать-конфликт-транзитивных-версий-в-gradle-и-закрепить-решение)
- [14. Почему сборка работает локально, но падает на CI или после запуска на Java 17?](dev-tools-and-processes.md#14-почему-сборка-работает-локально-но-падает-на-ci-или-после-запуска-на-java-17)
- [19. Как доказать, что на рабочий стенд попала та же сборка, которую проверяли?](dev-tools-and-processes.md#19-как-доказать-что-на-рабочий-стенд-попала-та-же-сборка-которую-проверяли)
- [20. Ошибка есть у пользователя, но на тестовом стенде всё работает. Как сузить поиск?](dev-tools-and-processes.md#20-ошибка-есть-у-пользователя-но-на-тестовом-стенде-всё-работает-как-сузить-поиск)
- [15. На подумать: как переименовать поле API и столбец БД при rolling deployment?](dev-tools-and-processes.md#15-на-подумать-как-переименовать-поле-api-и-столбец-бд-при-rolling-deployment)
- [16. После релиза вырос p99, но CPU низкий. Как искать причину без случайных рестартов?](dev-tools-and-processes.md#16-после-релиза-вырос-p99-но-cpu-низкий-как-искать-причину-без-случайных-рестартов)

</details>

<a id="11-лайвкодинг-и-алгоритмы-livecoding-algorithmsmd"></a>

<details>
<summary>Алгоритмы и лайвкодинг — 33 вопроса</summary>

### 14. Алгоритмы и лайвкодинг

- [15. Как вести себя на лайвкодинге](livecoding-algorithms.md#15-как-вести-себя-на-лайвкодинге)
- [1. Big O: как оценивать сложность алгоритма](livecoding-algorithms.md#1-big-o-как-оценивать-сложность-алгоритма)
- [12. FizzBuzz и что он на самом деле проверяет](livecoding-algorithms.md#12-fizzbuzz-и-что-он-на-самом-деле-проверяет)
- [2. Развернуть строку и проверить палиндром](livecoding-algorithms.md#2-развернуть-строку-и-проверить-палиндром)
- [3. Two Sum: найти пару чисел с заданной суммой](livecoding-algorithms.md#3-two-sum-найти-пару-чисел-с-заданной-суммой)
- [4. Проверить сбалансированность скобок в строке](livecoding-algorithms.md#4-проверить-сбалансированность-скобок-в-строке)
- [5. Найти дубликаты в массиве](livecoding-algorithms.md#5-найти-дубликаты-в-массиве)
- [24. Найти первый неповторяющийся элемент с сохранением порядка](livecoding-algorithms.md#24-найти-первый-неповторяющийся-элемент-с-сохранением-порядка)
- [25. Удалить дубликаты из отсортированного массива на месте](livecoding-algorithms.md#25-удалить-дубликаты-из-отсортированного-массива-на-месте)
- [26. Циклически сдвинуть массив на k позиций](livecoding-algorithms.md#26-циклически-сдвинуть-массив-на-k-позиций)
- [27. Слить два отсортированных массива без повторной сортировки](livecoding-algorithms.md#27-слить-два-отсортированных-массива-без-повторной-сортировки)
- [6. Бинарный поиск: итеративно и рекурсивно](livecoding-algorithms.md#6-бинарный-поиск-итеративно-и-рекурсивно)
- [9. Частотный анализ: посчитать слова и вывести топ-N](livecoding-algorithms.md#9-частотный-анализ-посчитать-слова-и-вывести-топ-n)
- [10. Типовые задачи на Stream API](livecoding-algorithms.md#10-типовые-задачи-на-stream-api)
- [20. Анаграммы и пропавшее число в массиве](livecoding-algorithms.md#20-анаграммы-и-пропавшее-число-в-массиве)
- [28. Самая длинная подстрока без повторяющихся символов](livecoding-algorithms.md#28-самая-длинная-подстрока-без-повторяющихся-символов)
- [29. Максимальная сумма непрерывного подмассива](livecoding-algorithms.md#29-максимальная-сумма-непрерывного-подмассива)
- [30. Посчитать подмассивы с заданной суммой через префиксы](livecoding-algorithms.md#30-посчитать-подмассивы-с-заданной-суммой-через-префиксы)
- [31. Объединить пересекающиеся интервалы](livecoding-algorithms.md#31-объединить-пересекающиеся-интервалы)
- [7. Развернуть односвязный список](livecoding-algorithms.md#7-развернуть-односвязный-список)
- [8. Найти N-й элемент с конца односвязного списка](livecoding-algorithms.md#8-найти-n-й-элемент-с-конца-односвязного-списка)
- [16. Проверить цикл в односвязном списке: алгоритм Флойда](livecoding-algorithms.md#16-проверить-цикл-в-односвязном-списке-алгоритм-флойда)
- [17. Бинарное дерево: обходы и проверка на BST](livecoding-algorithms.md#17-бинарное-дерево-обходы-и-проверка-на-bst)
- [18. LCA: наименьший общий предок и сумма пути](livecoding-algorithms.md#18-lca-наименьший-общий-предок-и-сумма-пути)
- [32. Кратчайший путь в невзвешенном графе и восстановление маршрута](livecoding-algorithms.md#32-кратчайший-путь-в-невзвешенном-графе-и-восстановление-маршрута)
- [11. Числа Фибоначчи: рекурсия, мемоизация, итерация](livecoding-algorithms.md#11-числа-фибоначчи-рекурсия-мемоизация-итерация)
- [33. Минимальное число монет: почему жадность требует доказательства](livecoding-algorithms.md#33-минимальное-число-монет-почему-жадность-требует-доказательства)
- [13. Потокобезопасный счётчик и producer-consumer](livecoding-algorithms.md#13-потокобезопасный-счётчик-и-producer-consumer)
- [14. LRU Cache на LinkedHashMap](livecoding-algorithms.md#14-lru-cache-на-linkedhashmap)
- [19. SQL-задачи: вторая зарплата, top-N в группе, кумулятивная сумма](livecoding-algorithms.md#19-sql-задачи-вторая-зарплата-top-n-в-группе-кумулятивная-сумма)
- [21. SQL: работа с дублями и самосоединения](livecoding-algorithms.md#21-sql-работа-с-дублями-и-самосоединения)
- [22. SQL: gaps and islands и последние записи](livecoding-algorithms.md#22-sql-gaps-and-islands-и-последние-записи)
- [23. SQL: условная агрегация и трансформация вывода](livecoding-algorithms.md#23-sql-условная-агрегация-и-трансформация-вывода)

</details>

<a id="12-код-ревью-на-собеседовании-code-reviewmd"></a>

<details>
<summary>Код-ревью: базовый практикум — 24 вопроса</summary>

### 15. Код-ревью: базовый практикум

- [1. Как отвечать на «поревьюйте этот код»](code-review.md#1-как-отвечать-на-поревьюйте-этот-код)
- [10. Как проводить код-ревью в команде](code-review.md#10-как-проводить-код-ревью-в-команде)
- [4. Сниппет: equals, hashCode и коллекции](code-review.md#4-сниппет-equals-hashcode-и-коллекции)
- [14. Сниппет: удаление элементов при обходе](code-review.md#14-сниппет-удаление-элементов-при-обходе)
- [15. Какие строки останутся в результате firstTen?](code-review.md#15-какие-строки-останутся-в-результате-firstten)
- [16. В каком порядке диспетчер покажет заявки с разным приоритетом?](code-review.md#16-в-каком-порядке-диспетчер-покажет-заявки-с-разным-приоритетом)
- [17. Кто сможет изменить состав команды после создания DTO?](code-review.md#17-кто-сможет-изменить-состав-команды-после-создания-dto)
- [18. Как построить индекс состояний по истории изменений?](code-review.md#18-как-построить-индекс-состояний-по-истории-изменений)
- [19. Когда нужно обращаться к внешнему сервису за значением?](code-review.md#19-когда-нужно-обращаться-к-внешнему-сервису-за-значением)
- [21. Что останется в списке после удаления единицы?](code-review.md#21-что-останется-в-списке-после-удаления-единицы)
- [7. Сниппет: BigDecimal и деньги](code-review.md#7-сниппет-bigdecimal-и-деньги)
- [8. Сниппет: строки и даты](code-review.md#8-сниппет-строки-и-даты)
- [9. Сниппет: Optional и Stream API](code-review.md#9-сниппет-optional-и-stream-api)
- [22. В каком порядке диспетчер покажет ближайшие задания?](code-review.md#22-в-каком-порядке-диспетчер-покажет-ближайшие-задания)
- [23. Как обрезать название для экспорта?](code-review.md#23-как-обрезать-название-для-экспорта)
- [24. Куда добавить новое время отправления?](code-review.md#24-куда-добавить-новое-время-отправления)
- [2. Сниппет: многопоточный синглтон и счётчик](code-review.md#2-сниппет-многопоточный-синглтон-и-счётчик)
- [3. Сниппет: ресурсы и исключения](code-review.md#3-сниппет-ресурсы-и-исключения)
- [20. Что происходит с чтением отчёта через секунду?](code-review.md#20-что-происходит-с-чтением-отчёта-через-секунду)
- [12. Завершатся ли два встречных перевода?](code-review.md#12-завершатся-ли-два-встречных-перевода)
- [13. Задача: банкомат (ATM)](code-review.md#13-задача-банкомат-atm)
- [5. Сниппет: JPA и Spring](code-review.md#5-сниппет-jpa-и-spring)
- [6. Сниппет: REST API контроллер](code-review.md#6-сниппет-rest-api-контроллер)
- [11. Сниппет: внешние вызовы внутри @Transactional](code-review.md#11-сниппет-внешние-вызовы-внутри-transactional)

</details>

<details>
<summary>Ревью: конкурентность и данные — 15 вопросов</summary>

### 16. Ревью: конкурентность и данные

- [1. Как передать чтение файла рабочему потоку?](review/concurrency-and-data.md#1-как-передать-чтение-файла-рабочему-потоку)
- [2. Что получит клиент занятого пула при отправке задания?](review/concurrency-and-data.md#2-что-получит-клиент-занятого-пула-при-отправке-задания)
- [3. Сколько запросов к партнёру может выполняться после отмены?](review/concurrency-and-data.md#3-сколько-запросов-к-партнёру-может-выполняться-после-отмены)
- [4. Как поведёт себя пул при ста одновременных запросах?](review/concurrency-and-data.md#4-как-поведёт-себя-пул-при-ста-одновременных-запросах)
- [5. Какую конфигурацию маршрута увидит запрос во время обновления?](review/concurrency-and-data.md#5-какую-конфигурацию-маршрута-увидит-запрос-во-время-обновления)
- [6. Какое значение вернёт кэш после обновления записи?](review/concurrency-and-data.md#6-какое-значение-вернёт-кэш-после-обновления-записи)
- [7. Как вернуть снимок событий одного клиента?](review/concurrency-and-data.md#7-как-вернуть-снимок-событий-одного-клиента)
- [8. Как сохранить черновик, который редактируют два автора?](review/concurrency-and-data.md#8-как-сохранить-черновик-который-редактируют-два-автора)
- [9. Какой обработчик получит следующее задание из очереди?](review/concurrency-and-data.md#9-какой-обработчик-получит-следующее-задание-из-очереди)
- [10. Сколько памяти займёт импорт миллиона строк?](review/concurrency-and-data.md#10-сколько-памяти-займёт-импорт-миллиона-строк)
- [11. Как получить страницу заказов со всеми позициями?](review/concurrency-and-data.md#11-как-получить-страницу-заказов-со-всеми-позициями)
- [12. Как выбрать первые сто различных идентификаторов из файла?](review/concurrency-and-data.md#12-как-выбрать-первые-сто-различных-идентификаторов-из-файла)
- [13. Какой язык увидит следующая задача пула?](review/concurrency-and-data.md#13-какой-язык-увидит-следующая-задача-пула)
- [14. Что вернёт обновление занятого отчёта?](review/concurrency-and-data.md#14-что-вернёт-обновление-занятого-отчёта)
- [15. Сколько начислит параллельный расчёт баллов?](review/concurrency-and-data.md#15-сколько-начислит-параллельный-расчёт-баллов)

</details>

<details>
<summary>Ревью: web, интеграции и безопасность — 12 вопросов</summary>

### 17. Ревью: web, интеграции и безопасность

- [1. Кто может удалить пакет черновиков?](review/web-and-integration.md#1-кто-может-удалить-пакет-черновиков)
- [2. Какие ответы сервиса отчётов обработает клиент?](review/web-and-integration.md#2-какие-ответы-сервиса-отчётов-обработает-клиент)
- [3. Какие токены примет платёжный API?](review/web-and-integration.md#3-какие-токены-примет-платёжный-api)
- [4. Когда можно подтвердить уведомление о начислении бонуса?](review/web-and-integration.md#4-когда-можно-подтвердить-уведомление-о-начислении-бонуса)
- [5. Какие данные сохранятся в журнале входа?](review/web-and-integration.md#5-какие-данные-сохранятся-в-журнале-входа)
- [6. Как загрузить файл с лимитом в пять мегабайт?](review/web-and-integration.md#6-как-загрузить-файл-с-лимитом-в-пять-мегабайт)
- [7. Что проверить перед публикацией файлов из ZIP?](review/web-and-integration.md#7-что-проверить-перед-публикацией-файлов-из-zip)
- [8. Что увидит клиент после ошибки регистрации?](review/web-and-integration.md#8-что-увидит-клиент-после-ошибки-регистрации)
- [9. Как повторить платёж после таймаута?](review/web-and-integration.md#9-как-повторить-платёж-после-таймаута)
- [10. В какой момент подтверждать обработку сообщения Kafka?](review/web-and-integration.md#10-в-какой-момент-подтверждать-обработку-сообщения-kafka)
- [11. Чьи права учитывать при отложенном экспорте?](review/web-and-integration.md#11-чьи-права-учитывать-при-отложенном-экспорте)
- [12. Когда сохранятся отметка об оплате и запись аудита?](review/web-and-integration.md#12-когда-сохранятся-отметка-об-оплате-и-запись-аудита)

</details>

<details>
<summary>Вопросы на подумать — 28 вопросов</summary>

### 18. Вопросы на подумать

- [1. Ключ виден при обходе, но поиск его не находит](reasoning-questions.md#1-ключ-виден-при-обходе-но-поиск-его-не-находит)
- [2. Почему переход с HashSet на TreeSet изменил число сумм](reasoning-questions.md#2-почему-переход-с-hashset-на-treeset-изменил-число-сумм)
- [3. Перегрузка с null: приложение сломалось после добавления метода](reasoning-questions.md#3-перегрузка-с-null-приложение-сломалось-после-добавления-метода)
- [4. Неизменяемый список не сохранил снимок заказа](reasoning-questions.md#4-неизменяемый-список-не-сохранил-снимок-заказа)
- [5. Какое исключение потеряется при закрытии ресурсов](reasoning-questions.md#5-какое-исключение-потеряется-при-закрытии-ресурсов)
- [6. Последовательный reduce дал правильный ответ, параллельный — другой](reasoning-questions.md#6-последовательный-reduce-дал-правильный-ответ-параллельный--другой)
- [7. Volatile опубликовал результат, но не спас счётчик](reasoning-questions.md#7-volatile-опубликовал-результат-но-не-спас-счётчик)
- [8. Два AtomicInteger нарушили инвариант склада](reasoning-questions.md#8-два-atomicinteger-нарушили-инвариант-склада)
- [9. Пул завис без единого synchronized](reasoning-questions.md#9-пул-завис-без-единого-synchronized)
- [10. Следующий запрос получил tenant предыдущего](reasoning-questions.md#10-следующий-запрос-получил-tenant-предыдущего)
- [11. CompletableFuture сообщил таймаут, но работа продолжается](reasoning-questions.md#11-completablefuture-сообщил-таймаут-но-работа-продолжается)
- [12. Виртуальных потоков 10 000, соединений с БД 20](reasoning-questions.md#12-виртуальных-потоков-10-000-соединений-с-бд-20)
- [13. ShutdownNow вызван, приложение не останавливается](reasoning-questions.md#13-shutdownnow-вызван-приложение-не-останавливается)
- [14. READ COMMITTED и последнее место на рейс](reasoning-questions.md#14-read-committed-и-последнее-место-на-рейс)
- [15. REPEATABLE READ сохранил снимок, но нарушил дежурство](reasoning-questions.md#15-repeatable-read-сохранил-снимок-но-нарушил-дежурство)
- [16. Проверка отсутствия пользователя прошла дважды](reasoning-questions.md#16-проверка-отсутствия-пользователя-прошла-дважды)
- [17. Все страницы отсортированы, но экспорт потерял строки](reasoning-questions.md#17-все-страницы-отсортированы-но-экспорт-потерял-строки)
- [18. SaveAndFlush прошёл, письмо отправлено, заказ исчез](reasoning-questions.md#18-saveandflush-прошёл-письмо-отправлено-заказ-исчез)
- [19. Публичный transactional-метод выполнился частично](reasoning-questions.md#19-публичный-transactional-метод-выполнился-частично)
- [20. Исключение поймали, но commit всё равно не состоялся](reasoning-questions.md#20-исключение-поймали-но-commit-всё-равно-не-состоялся)
- [21. REQUIRES_NEW заблокировал все соединения](reasoning-questions.md#21-requires_new-заблокировал-все-соединения)
- [22. Транзакция откатилась, а асинхронная запись осталась](reasoning-questions.md#22-транзакция-откатилась-а-асинхронная-запись-осталась)
- [23. Outbox опубликовал событие, но потерял подтверждение](reasoning-questions.md#23-outbox-опубликовал-событие-но-потерял-подтверждение)
- [24. HTTP-таймаут: заказ не создан или ответ потерян?](reasoning-questions.md#24-http-таймаут-заказ-не-создан-или-ответ-потерян)
- [25. Три уровня retry превратили один запрос в 27](reasoning-questions.md#25-три-уровня-retry-превратили-один-запрос-в-27)
- [26. Пул из 16 потоков съел всю heap](reasoning-questions.md#26-пул-из-16-потоков-съел-всю-heap)
- [27. Тест подписки падает только ночью и на CI](reasoning-questions.md#27-тест-подписки-падает-только-ночью-и-на-ci)
- [28. CPU 15%, а p99 вырос с 100 мс до 8 секунд](reasoning-questions.md#28-cpu-15-а-p99-вырос-с-100-мс-до-8-секунд)

</details>

<a id="13-поведенческие-вопросы-и-soft-skills-behavioral-questionsmd"></a>

<details>
<summary>Опыт и командная работа — 25 вопросов</summary>

### 19. Опыт и командная работа

- [1. Расскажите о себе](behavioral-questions.md#1-расскажите-о-себе)
- [2. Почему вы уходите с текущего места работы?](behavioral-questions.md#2-почему-вы-уходите-с-текущего-места-работы)
- [3. Расскажите о самом сложном проекте или задаче](behavioral-questions.md#3-расскажите-о-самом-сложном-проекте-или-задаче)
- [4. Расскажите о конфликте в команде и как вы его решили](behavioral-questions.md#4-расскажите-о-конфликте-в-команде-и-как-вы-его-решили)
- [5. Расскажите о вашей ошибке или провале](behavioral-questions.md#5-расскажите-о-вашей-ошибке-или-провале)
- [6. Приходилось ли работать с легаси и чужим кодом?](behavioral-questions.md#6-приходилось-ли-работать-с-легаси-и-чужим-кодом)
- [7. Как вы оцениваете задачи и что делаете, если не укладываетесь в срок?](behavioral-questions.md#7-как-вы-оцениваете-задачи-и-что-делаете-если-не-укладываетесь-в-срок)
- [8. Как вы относитесь к код-ревью?](behavioral-questions.md#8-как-вы-относитесь-к-код-ревью)
- [21. Как выбрать первую задачу в команде, где сервис уже работает в продакшене?](behavioral-questions.md#21-как-выбрать-первую-задачу-в-команде-где-сервис-уже-работает-в-продакшене)
- [22. Как разбить большую фичу, чтобы первый результат можно было проверить с пользователем?](behavioral-questions.md#22-как-разбить-большую-фичу-чтобы-первый-результат-можно-было-проверить-с-пользователем)
- [23. Как дать коллеге обратную связь, если его задачи регулярно зависают на передаче?](behavioral-questions.md#23-как-дать-коллеге-обратную-связь-если-его-задачи-регулярно-зависают-на-передаче)
- [24. Что делать, когда две команды одновременно просят заняться их задачей первой?](behavioral-questions.md#24-что-делать-когда-две-команды-одновременно-просят-заняться-их-задачей-первой)
- [9. Куда вы хотите развиваться в ближайшие 2-3 года?](behavioral-questions.md#9-куда-вы-хотите-развиваться-в-ближайшие-2-3-года)
- [10. Ваши сильные и слабые стороны](behavioral-questions.md#10-ваши-сильные-и-слабые-стороны)
- [11. Ожидания по зарплате: как обсуждать деньги](behavioral-questions.md#11-ожидания-по-зарплате-как-обсуждать-деньги)
- [12. Какие вопросы задать работодателю](behavioral-questions.md#12-какие-вопросы-задать-работодателю)
- [13. Почему именно наша компания? Что вы о нас знаете?](behavioral-questions.md#13-почему-именно-наша-компания-что-вы-о-нас-знаете)
- [25. Как показать результат спринта, если ваша работа была только на стороне backend?](behavioral-questions.md#25-как-показать-результат-спринта-если-ваша-работа-была-только-на-стороне-backend)
- [14. Инцидент в проде: что будете делать?](behavioral-questions.md#14-инцидент-в-проде-что-будете-делать)
- [15. Как рассказать о проекте, если архитектуру и инфраструктуру настраивали другие?](behavioral-questions.md#15-как-рассказать-о-проекте-если-архитектуру-и-инфраструктуру-настраивали-другие)
- [16. Что делать, если задача сформулирована как «сделайте быстро и удобно», а критериев нет?](behavioral-questions.md#16-что-делать-если-задача-сформулирована-как-сделайте-быстро-и-удобно-а-критериев-нет)
- [17. Как провести техническое несогласие, если окончательное решение принимает другой человек?](behavioral-questions.md#17-как-провести-техническое-несогласие-если-окончательное-решение-принимает-другой-человек)
- [18. На подумать: ревью блокирует релиз второй день. Как договориться о результате?](behavioral-questions.md#18-на-подумать-ревью-блокирует-релиз-второй-день-как-договориться-о-результате)
- [19. Как помогать менее опытному коллеге и передавать сервис без постоянной зависимости от себя?](behavioral-questions.md#19-как-помогать-менее-опытному-коллеге-и-передавать-сервис-без-постоянной-зависимости-от-себя)
- [20. Как рассказать об инциденте, если причина неизвестна или вы участвовали только в части разбора?](behavioral-questions.md#20-как-рассказать-об-инциденте-если-причина-неизвестна-или-вы-участвовали-только-в-части-разбора)

</details>

## Как улучшить материал

Нашли неточность, неоднозначный пример или полезный пробел — откройте issue или pull request. Принципы подготовки ответов и проверка оформления описаны в [руководстве для участников](CONTRIBUTING.md).

## Лицензия

MIT — см. [LICENSE](LICENSE).
