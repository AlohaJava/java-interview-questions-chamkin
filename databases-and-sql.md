# Базы Данных, SQL и Hibernate / JPA

<!-- learning-nav:start -->
[← Все разделы](README.md) · [Предыдущая тема](multithreading.md) · [Следующая тема →](spring-framework.md)
<!-- learning-nav:end -->

Маршрут подготовки: сначала SQL и кардинальность результата, затем индексы и планы, транзакции и эксплуатация, после этого JDBC и ORM. SQL-примеры ориентированы на PostgreSQL 17, если не указано иное; примеры ORM — на Jakarta Persistence 3.1 и Hibernate 6.x с Java 17–21. Отличия СУБД и версий отмечены отдельно. Номера вопросов — стабильные идентификаторы, поэтому порядок чтения не всегда совпадает с нумерацией.

## Оглавление

<!-- toc:start -->
<details>
<summary>Показать темы и вопросы</summary>

- [SQL: строки, связи и ограничения](#sql-строки-связи-и-ограничения)
  - [1. Что такое SQL? Группы операторов: DDL, DML, DCL, TCL](#1-что-такое-sql-группы-операторов-ddl-dml-dcl-tcl)
  - [2. Виды JOIN: INNER, LEFT, RIGHT, FULL OUTER, CROSS](#2-виды-join-inner-left-right-full-outer-cross)
  - [64. Как вывести сотрудников и их непосредственных руководителей из одной таблицы?](#64-как-вывести-сотрудников-и-их-непосредственных-руководителей-из-одной-таблицы)
  - [39. Почему JOIN двух коллекций завышает суммы и как сохранить кардинальность?](#39-почему-join-двух-коллекций-завышает-суммы-и-как-сохранить-кардинальность)
  - [3. GROUP BY и HAVING: агрегация и фильтрация групп](#3-group-by-и-having-агрегация-и-фильтрация-групп)
  - [4. UNION, UNION ALL, INTERSECT, EXCEPT: различия](#4-union-union-all-intersect-except-различия)
  - [5. Подзапросы и CTE (WITH)](#5-подзапросы-и-cte-with)
  - [40. Когда EXISTS лучше JOIN и почему NOT EXISTS отличается от NOT IN?](#40-когда-exists-лучше-join-и-почему-not-exists-отличается-от-not-in)
  - [6. Оконные функции: ROW_NUMBER, RANK, DENSE_RANK, LAG/LEAD](#6-оконные-функции-row_number-rank-dense_rank-laglead)
  - [66. Как вывести всех пользователей с повторяющимся именем, сохранив их идентификаторы?](#66-как-вывести-всех-пользователей-с-повторяющимся-именем-сохранив-их-идентификаторы)
  - [41. Чем ROWS отличается от RANGE и почему LAST_VALUE возвращает текущую строку?](#41-чем-rows-отличается-от-range-и-почему-last_value-возвращает-текущую-строку)
  - [7. Нюансы работы с NULL в SQL](#7-нюансы-работы-с-null-в-sql)
  - [8. Ограничения целостности: PRIMARY KEY vs UNIQUE vs FOREIGN KEY](#8-ограничения-целостности-primary-key-vs-unique-vs-foreign-key)
  - [42. Почему CHECK (amount &gt; 0) допускает NULL и где проверять инварианты?](#42-почему-check-amount--0-допускает-null-и-где-проверять-инварианты)
  - [10. SELECT * против явного списка колонок; count(*) против count(column)](#10-select--против-явного-списка-колонок-count-против-countcolumn)
  - [11. Нормализация (1NF, 2NF, 3NF) и денормализация](#11-нормализация-1nf-2nf-3nf-и-денормализация)
  - [61. Отчёт по продажам нужен каждую минуту. Когда пригодится материализованное представление?](#61-отчёт-по-продажам-нужен-каждую-минуту-когда-пригодится-материализованное-представление)
  - [12. Хранимые процедуры и триггеры](#12-хранимые-процедуры-и-триггеры)
  - [35. DELETE vs TRUNCATE vs DROP: механика, откат, identity-счётчики](#35-delete-vs-truncate-vs-drop-механика-откат-identity-счётчики)
- [Индексы, планы и постраничное чтение](#индексы-планы-и-постраничное-чтение)
  - [9. Индексы: B-Tree, Hash, составные и покрывающие индексы](#9-индексы-b-tree-hash-составные-и-покрывающие-индексы)
  - [43. Как выбрать порядок колонок индекса под фильтр, сортировку и LIMIT?](#43-как-выбрать-порядок-колонок-индекса-под-фильтр-сортировку-и-limit)
  - [65. Как найти сотрудников, принятых за календарную неделю, если время хранится с часовым поясом?](#65-как-найти-сотрудников-принятых-за-календарную-неделю-если-время-хранится-с-часовым-поясом)
  - [44. Когда нужны частичный и функциональный индексы и почему параметр мешает плану?](#44-когда-нужны-частичный-и-функциональный-индексы-и-почему-параметр-мешает-плану)
  - [13. EXPLAIN и план выполнения запроса](#13-explain-и-план-выполнения-запроса)
  - [45. На подумать: в плане actual rows = 2, loops = 100000. Почему запрос медленный?](#45-на-подумать-в-плане-actual-rows--2-loops--100000-почему-запрос-медленный)
  - [46. Почему ANALYZE не всегда исправляет ошибочную оценку числа строк?](#46-почему-analyze-не-всегда-исправляет-ошибочную-оценку-числа-строк)
  - [26. Пагинация: LIMIT/OFFSET против keyset (cursor) pagination](#26-пагинация-limitoffset-против-keyset-cursor-pagination)
  - [47. На подумать: как сделать курсор для ORDER BY score DESC, id ASC?](#47-на-подумать-как-сделать-курсор-для-order-by-score-desc-id-asc)
- [Транзакции, конкуренция и сохранность данных](#транзакции-конкуренция-и-сохранность-данных)
  - [14. Принципы ACID](#14-принципы-acid)
  - [15. Уровни изоляции транзакций и аномалии чтения](#15-уровни-изоляции-транзакций-и-аномалии-чтения)
  - [48. На подумать: почему REPEATABLE READ не спасает двух дежурных врачей?](#48-на-подумать-почему-repeatable-read-не-спасает-двух-дежурных-врачей)
  - [16. Блокировки в БД: оптимистичная и пессимистичная модели, deadlock](#16-блокировки-в-бд-оптимистичная-и-пессимистичная-модели-deadlock)
  - [49. Как списать остаток без lost update и отрицательного баланса?](#49-как-списать-остаток-без-lost-update-и-отрицательного-баланса)
  - [50. Как правильно повторять транзакцию после deadlock или serialization failure?](#50-как-правильно-повторять-транзакцию-после-deadlock-или-serialization-failure)
  - [51. Почему проверка existsByEmail перед INSERT не гарантирует уникальность?](#51-почему-проверка-existsbyemail-перед-insert-не-гарантирует-уникальность)
  - [52. Как таблица идемпотентности защищает от повторного запроса после таймаута?](#52-как-таблица-идемпотентности-защищает-от-повторного-запроса-после-таймаута)
  - [17. MVCC: многоверсионный контроль конкурентного доступа](#17-mvcc-многоверсионный-контроль-конкурентного-доступа)
  - [53. Почему длинная читающая транзакция мешает VACUUM, хотя ничего не блокирует FOR UPDATE?](#53-почему-длинная-читающая-транзакция-мешает-vacuum-хотя-ничего-не-блокирует-for-update)
- [Схема и эксплуатация](#схема-и-эксплуатация)
  - [25. Миграции БД: Flyway и Liquibase](#25-миграции-бд-flyway-и-liquibase)
  - [54. Как изменить обязательную колонку при rolling deployment без остановки записи?](#54-как-изменить-обязательную-колонку-при-rolling-deployment-без-остановки-записи)
  - [55. Чем CREATE INDEX CONCURRENTLY отличается от обычного создания индекса?](#55-чем-create-index-concurrently-отличается-от-обычного-создания-индекса)
  - [27. Партиционирование таблиц: range, list и hash в PostgreSQL](#27-партиционирование-таблиц-range-list-и-hash-в-postgresql)
  - [28. Репликация: синхронная и асинхронная, read replicas и replication lag](#28-репликация-синхронная-и-асинхронная-read-replicas-и-replication-lag)
  - [29. Шардинг: отличие от репликации и выбор ключа шардирования](#29-шардинг-отличие-от-репликации-и-выбор-ключа-шардирования)
  - [30. Пул соединений: HikariCP и выбор размера пула](#30-пул-соединений-hikaricp-и-выбор-размера-пула)
  - [38. HikariCP на собеседовании: глубокие вопросы по конфигурации и эксплуатации](#38-hikaricp-на-собеседовании-глубокие-вопросы-по-конфигурации-и-эксплуатации)
- [Java и ORM: контекст, связи и стоимость запросов](#java-и-orm-контекст-связи-и-стоимость-запросов)
  - [34. JDBC под капотом ORM: DriverManager/DataSource, Statement vs PreparedStatement, CallableStatement, ResultSet](#34-jdbc-под-капотом-orm-drivermanagerdatasource-statement-vs-preparedstatement-callablestatement-resultset)
  - [60. Как читать миллионы строк через JDBC, не загрузив весь результат в память?](#60-как-читать-миллионы-строк-через-jdbc-не-загрузив-весь-результат-в-память)
  - [18. ORM и JPA: Entity, EntityManager, Persistence Context](#18-orm-и-jpa-entity-entitymanager-persistence-context)
  - [36. Стратегии генерации первичных ключей в JPA: IDENTITY / SEQUENCE / TABLE / UUID](#36-стратегии-генерации-первичных-ключей-в-jpa-identity--sequence--table--uuid)
  - [62. Как выбор стратегии наследования в JPA изменит запрос к истории платежей?](#62-как-выбор-стратегии-наследования-в-jpa-изменит-запрос-к-истории-платежей)
  - [37. persist vs merge vs saveOrUpdate (и getById vs getReference)](#37-persist-vs-merge-vs-saveorupdate-и-getbyid-vs-getreference)
  - [56. На подумать: UPDATE виден в SQL-логе после flush. Почему запись не сохранилась?](#56-на-подумать-update-виден-в-sql-логе-после-flush-почему-запись-не-сохранилась)
  - [22. Связи сущностей: @OneToMany, @ManyToOne, @ManyToMany, mappedBy, CascadeType](#22-связи-сущностей-onetomany-manytoone-manytomany-mappedby-cascadetype)
  - [63. Как описать составной ключ строки заказа и проверить его в JPA?](#63-как-описать-составной-ключ-строки-заказа-и-проверить-его-в-jpa)
  - [31. equals и hashCode в JPA-сущностях](#31-equals-и-hashcode-в-jpa-сущностях)
  - [23. Оптимистичная блокировка через @Version в JPA](#23-оптимистичная-блокировка-через-version-в-jpa)
  - [24. @Transactional в Spring: propagation и типичные ловушки](#24-transactional-в-spring-propagation-и-типичные-ловушки)
  - [20. Стратегии загрузки LAZY vs EAGER и LazyInitializationException](#20-стратегии-загрузки-lazy-vs-eager-и-lazyinitializationexception)
  - [19. Проблема N+1 и способы её решения](#19-проблема-n1-и-способы-её-решения)
  - [58. Как загрузить страницу родителей с коллекциями без пагинации в памяти?](#58-как-загрузить-страницу-родителей-с-коллекциями-без-пагинации-в-памяти)
  - [21. Кэши Hibernate: первый уровень (L1), второй уровень (L2), кэш запросов](#21-кэши-hibernate-первый-уровень-l1-второй-уровень-l2-кэш-запросов)
  - [32. Пакетная вставка и обновление в Hibernate (batch)](#32-пакетная-вставка-и-обновление-в-hibernate-batch)
  - [59. Чем batch fetching отличается от JDBC batching и размера транзакции?](#59-чем-batch-fetching-отличается-от-jdbc-batching-и-размера-транзакции)
  - [57. Почему bulk UPDATE через JPQL оставляет старые значения в EntityManager?](#57-почему-bulk-update-через-jpql-оставляет-старые-значения-в-entitymanager)
  - [33. Частые антипаттерны в работе с БД](#33-частые-антипаттерны-в-работе-с-бд)

</details>
<!-- toc:end -->


## SQL: строки, связи и ограничения

### 1. Что такое SQL? Группы операторов: DDL, DML, DCL, TCL

**SQL (Structured Query Language)** — декларативный язык структурированных запросов, предназначенный для описания, изменения и выборки данных в реляционных СУБД. Ключевая особенность SQL — декларативность: мы описываем *что* хотим получить, а оптимизатор СУБД сам решает, *как* выполнить запрос (какой план выбрать, какие индексы использовать).

Операторы SQL принято делить на четыре группы:

- **DDL (Data Definition Language)** — язык определения данных. Управляет структурой объектов БД: `CREATE`, `ALTER`, `DROP`, `TRUNCATE`. В MySQL и Oracle многие DDL-операторы неявно фиксируют (commit) текущую транзакцию, а в PostgreSQL большинство DDL транзакционно — `CREATE TABLE` можно откатить вместе с остальной транзакцией (см. вопрос 14).
- **DML (Data Manipulation Language)** — язык манипуляции данными: `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `MERGE`. В некоторых классификациях `SELECT` выделяют в отдельную группу **DQL (Data Query Language)** — частый follow-up, достаточно знать, что обе классификации встречаются.
- **DCL (Data Control Language)** — язык управления доступом: `GRANT` (выдать привилегии), `REVOKE` (отозвать привилегии).
- **TCL (Transaction Control Language)** — язык управления транзакциями: `BEGIN` / `START TRANSACTION`, `COMMIT`, `ROLLBACK`, `SAVEPOINT`.

Примеры каждой группы:

```sql
-- DDL
CREATE TABLE employees (
    id          BIGINT PRIMARY KEY,
    full_name   VARCHAR(200) NOT NULL,
    salary      NUMERIC(12, 2) CHECK (salary >= 0),
    hired_at    DATE DEFAULT CURRENT_DATE
);
ALTER TABLE employees ADD COLUMN department_id BIGINT;
DROP TABLE IF EXISTS old_employees;

-- DML
INSERT INTO employees (id, full_name, salary) VALUES (1, 'Иван Иванов', 120000.00);
UPDATE employees SET salary = salary * 1.1 WHERE id = 1;
DELETE FROM employees WHERE id = 1;
SELECT id, full_name FROM employees WHERE salary > 100000;

-- DCL
GRANT SELECT, INSERT ON employees TO app_user;
REVOKE DELETE ON employees FROM app_user;

-- TCL
BEGIN;
UPDATE accounts SET balance = balance - 1000 WHERE id = 1;
SAVEPOINT after_debit;
UPDATE accounts SET balance = balance + 1000 WHERE id = 2;
COMMIT; -- или ROLLBACK TO SAVEPOINT after_debit;
```

---

### 2. Виды JOIN: INNER, LEFT, RIGHT, FULL OUTER, CROSS

**JOIN** объединяет строки двух таблиц по условию связи. Рассмотрим на примере таблиц `employees` и `departments`.

```sql
CREATE TABLE departments (
    id   BIGINT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE employees (
    id            BIGINT PRIMARY KEY,
    full_name     VARCHAR(200),
    department_id BIGINT REFERENCES departments(id)
);

INSERT INTO departments VALUES (1, 'IT'), (2, 'HR'), (3, 'Sales');
INSERT INTO employees VALUES
    (1, 'Анна', 1),
    (2, 'Борис', 1),
    (3, 'Виктор', 2),
    (4, 'Галина', NULL); -- сотрудник без отдела
```

Виды соединений:

- **INNER JOIN** — возвращает только те пары строк, для которых условие связи выполнено в обеих таблицах. Строки без совпадений отбрасываются.
- **LEFT (OUTER) JOIN** — возвращает все строки левой таблицы; если справа нет совпадения, колонки правой таблицы заполняются `NULL`.
- **RIGHT (OUTER) JOIN** — зеркален LEFT: все строки правой таблицы плюс `NULL` слева при отсутствии совпадения.
- **FULL OUTER JOIN** — объединение результатов LEFT и RIGHT: все строки обеих таблиц, несовпавшие стороны заполнены `NULL`.
- **CROSS JOIN** — декартово произведение: каждая строка первой таблицы соединяется с каждой строкой второй (N × M строк). Условие связи не указывается.

```sql
-- INNER JOIN: 3 строки (Галина и отдел Sales не попадут в результат)
SELECT e.full_name, d.name AS department
FROM employees e
INNER JOIN departments d ON d.id = e.department_id;

-- LEFT JOIN: 4 строки, у Галины department = NULL
SELECT e.full_name, d.name AS department
FROM employees e
LEFT JOIN departments d ON d.id = e.department_id;

-- FULL OUTER JOIN: 5 строк — Галина с NULL-отделом и отдел Sales с NULL-сотрудником
SELECT e.full_name, d.name AS department
FROM employees e
FULL OUTER JOIN departments d ON d.id = e.department_id;

-- CROSS JOIN: 4 * 3 = 12 строк
SELECT e.full_name, d.name
FROM employees e CROSS JOIN departments d;
```

Типовой приём — поиск «осиротевших» строк через LEFT JOIN с фильтром по `NULL`:

```sql
-- Сотрудники без отдела
SELECT e.*
FROM employees e
LEFT JOIN departments d ON d.id = e.department_id
WHERE d.id IS NULL;
```

Частая ловушка — условие на правую таблицу в `WHERE` вместо `ON`. Фильтр в `WHERE` применяется уже после соединения и отбрасывает строки с `NULL` справа, то есть молча превращает LEFT JOIN в INNER:

```sql
-- INNER JOIN по факту: строки с d.name IS NULL отфильтруются в WHERE
SELECT e.full_name, d.name
FROM employees e
LEFT JOIN departments d ON d.id = e.department_id
WHERE d.name <> 'HR';

-- Как задумывалось: условие переносим в ON, сотрудники без отдела остаются
SELECT e.full_name, d.name
FROM employees e
LEFT JOIN departments d ON d.id = e.department_id AND d.name <> 'HR';
```

---

### 64. Как вывести сотрудников и их непосредственных руководителей из одной таблицы?

У таблицы `employees(id, name, manager_id)` две роли. В левой части соединения строка описывает сотрудника, в правой — его непосредственного руководителя. Директор без руководителя тоже должен попасть в список.

```sql
SELECT e.id, e.name, m.name AS manager_name
FROM employees AS e
LEFT JOIN employees AS m ON m.id = e.manager_id
ORDER BY e.id;
```

Если у Анны нет руководителя, а Борис подчиняется Анне, получатся пары `Анна → NULL` и `Борис → Анна`. Соединение по имени вместо идентификатора привязало бы Бориса сразу к нескольким Аннам. Один такой запрос проходит одно звено иерархии. Для всей цепочки произвольной глубины нужен рекурсивный обход с условием остановки и защитой от циклов в ошибочных данных.

---

### 39. Почему JOIN двух коллекций завышает суммы и как сохранить кардинальность?

JOIN возвращает пары совпавших строк, поэтому независимые связи «один ко многим» перемножают результат. Если у заказа три позиции и два платежа, соединение `orders → items → payments` даст шесть строк. `SUM(i.amount)` удвоится, а `SUM(p.amount)` утроится. `DISTINCT` по итоговой строке не исправит уже посчитанные агрегаты; `SUM(DISTINCT amount)` также неверен, если две реальные позиции стоят одинаково.

Безопасный приём — сначала привести каждую дочернюю таблицу к требуемой гранулярности: одна строка на заказ.

```sql
WITH item_totals AS (
    SELECT order_id, SUM(amount) AS total
    FROM order_items GROUP BY order_id
), payment_totals AS (
    SELECT order_id, SUM(amount) AS paid
    FROM payments GROUP BY order_id
)
SELECT o.id, COALESCE(i.total, 0) AS total,
       COALESCE(p.paid, 0) AS paid
FROM orders o
LEFT JOIN item_totals i ON i.order_id = o.id
LEFT JOIN payment_totals p ON p.order_id = o.id;
```

`LEFT JOIN` сохраняет заказы без позиций или платежей; подстановка нуля — отдельное бизнес-решение. Если отсутствие платежей нужно отличать от платежа на ноль, сохраняйте `NULL` или выводите дополнительный счётчик.

На собеседовании полезно сначала назвать ключ результата: «одна строка на заказ», затем проверить максимальное число совпадений каждой связи. Это позволяет найти ошибку ещё до анализа плана. Предварительная агрегация может читать много дочерних строк: для небольшой страницы заказов стоит ограничить агрегаты выбранными `order_id` и сравнить планы.

---

### 3. GROUP BY и HAVING: агрегация и фильтрация групп

**GROUP BY** группирует строки с одинаковыми значениями указанных колонок в одну строку-итог, к которой применяются агрегатные функции: `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`.

В `SELECT` с `GROUP BY` обычно указывают ключи группировки и агрегаты. Есть исключение для функциональной зависимости: PostgreSQL позволяет выбрать другие колонки таблицы, если в `GROUP BY` включён её первичный ключ. Произвольную неагрегированную колонку, значение которой внутри группы неоднозначно, выбирать нельзя. Не рассчитывайте на одинаковое распознавание зависимостей во всех СУБД и подзапросах.

Порядок логической обработки запроса: `FROM` → `WHERE` → `GROUP BY` → `HAVING` → `SELECT` → `DISTINCT` → `ORDER BY` → `LIMIT`. Из этого следует главное отличие:

- **WHERE** фильтрует *строки* до группировки и не может содержать агрегатные функции того же уровня запроса; агрегат внутри отдельного подзапроса допустим.
- **HAVING** фильтрует *группы* после агрегации и может содержать агрегатные функции.

```sql
-- Отделы, в которых средняя зарплата выше 100 000,
-- при этом в выборку не попадают уволенные сотрудники (фильтр WHERE — до группировки)
SELECT e.department_id,
       COUNT(*)        AS employee_count,
       AVG(e.salary)   AS avg_salary
FROM employees e
WHERE e.fired_at IS NULL
GROUP BY e.department_id
HAVING AVG(e.salary) > 100000;
```

Особенности **NULL при группировке**: все строки с `NULL` в группируемой колонке попадают в **одну общую группу** (при группировке NULL объединяются, хотя сравнение `NULL = NULL` даёт UNKNOWN, а не FALSE). Агрегатные функции `SUM`, `AVG`, `MIN`, `MAX` игнорируют `NULL` в вычисляемой колонке; `COUNT(*)` считает все строки, а `COUNT(column)` — только строки с не-NULL значением:

```sql
SELECT department_id, COUNT(*) AS total, COUNT(department_id) AS with_dept
FROM employees
GROUP BY department_id;
-- Для группы NULL: total посчитает сотрудников без отдела, with_dept вернёт 0
```

---

### 4. UNION, UNION ALL, INTERSECT, EXCEPT: различия

Это операторы теоретико-множественных операций над результатами двух запросов. Требования: одинаковое число колонок и совместимые типы данных в соответствующих позициях.

| Оператор | Семантика | Дубли |
|---|---|---|
| `UNION` | Объединение множеств | Удаляются |
| `UNION ALL` | Объединение мультимножеств | Сохраняются |
| `INTERSECT` | Пересечение (строки, есть в обоих запросах) | Удаляются |
| `EXCEPT` (в Oracle — `MINUS`) | Разность (строки первого запроса без строк второго) | Удаляются |

Ключевой практический момент: `UNION` выполняет дедупликацию результата (фактически скрытый `DISTINCT` — сортировка или хеширование всего набора), что на больших объёмах дорого. Если дубли заведомо невозможны или допустимы, всегда используйте `UNION ALL` — он просто конкатенирует результаты.

```sql
-- Все уникальные email из двух источников
SELECT email FROM customers
UNION
SELECT email FROM newsletter_subscribers;

-- Быстрее и без дедупликации — для статистики показов
SELECT user_id, 'click'  AS event_type FROM clicks
UNION ALL
SELECT user_id, 'view'   AS event_type FROM page_views;

-- Клиенты, которые есть и в заказах, и в рассылке
SELECT customer_id FROM orders
INTERSECT
SELECT customer_id FROM newsletter_subscribers;

-- Клиенты без заказов
SELECT id FROM customers
EXCEPT
SELECT customer_id FROM orders;
```

---

### 5. Подзапросы и CTE (WITH)

**Подзапрос (subquery)** — запрос, вложенный в другой запрос. Виды:

- **Скалярный** — возвращает одно значение, используется как выражение в `SELECT` или `WHERE`.
- **Табличный (inline view)** — возвращает набор строк, используется в `FROM`.
- **С IN / EXISTS** — проверка вхождения в множество.
- **Коррелированный** — ссылается на колонки внешнего запроса и логически выполняется заново для каждой строки внешнего запроса; это логическая модель, а не обещание отдельного физического выполнения: оптимизатор может преобразовать подзапрос. Коррелированный запрос не обязательно медленнее JOIN — сравнивают эквивалентную семантику и планы.

```sql
-- Скалярный подзапрос: сотрудники с зарплатой выше средней
SELECT full_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- EXISTS: отделы, в которых есть хотя бы один сотрудник
SELECT d.name
FROM departments d
WHERE EXISTS (SELECT 1 FROM employees e WHERE e.department_id = d.id);

-- Коррелированный подзапрос: сотрудники с зарплатой выше средней В СВОЁМ отделе
SELECT e.full_name, e.salary, e.department_id
FROM employees e
WHERE e.salary > (SELECT AVG(x.salary)
                  FROM employees x
                  WHERE x.department_id = e.department_id);
```

**CTE (Common Table Expression, `WITH`)** — именованное выражение запроса, видимое в рамках одного оператора; оно не обязательно материализуется как временная таблица. Улучшает читаемость, позволяет переиспользовать промежуточные результаты и писать рекурсивные запросы (`WITH RECURSIVE`).

```sql
WITH dept_stats AS (
    SELECT department_id,
           AVG(salary) AS avg_salary,
           COUNT(*)    AS cnt
    FROM employees
    GROUP BY department_id
)
SELECT e.full_name, e.salary, s.avg_salary
FROM employees e
JOIN dept_stats s ON s.department_id = e.department_id
WHERE e.salary > s.avg_salary;

-- Рекурсивный CTE: иерархия сотрудников (менеджеры и подчинённые)
WITH RECURSIVE org_tree AS (
    SELECT id, full_name, manager_id, 1 AS level
    FROM employees
    WHERE manager_id IS NULL          -- якорь: корневой директор
    UNION ALL
    SELECT e.id, e.full_name, e.manager_id, t.level + 1
    FROM employees e
    JOIN org_tree t ON e.manager_id = t.id
)
SELECT * FROM org_tree ORDER BY level, id;
```

В PostgreSQL 12+ нерекурсивный CTE без побочных эффектов может быть встроен в основной запрос. `MATERIALIZED`/`NOT MATERIALIZED` управляют этим выбором в поддерживаемых случаях. CTE улучшает структуру SQL, но сам по себе не гарантирует ускорения.

---

### 40. Когда EXISTS лучше JOIN и почему NOT EXISTS отличается от NOT IN?

Когда нужен ответ «есть ли хотя бы одна подходящая строка», `EXISTS` точнее выражает задачу, чем JOIN. JOIN создаёт строку результата для каждого совпадения, а EXISTS сохраняет максимум одну строку внешнего набора. Оптимизатор может выбрать semi join и прекратить поиск после первого совпадения; конкретный физический алгоритм зависит от статистики и индексов.

```sql
-- Клиенты хотя бы с одним оплаченным заказом: без DISTINCT
SELECT c.id
FROM customers c
WHERE EXISTS (
    SELECT 1 FROM orders o
    WHERE o.customer_id = c.id AND o.status = 'PAID'
);

-- Клиенты без заказов, даже если orders.customer_id содержит NULL
SELECT c.id
FROM customers c
WHERE NOT EXISTS (
    SELECT 1 FROM orders o WHERE o.customer_id = c.id
);
```

Для первого запроса полезен индекс `orders(customer_id, status)` либо подходящий частичный индекс. JOIN нужен, если результат действительно должен содержать сведения о каждом заказе. Нельзя механически менять любой JOIN на EXISTS: изменится число и состав строк.

`NOT IN (subquery)` использует сравнения с трёхзначной логикой: наличие `NULL` справа может сделать результат `UNKNOWN`. `NOT EXISTS` проверяет отсутствие строки, удовлетворяющей корреляции, поэтому этой ловушки нет. Однако внешний `NULL` тоже требует решения: при `c.code = o.code` два `NULL` не совпадут. Если бизнес считает их одинаковыми, в PostgreSQL используйте `IS NOT DISTINCT FROM`. Сначала определяют семантику отсутствующего значения, затем выбирают форму запроса.

---

### 6. Оконные функции: ROW_NUMBER, RANK, DENSE_RANK, LAG/LEAD

**Оконная функция** вычисляет значение по «окну» строк, связанных с текущей строкой, **не схлопывая строки** (в отличие от `GROUP BY`). Синтаксис: `func() OVER (PARTITION BY ... ORDER BY ...)`.

- `ROW_NUMBER()` — уникальный номер строки в окне (1, 2, 3...), даже при одинаковых значениях сортировки.
- `RANK()` — ранг с пропусками: одинаковые значения получают один ранг, следующий ранг пропускается (1, 1, 3).
- `DENSE_RANK()` — ранг без пропусков (1, 1, 2).
- `LAG(col, n)` / `LEAD(col, n)` — значение колонки из строки на `n` позиций выше/ниже текущей в окне (удобно для сравнения с предыдущим периодом).
- Также: `SUM/COUNT/AVG(col) OVER (...)`, `FIRST_VALUE`, `LAST_VALUE`, `NTILE(n)`.

```sql
-- Топ-3 сотрудника по зарплате в каждом отделе
SELECT full_name, department_id, salary
FROM (
    SELECT e.*,
           ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC, id) AS rn,
           RANK()       OVER (PARTITION BY department_id ORDER BY salary DESC, id) AS rnk,
           DENSE_RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS drnk
    FROM employees e
) t
WHERE rn <= 3;
```

Пример с `LAG`: помесячная динамика выручки.

```sql
SELECT month,
       revenue,
       LAG(revenue) OVER (ORDER BY month)            AS prev_month_revenue,
       revenue - LAG(revenue) OVER (ORDER BY month)  AS growth_abs,
       ROUND(100.0 * revenue / NULLIF(LAG(revenue) OVER (ORDER BY month), 0) - 100, 2) AS growth_pct
FROM monthly_revenue;
```

Классическое применение `ROW_NUMBER` — дедупликация данных (оставить по одной «свежей» строке на ключ):

```sql
DELETE FROM events
WHERE id IN (
    SELECT id FROM (
        SELECT id,
               ROW_NUMBER() OVER (PARTITION BY user_id, event_type
                                  ORDER BY created_at DESC, id DESC) AS rn
        FROM events
    ) t
    WHERE rn > 1
);
```

---

### 66. Как вывести всех пользователей с повторяющимся именем, сохранив их идентификаторы?

`GROUP BY name HAVING COUNT(*) > 1` вернёт строку на имя. Если службе поддержки нужен каждый пользователь, посчитайте размер группы оконной функцией и отфильтруйте результат во внешнем запросе.

```sql
SELECT id, name
FROM (
    SELECT id, name, COUNT(*) OVER (PARTITION BY name) AS namesakes
    FROM users
    WHERE name IS NOT NULL
) AS counted
WHERE namesakes > 1
ORDER BY name, id;
```

Для строк `(1, Ира), (2, Лев), (3, Ира)` вернутся идентификаторы 1 и 3. Сначала договоритесь, как поступать с неизвестным именем и разными написаниями. Например, чтобы игнорировать крайние пробелы и регистр, группируют по `lower(trim(name))`, сохранив исходное имя для вывода. Совпадения имени недостаточно для объединения учётных записей.

---

### 41. Чем ROWS отличается от RANGE и почему LAST_VALUE возвращает текущую строку?

`PARTITION BY` задаёт группу, а frame — часть группы, по которой считается оконный агрегат. В PostgreSQL при `ORDER BY` рамка по умолчанию заканчивается текущей строкой и её peers — строками с одинаковым ключом сортировки (`RANGE ... CURRENT ROW`). Поэтому `LAST_VALUE` обычно означает последнее значение текущей рамки, а не всей группы.

```sql
SELECT id, account_id, happened_at, amount,
       SUM(amount) OVER (
           PARTITION BY account_id
           ORDER BY happened_at, id
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
       ) AS running_total,
       LAST_VALUE(amount) OVER (
           PARTITION BY account_id
           ORDER BY happened_at, id
           ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
       ) AS final_amount
FROM account_events;
```

`ROWS` задаёт границы по позициям строк. `RANGE` ориентируется на значения сортировки: с `ORDER BY happened_at` события с одинаковым временем попадут в общую границу. Поэтому накопительная сумма может изменяться сразу на сумму нескольких событий, хотя читатель ожидал отдельный шаг для каждого события. Уникальный tie-breaker `id` и явный `ROWS` делают намерение понятным.

Оконный `ORDER BY` определяет вычисление, но не гарантирует порядок окончательного результата: для вывода нужен внешний `ORDER BY`. При очень больших группах сортировка и рамка могут потребовать значительной памяти или временных файлов. Типичная ошибка — лечить результат `LAST_VALUE` заменой сортировки на `DESC`, не выяснив границы рамки и то, какое событие считается последним при равных датах.

---

### 7. Нюансы работы с NULL в SQL

**NULL** — это не значение, а маркер отсутствия значения («неизвестно»). Из-за этого SQL использует **тривалентную логику (Three-Valued Logic)**: результат любого сравнения может быть `TRUE`, `FALSE` или `UNKNOWN`.

Ключевые ловушки:

1. **Обычные сравнения (`=`, `<>`, `<` и т. п.) с NULL дают UNKNOWN**, а не TRUE/FALSE. `NULL = NULL` → UNKNOWN, `NULL <> 5` → UNKNOWN. Строки с результатом UNKNOWN отфильтровываются из `WHERE`. Для проверки отсутствия значения используют `IS NULL` / `IS NOT NULL`; для null-safe сравнения двух значений в PostgreSQL — `IS [NOT] DISTINCT FROM`.

```sql
SELECT * FROM employees WHERE salary = NULL;        -- вернёт 0 строк (неправильно)
SELECT * FROM employees WHERE salary IS NULL;       -- корректно
SELECT * FROM employees WHERE department_id <> 3;   -- строки с department_id = NULL НЕ попадут в выборку!
```

2. **`NOT IN` с NULL в подзапросе возвращает пустой результат**: условие `x NOT IN (1, 2, NULL)` эквивалентно `x <> 1 AND x <> 2 AND x <> NULL` — последнее сравнение всегда UNKNOWN, поэтому ни одна строка не проходит фильтр. Безопасная альтернатива — `NOT EXISTS`.

```sql
-- Опасно: если в archived_ids есть хоть один NULL, запрос вернёт пустоту
SELECT * FROM orders WHERE id NOT IN (SELECT order_id FROM archived_orders);
-- Надёжно:
SELECT * FROM orders o
WHERE NOT EXISTS (SELECT 1 FROM archived_orders a WHERE a.order_id = o.id);
```

3. **Агрегатные функции игнорируют NULL**: `SUM`, `AVG`, `MIN`, `MAX`, `COUNT(col)` пропускают NULL-значения; только `COUNT(*)` считает все строки. Это влияет на `AVG`: среднее считается по не-NULL строкам, а не по всем.

4. **NULL в UNIQUE-ограничении**: в PostgreSQL по умолчанию разрешено несколько NULL; это правило проверки уникальности, а не результат сравнения `NULL ≠ NULL`. Начиная с PostgreSQL 15, `UNIQUE NULLS NOT DISTINCT` позволяет считать NULL одинаковыми для ограничения. Поведение между СУБД различается; стандарт не задаёт единую переносимую трактовку этого случая.

5. **Арифметика и конкатенация с NULL дают NULL**: `100 + NULL` → NULL, `'a' || NULL` → NULL (в PostgreSQL). Для подстановки значения по умолчанию используют `COALESCE(col, 0)` (первый не-NULL аргумент) или `NULLIF(a, b)` (возвращает NULL, если `a = b` — удобно для защиты от деления на ноль: `x / NULLIF(y, 0)`).

---

### 8. Ограничения целостности: PRIMARY KEY vs UNIQUE vs FOREIGN KEY

**Ограничения целостности (constraints)** — правила на уровне БД, гарантирующие корректность данных независимо от кода приложения.

| Свойство | PRIMARY KEY | UNIQUE | FOREIGN KEY |
|---|---|---|---|
| Уникальность | Да | Да | Нет |
| NULL допустим | Нет | Да (в большинстве СУБД) | Да |
| Количество на таблицу | Один | Сколько угодно | Сколько угодно |
| Индекс | Создаётся автоматически | Создаётся автоматически | PostgreSQL — нет, нужно добавлять вручную (MySQL InnoDB создаёт сам) |
| Ссылочная целостность | Может быть целью FK | Может быть целью FK | Ссылается на PK/UNIQUE другой таблицы |

**PRIMARY KEY** — основной идентификатор строки: уникален и `NOT NULL`. Рекомендуется использовать суррогатные ключи (`BIGSERIAL` / `IDENTITY`, UUID), не несущие бизнес-смысла.

**UNIQUE** — альтернативный ключ (например, email). В PostgreSQL допускает несколько NULL по умолчанию; `NULLS NOT DISTINCT` меняет эту семантику. Для обязательного бизнес-ключа всё равно нужен NOT NULL.

**FOREIGN KEY** — ссылка на PK/UNIQUE родительской таблицы. **FK может быть NULL** — это означает «связь не установлена», и такая строка не проверяется на существование в родительской таблице. Для не-NULL значений БД гарантирует, что они существуют в родителе.

Поведение при удалении/обновлении родителя задаётся правилами:

- `ON DELETE NO ACTION` — проверка ссылочной целостности (по умолчанию), которую при DEFERRABLE-ограничении можно отложить; `RESTRICT` запрещает удаление сразу и не откладывается.
- `ON DELETE CASCADE` — удалить потомков вместе с родителем.
- `ON DELETE SET NULL` — обнулить FK у потомков.
- `ON UPDATE CASCADE` — распространить изменение ключа родителя.

```sql
CREATE TABLE orders (
    id          BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    number      VARCHAR(50) NOT NULL UNIQUE,          -- бизнес-ключ
    customer_id BIGINT NOT NULL REFERENCES customers(id) ON DELETE RESTRICT,
    coupon_id   BIGINT REFERENCES coupons(id) ON DELETE SET NULL,  -- NULL допустим
    total       NUMERIC(12, 2) CHECK (total >= 0)
);
```

Дополнительно: `NOT NULL`, `CHECK`, `DEFAULT`, `EXCLUDE` (PostgreSQL) и составные ограничения: `UNIQUE (tenant_id, email)`.

---

### 42. Почему CHECK (amount > 0) допускает NULL и где проверять инварианты?

CHECK отклоняет строку, когда выражение равно `FALSE`; результат `UNKNOWN` допустим. Поэтому `CHECK (amount > 0)` без `NOT NULL` разрешает `NULL`. Проверка в Java нужна для понятного ответа пользователю, а ограничение БД защищает от остальных клиентов и конкурентных записей.

```sql
CREATE TABLE transfers (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    request_id UUID NOT NULL UNIQUE,
    amount NUMERIC(18, 2) NOT NULL CHECK (amount > 0),
    from_id BIGINT NOT NULL REFERENCES accounts(id),
    to_id BIGINT NOT NULL REFERENCES accounts(id),
    CHECK (from_id <> to_id)
);
```

Каждый вид ограничения решает свою задачу: `NOT NULL` — обязательность, `UNIQUE` — межстрочную уникальность, FK — существование связанного ключа, CHECK — условие для текущей строки. CHECK не подходит для проверки «сумма всех переводов клиента за день не превышает лимит»: результат зависит от других строк и конкурентных транзакций. Нужен другой дизайн, например атомарно обновляемая строка дневного лимита, блокировка общей строки или SERIALIZABLE с повторами.

Частая ошибка — надеяться только на `@Column(nullable = false)` при отключённой генерации схемы. Аннотация описывает mapping; фактический `NOT NULL` должен присутствовать в миграции. При нарушении ограничения приложение разбирает тип ошибки и имя известного constraint, а не произвольный текст драйвера. Не всякая `DataIntegrityViolationException` означает «такой email уже существует»: это может быть FK, NULL или CHECK.

---

### 10. SELECT * против явного списка колонок; count(*) против count(column)

**SELECT * в промышленном коде — плохая привычка**, хотя в ad-hoc запросах она удобна. Причины конкретные, не эстетические:

1. **Лишний трафик и I/O.** Тащатся все колонки, включая тяжёлые (`text`, `jsonb`, BLOB). Если из 20 колонок нужны 3, остальные создают лишнюю работу; её доля зависит от ширины полей, а не от числа колонок. На ORM это ещё и гидратация ненужных полей в объекты.
2. **Ломает покрывающие индексы.** Со списком колонок запрос может выполниться как Index-Only Scan (см. вопрос 9) вообще без чтения таблицы. `SELECT *` обычно мешает покрытию, если индекс не содержит все выбранные колонки; абсолютного запрета на index-only scan здесь нет.
3. **Хрупкость для потребителей.** Порядок и состав колонок в `*` не закреплён: добавление колонки в таблицу молча меняет результат. Код, читающий результат позиционно (`rs.getString(2)`, маппинг по индексу), ломается без единой ошибки компиляции.
4. **Неявность.** По запросу `SELECT *` не видно, что реально нужно вызывающему коду, — рефакторить схему и оценивать нагрузку сложнее.

Вьюхи и подзапросы с `*` внутри — та же проблема плюс риск конфликта имён при JOIN. Легитимное место для `*` — `EXISTS (SELECT * ...)` (СУБД всё равно игнорирует список) и исследовательские запросы руками.

**count(*) / count(1) / count(column) — классический вопрос-ловушка:**

- `COUNT(*)` — считает **строки**, включая строки с NULL во всех колонках.
- `COUNT(1)` — то же самое, что `COUNT(*)`: константа не NULL, значит считаются все строки. Семантика совпадает; обещание «count(1) быстрее» без плана и измерения необоснованно. Реализация вычисления может различаться.
- `COUNT(column)` — считает только строки, где колонка **не NULL**. На колонке с NULL-ами результат будет меньше, чем `COUNT(*)`.
- `COUNT(DISTINCT column)` — уникальные не-NULL значения.

```sql
SELECT COUNT(*),            -- 5 строк всего
       COUNT(middle_name),  -- 3 (у двух сотрудников NULL)
       COUNT(DISTINCT dept) -- 2 отдела
FROM employees;
```

Нюанс для follow-up: `COUNT(*)` без `WHERE` на большой таблице — в PostgreSQL требует проверки видимых строк через таблицу или подходящий индекс. Мгновенного точного счётчика всех MVCC-видимых строк нет. Для «примерно сколько строк» используют статистику: в PostgreSQL `SELECT reltuples FROM pg_class WHERE relname = 'orders'`.

---

### 11. Нормализация (1NF, 2NF, 3NF) и денормализация

**Нормализация** — процесс организации схемы для устранения избыточности данных и аномалий вставки/обновления/удаления.

Исходная денормализованная таблица заказов:

| order_id | customer | phones | product | product_price | category | category_discount |
|---|---|---|---|---|---|---|
| 1 | Иван | 111, 222 | Мышь | 1000 | Электроника | 5% |
| 1 | Иван | 111, 222 | Кабель | 500 | Электроника | 5% |
| 2 | Иван | 111, 222 | Кружка | 300 | Дом | 0% |

**1NF (первая нормальная форма)**: каждая ячейка содержит атомарное (неделимое) значение, нет повторяющихся групп. Нарушение выше: `phones = '111, 222'` — список в одной ячейке; несколько телефонов записаны текстом без отдельной структуры. Несколько строк с одним order_id сами по себе 1NF не нарушают. Решение: одна строка = одна позиция заказа, телефоны — в отдельной таблице.

**2NF (вторая нормальная форма)**: таблица в 1NF и каждый неключевой атрибут зависит от **всего составного ключа**, а не от его части. В строке `(order_id, product)` атрибут `customer` зависит только от `order_id`, а `product_price` — только от `product`. Решение: разнести на `orders`, `products`, `order_items`.

**3NF (третья нормальная форма)**: таблица в 2NF и нет **транзитивных зависимостей** — неключевой атрибут не зависит от другого неключевого атрибута. `category_discount` зависит от `category`, а не от товара. Решение: таблица `categories(category, discount)`.

Итоговая нормализованная схема:

```sql
CREATE TABLE customers (id BIGINT PRIMARY KEY, name VARCHAR(200));
CREATE TABLE customer_phones (customer_id BIGINT REFERENCES customers(id), phone VARCHAR(20));
CREATE TABLE categories (name VARCHAR(100) PRIMARY KEY, discount NUMERIC(4,2));
CREATE TABLE products (
    id BIGINT PRIMARY KEY,
    name VARCHAR(200),
    price NUMERIC(12,2),
    category VARCHAR(100) REFERENCES categories(name)
);
CREATE TABLE orders (id BIGINT PRIMARY KEY, customer_id BIGINT REFERENCES customers(id));
CREATE TABLE order_items (
    order_id BIGINT REFERENCES orders(id),
    product_id BIGINT REFERENCES products(id),
    quantity INT,
    PRIMARY KEY (order_id, product_id)
);
```

**Денормализация** — осознанное внесение избыточности для ускорения чтения (например, хранить `total_amount` в заказе вместо пересчёта по позициям, или дублировать `customer_name` в таблице заказов для отчётов). Применяется в аналитических системах и «горячих» read-heavy участках. Плата — сложность поддержания консистентности (триггеры, приложение, материализованные представления).

---

### 61. Отчёт по продажам нужен каждую минуту. Когда пригодится материализованное представление?

Обычное представление (`VIEW`) хранит определение запроса. При чтении PostgreSQL обращается к исходным данным по правилам видимости текущей транзакции. Материализованное представление хранит уже вычисленные строки. Новая продажа появляется в таком отчёте только после `REFRESH MATERIALIZED VIEW`.

Если отчёт строится 40 секунд, а менеджеры допускают отставание на пять минут, материализация может разгрузить базу. Подбирайте расписание с учётом длительности обновления. Покажите пользователю, до какого момента учтены данные. Если обновление сломалось, старое значение всё ещё может выглядеть правдоподобно.

Если в отчёте сразу после подтверждения оплаты должна быть видна эта оплата, рассмотрите запрос к исходным таблицам или обновление агрегата в той же транзакции. Сравните стоимость чтения, дополнительную работу при записи и допустимое отставание.

---

### 12. Хранимые процедуры и триггеры

**Хранимая процедура (stored procedure)** — именованный блок кода, скомпилированный и хранящийся в БД, выполняемый на стороне сервера.

```sql
CREATE OR REPLACE PROCEDURE transfer_money(
    p_from BIGINT, p_to BIGINT, p_amount NUMERIC
)
LANGUAGE plpgsql AS $$
BEGIN
    UPDATE accounts SET balance = balance - p_amount WHERE id = p_from;
    IF (SELECT balance FROM accounts WHERE id = p_from) < 0 THEN
        RAISE EXCEPTION 'Insufficient funds';
    END IF;
    UPDATE accounts SET balance = balance + p_amount WHERE id = p_to;
END;
$$;

CALL transfer_money(1, 2, 500);
```

Плюсы процедур: сокращение сетевых round-trip'ов (сложная логика выполняется одним вызовом), переиспользование и централизация бизнес-правил, дополнительный уровень безопасности (доступ только через процедуры, без прямого доступа к таблицам).

Минусы: бизнес-логика «уплывает» из приложения в БД (сложнее версионировать, тестировать и ревьюить), привязка к диалекту конкретной СУБД, сложности с горизонтальным масштабированием (БД становится узким местом), трудности с CI/CD.

**Триггер (trigger)** — процедура, автоматически выполняемая при событиях `INSERT/UPDATE/DELETE` на таблице (`BEFORE`/`AFTER`, `FOR EACH ROW` или `FOR EACH STATEMENT`).

```sql
CREATE OR REPLACE FUNCTION log_salary_change() RETURNS trigger AS $$
BEGIN
    IF NEW.salary IS DISTINCT FROM OLD.salary THEN
        INSERT INTO salary_audit(employee_id, old_salary, new_salary, changed_at)
        VALUES (OLD.id, OLD.salary, NEW.salary, now());
    END IF;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_salary_audit
BEFORE UPDATE ON employees
FOR EACH ROW EXECUTE FUNCTION log_salary_change();
```

Типовые применения триггеров: аудит изменений, поддержание денормализованных счётчиков, сложные проверки целостности, недоступные для CHECK. Минусы: «магическое» скрытое поведение (сложно отлаживать), каскады триггеров могут неожиданно удлинять транзакции, порядок срабатывания нескольких триггеров неочевиден.

---

### 35. DELETE vs TRUNCATE vs DROP: механика, откат, identity-счётчики

Три способа «убрать данные», работающие принципиально по-разному:

- `DELETE FROM t [WHERE ...]` — DML: удаляет строки по одной. В PostgreSQL каждая удалённая строка помечается как удалённая для MVCC (см. вопрос 17) и физически остаётся в таблице до vacuum — после массового DELETE файл обычно не уменьшается: растёт доля места, занятого устаревшими версиями (bloat). Каждая строка пишется в WAL, срабатывают row-триггеры, проверяются FK.
- `TRUNCATE t` — не сканирует строки, а сбрасывает файлы данных целиком. Почти мгновенно даже на сотнях миллионов строк, минимум WAL, нет bloat и шторма autovacuum. Row-триггеры DELETE не срабатывают (в PostgreSQL есть отдельные TRUNCATE-триггеры уровня statement). Ограничение: на таблицу не должны ссылаться внешние ключи других таблиц — иначе ошибка, нужен `TRUNCATE ... CASCADE` либо перечислить связанные таблицы в одном TRUNCATE. Пустота дочерней таблицы сама по себе не отменяет существующий FK.
- `DROP TABLE t` — удаляет сам объект: данные, индексы, ограничения, триггеры, гранты. Таблицы после этого просто нет.

**Транзакционность** — частая ловушка. В PostgreSQL все три операции транзакционны: `TRUNCATE` и даже `DROP TABLE` можно откатить вместе с транзакцией (TRUNCATE/DROP требуют сильной блокировки ACCESS EXCLUSIVE). В MySQL `TRUNCATE` и `DROP` — DDL с неявным коммитом: откатить их нельзя, и они заодно фиксируют вашу текущую транзакцию.

**Identity-счётчики.** `DELETE` не трогает sequence/auto-increment: следующие id продолжатся с того места, где остановились. `TRUNCATE` в PostgreSQL по умолчанию тоже не сбрасывает счётчик — нужен явный `TRUNCATE t RESTART IDENTITY`; в MySQL TRUNCATE сбрасывает AUTO_INCREMENT всегда (по сути это drop + create таблицы). `DROP` удаляет принадлежащую колонке последовательность (OWNED BY); независимая sequence может сохраниться.

**Права.** Для `DELETE` достаточно привилегии DELETE на таблицу; `TRUNCATE` требует права TRUNCATE или владения таблицей, `DROP` — владельца. Поэтому приложению права на TRUNCATE/DROP обычно не выдают — это инструмент миграций и административных скриптов.

Когда что выбирать: точечное удаление — DELETE; «очистить staging-таблицу перед загрузкой» — TRUNCATE; убрать объект из схемы — DROP. Отдельное правило: массовое удаление старых данных большими DELETE на живой таблице — антипаттерн (bloat, долгий vacuum, лишний WAL); для удаления целых временных диапазонов часто выгодно заранее продуманное партиционирование (вопрос 27); в остальных случаях уместен порционный DELETE с контролем WAL и vacuum.

## Индексы, планы и постраничное чтение

### 9. Индексы: B-Tree, Hash, составные и покрывающие индексы

**Индекс** — отдельная структура данных, ускоряющая поиск ценой дополнительного места и замедления записи (вставки и изменения индексируемых данных требуют обслуживания индексов; детали зависят от СУБД, например PostgreSQL HOT может избежать новых индексных записей при части UPDATE).

**B-Tree** (сбалансированное дерево) — тип индекса по умолчанию в PostgreSQL, MySQL, Oracle. Поддерживает поиск по равенству, диапазоны (`>`, `<`, `BETWEEN`), сортировку (`ORDER BY`), префиксный поиск `LIKE 'abc%'` при подходящих collation/operator class. Высота дерева обычно 3–4 уровня, поиск — O(log N).

**Hash** — поддерживает только поиск по точному равенству (`=`), с ожидаемой, а не безусловно гарантированной константной стоимостью поиска по хешу. Не подходит для диапазонов и сортировки. В PostgreSQL используется редко, в MySQL — в MEMORY-таблицах и адаптивном хеш-индексе InnoDB.

Также существуют специализированные индексы: **GIN/GiST** (полнотекстовый поиск, JSONB, массивы), **BRIN** (очень большие упорядоченные таблицы), **Bitmap** (низкая кардинальность, аналитика).

**Составной B-tree индекс** `(a, b, c)` обычно эффективнее при ограничении ведущих колонок: равенства по первым ключам и диапазон по следующему сужают просматриваемую область. Условия только по `b`/`c` не делают использование невозможным, но могут потребовать сканирования значительной части индекса. PostgreSQL 18 добавил B-tree skip scan; на более ранних версиях нельзя рассчитывать на эту оптимизацию. Порядок выбирают под WHERE, ORDER BY и LIMIT, а не только под кардинальность колонок (практика — вопрос 43).

**Покрывающий индекс (covering index)** содержит все колонки запроса, что делает **Index-Only Scan** возможным. В PostgreSQL отсутствие heap fetches дополнительно зависит от visibility map:

```sql
CREATE INDEX idx_orders_customer ON orders(customer_id, status, created_at);

-- Кандидат на Index-Only Scan: все нужные колонки есть в индексе
SELECT customer_id, created_at
FROM orders
WHERE customer_id = 42 AND status = 'PAID';
-- В PostgreSQL покрытие можно расширить: CREATE INDEX ... ON orders(customer_id) INCLUDE (total);
```

**Когда обычный индекс может не помочь:**

- **Функция или выражение над колонкой**: `WHERE UPPER(email) = 'A@B.RU'`, `WHERE created_at::date = '2024-01-01'`, `WHERE salary * 12 > 1000000`. Решение — индекс на нужном выражении (`CREATE INDEX ON employees (UPPER(email))`) либо эквивалентный диапазон по исходной колонке. Например, вместо cast даты — `created_at >= TIMESTAMP '2024-01-01' AND created_at < TIMESTAMP '2024-01-02'`. При TIMESTAMPTZ границы задают в нужном часовом поясе; арифметические преобразования проверяют на округление и переполнение.
- **Leading wildcard**: `LIKE '%abc'` — нет префикса для спуска по дереву (работает только `LIKE 'abc%'`).
- **Низкая селективность**: если условие возвращает большую долю таблицы (универсального процента нет: важны стоимость I/O, корреляция с физическим порядком, кэш и ширина строк), оптимизатор осознанно выбирает Seq Scan — он дешевле случайных чтений.
- **Несовпадение типов**: сравнение `varchar`-колонки с числом может привести к неявному касту и отказу от индекса.
- **Небольшие таблицы**: читать таблицу целиком быстрее.
- **Устаревшая статистика**: оптимизатор принимает решения на основе статистики (`ANALYZE`).

---

### 43. Как выбрать порядок колонок индекса под фильтр, сортировку и LIMIT?

Начинайте с конкретного запроса, а не с правила «самое селективное поле первым». Для списка последних заказов клиента равенство по клиенту фиксирует первый сегмент дерева, а следующие ключи дают нужный порядок без отдельной сортировки.

```sql
CREATE INDEX orders_customer_feed_idx
    ON orders(customer_id, created_at DESC, id DESC);

SELECT id, created_at
FROM orders
WHERE customer_id = 42
ORDER BY created_at DESC, id DESC
LIMIT 30;
```

Индекс `(created_at, customer_id)` удобен для другого запроса — общего диапазона дат. Для клиента с небольшим числом заказов он может заставить просмотреть множество чужих строк, прежде чем найти 30 подходящих. Если дополнительно нужен `status = 'PAID'`, варианты — `(customer_id, status, created_at DESC, id DESC)` или частичный индекс для PAID. Выбор зависит и от остальных запросов: без фильтра по status первый вариант уже не даёт один непрерывный порядок дат для клиента.

Два отдельных индекса не эквивалентны составному. PostgreSQL может объединить их через bitmap, но при этом теряется порядок B-tree и может понадобиться Sort. Составной индекс тоже не бесплатен: занимает место, влияет на запись и обслуживание.

Условие только по неведущей колонке не означает абсолютного запрета на использование индекса; важен объём сканирования. В PostgreSQL 18 появился B-tree skip scan. Его наличие не заменяет проверку плана на целевой версии. Сравнивайте фактические buffers и число просмотренных строк при реалистичном распределении клиентов.

---

### 65. Как найти сотрудников, принятых за календарную неделю, если время хранится с часовым поясом?

Уточните, чья это календарная неделя. Для PostgreSQL-колонки `hired_at timestamptz` вычислите два момента времени в зоне офиса — начало понедельника и начало следующего понедельника. Передайте их в параметрах соответствующего типа.

```sql
SELECT id, hired_at
FROM employees
WHERE hired_at >= :week_start AND hired_at < :next_week_start;
```

Полуоткрытый интервал включает начало недели, а полночь следующего понедельника оставляет следующей выборке. Не нужно угадывать последнюю долю секунды. В зоне с переводом часов между локальными границами не всегда ровно 168 часов. Следующую границу вычисляют календарной операцией в этой зоне.

Сравнение исходной колонки с границами позволяет использовать обычный B-tree по `hired_at`. Если запрос возвращает большую долю таблицы, последовательное чтение может оказаться дешевле. Проверьте фактический план и число прочитанных страниц.

---

### 44. Когда нужны частичный и функциональный индексы и почему параметр мешает плану?

Частичный индекс хранит только строки, удовлетворяющие предикату. Он полезен, когда рабочий набор мал по сравнению с историей: например, 1% задач остаются PENDING. Функциональный индекс хранит результат выражения, по которому реально ищут.

```sql
CREATE INDEX jobs_pending_idx ON jobs(id)
    WHERE status = 'PENDING';

CREATE UNIQUE INDEX users_active_email_idx
    ON users(lower(email)) WHERE deleted_at IS NULL;

SELECT id FROM jobs
WHERE status = 'PENDING' ORDER BY id LIMIT 20;

SELECT id FROM users
WHERE deleted_at IS NULL AND lower(email) = lower('User@Example.org');
```

Второй индекс одновременно ускоряет поиск и запрещает два одинаковых в нижнем регистре email среди активных пользователей. Это ещё не полная универсальная нормализация email: правила регистра, collation и продуктовую семантику нужно согласовать отдельно.

Для частичного индекса планировщик должен доказать, что условие запроса подразумевает предикат индекса. Generic plan для `WHERE status = $1` не знает значение параметра и обычно не может использовать индекс только для PENDING. Custom plan при известном параметре может вести себя иначе. Поэтому тест с SQL-литералом не всегда воспроизводит поведение prepared statement приложения.

Условия и выражения индекса имеют ограничения: нельзя сделать движущийся индекс `WHERE created_at > now() - interval '7 days'`, потому что время меняется без обновления строк. Обычный индекс по времени или партиционирование решают другую задачу. Типичная ошибка — создать десятки частичных индексов по каждому статусу без оценки размера рабочего набора и стоимости записи.

---

### 13. EXPLAIN и план выполнения запроса

**EXPLAIN** показывает план, который оптимизатор СУБД выбрал для выполнения запроса: как читаются таблицы, какие индексы и методы соединения используются, сколько строк ожидается на каждом шаге и оценочная стоимость (cost).

```sql
EXPLAIN (ANALYZE, BUFFERS)
SELECT o.id, o.total
FROM orders o
WHERE o.customer_id = 42
ORDER BY o.created_at DESC
LIMIT 10;
```

- `EXPLAIN` — только оценочный план (запрос не выполняется).
- `EXPLAIN ANALYZE` — реально выполняет запрос и показывает фактические времена и число строк (осторожно с DML — изменения применятся; для исследования изменений используйте тестовое окружение; rollback не отменяет все внешние эффекты, работу sequence и стоимость блокировок/WAL).

Ключевые узлы плана (PostgreSQL):

- **Seq Scan** — последовательное чтение всей таблицы. Нормально для маленьких таблиц и низкоселективных условий; тревожный сигнал на большой таблице с селективным фильтром (вероятно, нет индекса).
- **Index Scan** — поиск по индексу + чтение нужных страниц таблицы (heap fetches). Эффективен при высокой селективности.
- **Index Only Scan** — значения берутся из покрывающего индекса; в PostgreSQL проверки видимости всё ещё могут требовать чтения heap. Смотрите `Heap Fetches`.
- **Bitmap Heap Scan + Bitmap Index Scan** — компромисс: по индексу строится битовая карта страниц, затем таблица читается последовательно. Типичен при средней селективности.
- **Методы соединения**: **Nested Loop** (для малого внешнего набора + индекс по внутреннему), **Hash Join** (для больших несортированных наборов), **Merge Join** (для предварительно отсортированных наборов).

Как искать узкие места:

1. Сравнить **estimated rows vs actual rows** — расхождение на порядки означает устаревшую статистику (`ANALYZE table`) или сложную для оценки конструкцию.
2. Найти самые «горячие» узлы по **actual time** в `EXPLAIN ANALYZE`.
3. Смотреть **Buffers** (`shared hit/read`): много `read` — данные не в кэше; большой `hit` при малых строках — лишние чтения.
4. Проверить `Rows Removed by Filter` — большое значение указывает на отсутствие подходящего индекса.
5. Проверить сортировки: `Sort Method: external merge Disk` означает, что сортировка не влезла в `work_mem` и ушла на диск.

---

### 45. На подумать: в плане actual rows = 2, loops = 100000. Почему запрос медленный?

В PostgreSQL внутренний узел Nested Loop показывает `rows=1` в оценке и `actual rows=2 loops=100000` в факте. Внешний узел должен был вернуть 100 строк, но вернул 100000. Внутренний поиск занимает в среднем 0,05 мс. Коллега предлагает игнорировать узел: «он читает всего две строки». Что проверить до добавления индекса?

<details><summary>Разбор</summary>

`actual rows` и время узла с несколькими loops приводятся в среднем на один запуск. За все запуски внутренний узел вернул примерно 200000 строк, а суммарное время его выполнения порядка 5 секунд. Указанные числа — учебные, не результат измерения конкретной БД.

Сначала найдите самое раннее значительное расхождение cardinality: внешний набор недооценён в 1000 раз. Nested Loop мог быть разумным для 100 строк и дорогим для 100000. Проверьте статистику после массовой загрузки, перекос популярных значений, коррелированные фильтры и условия JOIN. Затем сравните buffers, фактический объём результата и альтернативы запроса.

```sql
EXPLAIN (ANALYZE, BUFFERS, TIMING OFF)
SELECT o.id, i.id
FROM orders o JOIN order_items i ON i.order_id = o.id
WHERE o.tenant_id = 42 AND o.status = 'OPEN';
```

`TIMING OFF` уменьшает накладные расходы измерения узлов, сохраняя фактические counts и общее время. Cost — условные единицы планировщика, их нельзя сравнивать с миллисекундами как одинаковые величины. Время родителя включает дочерние узлы, поэтому простое суммирование всех `actual time` завышает итог; параллельные планы требуют дополнительного внимания к workers.

Опасный ответ — «Nested Loop всегда плох, выключим его глобально». Он маскирует неверную оценку и ломает хорошие планы небольших запросов. Любую настройку проверяют на целевой нагрузке.

</details>

---

### 46. Почему ANALYZE не всегда исправляет ошибочную оценку числа строк?

`ANALYZE` собирает выборочную статистику, а не знание всех комбинаций данных. Распределение отдельной колонки может быть известно хорошо, но два условия коррелируют: например, почти все заказы конкретного tenant находятся в одном статусе. Перемножение независимых селективностей даст неверную оценку даже после свежего ANALYZE.

```sql
CREATE STATISTICS orders_tenant_status_stats
    (dependencies, mcv)
    ON tenant_id, status FROM orders;
ANALYZE orders;

EXPLAIN
SELECT id FROM orders
WHERE tenant_id = 42 AND status = 'OPEN';
```

Extended statistics помогают планировщику учитывать зависимости и частые комбинации колонок одной таблицы. Это не универсальное лечение: они применяются к поддерживаемым типам условий и не решают произвольную оценку соединений между таблицами. Для сильного перекоса отдельной колонки иногда полезно повысить её statistics target, но сбор и хранение статистики тоже имеют цену.

Диагностика начинается с проверки актуальности статистики, значений параметров и первой точки расхождения плана с фактом. Хороший план для клиента с десятью заказами может оказаться плохим для клиента с миллионами. Сравните несколько характерных значений и поведение prepared statements, где выбор generic/custom plan может отличаться от ручного запроса с литералом.

Частая ошибка — запускать ANALYZE после каждого INSERT или добавлять статистику на все сочетания полей. Нужны доказанная ошибка оценивания, понятный кандидат на корреляцию и повторное измерение. Иногда запрос просто возвращает слишком много данных: более точная оценка не отменит стоимость передачи и обработки результата.

---

### 26. Пагинация: LIMIT/OFFSET против keyset (cursor) pagination

**OFFSET-пагинация** заставляет БД физически прочитать и отбросить N первых строк — на больших смещениях запрос деградирует линейно. **Keyset-пагинация** вместо смещения запоминает значение ключа сортировки последней строки страницы и начинает следующую сразу после него — стоимость не растёт только из-за номера страницы при подходящем индексе и селективных фильтрах; это не гарантия постоянной latency.

Классический вариант:

```sql
SELECT * FROM orders
ORDER BY created_at, id
LIMIT 20 OFFSET 100000;
```

БД идёт по индексу (или сортирует) и пропускает 100 000 строк: работа выполняется, но результат выбрасывается. Пятьсотая страница стоит в сотни раз дороже первой. Вторая проблема — нестабильность: пока пользователь листает, вставки сдвигают окно, и записи дублируются или пропадают между страницами.

Keyset-вариант:

```sql
-- первая страница
SELECT * FROM orders
ORDER BY created_at, id
LIMIT 20;

-- следующая: клиент передаёт (created_at, id) последней строки предыдущей страницы
SELECT * FROM orders
WHERE (created_at, id) > ('2026-07-20 14:03:11', 481523)
ORDER BY created_at, id
LIMIT 20;
```

При индексе `(created_at, id)` это обычный index scan с seek, и нет необходимости последовательно пропускать все предыдущие страницы; реальные затраты зависят от фильтров, кэша и плана. Обязательное условие — детерминированная сортировка: ключ должен быть уникальным, поэтому к неуникальному полю добавляют `id`.

Ограничения keyset: нельзя прыгнуть на произвольную страницу N, только «вперёд/назад»; многосортировочные кейсы с разным направлением сортировки писать неудобно. Частый follow-up: что выбрать для API с «бесконечной лентой» — keyset; для админки с номерами страниц — OFFSET допустим, смещения там маленькие.

---

### 47. На подумать: как сделать курсор для ORDER BY score DESC, id ASC?

Лента сортируется по `score DESC, id ASC`; score и id — NOT NULL, id уникален. Последняя строка страницы имеет `(score=100, id=42)`. Почему `WHERE (score, id) < (100, 42)` пропускает часть строк? Будет ли правильный курсор гарантировать неизменный список при обновлении score?

<details><summary>Разбор</summary>

Лексикографическое сравнение кортежа использует одно направление для всех компонентов. В смешанной сортировке нужны более низкие score либо тот же score и больший id:

```sql
SELECT id, score
FROM posts
WHERE score < :last_score
   OR (score = :last_score AND id > :last_id)
ORDER BY score DESC, id ASC
LIMIT 21;
```

Для страницы из 20 строк двадцать первая служит признаком `hasNext`; курсор строится по последней реально возвращённой строке. Индекс `(score DESC, id ASC)` соответствует порядку. Сам факт наличия индекса ещё не гарантирует одинаковое время: смотрите план для OR-условия, фильтры доступа и распределение score.

Keyset защищает от сдвига OFFSET при вставках, но не создаёт снимок на всю сессию пользователя. Если уже показанная запись потеряет score, она может оказаться за курсором и появиться повторно; выросший score у непоказанной записи может перенести её перед курсором. Нужны продуктовые гарантии: неизменный ключ публикации, зафиксированный набор/версия рейтинга либо допустимость изменений живой ленты.

Если ключ nullable, требуется явно определить `NULLS FIRST/LAST` и переход между областями; обычное сравнение с NULL даст UNKNOWN. Курсор должен закреплять сортировку и фильтры, а tenant и доступ проверяются сервером на каждом запросе. Опасный ответ — «закодируем OFFSET в Base64»: это меняет формат API, но сохраняет стоимость и нестабильность OFFSET.

</details>

## Транзакции, конкуренция и сохранность данных

### 14. Принципы ACID

**ACID** — набор гарантий, которым должна удовлетворять транзакция (логическая единица работы, объединяющая несколько операций).

**A — Atomicity (Атомарность)**: транзакция либо выполняется целиком, либо не выполняется вовсе. При сбое или `ROLLBACK` все частичные изменения откатываются. Пример: перевод денег — списание со счёта А и зачисление на счёт B должны произойти вместе; недопустимо состояние, когда деньги списаны, но не зачислены. Механизм зависит от СУБД: PostgreSQL использует состояния транзакций, MVCC и WAL; не следует приписывать ему ту же undo-архитектуру, что у InnoDB.

```sql
BEGIN;
UPDATE accounts SET balance = balance - 1000 WHERE id = 1;
UPDATE accounts SET balance = balance + 1000 WHERE id = 2;
COMMIT; -- обе операции зафиксированы; при ошибке на любом шаге — ROLLBACK обеих
```

**C — Consistency (Согласованность)**: транзакция переводит БД из одного корректного состояния в другое корректное, не нарушая ограничения целостности (constraints, триггеры, каскады). Бизнес-инварианты, не выраженные ограничениями, остаются ответственностью приложения и выбранного протокола конкуренции. Например, невозможно зафиксировать заказ, ссылающийся на несуществующего клиента, или отрицательный остаток при наличии `CHECK (balance >= 0)`.

**I — Isolation (Изолированность)**: определяет допустимые наблюдения и конфликты параллельных транзакций. SERIALIZABLE даёт результат, эквивалентный некоторому последовательному порядку; более слабые уровни допускают аномалии. Одного запрета грязного чтения недостаточно для serializable. Выбор уровня связан с бизнес-инвариантами и стоимостью координации (см. следующий вопрос).

**D — Durability (Долговечность)**: после `COMMIT` изменения не будут потеряны даже при сбое питания или крахе сервера. Реализуется через **WAL (Write-Ahead Log)**: журнал должен попасть в устойчивое хранилище раньше соответствующих изменённых страниц данных; сами страницы могут изменяться в памяти до fsync WAL. После сбоя БД восстанавливается по журналу (redo). Гарантии подтверждения зависят от настроек durability и исправности хранилища.

Практические замечания: `COMMIT` может быть медленным из-за fsync (настраивается `synchronous_commit`); атомарность распространяется и на DDL в PostgreSQL (транзакционный DDL), чего нет, например, в MySQL.

Частый follow-up — противопоставление **BASE** (Basically Available, Soft state, Eventually consistent): модель распределённых систем (NoSQL, микросервисы), жертвующая немедленной согласованностью ради доступности — данные между узлами сходятся со временем. ACID — про гарантии транзакций (в том числе возможных распределённых), BASE — описание подхода с ослабленной немедленной согласованностью (теорема CAP: при сетевом разделении выбирают между консистентностью и доступностью).

---

### 15. Уровни изоляции транзакций и аномалии чтения

При одновременном выполнении транзакций возможны аномалии:

- **Dirty Read (грязное чтение)** — транзакция читает изменения другой, ещё не зафиксированной транзакции. Если та откатится, прочитаны данные, которых «никогда не было».
- **Non-Repeatable Read (неповторяющееся чтение)** — повторное чтение той же строки внутри транзакции возвращает другое значение: между чтениями другая транзакция обновила и зафиксировала эту строку.
- **Phantom Read (фантомное чтение)** — повторный запрос с тем же `WHERE` возвращает другой набор строк: другая транзакция добавила/удалила строки, попадающие под условие.

Стандарт SQL определяет четыре уровня изоляции:

| Уровень | Dirty Read | Non-Repeatable Read | Phantom Read |
|---|:---:|:---:|:---:|
| READ UNCOMMITTED | Допустимо* | Допустимо | Допустимо |
| READ COMMITTED | Нет | Допустимо | Допустимо |
| REPEATABLE READ | Нет | Нет | Допустимо* |
| SERIALIZABLE | Нет | Нет | Нет |

\* Особенности PostgreSQL:

- **READ UNCOMMITTED** в PostgreSQL фактически работает как READ COMMITTED — грязное чтение невозможно ни на одном уровне (MVCC никогда не показывает незафиксированные данные).
- **REPEATABLE READ** в PostgreSQL также исключает фантомные чтения (snapshot на всю транзакцию). В InnoDB обычные consistent reads используют снимок, а locking reads/UPDATE опираются на блокировки, включая next-key locks в соответствующих случаях. Нельзя описывать все чтения одной фразой про gap locks.
- **READ COMMITTED** — уровень по умолчанию в PostgreSQL: каждый *оператор* видит снимок данных на момент своего начала. В REPEATABLE READ снимок фиксируется при первом операторе, которому нужен снимок, а не обязательно при самом BEGIN; собственные изменения транзакция видит. В MySQL (InnoDB) умолчание другое — REPEATABLE READ; на собеседовании это любят спрашивать именно в связке «PostgreSQL vs MySQL».
- **SERIALIZABLE** в PostgreSQL реализован через SSI (Serializable Snapshot Isolation): добавленная проверка зависимостей чтения/записи не блокирует по предикатам как традиционная 2PL, но обычные блокировки записи остаются. При обнаружении опасной структуры одна из транзакций откатывается с ошибкой `could not serialize access` — приложение должно быть готово к повторным попыткам (retry).

Пример установки уровня:

```sql
BEGIN ISOLATION LEVEL REPEATABLE READ;
SELECT balance FROM accounts WHERE id = 1;  -- 1000
-- В это время другая транзакция обновила balance на 500 и зафиксировала
SELECT balance FROM accounts WHERE id = 1;  -- всё ещё 1000 (снимок не изменился)
COMMIT;
```

Дополнительная аномалия, не покрытая таблицей, — **Lost Update (потерянное обновление)**: две транзакции читают одно значение, обе пишут на его основе, и одно обновление затирается. На уровне READ COMMITTED оно возможно, поэтому используют атомарные операции (`UPDATE ... SET x = x + 1`), `SELECT ... FOR UPDATE` или оптимистичные блокировки.

---

### 48. На подумать: почему REPEATABLE READ не спасает двух дежурных врачей?

В PostgreSQL два врача находятся на дежурстве. Каждая транзакция на REPEATABLE READ проверяет `COUNT(*) WHERE on_call = true`, получает 2 и снимает с дежурства только своего врача. Обе фиксируются. Нарушено правило «хотя бы один дежурный». Фантомов и обновления одной строки двумя транзакциями нет. Где ошибка?

<details><summary>Разбор</summary>

Это write skew: два решения опираются на один старый снимок, но меняют разные строки. Стабильность снимка не означает serializable. `@Version` только в строке каждого врача тоже не обнаружит конфликт: версии изменяются у разных объектов.

Один вариант — PostgreSQL SERIALIZABLE с повтором всей транзакции при `40001`. Другой — общая строка отделения, которую все операции смены дежурства блокируют перед проверкой:

```sql
BEGIN;
SELECT id FROM departments WHERE id = :department_id FOR UPDATE;
SELECT COUNT(*) FROM doctors
WHERE department_id = :department_id AND on_call;
-- Приложение снимает врача только если дежурных больше одного
UPDATE doctors SET on_call = false WHERE id = :doctor_id;
COMMIT;
```

Для этого протокола используйте READ COMMITTED: после получения блокировки отдельный SELECT получает свежий снимок. Простая блокировка без изменения общей строки на REPEATABLE READ не обновляет уже созданный снимок и не даёт такой гарантии. Все пути изменения должны соблюдать один протокол; обходной административный UPDATE нарушит инвариант.

Общая блокировка сериализует изменения одного отделения и уменьшает параллелизм, но обычно понятнее сложной схемы предикатных проверок. Опасный ответ — «возьмём FOR UPDATE только на своей строке»: конфликтующие решения всё ещё не имеют общего блокируемого ресурса. Также нельзя обещать, что SERIALIZABLE выполнит обе операции успешно: корректный исход может включать откат и повтор.

</details>

---

### 16. Блокировки в БД: оптимистичная и пессимистичная модели, deadlock

**Пессимистичная блокировка** исходит из того, что конфликт вероятен: ресурс блокируется заранее, и конкуренты ждут.

```sql
BEGIN;
SELECT balance FROM accounts WHERE id = 1 FOR UPDATE;  -- эксклюзивная блокировка строки
-- Другие транзакции, пытающиеся заблокировать/изменить эту строку, будут ждать
UPDATE accounts SET balance = balance - 1000 WHERE id = 1;
COMMIT;  -- блокировка снята
```

#### Практика SELECT FOR UPDATE

`FOR UPDATE` берёт блокировки выбранных строк и сопутствующую табличную блокировку ROW SHARE; и благодаря MVCC она не мешает обычным `SELECT` без блокировочной клаузы — читатели по-прежнему видят снимок. Блокируются только те, кто хочет изменить эти строки или тоже взять на них блокировку. Обычно блокировка живёт до конца транзакции; исключение — блокировки, полученные после savepoint, снимаются при откате к нему.

По силе блокировки режимы выстраиваются так (PostgreSQL):

- `FOR UPDATE` — самая сильная: не даёт изменять строку, удалять её и брать на неё любую другую блокировку.
- `FOR NO KEY UPDATE` — как FOR UPDATE, но не конфликтует с `FOR KEY SHARE`: применяется, когда UPDATE не трогает колонки ключа (какой SQL выберет Hibernate, проверяют для конкретной версии и диалекта).
- `FOR SHARE` — разделяемая: строку можно читать и снова брать в SHARE, но нельзя изменять и брать FOR UPDATE. Типичный сценарий — прочитать родителя и гарантировать, что его не изменят и не удалят, пока мы вставляем потомков.
- `FOR KEY SHARE` — самая слабая: запрещает только изменение колонок ключа и удаление. Именно её неявно берёт проверка FOREIGN KEY при вставке потомка — поэтому массовые вставки в дочернюю таблицу могут конфликтовать с обновлением ключа родителя.

**NOWAIT.** По умолчанию конкурирующая транзакция просто ждёт освобождения строки (до `lock_timeout` или бесконечно). С `NOWAIT` она сразу получает ошибку `55P03 could not obtain lock` — правильный выбор, когда лучше быстро ответить «занято», чем копить ожидающие потоки и держать соединения. В JPA ожидание задают хинтом `jakarta.persistence.lock.timeout = 0`; поддержка и перевод в SQL зависят от провайдера/диалекта.

**SKIP LOCKED** — пропустить заблокированные строки и взять следующие свободные. Это превращает обычную таблицу в рабочую очередь: N воркеров параллельно выполняют один и тот же запрос, и каждый получает свою пачку задач — без отдельного координатора для захвата; повторная обработка после сбоя всё равно требует отдельного протокола:

```sql
-- воркер захватывает пачку свободных задач
BEGIN;

SELECT id, payload
FROM jobs
WHERE status = 'PENDING'
ORDER BY id
LIMIT 10
FOR UPDATE SKIP LOCKED;

UPDATE jobs SET status = 'PROCESSING' WHERE id = ANY(?);

COMMIT;  -- блокировки сняты, задачи помечены — другие воркеры их не возьмут
```

Этот приём используют для очередей на PostgreSQL и выборки записей transactional outbox. Два практических момента. Первый: блокировки живут до конца транзакции, поэтому захват задачи и смену статуса делают в короткой транзакции, а тяжёлую обработку — уже после COMMIT, иначе воркер будет держать строки, пока ходит по сети. Второй: PostgreSQL прекращает блокирование, когда набрано достаточно строк для LIMIT; SKIP LOCKED позволяет пропускать занятые и искать следующие. Очередь всё равно может вернуть неполную пачку, если доступных строк мало, а справедливость и отсутствие starvation не гарантированы. Подзапрос с блокировкой способен расширить область блокирования и не является универсальным исправлением. Для PROCESSING-задач нужен lease/таймаут и возврат после падения воркера; внешние эффекты должны допускать повтор.

**PostgreSQL vs MySQL.** В MySQL (InnoDB) синтаксис почти совпадает: `FOR UPDATE`, `FOR SHARE` (с версии 8.0; раньше — `LOCK IN SHARE MODE`), `NOWAIT` и `SKIP LOCKED` тоже доступны с 8.0. Но механика другая, и это любимая тема follow-up'ов:

- InnoDB блокирует записи индексов, а не «строки» как таковые. Если условие идёт не по индексу, блокируется всё просканированное — вплоть до всей таблицы.
- На уровне изоляции по умолчанию (REPEATABLE READ) InnoDB для многих locking reads берёт next-key locks: блокируется не только найденная строка, но и «промежуток» до соседней, чтобы не появлялись фантомы. `SELECT ... WHERE id > 10 FOR UPDATE` может заблокировать вставку id = 15, хотя такой строки ещё нет. В PostgreSQL SELECT FOR UPDATE не берёт gap locks для отсутствующих строк. На SERIALIZABLE есть predicate locks для обнаружения зависимостей; они не блокируют вставки как InnoDB next-key locks.
- Из-за этого один и тот же код очереди ведёт себя по-разному: на InnoDB легче словить deadlock и лишние ожидания, особенно без подходящего индекса под `WHERE status = 'PENDING'`.

**Оптимистичная блокировка** исходит из того, что конфликт редок: длительную блокировку на время чтения/решения заранее не берут, но при записи проверяется, что она не изменилась с момента чтения. Реализуется через колонку версии:

```sql
-- Прочитали: balance = 1000, version = 3
UPDATE accounts
SET balance = 900, version = version + 1
WHERE id = 1 AND version = 3;
-- Если затронуто 0 строк — кто-то успел изменить запись, нужно перечитать и повторить
```

В JPA оптимистичная блокировка встроена через `@Version` (см. вопрос 23).

Сравнение: пессимистичная модель сериализует конфликтующие доступы к захваченным строкам, но не гарантирует успех всей операции: остаются deadlock, ограничения и сбои; она снижает параллелизм и требует держать транзакцию открытой; оптимистичная масштабируется лучше, но требует обработки неудач (retry).

**Deadlock (взаимная блокировка)** — цикл ожидания: транзакция T1 держит ресурс A и ждёт B, транзакция T2 держит B и ждёт A. СУБД детектирует deadlock и принудительно откатывает одну из транзакций (PostgreSQL: `deadlock detected`, жертва выбирается автоматически). Профилактика:

- Захватывать ресурсы всегда в одном и том же порядке (например, по возрастанию id).
- Делать транзакции короткими, не включать в них сетевые вызовы и «раздумья».
- Использовать необходимый уровень блокировки и подходящие индексы: это сокращает время работы. Не переносить правило «скан блокирует все просмотренные строки» из InnoDB на обычный UPDATE PostgreSQL.
- Настроить `lock_timeout` и реализовывать retry при ошибке `40P01` (deadlock).

---

### 49. Как списать остаток без lost update и отрицательного баланса?

Для простого условного изменения одной строки лучше выразить проверку и изменение одним UPDATE. Раздельные SELECT и UPDATE по абсолютному значению создают окно гонки: два клиента могут прочитать один остаток и оба решить, что товара хватает.

```sql
UPDATE inventory
SET available = available - :quantity
WHERE product_id = :product_id
  AND available >= :quantity
RETURNING available;
```

При положительном quantity одна возвращённая строка означает успешное резервирование, ноль строк — отсутствие товара либо недостаточный остаток. `quantity > 0` проверяют отдельно; `available NOT NULL CHECK (available >= 0)` остаётся защитой данных. На PostgreSQL READ COMMITTED конкурирующий UPDATE ожидает блокировку и затем перепроверяет условие на актуальной версии строки. На более строгом уровне возможна ошибка сериализации, которую обрабатывают повтором.

Этот приём подходит для изменения, целиком описываемого выражением и условием одной строки. Если решение зависит от нескольких таблиц, тарифов и лимитов, может понадобиться транзакция с общим ресурсом блокировки или SERIALIZABLE. При резервировании нескольких товаров фиксируйте все позиции одной транзакцией и соблюдайте единый порядок ключей для уменьшения deadlock.

В Java проверяют число затронутых строк или результат RETURNING, а не считают отсутствие исключения подтверждением списания. Повтор после сетевого таймаута требует идемпотентности: первый UPDATE мог уже закоммититься. Атомарность изменения строки защищает остаток, но сама по себе не предотвращает повторное списание по одному запросу пользователя.

---

### 50. Как правильно повторять транзакцию после deadlock или serialization failure?

Повторяют всю бизнес-транзакцию в новой транзакции с повторным чтением исходных данных. Повтор последнего UPDATE в прежнем контексте неверен: PostgreSQL после ошибки обычно требует rollback, а решение приложения могло опираться на устаревшие значения.

В Spring удобно сделать внешний цикл, а каждую попытку выполнять через `TransactionTemplate`, когда окружающей транзакции нет. Вложенный вызов должен действительно создавать новую границу; self-invocation аннотированного метода её не создаёт.

```java
for (int attempt = 0; ; attempt++) {
    try {
        return transactionTemplate.execute(status -> reserveOnce(command));
    } catch (RuntimeException error) {
        if (!isRetryableDatabaseConflict(error) || attempt >= 2) {
            throw error;
        }
        pauseWithJitter(attempt); // вне транзакции; уважает interrupt
    }
}
```

`reserveOnce`, классификатор причины и пауза здесь — функции приложения. Для PostgreSQL типичные retryable SQLSTATE — `40001` и `40P01`; разбирайте цепочку причин драйвера/переводчика Spring. Ошибки CHECK, произвольный unique violation и некорректный запрос не становятся временными от повторения.

Попытки ограничивают общим deadline, добавляют backoff и jitter, измеряют частоту конфликтов. Повтор не лечит постоянный неправильный порядок блокировок или перегрузку. Объекты managed предыдущей попытки не переносят в новый persistence context; заново читают необходимые сущности.

Внешние эффекты — отправка письма, списание в платёжном API — нельзя бездумно помещать внутрь повторяемой функции. БД откатит свои записи, но не письмо. Для таких действий нужны идемпотентный внешний API или запись outbox в той же транзакции.

---

### 51. Почему проверка existsByEmail перед INSERT не гарантирует уникальность?

Между проверкой и вставкой другая транзакция может записать тот же ключ. На READ COMMITTED обе проверки законно вернут false. `synchronized` в одном процессе тоже не защищает от второй реплики приложения и прямого клиента БД.

```sql
CREATE TABLE registrations (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    tenant_id BIGINT NOT NULL,
    email TEXT NOT NULL,
    CONSTRAINT registrations_tenant_email_uk UNIQUE (tenant_id, email)
);

INSERT INTO registrations(tenant_id, email)
VALUES (:tenant_id, :email)
ON CONFLICT (tenant_id, email) DO NOTHING
RETURNING id;
```

Победитель получает id; пустой результат означает, что вставки не было. Если нужен существующий id, сделайте отдельный SELECT и определите поведение при конкурентном удалении. В READ COMMITTED `DO NOTHING` может пропустить вставку из-за конкурента, ещё невидимого снимку этого оператора: хитрый CTE «вставить или прочитать в том же statement» способен вернуть пустоту. Нельзя обещать, что он всегда вернёт одну строку.

`DO UPDATE` подходит, когда обновление действительно часть бизнес-операции. Фиктивный UPDATE ради RETURNING берёт блокировки, может запускать триггеры и создавать новые версии строк. Это измеримый компромисс.

В JPA ошибка unique constraint может проявиться при flush/commit. После нарушения транзакцию откатывают; нельзя поймать исключение внутри помеченной rollback-only транзакции и продолжить работать как ни в чём не бывало. Предварительная проверка полезна для UX, но окончательное решение обеспечивает именованное ограничение БД. Нормализацию регистра и область уникальности tenant согласуют до создания индекса.

---

### 52. Как таблица идемпотентности защищает от повторного запроса после таймаута?

Ключ идемпотентности связывает повтор запроса с уже выполненной бизнес-операцией. Он должен быть уникален в области клиента/tenant и операции (тип операции можно включить в namespace operation_key); одинаковый ключ с другим содержимым запроса — конфликт, а не повтор.

```sql
CREATE TABLE operation_results (
    tenant_id BIGINT NOT NULL,
    operation_key TEXT NOT NULL,
    request_hash TEXT NOT NULL,
    response_body JSONB,
    PRIMARY KEY (tenant_id, operation_key)
);
```

Для короткой операции, целиком выполняемой в одной БД, в одной транзакции вставляют ключ, меняют бизнес-данные и сохраняют результат. Unique constraint выбирает одного исполнителя. Если транзакция откатится, исчезнут и ключ, и изменения; повтор сможет выполнить операцию. Если commit состоялся, но ответ потерялся, повтор читает сохранённый результат и сверяет request_hash.

```sql
BEGIN;
INSERT INTO operation_results(tenant_id, operation_key, request_hash)
VALUES (:tenant, :key, :hash)
ON CONFLICT DO NOTHING
RETURNING operation_key;
-- Только получивший строку выполняет изменение и сохраняет ответ
COMMIT;
```

Пустой результат INSERT требует отдельного чтения существующего результата после ожидания конкурента, а не повторного выполнения бизнес-изменения. Если строку заранее коммитят в статусе PROCESSING, нужен другой протокол: lease, восстановление после падения и правила для зависшего исполнителя.

Срок хранения ключей определяет окно гарантии: после удаления записи старый запрос снова выглядит новым. Hash строят по согласованному представлению значимых полей. Таблица одной БД не атомарна с внешним платёжным API: там нужен его ключ идемпотентности и/или outbox с согласованием результата. Ошибка — хранить ключ отдельно после списания: падение между ними оставляет возможность второго списания.

---

### 17. MVCC: многоверсионный контроль конкурентного доступа

**MVCC (Multi-Version Concurrency Control)** — механизм, при котором СУБД хранит несколько версий одной строки, позволяя обычным snapshot-чтениям и изменениям строк **обычно не блокировать друг друга**: читающая транзакция видит согласованный снимок (snapshot) данных на определённый момент, а пишущая создаёт новую версию строки.

Как это работает в PostgreSQL:

1. У каждой строки (tuple) есть системные поля `xmin` (xid транзакции, создавшей версию) и `xmax` (xid транзакции, удалившей/устаревшей версию).
2. `UPDATE` физически **не изменяет строку на месте**: старая версия помечается `xmax = текущая транзакция`, и вставляется новая версия с `xmin = текущая транзакция`. `DELETE` — только проставление `xmax`.
3. Каждая транзакция получает **снимок (snapshot)**: данные о границах xid и активных транзакциях на момент снимка. Видимость зависит от статуса создателя/удалителя относительно снимка и собственных изменений. `xmax` также может кодировать блокировки/MultiXact, поэтому ненулевой xmax сам по себе не доказывает удаление строки.
4. READ COMMITTED берёт новый снимок на каждый оператор; REPEATABLE READ — один снимок на всю транзакцию.

Последствия и цена MVCC:

- **Bloat (раздувание таблиц)**: устаревшие версии строк (dead tuples) накапливаются. Их очищает VACUUM (обычно запущенный **autovacuum**), только когда версии больше не нужны снимкам; место обычно переиспользуется внутри файла, а не возвращается ОС. Длинные снимки задерживают очистку; агрессивные UPDATE-нагрузки требуют тюнинга vacuum. `VACUUM FULL` переписывает таблицу с блокировкой.
- **Transaction ID wraparound**: счётчик xid 32-битный, поэтому старые версии обязаны быть «заморожены» vacuum'ом; нельзя выключать autovacuum надолго.
- Индексы в PostgreSQL не хранят информации о видимости версий, поэтому обычный Index Scan вынужден идти в таблицу (heap fetch) и проверять видимость каждой строки. Index Only Scan возможен благодаря **visibility map**: если страница помечена как «вся видимая», обращение к куче пропускается — поэтому регулярный vacuum важен и для производительности чтения, а не только для очистки bloat.
- В MySQL (InnoDB) MVCC реализован иначе: старые версии хранятся в **undo log**, и актуальная строка читается «назад» по цепочке версий; длинные транзакции раздувают undo log.

Главный вывод для собеседования: MVCC даёт высокую конкурентность чтения/записи («обычные snapshot-чтения и запись строк обычно не блокируют друг друга»; DDL, явные блокировки и конфликты писателей остаются), но требует фоновой очистки версий и аккуратности с длинными транзакциями.

---

### 53. Почему длинная читающая транзакция мешает VACUUM, хотя ничего не блокирует FOR UPDATE?

Старые версии нельзя удалить, пока они потенциально нужны активному снимку. Долгий отчёт на REPEATABLE READ может часами удерживать горизонт видимости: приложение только читает, а autovacuum не может убрать часть версий, созданных параллельными UPDATE/DELETE.

```sql
SELECT pid, state, xact_start, backend_xmin,
       wait_event_type, wait_event
FROM pg_stat_activity
WHERE xact_start IS NOT NULL
ORDER BY xact_start;
```

Ищут долгие транзакции и `idle in transaction`, затем связывают pid с владельцем и запросом. Отдельные источники удержания WAL и старых версий — replication slots и настройки реплик; их проверяют отдельно, а не объясняют всё одной активной сессией. Одна давняя `xact_start` сама по себе ещё не доказывает удержание нужного snapshot: смотрят backend_xmin и контекст.

Обычный VACUUM освобождает место для повторного использования внутри таблицы и обслуживает visibility map; размер файла часто не уменьшается. VACUUM FULL переписывает таблицу, требует дополнительного места и сильной блокировки. Запускать его автоматически при любом bloat — опасная реакция.

Для отчётов выбирают короткие чтения, отдельную аналитическую копию или осознанно платят за согласованный длительный снимок. Разбиение отчёта на отдельные транзакции снижает удержание версий, но теряет единый snapshot: итог может смешать разные состояния. Таймауты зависших транзакций и корректное закрытие ресурсов полезнее увеличения частоты vacuum без устранения причины.

## Схема и эксплуатация

### 25. Миграции БД: Flyway и Liquibase

**Миграции БД** — версионируемые, воспроизводимые изменения схемы (и данных), применяемые автоматически при развёртывании приложения. Заменяют ручное накатывание SQL и режим `hibernate.hbm2ddl.auto=update` (который годится только для локальной разработки: он не версионирует изменения и опасен в проде).

**Flyway** — миграции в виде обычных SQL-файлов (или Java-классов), названия по конвенции:

```text
db/migration/
├── V1__create_customers.sql
├── V2__add_orders_table.sql
└── V3__add_index_orders_customer.sql
```

```sql
-- V2__add_orders_table.sql
CREATE TABLE orders (
    id          BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    customer_id BIGINT NOT NULL REFERENCES customers(id),
    total       NUMERIC(12, 2) NOT NULL DEFAULT 0,
    created_at  TIMESTAMP NOT NULL DEFAULT now()
);
CREATE INDEX idx_orders_customer ON orders(customer_id);
```

При старте приложения Flyway проверяет служебную таблицу `flyway_schema_history`, применяет новые версии по порядку и контролирует **checksum** файлов: изменение уже применённой миграции приводит к ошибке валидации (защита от «подмены истории»). Команды: `migrate`, `validate`, `info`, `baseline`, `repair`.

**Liquibase** — изменения описываются декларативно в XML/YAML/JSON/SQL (changeset'ы), хранится история в таблице `databasechangelog`:

```xml
<databaseChangeLog xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
                   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                   xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
                   http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-latest.xsd">
    <changeSet id="2" author="dev">
        <createTable tableName="orders">
            <column name="id" type="BIGINT" autoIncrement="true">
                <constraints primaryKey="true"/>
            </column>
            <column name="customer_id" type="BIGINT">
                <constraints nullable="false" foreignKeyName="fk_orders_customer"
                             references="customers(id)"/>
            </column>
            <column name="total" type="NUMERIC(12,2)" defaultValueNumeric="0"/>
        </createTable>
        <rollback>
            <dropTable tableName="orders"/>
        </rollback>
    </changeSet>
</databaseChangeLog>
```

Сравнение:

- **Flyway**: проще и ближе к SQL; миграции — обычные скрипты, легко читать и ревьюить; откатов из коробки нет (в платной версии — undo). Идемпотентность обеспечивается версионированием, а не rollback'ами: исправления вносятся новой миграцией.
- **Liquibase**: декларативные change types абстрагируют часть различий СУБД; SQL, типы, ограничения и online DDL всё равно требуют проверки на каждом диалекте, поддержка rollback и генерации diff между схемами; цена — более многословный XML/YAML и дополнительная абстракция.

Общие правила: миграции неизменяемы после попадания в общие окружения, обратно-совместимые изменения схемы при zero-downtime-деплое делаются в несколько шагов (expand-and-contract: добавить колонку → переключить код → удалить старую колонку отдельной миграцией).

---

### 54. Как изменить обязательную колонку при rolling deployment без остановки записи?

Миграция должна быть совместима одновременно со старой и новой версиями приложения. Опасный шаг — сразу переименовать или удалить колонку, которую ещё читает старый экземпляр. Используйте expand → migrate → contract и явно определите владельца актуального значения в переходный период.

Например, `full_name` заменяется на `display_name`:

```sql
ALTER TABLE users ADD COLUMN display_name TEXT;
-- Backfill выполняют повторяемыми небольшими порциями по диапазонам id
UPDATE users SET display_name = full_name
WHERE id > :from_id AND id <= :to_id AND display_name IS NULL;
```

Сначала выпускают совместимый код: читает новое поле с fallback, поддерживает нужные записи в обе колонки. Но старые экземпляры пока пишут только full_name. Поэтому разовую копию нельзя считать синхронизацией: либо backfill запускают после ухода старых writers и затем сверяют данные, либо вводят временный механизм синхронизации, например временный триггер с согласованными правилами обновления. Если имена могут меняться, `WHERE display_name IS NULL` сам по себе не переносит позднее изменение старого поля.

Затем переводят чтение на новую колонку, проверяют заполнение и бизнес-ограничения. `NOT NULL` добавляют после валидации; на большой таблице оценивают блокировки и способ проверки для версии PostgreSQL. Старую колонку удаляют отдельным релизом после окончания окна отката к старому приложению.

Порционный backfill ограничивает WAL, длительность блокировок и лаг реплик; он должен иметь checkpoint и безопасный повтор. Наличие rollback SQL не возвращает удалённые данные автоматически. Частая ошибка — считать успешный прогон миграции на пустой тестовой БД доказательством безопасного продакшен-деплоя: нужны объём, конкурентная запись и тест совместимости двух версий.

---

### 55. Чем CREATE INDEX CONCURRENTLY отличается от обычного создания индекса?

В PostgreSQL обычный CREATE INDEX блокирует запись в таблицу на время построения. `CONCURRENTLY` позволяет продолжать INSERT/UPDATE/DELETE, но выполняет больше работы, ждёт определённые транзакции и обычно длится дольше. Это инструмент уменьшения блокировок, а не обещание «нулевого влияния» на нагрузку.

```sql
CREATE INDEX CONCURRENTLY orders_customer_time_idx
    ON orders(customer_id, created_at DESC);

SELECT c.relname, i.indisvalid, i.indisready
FROM pg_index i
JOIN pg_class c ON c.oid = i.indexrelid
WHERE c.relname = 'orders_customer_time_idx';
```

CONCURRENTLY нельзя выполнять внутри обычного transaction block. В Flyway/Liquibase такую миграцию настраивают как нетранзакционную по правилам используемой версии инструмента и выносят в отдельный шаг. Иначе корректный SQL упадёт из-за обёртки мигратора.

После отмены или ошибки может остаться INVALID-индекс: планировщик его не использует, но он способен добавлять стоимость записи. Нельзя слепо повторять `IF NOT EXISTS`: совпадение имени ещё не подтверждает работоспособность и нужное определение индекса. Проверяют состояние и причину, затем удаляют неудачный объект/перестраивают его подходящим способом. Уникальный индекс при конкурентном построении имеет дополнительные нюансы: предварительно ищут дубли и планируют реакцию на новые конфликты.

Миграция требует наблюдения за I/O, свободным местом, ожиданиями и репликацией. Даже краткая блокировка метаданных может ждать за долгой транзакцией. Ошибка на собеседовании — «CONCURRENTLY вообще не берёт блокировок, значит запускаем любое число индексов одновременно»: конкуренция за ресурсы и ограничения самой команды никуда не исчезают.

---

### 27. Партиционирование таблиц: range, list и hash в PostgreSQL

**Партиционирование** — разбиение одной логической таблицы на физические части по значению ключа. Нужно, когда таблица выросла настолько, что индексы и autovacuum уже не спасают: универсального порога по числу строк нет: важны размер, запросы, retention и обслуживание.

Виды декларативного партиционирования в PostgreSQL:

- **RANGE** — диапазоны значений. Классика: по дате, помесячно или поквартально.
- **LIST** — по явному списку значений: регион, статус, тип.
- **HASH** — по хешу ключа, равномерное распределение на N партиций, когда нет естественного диапазона.

```sql
CREATE TABLE events (
    id         BIGINT NOT NULL,
    created_at TIMESTAMPTZ NOT NULL,
    payload    JSONB
) PARTITION BY RANGE (created_at);

CREATE TABLE events_2026_07 PARTITION OF events
    FOR VALUES FROM ('2026-07-01') TO ('2026-08-01');
CREATE TABLE events_2026_08 PARTITION OF events
    FOR VALUES FROM ('2026-08-01') TO ('2026-09-01');
```

Что это даёт на практике:

- **partition pruning** — планировщик читает только партиции, попадающие под условие `WHERE`, а не всю таблицу;
- **дешёвое удаление старых данных**: `DROP TABLE events_2025_01` вместо `DELETE` на миллионы строк — без построчного DELETE и создаваемых им dead tuples; операция всё равно требует блокировок метаданных;
- индексы и vacuum обслуживают маленькие партиции, а не одну гигантскую таблицу.

Оговорки: PRIMARY KEY и UNIQUE обязаны включать ключ партиционирования, поэтому PK часто становится составным `(id, created_at)`; запрос без фильтра по ключу сканирует все партиции и может оказаться медленнее, чем по одной таблице. Ловушка в том, что партиционирование «на вырост» для маленькой таблицы — типичный оверинжиниринг: оно усложняет схему и миграции, не давая ничего взамен.

---

### 28. Репликация: синхронная и асинхронная, read replicas и replication lag

**Репликация** — копирование данных с primary (master) на одну или несколько реплик для отказоустойчивости и масштабирования чтения. Каждая реплика хранит полную копию данных.

- **Асинхронная** (по умолчанию): primary подтверждает commit клиенту, не дожидаясь реплик. Быстро, но при внезапном падении primary последние транзакции могут потеряться (RPO > 0).
- **Синхронная**: primary ждёт подтверждения записи WAL хотя бы на одной реплике (в PostgreSQL — `synchronous_standby_names` вместе с уровнем `synchronous_commit`). Значение `on` ждёт устойчивой записи WAL на выбранных standby, `remote_apply` дополнительно ждёт применения, важного для видимости чтений. Гарантии зависят от выбранных реплик и сценария failover; цена — latency и зависимость от доступности standby.

**Read replicas**: чтение (`SELECT`) раздаём на реплики, запись идёт только на primary. Главная ловушка — **replication lag**: реплика отстаёт на миллисекунды, а под нагрузкой на секунды. Отсюда классическая проблема **read-your-writes**: пользователь сохранил форму (запись на primary), UI перечитал данные с реплики и не видит собственных изменений. Внешне выглядит как «запрос не сохранился».

Варианты решения:

- сразу после записи читать с primary (или помечать сессии, которые недавно писали, и направлять их чтение на primary);
- монотонное чтение: запоминать LSN позиции записи и ждать, пока реплика её догонит;
- синхронную репликацию ставить только там, где потеря недопустима (деньги, заказы), остальное — асинхронно.

Частый follow-up — чем репликация отличается от шардинга: реплика хранит полную копию данных и масштабирует только чтение; шард хранит часть данных и масштабирует объём и запись. Тема следующего вопроса.

---

### 29. Шардинг: отличие от репликации и выбор ключа шардирования

**Шардинг** — горизонтальное разбиение данных между независимыми узлами (шардами): каждый хранит свой поднабор строк, данные не дублируются. В отличие от репликации, шардинг масштабирует объём данных и нагрузку на запись, ; чтение также может распределяться между шардами, если запросы локальны.

**Ключ шардирования** — главное решение, поменять его потом очень больно. Требования: равномерное распределение нагрузки (нет «горячего» шарда) и попадание типичных запросов в один шард. Классика — id владельца данных: `user_id` для соцсети, `tenant_id` для SaaS. Антипример — шардирование по дате: вся запись идёт в «текущий» шард, и он становится горячим по определению.

Что ломается после шардинга:

- **joins между шардами** — либо денормализация и дублирование справочников на все шарды, либо склейка данных в приложении;
- **глобально уникальные id** — auto-increment не работает, нужны UUID, snowflake-id или отдельный сервис диапазонов;
- **распределённые транзакции** — 2PC медленный и хрупкий, на практике его избегают: Saga, идемпотентные операции, проектирование так, чтобы атомарность требовалась только внутри одного шарда;
- **resharding** — рост данных требует перекладывать строки между узлами. Решают виртуальными шардами и consistent hashing (так делают Vitess, Citus): логических шардов много больше, чем физических узлов, и переносят их целиком;
- **запросы без ключа шарда** — scatter-gather по всем узлам, дорого и медленно; обычно рядом заводят поисковый индекс (Elasticsearch/OpenSearch).

На практике шардинг — крайняя мера: сначала индексы, партиционирование, реплики, вертикальное масштабирование. Частый follow-up «с какого объёма шардировать» — универсального числа нет: разговор начинается, когда один узел объективно не тянет по объёму или записи, а не когда «у нас будет миллион пользователей».

---

### 30. Пул соединений: HikariCP и выбор размера пула

**Пул соединений** держит набор готовых соединений с БД и выдаёт их по запросу, потому что установка TCP-соединения плюс аутентификация стоят миллисекунды — платить это на каждый запрос непозволительно дорого. После использования соединение возвращается в пул, а не закрывается. Вторая функция пула — ограничить число одновременных соединений к БД, чтобы приложение не уронило базу под пиковой нагрузкой.

**HikariCP** — де-факто стандарт в Spring Boot. Быстрый за счёт минимума блокировок (lock-free структуры, кэш возвращённых соединений в ThreadLocal), аккуратно оптимизированного байткода и лёгких proxy-обёрток через Javassist. На практике важнее другое: он предсказуемо ведёт себя под нагрузкой и умеет обнаруживать утечки.

Размер пула. Эвристика из материалов HikariCP для начальной оценки, а не рекомендация PostgreSQL для любой нагрузки:

```text
connections = ((core_count * 2) + effective_spindle_count)
```

На SSD параметр effective_spindle_count не равен автоматически единице. Формула — отправная точка для нагрузочного теста, а не вычисление обязательного размера. Контринтуитивно, но это нормально: соединение большую часть времени ждёт диск или сеть, а лишние соединения добавляют только contention и context switching. Ловушка в том, что пул из 50 соединений «потому что в Tomcat 50 потоков» обычно работает хуже, чем пул из 10.

```yaml
spring:
  datasource:
    hikari:
      maximum-pool-size: 10
      connection-timeout: 3000     # сколько ждать свободное соединение
      max-lifetime: 1800000        # согласовать с лимитом жизни соединения в инфраструктуре
```

Типичные ошибки: утечка соединений (`leakDetectionThreshold` помогает найти место выдачи, но не закрывает утечку за приложение), слишком большой пул, `maxLifetime` больше таймаута БД или файрвола (пул выдаёт «мёртвые» соединения), удержание соединения на время не-БД работы — например, HTTP-вызов внутри открытой транзакции.

---

### 38. HikariCP на собеседовании: глубокие вопросы по конфигурации и эксплуатации

Базовый разбор пула есть в вопросе 30; здесь — то, чем на собеседовании отличают «читал документацию» от «эксплуатировал в проде»: четыре таймаута, поведение под нагрузкой и диагностика.

**Четыре таймаута** — их путают чаще всего:

- `connectionTimeout` (по умолчанию 30 с) — сколько поток ждёт свободное соединение из пула, прежде чем получить `SQLTransientConnectionException`. Не путать с TCP connect timeout драйвера (установка физического соединения): это разные часы.
- `validationTimeout` (по умолчанию 5 с) — сколько пул готов ждать проверку живости соединения (`isValid`) перед выдачей.
- `idleTimeout` (по умолчанию 10 мин) — простаивающее соединение закрывается по истечении этого времени, но пул никогда не опустится ниже `minimumIdle`: idleTimeout работает только «сверх» minimumIdle.
- `maxLifetime` (по умолчанию 30 мин) — предельная жизнь соединения: по истечении пул мягко закрывает его (после возврата в пул) и создаёт новое. Ключевое правило: `maxLifetime` согласуют с принудительным пределом жизни соединения на стороне БД/инфраструктуры. Idle timeout — отдельный механизм: если соединение разрывают только за бездействие, учитывают idleTimeout/keepaliveTime, а не автоматически приравнивают его к общей жизни соединения. Иначе соединение на том конце уже разорвано, а пул об этом не знает и выдаёт мёртвое — первый запрос после простоя падает с `Connection reset` / `Broken pipe`. Запас и keepalive выбирают по реальным сетевым ограничениям; значение 0 отключает maxLifetime и требует осознанного решения.

**maximumPoolSize.** Эвристика из материалов HikariCP, которую проверяют нагрузочным тестом:

```text
connections = ((core_count * 2) + effective_spindle_count)
```

Почему «больше пул = быстрее» неверно: каждое соединение — это backend-процесс на стороне БД со своей памятью и планировкой ОС; десятки активных соединений начинают конкурировать за блокировки, буферы и CPU, растёт context switching. Пропускная способность перестаёт расти, а latency продолжает ухудшаться — классическая кривая Universal Scalability Law. Универсального размера на инстанс нет: общий бюджет соединений делят между всеми экземплярами, фоновой работой и другими клиентами, затем измеряют throughput и ожидание. Обязательный follow-up: `maximumPoolSize × число инстансов` не должно превышать `max_connections` БД, причём с запасом под админские сессии и служебные процессы.

**minimumIdle и фиксированный пул.** По умолчанию `minimumIdle == maximumPoolSize`, и это рекомендуемый режим для продакшена: пул не прогревается под пиком, нет эффекта «первый запрос после ночного простоя открывает 20 физических соединений», поведение предсказуемо. Раздельные значения (minimumIdle меньше) оправданы при сильно волнообразной нагрузке, но тогда открытием соединений платят запросы прямо в пике.

**Диагностика.** Ошибка `Connection is not available, request timed out after 30000ms` означает, что запрос не смог получить пригодное соединение за отведённое время. Часто пул занят, но возможны и ошибки создания/проверки соединений при недоступной БД. Проверяют следующие причины:

- утечка соединений — взяли через `getConnection()` и не закрыли (нет try-with-resources, исключение случилось до close); с пулом `close()` — это возврат соединения, забытый close крадёт его навсегда;
- долгие транзакции и медленные запросы — соединение занято, но «честно»;
- lock wait в БД — соединения висят в ожидании блокировок; смотреть `pg_stat_activity`, а `state = 'idle in transaction'` — почти всегда плохой знак;
- внешний HTTP-вызов при открытом соединении или транзакции.

`leakDetectionThreshold` — ловушка утечек для dev и test: если соединение не вернули за указанное время, HikariCP логирует stack trace места, где его взяли:

```text
java.lang.Exception: Apparent connection leak detected
    at com.zaxxer.hikari.HikariDataSource.getConnection(...)
```

В проде его обычно выключают или ставят большим: на легитимных долгих операциях это ложные срабатывания, а сама утечка к моменту записи в лог уже случилась. И отдельно: увеличение пула отодвигает падение, но утечку не чинит — лечить надо причину.

**Что полезно знать об устройстве.** HikariCP уменьшает стоимость выдачи/возврата соединений и синхронизации. Внутренние структуры и способы генерации прокси меняются между версиями; для эксплуатации важнее измерить ожидание соединений, длительность транзакций и реальные ограничения БД, чем заучивать названия внутренних классов.

**Поведение при падении БД.** Для проверки пригодности соединения пул использует `Connection.isValid()` из JDBC4 при подходящем драйвере — это ping средствами драйвера, поэтому `connection-test-query` с современными драйверами задавать не нужно: способ проверки зависит от драйвера, а отдельный connectionTestQuery обычно не требуется. Мёртвые соединения пул отбрасывает и создаёт новые; если БД лежит целиком, выдача начнёт падать по `connectionTimeout` — retry HikariCP за вас не делает. Нюанс, который любят спрашивать: валидация не гарантирует успех — между isValid и выполнением запроса БД может умереть (гонка), поэтому обработка ошибок и ретраи нужны в приложении в любом случае.

**Настройка в Spring Boot.** Свойства вида `spring.datasource.hikari.*`:

```properties
spring.datasource.hikari.maximum-pool-size=10
spring.datasource.hikari.minimum-idle=10
spring.datasource.hikari.connection-timeout=3000
spring.datasource.hikari.validation-timeout=1000
spring.datasource.hikari.idle-timeout=600000
spring.datasource.hikari.max-lifetime=1700000
spring.datasource.hikari.leak-detection-threshold=60000
```

Через Micrometer пул публикует метрики: `hikaricp.connections.active` (занятые), `hikaricp.connections.idle` (свободные), `hikaricp.connections.pending` (потоки в очереди за соединением — устойчивое значение больше нуля говорит, что пул стал узким местом), `hikaricp.connections.max`, таймер `hikaricp.connections.acquire` и счётчик `hikaricp.connections.timeout`. Практичный алерт: растущий pending при active, прижатом к max, в течение пары минут — либо нагрузка выросла, либо где-то удерживают соединения.

## Java и ORM: контекст, связи и стоимость запросов

### 34. JDBC под капотом ORM: DriverManager/DataSource, Statement vs PreparedStatement, CallableStatement, ResultSet

Любой ORM в Java в конечном счёте говорит с БД через JDBC: Hibernate генерирует SQL и выполняет его через `PreparedStatement` поверх соединения из пула. Понимать этот слой нужно, чтобы читать логи, настраивать пул и не удивляться утечкам соединений.

**Получение соединения.** `DriverManager.getConnection(url, user, password)` создаёт новое физическое соединение на каждый вызов — для учебных примеров годится, в проде нет. В приложениях используется `DataSource` — фабрика соединений, за которой обычно стоит пул (HikariCP, см. вопрос 30): `getConnection()` возвращает логическую обёртку, а `close()` не закрывает физическое соединение, а возвращает его в пул.

**Шаги выполнения запроса**: загрузить драйвер (в современных версиях — автоматически через механизм JDBC service providers), получить соединение, создать statement, выполнить, обойти ResultSet, закрыть ресурсы в обратном порядке. Ручной close почти всегда заменяют try-with-resources:

```java
String sql = "SELECT id, full_name, salary FROM employees WHERE department_id = ?";

try (Connection conn = dataSource.getConnection();
     PreparedStatement ps = conn.prepareStatement(sql)) {

    ps.setLong(1, departmentId);            // параметры нумеруются с 1

    try (ResultSet rs = ps.executeQuery()) {
        while (rs.next()) {                 // курсор изначально стоит ДО первой строки
            long id = rs.getLong("id");
            String name = rs.getString("full_name");
            BigDecimal salary = rs.getBigDecimal("salary");
            // маппинг в объекты
        }
    }
}   // rs, ps, conn закроются сами; conn вернётся в пул
```

**Statement vs PreparedStatement** — классическая пара на собеседовании:

- `Statement` выполняет SQL «как строку»: запрос разбирается на каждый вызов, а значения подставляются конкатенацией — прямая дорога к SQL-инъекции:

```java
// ОПАСНО: name = "' OR '1'='1" превратит условие в всегда-истинное
Statement st = conn.createStatement();
ResultSet rs = st.executeQuery("SELECT * FROM users WHERE name = '" + name + "'");
```

- `PreparedStatement` — параметризованный запрос с плейсхолдерами `?`: подготовка на сервере и переиспользование плана зависят от драйвера, порогов и СУБД, а параметры передаются отдельно от текста запроса, поэтому драйвер и СУБД гарантированно не интерпретируют их как SQL. Защита от инъекций здесь — не «экранирование», а сам принцип раздельной передачи запроса и данных:

```java
PreparedStatement ps = conn.prepareStatement("SELECT * FROM users WHERE name = ?");
ps.setString(1, name);   // значение уходит как данные, а не как часть запроса
```

- `CallableStatement` — наследник PreparedStatement для вызова хранимых процедур, умеет OUT/INOUT-параметры:

```java
try (CallableStatement cs = conn.prepareCall("{ call transfer_money(?, ?, ?) }")) {
    cs.setLong(1, fromId);
    cs.setLong(2, toId);
    cs.setBigDecimal(3, amount);
    cs.execute();
}
```

**ResultSet** — интерфейс обхода результата; он не гарантирует потоковую загрузку с сервера. pgJDBC по умолчанию собирает результат целиком, а для cursor fetching нужны autoCommit=false, forward-only результат и положительный fetchSize (пример — вопрос 60). По умолчанию курсор forward-only и read-only; прокручиваемый (`TYPE_SCROLL_INSENSITIVE`) и обновляемый (`CONCUR_UPDATABLE`) варианты существуют, но на практике почти не используются. Связка с ORM простая: Hibernate за вас выполняет ровно этот цикл — prepare, bind, iterate, map. Никакой магии там нет.

---

### 60. Как читать миллионы строк через JDBC, не загрузив весь результат в память?

Наличие ResultSet не гарантирует потоковую загрузку. В pgJDBC по умолчанию драйвер собирает результат целиком. Для cursor-based fetching нужны выключенный auto-commit, forward-only результат и положительный fetchSize; точное поведение зависит от драйвера.

```java
try (Connection connection = dataSource.getConnection()) {
    connection.setAutoCommit(false);
    try {
        try (PreparedStatement statement = connection.prepareStatement(
                "SELECT id, total FROM orders ORDER BY id",
                ResultSet.TYPE_FORWARD_ONLY, ResultSet.CONCUR_READ_ONLY)) {
            statement.setFetchSize(1000);
            try (ResultSet result = statement.executeQuery()) {
                while (result.next()) {
                    exportRow(result.getLong("id"), result.getBigDecimal("total"));
                }
            }
        }
        connection.commit();
    } catch (SQLException | RuntimeException error) {
        connection.rollback();
        throw error;
    }
}
```

`exportRow` здесь обрабатывает одну строку без накопления списка. Если внутри всё равно складывать данные в ArrayList, преимущество исчезнет. Для JDBC-вызова внутри Spring-транзакции не управляйте commit вручную: используйте transaction-aware доступ и границу Spring.

Меньший fetchSize снижает клиентскую память, но увеличивает число обращений; больший делает наоборот. Долгое чтение удерживает соединение и снимок, а медленная внешняя отправка внутри цикла растягивает транзакцию. При допустимости разных снимков часто удобнее короткие keyset-порции с checkpoint, но нужно определить поведение при конкурентных вставках и обновлениях.

Для ORM query stream дополнительно важны закрытие stream и рост persistence context: ленивое получение строк не отменяет удержание загруженных сущностей. Частая ошибка — вернуть Stream из уже завершённого транзакционного метода и начать читать его в контроллере, когда соединение и контекст уже закрыты.

---

### 18. ORM и JPA: Entity, EntityManager, Persistence Context

**ORM (Object-Relational Mapping)** — технология отображения объектно-ориентированной модели на реляционную схему БД: классы ↔ таблицы, поля ↔ колонки, связи объектов ↔ внешние ключи. ORM избавляет от ручного написания рутинного SQL и от «impedance mismatch» (разницы между объектной и реляционной моделями).

**JPA (Jakarta Persistence API)** — стандартная спецификация ORM в Java (интерфейсы и аннотации из `jakarta.persistence` / `javax.persistence`). **Hibernate** — наиболее популярная реализация JPA (плюс собственные расширения). Альтернативы: EclipseLink, OpenJPA.

Ключевые понятия:

- **Entity** — класс, отображаемый на таблицу. Требования: аннотация `@Entity`, поле-идентификатор `@Id`, конструктор без аргументов, класс не `final`.
- **EntityManager** — главный интерфейс JPA: `persist`, `merge`, `remove`, `find`, JPQL-запросы. Не потокобезопасен — обычно один EntityManager на транзакцию/запрос.
- **EntityManagerFactory** — тяжеловесная потокобезопасная фабрика EntityManager'ов (одна на persistence unit).
- **Persistence Context (контекст персистентности)** — кэш первого уровня: набор сущностей, управляемых данным EntityManager'ом. Гарантирует **identity map**: в рамках контекста на одну строку БД существует ровно один Java-объект (повторный `find(id)` вернёт тот же экземпляр). Отслеживает изменения сущностей (dirty checking) и при `flush`/`commit` автоматически генерирует `UPDATE` для изменённых объектов.

**Жизненный цикл сущности:**

- **Transient (New)** — объект создан через `new`, не связан с контекстом, в БД его нет.
- **Managed (Persistent)** — объект ассоциирован с контекстом: после `persist()`, `find()`, `merge()` или загрузки запросом. Изменения полей автоматически синхронизируются с БД при flush.
- **Detached** — объект был управляемым, но контекст закрыт/очищен (`close()`, `clear()`, `detach()`). Изменения больше не отслеживаются; вернуть в Managed можно через `merge()` (который скопирует состояние в управляемый экземпляр).
- **Removed** — объект помечен на удаление (`remove()`); строка удалится при flush.

```text
                persist()                     remove()
  new ───────────────► MANAGED ─────────────────► REMOVED ──flush──► (строка удалена)
 TRANSIENT              │  ▲
                        │  │ find() / merge() / запрос
        close()/clear()/detach()
                        ▼  │
                     DETACHED
                  (изменения не отслеживаются)
```

```java
EntityManager em = emf.createEntityManager();
em.getTransaction().begin();

User user = new User("Анна");   // Transient
em.persist(user);               // Managed; момент INSERT зависит от генератора id
user.setName("Анна Смирнова");  // setter — dirty checking, UPDATE сгенерируется сам

em.flush();                     // пример не зависит от отложенного присвоения id
User found = em.find(User.class, user.getId()); // тот же объект из L1-кэша

em.getTransaction().commit();   // flush + COMMIT
em.close();                     // found становится Detached
```

---

### 36. Стратегии генерации первичных ключей в JPA: IDENTITY / SEQUENCE / TABLE / UUID

Стратегия `@GeneratedValue` определяет, откуда Hibernate берёт id для новой сущности, и у каждой есть своя цена.

**IDENTITY** — колонка auto-increment (`BIGSERIAL`, `GENERATED AS IDENTITY`, `AUTO_INCREMENT`): id присваивает сама БД в момент INSERT.

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```

В Hibernate 6.x IDENTITY несовместим с обычным JDBC batching INSERT для этих сущностей: провайдер должен выполнить вставку, чтобы узнать сгенерированный id. В типичной активной транзакции SQL выполняется уже при persist; детали отложенного выполнения зависят от контекста и версии. Поэтому для batch-нагрузок IDENTITY не подходит (см. вопрос 32).

**SEQUENCE** — id берётся из объекта-последовательности БД, который выдаёт значения без вставки в таблицу. INSERT откладывается до flush, батчинг работает.

```java
@Id
@GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "order_seq")
@SequenceGenerator(name = "order_seq", sequenceName = "order_seq", allocationSize = 50)
private Long id;
```

Ключевой параметр — `allocationSize`: Hibernate резервирует диапазон id одним обращением к последовательности и раздаёт значения из памяти (алгоритм hi/lo, в Hibernate 6 — pooled optimizer), не ходя в БД на каждую вставку. Значение по умолчанию — 50, и это источник классического бага: если последовательность в БД создана с `INCREMENT BY 1`, а Hibernate считает allocationSize равным 50, современный Hibernate обычно обнаружит несогласованный increment при проверке схемы/генератора; отключение проверки может дать неверное распределение ключей. Пропуски id сами по себе нормальны и не доказывают конфликт. Правило: `INCREMENT BY` последовательности в миграции должен совпадать с allocationSize в аннотации.

```sql
CREATE SEQUENCE order_seq START WITH 1 INCREMENT BY 50;
```

**TABLE** — эмуляция последовательности через отдельную таблицу со счётчиком. Работает на любой СУБД, но каждый блок id — это UPDATE строки-счётчика в отдельной транзакции: медленно и превращается в точку contention под нагрузкой. Наследие времён, когда последовательности были не везде; в новых проектах не используется.

**UUID** — id генерирует приложение (`GenerationType.UUID` стандартизован в Jakarta Persistence 3.1; поддержка зависит от реализации, либо ключ назначают вручную), БД не участвует. Плюсы: id известен до INSERT (удобно для распределённых систем, шардинга, идемпотентных ретраев), нет обращений к БД за ключом. Минусы:

- размер: 16 байт против 8 у BIGINT — ключи занимают вдвое больше байт; итоговый размер индексов растёт не строго в два раза из-за служебных данных и других колонок;
- случайный UUID (v4) вставляется в B-Tree в случайные позиции: страницы индекса постоянно расщепляются (page splits), индекс фрагментируется и растёт быстрее, чем при монотонных ключах; в MySQL, где PK — кластерный индекс, страдает и сама таблица;
- читаемость при отладке: сравнивать и джойнить «глазами» UUID неудобно.

Смягчение — упорядоченные UUID: v7 (time-ordered) или UUID с временным префиксом — они улучшают локальность вставок; page splits и стоимость индекса всё равно остаются. Про `GenerationType.AUTO` тоже спросят: провайдер выбирает стратегию с учётом типа id и диалекта; это не универсальное обещание SEQUENCE, явно полагаться на него не стоит.

Практический вывод: на PostgreSQL для batch-вставок — SEQUENCE с согласованным allocationSize; MySQL не предоставляет обычные SQL sequences как PostgreSQL, поэтому стратегию выбирают отдельно; в распределённой системе, где id нужен до записи в конкретный шард, — UUIDv7; IDENTITY — только там, где батчинг точно не понадобится.

---

### 62. Как выбор стратегии наследования в JPA изменит запрос к истории платежей?

Пусть у `Payment` есть два подтипа — банковский перевод и оплата картой. Экран истории выводит платежи обоих видов по дате. При `SINGLE_TABLE` оба подтипа лежат в одной таблице с признаком типа. Экран может читать один индекс. Для обязательного реквизита карты понадобится ограничение с учётом типа платежа, поскольку у перевода этот столбец пуст.

При `JOINED` общие поля находятся в таблице родителя, детали имеют тот же ключ в таблицах подтипов. Обязательность реквизитов проще задать в схеме. При загрузке сущностей конкретных типов могут понадобиться дополнительные соединения. При `TABLE_PER_CLASS` каждый конкретный тип хранит и общие, и собственные поля в своей таблице. Полиморфный запрос Hibernate обычно объединяет из нескольких выборок.

Для выбора сравните SQL двух операций — истории платежей и карточки одного перевода. Учитывайте, какие поля им нужны. `@MappedSuperclass` переиспользует объявленные поля, однако его нельзя сделать корнем полиморфного JPQL-запроса.

---

### 37. persist vs merge vs saveOrUpdate (и getById vs getReference)

Все эти операции — про перевод объектов между состояниями жизненного цикла (см. вопрос 18), и путаница между ними — источник реальных багов.

**persist(entity)** — принимает transient-объект и делает его managed: INSERT случится при flush, id появится по стратегии генерации (при SEQUENCE — сразу, при IDENTITY в типичной активной транзакции — через немедленный INSERT). Вызов persist на detached-сущности (наличие id само по себе не доказывает detached: новый объект тоже может иметь назначенный id) — ошибка `PersistentObjectException: detached entity passed to persist`. persist ничего не возвращает — managed становится сам переданный объект.

**merge(entity)** — для detached-сущностей: Hibernate загружает managed-экземпляр из БД (или берёт из persistence context), копирует в него состояние переданного объекта и возвращает managed-копию. Переданный объект остаётся detached — типичная ошибка: продолжать работать с аргументом вместо возвращённого значения. Цена merge — лишний SELECT: чтобы слить состояние, текущую строку сначала надо прочитать. Если сущность уже загружена в этом контексте, SELECT не будет — возьмётся экземпляр из L1.

```java
Order detached = orderFromForm;              // пришёл извне, id заполнен
Order managed = em.merge(detached);          // SELECT, копирование полей
managed.setStatus(Status.PAID);              // изменение отслеживается
detached.setStatus(Status.CANCELLED);        // НИЧЕГО не сохранит: объект detached
// UPDATE выполнится при commit за счёт dirty checking — save() не нужен
```

Ещё одна ловушка merge: поля, которые во входящем объекте null, затирают значения в БД — merge копирует состояние целиком, это не «патч».

**saveOrUpdate(entity)** — Hibernate-специфичный (не JPA) метод: смотрит на id и для transient вызывает save, а для detached — update. В отличие от merge, `update` не копирует состояние, а присоединяет сам переданный объект к сессии — и упадёт с `NonUniqueObjectException`, если экземпляр с таким id уже есть в контексте. В чистом Hibernate метод почти не используется, но Spring Data JPA выбирает persist/merge по собственной проверке новизны. По умолчанию сначала смотрит nullable, непримитивное поле @Version, затем id; `Persistable.isNew()` позволяет задать явное правило. Вручную назначенный id без nullable @Version действительно часто приводит к merge. `@CreatedDate` сам по себе не является стандартным маркером isNew. saveOrUpdate — устаревший Hibernate API; в современном коде используют persist/merge, а не переносят его контракт на repository.save.

**find/getById vs getReference.** `em.find(Entity.class, id)` (и `findById` в Spring Data) — поиск с возможным использованием контекста/L2; SQL нужен, если данных там нет. При отсутствии строки вернёт null/Optional.empty. `em.getReference(Entity.class, id)` (и `getById`/`getReferenceById` в Spring Data) — позволяет получить ссылку без немедленной загрузки состояния; Hibernate часто возвращает прокси, но JPA не гарантирует отсутствие SQL в любой реализации/ситуации. Состояние может загрузиться при обращении к данным. Если строки нет, упадёт `EntityNotFoundException` — но не в точке вызова, а позже, что усложняет отладку. Зато getReference идеален для простановки FK без чтения родителя:

```java
Order order = new Order();
order.setCustomer(em.getReference(Customer.class, customerId));  // без SELECT в customers
em.persist(order);  // INSERT только в orders
```

**Dirty checking и автоматический flush** связывают всё это вместе: Hibernate хранит снапшот состояния managed-сущностей и при flush (автоматически перед commit и перед запросами, которым нужны свежие данные, — FlushModeType.AUTO) сравнивает текущее состояние со снапшотом и генерирует UPDATE только для изменённых. Поэтому паттерн «загрузил → поменял поле → вызвал save()» избыточен: save() нужен только для новых и detached-объектов, managed-сущность синхронизируется сама. Обратная сторона: flush сессии с тысячей managed-сущностей — это тысяча сравнений со снапшотами, одна из причин периодически чистить контекст в batch-сценариях (вопрос 32).

---

### 56. На подумать: UPDATE виден в SQL-логе после flush. Почему запись не сохранилась?

В транзакционном методе загрузили заказ, поменяли status, вызвали `em.flush()`, увидели UPDATE в логах и вернули id. Позже commit завершился ошибкой. Коллега предлагает отправлять событие клиенту сразу после flush: «БД уже всё записала». Какие гарантии были на каждом шаге?

<details><summary>Разбор</summary>

Setter изменил managed-объект в памяти. Flush синхронизировал контекст с БД: Hibernate выполнил SQL, ограничения могли провериться, строки могли заблокироваться. Но транзакция ещё не подтверждена: rollback отменит изменения, а другие обычные транзакции не обязаны их видеть. Commit — отдельная точка, которая также может завершиться ошибкой, например при отложенном ограничении или конфликте сериализации.

```java
@Transactional
public void updateOrder(long id) {
    Order order = em.find(Order.class, id);
    order.setStatus(Status.PAID);
    em.flush();
    throw new IllegalStateException("последующая ошибка");
} // UPDATE откатится при стандартных правилах Spring
```

Flush может произойти автоматически перед запросом с AUTO, даже без явного вызова; save не является его синонимом. `clear()` тоже не означает flush: очистка до синхронизации может потерять несохранённые изменения.

Подтверждение внешнему миру требует определённой границы. После успешного commit можно обработать after-commit callback, но процесс способен упасть между commit и отправкой. Для надёжной доставки событие пишут в outbox в той же транзакции, а отдельный отправитель повторяет доставку с дедупликацией получателя.

Есть ещё неопределённый исход: соединение оборвалось во время COMMIT. Приложение может не знать, зафиксировалась ли транзакция. Простого «всегда повторим UPDATE» недостаточно; нужен ключ операции и проверка результата. Опасный ответ — принимать наличие SQL в логе или выданного sequence id за доказательство успешного commit.

</details>

---

### 22. Связи сущностей: @OneToMany, @ManyToOne, @ManyToMany, mappedBy, CascadeType

**@ManyToOne и владеющая @OneToOne с @JoinColumn** задают owning side: в обычном mapping на этой стороне находится FK. @OneToOne с mappedBy — обратная сторона; варианты с общей PK или join table уточняют отдельно.

```java
@Entity
public class Order {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY, optional = false)
    @JoinColumn(name = "customer_id")
    private Customer customer;
}
```

**@OneToMany(mappedBy = ...) в двунаправленной связи** — обратная сторона. Однонаправленная OneToMany может владеть связью через JoinColumn или join table. Важное понятие — **mappedBy**: указывает, каким полем на противоположной стороне владеется связь. Сторона с `mappedBy` является «зеркальной» (inverse side): Hibernate не читает из неё FK и не пишет по ней изменения — только owning side (`@ManyToOne`) управляет колонкой внешнего ключа. Поэтому при двунаправленной связи обязательно поддерживают обе стороны в консистентном состоянии вспомогательными методами:

```java
@Entity
public class Customer {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToMany(mappedBy = "customer", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Order> orders = new ArrayList<>();

    public void addOrder(Order order) {
        orders.add(order);
        order.setCustomer(this);   // owning side обязана быть заполнена!
    }
}
```

**@ManyToMany** — реализуется через связующую таблицу (join table). Owning side описывает её через `@JoinTable`, обратная — через `mappedBy`. На практике часто заменяют явной сущностью-связкой (например, `Enrollment` между `Student` и `Course`), если у связи есть собственные атрибуты.

```java
@Entity
public class Student {
    @ManyToMany
    @JoinTable(name = "student_course",
               joinColumns = @JoinColumn(name = "student_id"),
               inverseJoinColumns = @JoinColumn(name = "course_id"))
    private Set<Course> courses = new HashSet<>();
}

@Entity
public class Course {
    @ManyToMany(mappedBy = "courses")
    private Set<Student> students = new HashSet<>();
}
```

**CascadeType** — распространение операций с родителя на потомков:

- `PERSIST` — при `persist(parent)` сохранятся и новые потомки.
- `MERGE`, `REMOVE`, `REFRESH`, `DETACH` — аналогично для соответствующих операций.
- `ALL` — все перечисленные.

Внимание: `CascadeType.REMOVE` (и `ALL`) распространяет remove на связанные сущности — не путать с `orphanRemoval` и SQL ON DELETE CASCADE. Для ManyToMany REMOVE опасен: можно удалить общий объект, на который ссылаются другие владельцы, вместо одной строки связующей таблицы.

**orphanRemoval = true** — удаляет из БД потомка, **убранного из коллекции** родителя (сироту), даже если родитель не удалён: `customer.getOrders().remove(order)` при flush сгенерирует `DELETE` для этого заказа. Применяется только к `@OneToOne`/`@OneToMany` (у `@ManyToMany` «сирот» нет — запись может ссылаться на других родителей).

---

### 63. Как описать составной ключ строки заказа и проверить его в JPA?

Пусть номер позиции уникален внутри заказа. В БД нужен `PRIMARY KEY (order_id, line_no)`. Строки `(10, 1)` и `(11, 1)` допустимы, повтор `(10, 1)` нарушает ограничение. Две отдельные уникальности запретили бы даже вторую позицию первого заказа.

С `@EmbeddedId` сущность содержит объект ключа, тип которого помечен `@Embeddable`. Поиск выглядит как `em.find(OrderLine.class, new OrderLineId(10L, 1))`. С `@IdClass` поля ключа объявляют прямо в сущности и связывают с отдельным классом ключа. Для простых скалярных полей их имена и типы должны совпасть. Для переносимости на Jakarta Persistence 3.1 класс составного ключа должен быть публичным, сериализуемым, иметь публичный конструктор без аргументов и определять равенство и хеширование по компонентам ключа.

Проверьте сохранение двух разрешённых пар, чтение по заново созданному равному ключу и отклонение повтора при записи в БД. Первичный ключ сохранённой сущности не меняют. Если позиции часто перенумеровываются, выделите стабильный идентификатор, а текущий номер храните отдельно.

---

### 31. equals и hashCode в JPA-сущностях

Генерировать `equals`/`hashCode` по всем полям (что делает `@Data` у Lombok или «generate all» в IDE) для сущности — ошибка. Сущность мутабельна и имеет жизненный цикл: до `persist` у неё нет id, поля меняются, а hashCode объекта в `HashSet`/`HashMap` меняться не должен.

Конкретные поломки:

- id генерируется при INSERT: сущность до `save()` и после него имеет разный hashCode — положили в `HashSet` до сохранения, после сохранения объект там не находится;
- «все поля» включают LAZY-коллекции и связи — простой `set.contains(e)` тянет лишние запросы и падает с `LazyInitializationException`;
- две разные строки в БД с совпадающими полями считаются равными — семантически неверно: идентичность сущности определяется ключом, а не текущим состоянием.

Варианты решения:

**1. Бизнес-ключ (лучший)** — неизменное уникальное поле: email, ISBN, номер договора. `equals`/`hashCode` только по нему:

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE)
    private Long id;

    @Column(nullable = false, unique = true)
    private String email;  // бизнес-ключ: в этом дизайне не меняется после создания

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof User other)) return false;
        return email != null && email.equals(other.getEmail());
    }

    @Override
    public int hashCode() {
        return 31; // одинаково для сущности и её прокси
    }
}
```

Два нюанса, которые всплывают follow-up'ами. Первый: обращаться к полям другого объекта нужно через геттер (`other.getEmail()`, а не `other.email`) — у Hibernate-прокси поля не инициализированы, и прямой доступ к полю вернёт null до загрузки сущности. Второй: фиксированная константа соблюдает контракт с прокси, но даёт коллизии в больших hash-коллекциях. При действительно неизменном бизнес-ключе допустим hashCode по ключу; тогда учитывают инициализацию прокси. Email подходит лишь в модели, где его нельзя изменить.

**2. Только id, с оговорками**: `equals` по id (объект с null-id равен только самому себе), `hashCode` — константа, например `31` для данного типа, чтобы сущность не «переезжала» по бакетам при присвоении id. Работает, но до persist все новые сущности «разные», что иногда неудобно.

В обоих вариантах: `instanceof` вместо сравнения `getClass()` (иначе equals ломается на Hibernate-прокси) и не включайте изменяемые поля/коллекции. Если сравнение бизнес-ключа инициализирует прокси, стоимость и допустимость SQL нужно учитывать; универсального equals для любой иерархии сущностей нет.

---

### 23. Оптимистичная блокировка через @Version в JPA

JPA поддерживает оптимистичную блокировку из коробки: достаточно добавить в сущность поле с аннотацией **@Version** (для переносимого mapping Jakarta Persistence 3.1, например `long`/`Long` или `java.sql.Timestamp`; поддержка `Instant` как версии зависит от более новой спецификации/провайдера).

```java
@Entity
public class Account {
    @Id
    private Long id;

    private BigDecimal balance;

    @Version
    private long version;   // управляется Hibernate, вручную не изменять
}
```

Механика: при `UPDATE`/`DELETE` Hibernate добавляет версию в условие и инкрементирует её:

```sql
UPDATE account SET balance = ?, version = version + 1
WHERE id = ? AND version = ?;
```

Если параллельная транзакция уже изменила строку, условие `version = ?` не совпадёт, запрос затронет 0 строк, и Hibernate выбросит `OptimisticLockException` (в Spring конвертируется в `ObjectOptimisticLockingFailureException`). Приложение обязано обработать конфликт: перечитать сущность и повторить операцию (retry), либо вернуть пользователю ошибку «данные изменились, обновите форму».

Сценарий конфликта:

```java
// Транзакция A                          // Транзакция B
Account a = repo.findById(1L).get();     Account b = repo.findById(1L).get();
a.withdraw(100);                         b.withdraw(200);
em.flush();    // version 3 → 4         em.flush();    // OptimisticLockException:
                                        // в БД уже version = 4
```

Плюсы: нет долгих блокировок БД, высокая конкурентность, работает в том числе с detached-сущностями (типичный web-сценарий «прочитали форму → пользователь подумал → сохранили»). Минусы: конфликт обнаруживается постфактум, нужна логика повторения.

Для пессимистичной блокировки в JPA используют `em.lock(entity, LockModeType.PESSIMISTIC_WRITE)` или `@Lock(LockModeType.PESSIMISTIC_WRITE)` на методе репозитория — под капотом это `SELECT ... FOR UPDATE`.

---

### 24. @Transactional в Spring: propagation и типичные ловушки

**@Transactional** — декларативное управление транзакциями через AOP-прокси: Spring открывает транзакцию перед вызовом метода и фиксирует (или откатывает) после его завершения.

**Propagation (распространение)** — что делать, если метод вызывается в контексте уже существующей транзакции:

- **REQUIRED** (по умолчанию) — присоединиться к существующей транзакции, иначе создать новую. Откат общий: ошибка внутри любого REQUIRED-метода помечает всю транзакцию как rollback-only.
- **REQUIRES_NEW** — приостановить текущую транзакцию и создать новую независимую. Внутренняя коммитится/откатывается сама по себе. Типичное применение — аудит/логирование, которое должно сохраниться даже при откате основной транзакции. Цена: занимает второе соединение с БД.
- **NESTED** — выполнить внутри вложенной транзакции через `SAVEPOINT`: при откате вложенной части откатывается только она (до savepoint), внешняя продолжается. Поддержка зависит от transaction manager и драйвера: JDBC-менеджеры используют savepoints. JpaTransactionManager при специальной настройке может дать savepoints JDBC-работе, но не откатывает состояние JPA persistence context как вложенную JPA-транзакцию.

Другие режимы: `SUPPORTS` (транзакция опциональна), `NOT_SUPPORTED` (приостановить и выполнить без транзакции), `MANDATORY` (требует существующую, иначе исключение), `NEVER` (запрещает).

**Типичные ловушки:**

1. **Self-invocation**: `@Transactional` работает через прокси, поэтому вызов `this.inner()` из другого метода того же бина **не проходит через прокси** — транзакция не создаётся и propagation не применяется. Решение: вынести метод в отдельный бин, инжектить бин самого в себя, или использовать `TransactionTemplate`.

```java
@Service
public class OrderService {
    public void create(OrderDto dto) {
        this.saveWithAudit(dto); // ПЛОХО: @Transactional на saveWithAudit НЕ сработает
    }

    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void saveWithAudit(OrderDto dto) { /* ... */ }
}
```

2. **Откат только по RuntimeException**: по умолчанию транзакция откатывается при unchecked-исключениях (`RuntimeException`, `Error`), а **checked-исключения приводят к commit**. Для отката на checked-исключения: `@Transactional(rollbackFor = Exception.class)` или конкретный класс.

3. **Приватные/внутренние методы**: для интерфейсного прокси нужны public-методы интерфейса. Class-based прокси в Spring 6+ поддерживает также protected/package-visible методы; private не перехватываются. Вызов извне через прокси остаётся необходимым.

4. **Прокси-классы**: по умолчанию используются JDK-прокси на интерфейсах; для классов — CGLIB (в Spring Boot 2+ по умолчанию). Для CGLIB final-класс нельзя расширить, а final-метод нельзя перехватить через override. JDK-прокси перехватывает интерфейсный вызов и может делегировать final-методу final target-класса.

5. Дополнительные атрибуты: `readOnly = true` (подсказка для оптимизации, конкретная интеграция Spring/Hibernate может менять flush mode и режим чтения сущностей; это не универсальный запрет записи на уровне БД), `timeout`, `isolation` (переопределение уровня изоляции БД), `noRollbackFor`.

---

### 20. Стратегии загрузки LAZY vs EAGER и LazyInitializationException

**FetchType** определяет, когда загружается связанная сущность/коллекция:

- **LAZY (ленивая)** — связь не загружается вместе с родителем; Hibernate подставляет прокси (для одиночных связей) или PersistentCollection (для коллекций), а реальный SELECT выполняется при первом обращении к данным связи. По умолчанию LAZY для `@OneToMany` и `@ManyToMany`.
- **EAGER (жадная)** — связь загружается немедленно вместе с родителем (отдельным SELECT или JOIN). По умолчанию EAGER для `@ManyToOne` и `@OneToOne` — это частая скрытая причина проблем, обычно явно выбирают LAZY и задают загрузку под сценарий. По JPA EAGER — требование загрузить данные, LAZY — подсказка; реализация ленивой to-one загрузки зависит также от прокси/bytecode enhancement.

```java
@Entity
public class Order {
    @ManyToOne(fetch = FetchType.LAZY)   // переопределяем дефолтный EAGER
    private Customer customer;

    @OneToMany(mappedBy = "order")       // LAZY по умолчанию
    private List<OrderItem> items;
}
```

**LazyInitializationException** возникает при обращении к неинициализированной LAZY-связи вне активного Persistence Context (сессия закрыта, сущность detached). Типичный сценарий: сервис вернул сущность в контроллер, транзакция завершилась, а сериализатор JSON обратился к `order.getItems()`.

Способы избежать:

1. **Загрузить данные внутри транзакции** заранее: `JOIN FETCH`, EntityGraph, или принудительная инициализация `Hibernate.initialize(order.getItems())` до выхода из `@Transactional`-метода.
2. **Open Session in View** (`spring.jpa.open-in-view=true`, в Spring Boot включён по умолчанию) — держит сессию открытой до конца HTTP-запроса. Маскирует проблему N+1 и держит соединение с БД дольше необходимого; не устраняет потребность проектировать загрузку. Часто его отключают, чтобы SQL был ограничен сервисной границей; решение зависит от архитектуры.
3. **DTO-проекции** — возвращать из сервисного слоя не сущности, а DTO с заранее выбранными полями; сущности не покидают транзакционную границу.
4. `@Transactional(readOnly = true)` на сервисном чтении помогает задать границу, но не разрешает обращаться к ленивым данным после возврата метода и закрытия контекста.

Принцип: LAZY по умолчанию + точечная жадная загрузка там, где связь реально нужна (через JOIN FETCH / EntityGraph) — безопаснее, чем EAGER, который грузит лишнее при каждом чтении сущности.

---

### 19. Проблема N+1 и способы её решения

**Проблема N+1** — ситуация, когда загрузка N сущностей со связанными данными порождает 1 запрос на список + N дополнительных запросов (по одному на каждую связь), вместо одного-двух запросов.

```java
// 1 запрос: SELECT * FROM orders
List<Order> orders = em.createQuery("SELECT o FROM Order o", Order.class).getResultList();

for (Order order : orders) {
    // LAZY-загрузка customer при первом обращении: +N запросов SELECT * FROM customer WHERE id = ?
    System.out.println(order.getCustomer().getName());
}
```

Если у каждого заказа отдельный customer и данные не найдены в кэшах, при N = 1000 возможен 1001 запрос — типичная причина деградации производительности в проде.

Способы решения:

**1. JOIN FETCH в JPQL** — загрузить связь одним запросом с соединением:

```java
List<Order> orders = em.createQuery(
        "SELECT o FROM Order o JOIN FETCH o.customer", Order.class)
    .getResultList();
// Один запрос: SELECT o.*, c.* FROM orders o JOIN customer c ON ...
```

Внимание: `JOIN FETCH` коллекции (`JOIN FETCH o.items`) размножает строки родителя и несовместим с пагинацией на уровне БД (Hibernate делает пагинацию в памяти с предупреждением `HHH000104`). Выбор — batch fetching либо двухэтапная выборка id страницы и связей (вопрос 58). To-one JOIN FETCH обычно не размножает родителей. EAGER тоже может породить N+1 через secondary SELECT: LAZY — не единственная причина.

**2. EntityGraph** — декларативное описание графа загрузки без изменения запроса:

```java
@Entity
@NamedEntityGraph(name = "Order.withCustomer",
        attributeNodes = @NamedAttributeNode("customer"))
public class Order { /* ... */ }

EntityGraph<?> graph = em.getEntityGraph("Order.withCustomer");
Order order = em.find(Order.class, 1L,
        Map.of("jakarta.persistence.fetchgraph", graph));
```

**3. Batch fetching (`@BatchSize`)** — загружать ленивые связи пачками по N штук через `IN`:

```java
@OneToMany(mappedBy = "order")
@BatchSize(size = 50)
private List<OrderItem> items;
// Вместо N запросов — N/50 запросов вида WHERE order_id IN (?, ?, ..., ?)
```

Глобально: `hibernate.default_batch_fetch_size=50`.

**4. DTO-проекции** — когда сущность целиком не нужна, выбирать только требуемые поля одним запросом:

```java
List<OrderSummary> list = em.createQuery(
        "SELECT new com.example.OrderSummary(o.id, c.name, o.total) " +
        "FROM Order o JOIN o.customer c", OrderSummary.class)
    .getResultList();
```

Как обнаружить N+1: включить логирование SQL (`spring.jpa.show-sql` / `hibernate.generate_statistics`) и посчитать запросы на один HTTP-вызов; мониторинг через p6spy, datasource-proxy или APM (гистограмма запросов на эндпоинт).

---

### 58. Как загрузить страницу родителей с коллекциями без пагинации в памяти?

Пагинируют набор родителей, затем загружают связи только для выбранных id. LIMIT поверх JOIN коллекции ограничивает строки соединения, а не число уникальных родителей: один заказ с сотней позиций способен занять всю страницу. Hibernate может перенести ограничение в память, что делает запрос неожиданно дорогим.

```java
List<Long> ids = em.createQuery(
        "select o.id from Order o order by o.createdAt desc, o.id desc", Long.class)
    .setFirstResult(offset)
    .setMaxResults(pageSize)
    .getResultList();

List<Order> orders = ids.isEmpty() ? List.of() : em.createQuery(
        "select distinct o from Order o left join fetch o.items where o.id in :ids",
        Order.class)
    .setParameter("ids", ids)
    .getResultList();
```

Второй запрос не сохраняет порядок списка id: восстановите его в Java по позиции id или задайте эквивалентный ORDER BY. Обработка пустого списка не зависит от особенностей SQL-диалекта. Для больших страниц нужно учитывать размер IN и количество дочерних строк: pageSize=20 не ограничивает размер двадцати огромных коллекций.

Два запроса на READ COMMITTED могут видеть разные состояния: заказ может исчезнуть или поменять сортировочное поле между ними. Стабильный snapshot в одной REPEATABLE READ транзакции решает часть этих требований ценой более долгой транзакции; иногда допустима короткая страница или повторное чтение. Если UI нужны только итоги, DTO-агрегация может быть дешевле полной загрузки коллекции.

В Hibernate 6.x настройка `hibernate.query.fail_on_pagination_over_collection_fetch=true` помогает обнаружить опасную комбинацию. To-one fetch join обычно не размножает родителей, поэтому сам по себе не создаёт той же проблемы. Частая ошибка — «добавим DISTINCT»: он не превращает LIMIT по joined rows в LIMIT по заказам.

---

### 21. Кэши Hibernate: первый уровень (L1), второй уровень (L2), кэш запросов

**L1 (First-Level Cache)** — Persistence Context: всегда включён, живёт в рамках одного EntityManager/транзакции. Гарантирует, что повторный `find(id)` вернёт тот же объект без запроса в БД, и обеспечивает identity map. Очистить: `em.clear()`, `em.detach(entity)`.

**L2 (Second-Level Cache)** — общий кэш уровня SessionFactory, разделяемый между транзакциями и сессиями. По умолчанию выключен; требует провайдера (Ehcache, Caffeine, Infinispan, Hazelcast) и явной разметки сущностей.

Настройка с Caffeine (или Ehcache аналогично):

```properties
# application.properties
spring.jpa.properties.hibernate.cache.use_second_level_cache=true
spring.jpa.properties.hibernate.cache.region.factory_class=org.hibernate.cache.jcache.JCacheRegionFactory
```

```xml
<!-- зависимость: org.hibernate.orm:hibernate-jcache + com.github.ben-manes.caffeine:jcache
     + javax.cache:cache-api -->
```

```java
@Entity
@Cacheable
@org.hibernate.annotations.Cache(usage = CacheConcurrencyStrategy.READ_WRITE)
public class Country {   // справочник: читается часто, меняется редко
    @Id private Long id;
    private String name;
}
```

Стратегии конкурентного доступа L2:

- `READ_ONLY` — для неизменяемых справочников (самая быстрая).
- `NONSTRICT_READ_WRITE` — допускает кратковременную неконсистентность, кэш инвалидируется после commit.
- `READ_WRITE` — консистентность через блокировки, подходит для часто читаемых и редко пишущихся данных.
- `TRANSACTIONAL` — полная транзакционность (Infinispan).

**Query Cache (кэш запросов)** — кэширует **результат запроса** с учётом текста и параметров. Представление entity-результатов зависит от версии Hibernate: исторически это были id, а Hibernate 6 изменял layout вплоть до хранения состояния. Нельзя универсально обещать «только id». Проверяют документацию и конфигурацию целевой версии. Включается отдельно и требует пометки каждого запроса:

```properties
spring.jpa.properties.hibernate.cache.use_query_cache=true
```

```java
List<Country> countries = em.createQuery("SELECT c FROM Country c", Country.class)
        .setHint("org.hibernate.cacheable", true)
        .getResultList();
```

Практика: L2 эффективен для read-mostly данных с доступом по первичному ключу; кэш запросов оправдан редко (малейший INSERT/UPDATE в таблицу инвалидирует все кэшированные запросы по ней); сравнивать его с кэшем DTO в Redis нужно по конкретной нагрузке и требованиям к инвалидированию.

---

### 32. Пакетная вставка и обновление в Hibernate (batch)

По умолчанию Hibernate выполняет каждый INSERT/UPDATE отдельным round-trip. Для массовых операций включают JDBC batching — драйвер накапливает операторы и отправляет их пачкой:

```properties
spring.jpa.properties.hibernate.jdbc.batch_size=30
spring.jpa.properties.hibernate.order_inserts=true
spring.jpa.properties.hibernate.order_updates=true
```

`order_inserts`/`order_updates` сортируют операторы так, чтобы однотипные попадали в один батч.

**Ловушка №1 — IDENTITY.** `GenerationType.IDENTITY` требует выполнить INSERT немедленно, чтобы получить сгенерированный id, поэтому батчинг молча отключается. На PostgreSQL для этого обычно выбирают `SEQUENCE`; возможны также назначаемые приложением ключи:

```java
@Id
@GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "order_seq")
@SequenceGenerator(name = "order_seq", sequenceName = "order_seq", allocationSize = 50)
private Long id;
```

`allocationSize = 50` позволяет Hibernate резервировать id блоками и не ходить за последовательностью на каждую вставку.

**Ловушка №2 — раздувание persistence context.** Все сохранённые сущности остаются в L1-кэше: растёт память и стоимость dirty checking при каждом flush. Лечится периодическими `flush` + `clear`:

```java
for (int i = 0; i < items.size(); i++) {
    em.persist(items.get(i));
    if ((i + 1) % 50 == 0) {
        em.flush();  // отправить накопленный батч в БД
        em.clear();  // очистить persistence context
    }
}
em.flush(); // отправить оставшийся хвост до завершения обработки
em.clear();
```

Для чистой массовой загрузки без каскадов и dirty checking есть `StatelessSession` — без persistence context, L1-кэша и снапшотов состояния:

```java
try (StatelessSession session = sessionFactory.openStatelessSession()) {
    Transaction tx = session.beginTransaction();
    for (Item item : items) {
        session.insert(item);
    }
    tx.commit();
}
```

Оговорки: `StatelessSession` не выполняет каскады (дочерние сущности вставляем руками) ; поведение L2-кэша зависит от версии Hibernate, здесь описывается модель Hibernate 6.x, а не обещание для всех выпусков. Для действительно больших объёмов ORM вообще не нужен: `COPY` в PostgreSQL или прямой JDBC `addBatch()/executeBatch()` могут быть существенно быстрее; коэффициент зависит от схемы, драйвера, индексов и нагрузки, его измеряют.

---

### 59. Чем batch fetching отличается от JDBC batching и размера транзакции?

Это три независимых размера. Batch fetching уменьшает N+1 при чтении связей. JDBC batching группирует совместимые INSERT/UPDATE/DELETE. Размер транзакции определяет атомарность, удержание блокировок и объём отката. Совпадение чисел в настройках не связывает их автоматически.

```properties
hibernate.default_batch_fetch_size=32
hibernate.jdbc.batch_size=50
hibernate.order_inserts=true
```

При чтении 100 заказов ленивые коллекции могут загрузиться несколькими запросами вместо ста, но точное число зависит от состояния контекста, доступов и Hibernate. При записи batch_size=50 не означает commit каждых 50 строк и не гарантирует один сетевой пакет: это зависит также от JDBC-драйвера и формы SQL.

Для ограниченной памяти в длинной транзакции делают flush/clear каждые N объектов, включая хвост:

```java
for (int i = 0; i < rows.size(); i++) {
    em.persist(rows.get(i));
    if ((i + 1) % 50 == 0) {
        em.flush();
        em.clear();
    }
}
em.flush();
em.clear();
```

Эти flush не освобождают блокировки до commit. После clear ссылки на связанные сущности могут стать detached; каскады PERSIST и повторное использование старого родителя требуют внимания. Для по-настоящему коротких транзакций импорт разбивают на chunks с отдельным commit, но тогда «весь файл или ничего» больше не гарантируется: нужны checkpoint, уникальные ключи и безопасный повтор.

Проверяют поведение измерением JDBC batch-выполнений, числа SQL, памяти и throughput. Один `saveAll` не доказывает батчинг, а красивые одинаковые INSERT в логе его не опровергают: SQL-лог и round-trip — разные уровни наблюдения.

---

### 57. Почему bulk UPDATE через JPQL оставляет старые значения в EntityManager?

Bulk DML работает с набором строк непосредственно в БД и не синхронизирует уже загруженные managed-объекты. Это другая модель, чем изменение поля каждой сущности с dirty checking. Стандартный bulk UPDATE также не выполняет обычную автоматическую проверку @Version для каждой строки и не запускает entity callbacks как цикл remove/update сущностей.

```java
Order order = em.find(Order.class, orderId);
em.flush(); // сохранить накопленные изменения до bulk DML
int changed = em.createQuery(
        "update Order o set o.status = :newStatus where o.status = :oldStatus")
    .setParameter("newStatus", Status.EXPIRED)
    .setParameter("oldStatus", Status.NEW)
    .executeUpdate();
em.clear(); // прежний order теперь detached
Order fresh = em.find(Order.class, orderId);
```

Последующий `find` без clear может вернуть старый экземпляр из L1. Ещё опаснее позже записать его устаревшее состояние обратно и затереть результат bulk DML. Вместо clear можно обновить конкретные объекты через refresh, если их мало; отдельный короткий persistence context для массовой операции часто проще.

В Spring Data `@Modifying(flushAutomatically = true, clearAutomatically = true)` позволяет явно задать границы, но очистка отсоединяет все сущности контекста, и это должно быть понятно вызывающему коду. Нельзя терять несохранённые изменения при автоматическом clear.

Если bulk-запрос должен конкурировать с версиями, включайте проверку ожидаемой версии и её изменение в SQL/JPQL явно либо выбирайте другой протокол. Ограничения БД и триггеры при этом продолжают работать. Частая ошибка — считать `UPDATE ... WHERE status=...` эквивалентным загрузке тысяч сущностей и вызову setter с теми же каскадами и callback.

---

### 33. Частые антипаттерны в работе с БД

Короткий список того, что спрашивают follow-up'ами и что регулярно встречается в ревью:

- **SELECT \*** — тянем все колонки, включая тяжёлые `text`/`jsonb`, хотя нужны три. Лишний трафик и память, ухудшение шансов на покрывающий индекс (index only scan), хрупкость к изменениям схемы. ORM-аналог — грузить всю сущность ради двух полей; лечится DTO-проекциями.
- **EAGER на всех связях** — заставляет загружать ненужный граф и может приводить к большим JOIN либо дополнительным SELECT/N+1: EAGER определяет необходимость загрузки, а не обязательный SQL JOIN. Одновременный JOIN нескольких независимых to-many коллекций перемножает строки; одна `@ManyToMany` сама по себе такого перемножения коллекций не означает. Обычно связи задают LAZY, а нужный граф конкретного сценария — через `JOIN FETCH` или entity graph, проверяя фактический SQL.
- **Нет индексов под внешними ключами** — PostgreSQL индекс под FK сам не создаёт. Удаление родительской строки и проверка FK идут полным сканом дочерней таблицы, join'ы по FK медленные. Индекс под каждый FK нужен почти всегда.
- **Запросы в цикле** — разновидность N+1: `for (id : ids) repo.findById(id)` даёт тысячу round-trip'ов вместо одного `findAllById(ids)` / `WHERE id IN (...)`.
- **Деньги во float/double** — двоичное представление не хранит десятичные дроби точно: `0.1 + 0.2 != 0.3`. Для денег — `BigDecimal` в Java и `NUMERIC`/`DECIMAL` в БД, никаких `FLOAT`/`DOUBLE`. Смежная ошибка — суммировать `double` в стриме и округлять «в конце».
- Бонусом, если спросят ещё: выборки без `LIMIT` на потенциально больших таблицах, индексы «на всякий случай» по каждой колонке (тормозят запись и раздувают БД), транзакция, открытая на время HTTP-вызова, `LIKE '%...%'` с ведущим wildcard — B-Tree индекс при таком шаблоне не используется.

---

[← Вернуться на главную страницу](README.md)
