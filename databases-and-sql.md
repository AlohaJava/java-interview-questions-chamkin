# Базы Данных, SQL и Hibernate / JPA

В данном разделе представлены канонические вопросы по реляционным базам данных, языку SQL, транзакциям и стеку Hibernate / JPA. Ответы рассчитаны на уровень Middle/Senior и включают практические примеры кода.

## Оглавление

**SQL и реляционные БД**
1. [Что такое SQL? Группы операторов: DDL, DML, DCL, TCL](#1-что-такое-sql-группы-операторов-ddl-dml-dcl-tcl)
2. [Виды JOIN: INNER, LEFT, RIGHT, FULL OUTER, CROSS](#2-виды-join-inner-left-right-full-outer-cross)
3. [GROUP BY и HAVING: агрегация и фильтрация групп](#3-group-by-и-having-агрегация-и-фильтрация-групп)
4. [UNION, UNION ALL, INTERSECT, EXCEPT: различия](#4-union-union-all-intersect-except-различия)
5. [Подзапросы и CTE (WITH)](#5-подзапросы-и-cte-with)
6. [Оконные функции: ROW_NUMBER, RANK, DENSE_RANK, LAG/LEAD](#6-оконные-функции-row_number-rank-dense_rank-laglead)
7. [Нюансы работы с NULL в SQL](#7-нюансы-работы-с-null-в-sql)
8. [Ограничения целостности: PRIMARY KEY vs UNIQUE vs FOREIGN KEY](#8-ограничения-целостности-primary-key-vs-unique-vs-foreign-key)
9. [Индексы: B-Tree, Hash, составные и покрывающие индексы](#9-индексы-b-tree-hash-составные-и-покрывающие-индексы)
10. [SELECT * против явного списка колонок; count(*) против count(column)](#10-select--против-явного-списка-колонок-count-против-countcolumn)
11. [Нормализация (1NF, 2NF, 3NF) и денормализация](#11-нормализация-1nf-2nf-3nf-и-денормализация)
12. [Хранимые процедуры и триггеры](#12-хранимые-процедуры-и-триггеры)
13. [EXPLAIN и план выполнения запроса](#13-explain-и-план-выполнения-запроса)

**Транзакции**
14. [Принципы ACID](#14-принципы-acid)
15. [Уровни изоляции транзакций и аномалии чтения](#15-уровни-изоляции-транзакций-и-аномалии-чтения)
16. [Блокировки в БД: оптимистичная и пессимистичная модели, deadlock](#16-блокировки-в-бд-оптимистичная-и-пессимистичная-модели-deadlock)
17. [MVCC: многоверсионный контроль конкурентного доступа](#17-mvcc-многоверсионный-контроль-конкурентного-доступа)

**Hibernate / JPA**
18. [ORM и JPA: Entity, EntityManager, Persistence Context](#18-orm-и-jpa-entity-entitymanager-persistence-context)
19. [Проблема N+1 и способы её решения](#19-проблема-n1-и-способы-её-решения)
20. [Стратегии загрузки LAZY vs EAGER и LazyInitializationException](#20-стратегии-загрузки-lazy-vs-eager-и-lazyinitializationexception)
21. [Кэши Hibernate: первый уровень (L1), второй уровень (L2), кэш запросов](#21-кэши-hibernate-первый-уровень-l1-второй-уровень-l2-кэш-запросов)
22. [Связи сущностей: @OneToMany, @ManyToOne, @ManyToMany, mappedBy, CascadeType](#22-связи-сущностей-onetomany-manytoone-manytomany-mappedby-cascadetype)
23. [Оптимистичная блокировка через @Version в JPA](#23-оптимистичная-блокировка-через-version-в-jpa)
24. [@Transactional в Spring: propagation и типичные ловушки](#24-transactional-в-spring-propagation-и-типичные-ловушки)
25. [Миграции БД: Flyway и Liquibase](#25-миграции-бд-flyway-и-liquibase)

**Практика и эксплуатация**
26. [Пагинация: LIMIT/OFFSET против keyset (cursor) pagination](#26-пагинация-limitoffset-против-keyset-cursor-pagination)
27. [Партиционирование таблиц: range, list и hash в PostgreSQL](#27-партиционирование-таблиц-range-list-и-hash-в-postgresql)
28. [Репликация: синхронная и асинхронная, read replicas и replication lag](#28-репликация-синхронная-и-асинхронная-read-replicas-и-replication-lag)
29. [Шардинг: отличие от репликации и выбор ключа шардирования](#29-шардинг-отличие-от-репликации-и-выбор-ключа-шардирования)
30. [Пул соединений: HikariCP и выбор размера пула](#30-пул-соединений-hikaricp-и-выбор-размера-пула)
31. [equals и hashCode в JPA-сущностях](#31-equals-и-hashcode-в-jpa-сущностях)
32. [Пакетная вставка и обновление в Hibernate (batch)](#32-пакетная-вставка-и-обновление-в-hibernate-batch)
33. [Частые антипаттерны в работе с БД](#33-частые-антипаттерны-в-работе-с-бд)

---

### 1. Что такое SQL? Группы операторов: DDL, DML, DCL, TCL

**SQL (Structured Query Language)** — декларативный язык структурированных запросов, предназначенный для описания, изменения и выборки данных в реляционных СУБД. Ключевая особенность SQL — декларативность: мы описываем *что* хотим получить, а оптимизатор СУБД сам решает, *как* выполнить запрос (какой план выбрать, какие индексы использовать).

Операторы SQL принято делить на четыре группы:

- **DDL (Data Definition Language)** — язык определения данных. Управляет структурой объектов БД: `CREATE`, `ALTER`, `DROP`, `TRUNCATE`. В MySQL и Oracle DDL-операторы неявно фиксируют (commit) текущую транзакцию, а в PostgreSQL DDL транзакционен — `CREATE TABLE` можно откатить вместе с остальной транзакцией (см. вопрос 14).
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

### 3. GROUP BY и HAVING: агрегация и фильтрация групп

**GROUP BY** группирует строки с одинаковыми значениями указанных колонок в одну строку-итог, к которой применяются агрегатные функции: `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`.

Важное правило: в `SELECT` при наличии `GROUP BY` можно указывать только колонки, перечисленные в `GROUP BY`, либо агрегатные функции от других колонок (MySQL исторически позволял нарушать это правило, что приводило к недетерминированным результатам; в PostgreSQL и стандарте SQL это ошибка).

Порядок логической обработки запроса: `FROM` → `WHERE` → `GROUP BY` → `HAVING` → `SELECT` → `DISTINCT` → `ORDER BY` → `LIMIT`. Из этого следует главное отличие:

- **WHERE** фильтрует *строки* до группировки и не может содержать агрегатных функций.
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

Особенности **NULL при группировке**: все строки с `NULL` в группируемой колонке попадают в **одну общую группу** (NULL трактуется как отдельное значение для группировки, хотя `NULL = NULL` ложно при сравнении). Агрегатные функции `SUM`, `AVG`, `MIN`, `MAX` игнорируют `NULL` в вычисляемой колонке; `COUNT(*)` считает все строки, а `COUNT(column)` — только строки с не-NULL значением:

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
- **Коррелированный** — ссылается на колонки внешнего запроса и логически выполняется заново для каждой строки внешнего запроса (часто медленнее, чем эквивалентный JOIN).

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

**CTE (Common Table Expression, `WITH`)** — именованный временный результирующий набор, видимый в рамках одного запроса. Улучшает читаемость, позволяет переиспользовать промежуточные результаты и писать рекурсивные запросы (`WITH RECURSIVE`).

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
           ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC) AS rn,
           RANK()       OVER (PARTITION BY department_id ORDER BY salary DESC) AS rnk,
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
       ROUND(100.0 * revenue / LAG(revenue) OVER (ORDER BY month) - 100, 2) AS growth_pct
FROM monthly_revenue;
```

Классическое применение `ROW_NUMBER` — дедупликация данных (оставить по одной «свежей» строке на ключ):

```sql
DELETE FROM events
WHERE id IN (
    SELECT id FROM (
        SELECT id,
               ROW_NUMBER() OVER (PARTITION BY user_id, event_type
                                  ORDER BY created_at DESC) AS rn
        FROM events
    ) t
    WHERE rn > 1
);
```

---

### 7. Нюансы работы с NULL в SQL

**NULL** — это не значение, а маркер отсутствия значения («неизвестно»). Из-за этого SQL использует **тривалентную логику (Three-Valued Logic)**: результат любого сравнения может быть `TRUE`, `FALSE` или `UNKNOWN`.

Ключевые ловушки:

1. **Любое сравнение с NULL даёт UNKNOWN**, а не TRUE/FALSE. `NULL = NULL` → UNKNOWN, `NULL <> 5` → UNKNOWN. Строки с результатом UNKNOWN отфильтровываются из `WHERE`. Поэтому NULL проверяют только через `IS NULL` / `IS NOT NULL`.

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

4. **NULL в UNIQUE-ограничении**: по стандарту и в PostgreSQL в UNIQUE-колонке может быть **несколько NULL** (NULL ≠ NULL), в SQL Server — только один, в MySQL (InnoDB) — несколько.

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

**UNIQUE** — альтернативный ключ (например, email). Отличается от PK тем, что допускает NULL и их может быть несколько.

**FOREIGN KEY** — ссылка на PK/UNIQUE родительской таблицы. **FK может быть NULL** — это означает «связь не установлена», и такая строка не проверяется на существование в родительской таблице. Для не-NULL значений БД гарантирует, что они существуют в родителе.

Поведение при удалении/обновлении родителя задаётся правилами:

- `ON DELETE RESTRICT` / `NO ACTION` — запретить удаление, если есть потомки (по умолчанию).
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

### 9. Индексы: B-Tree, Hash, составные и покрывающие индексы

**Индекс** — отдельная структура данных, ускоряющая поиск ценой дополнительного места и замедления записи (каждый `INSERT/UPDATE/DELETE` обновляет индексы).

**B-Tree** (сбалансированное дерево) — тип индекса по умолчанию в PostgreSQL, MySQL, Oracle. Поддерживает поиск по равенству, диапазоны (`>`, `<`, `BETWEEN`), сортировку (`ORDER BY`), префиксный поиск `LIKE 'abc%'`. Высота дерева обычно 3–4 уровня, поиск — O(log N).

**Hash** — поддерживает только поиск по точному равенству (`=`), O(1). Не подходит для диапазонов и сортировки. В PostgreSQL используется редко, в MySQL — в MEMORY-таблицах и адаптивном хеш-индексе InnoDB.

Также существуют специализированные индексы: **GIN/GiST** (полнотекстовый поиск, JSONB, массивы), **BRIN** (очень большие упорядоченные таблицы), **Bitmap** (низкая кардинальность, аналитика).

**Составной индекс** `INDEX(a, b, c)` работает по правилу **левого префикса (leftmost prefix)**: он эффективен для условий на `a`; на `a, b`; на `a, b, c` — но не для условий только на `b` или `c`. Порядок колонок критичен: сначала колонки равенства, затем диапазона/сортировки.

**Покрывающий индекс (covering index)** содержит все колонки запроса, поэтому СУБД выполняет **Index-Only Scan** без обращения к таблице:

```sql
CREATE INDEX idx_orders_customer ON orders(customer_id, status, created_at);

-- Index-Only Scan: все нужные колонки есть в индексе
SELECT customer_id, created_at
FROM orders
WHERE customer_id = 42 AND status = 'PAID';
-- В PostgreSQL покрытие можно расширить: CREATE INDEX ... ON orders(customer_id) INCLUDE (total);
```

**Когда индекс НЕ используется:**

- **Функция или выражение над колонкой**: `WHERE UPPER(email) = 'A@B.RU'`, `WHERE created_at::date = '2024-01-01'`, `WHERE salary * 12 > 1000000`. Решение — функциональный индекс `CREATE INDEX ON employees (UPPER(email))` или переписать условие так, чтобы колонка была «чистой» (`sargable`): `WHERE salary > 1000000 / 12`.
- **Leading wildcard**: `LIKE '%abc'` — нет префикса для спуска по дереву (работает только `LIKE 'abc%'`).
- **Низкая селективность**: если условие возвращает большую долю таблицы (обычно > 5–10%), оптимизатор осознанно выбирает Seq Scan — он дешевле случайных чтений.
- **Несовпадение типов**: сравнение `varchar`-колонки с числом может привести к неявному касту и отказу от индекса.
- **Небольшие таблицы**: читать таблицу целиком быстрее.
- **Устаревшая статистика**: оптимизатор принимает решения на основе статистики (`ANALYZE`).

---

### 10. SELECT * против явного списка колонок; count(*) против count(column)

**SELECT * в промышленном коде — плохая привычка**, хотя в ad-hoc запросах она удобна. Причины конкретные, не эстетические:

1. **Лишний трафик и I/O.** Тащатся все колонки, включая тяжёлые (`text`, `jsonb`, BLOB). Если из 20 колонок нужны 3 — 85% данных гоняется по сети и разбирается зря. На ORM это ещё и гидратация ненужных полей в объекты.
2. **Ломает покрывающие индексы.** Со списком колонок запрос может выполниться как Index-Only Scan (см. вопрос 9) вообще без чтения таблицы. `SELECT *` гарантированно требует обращения к куче — оптимизатору не остаётся выбора.
3. **Хрупкость для потребителей.** Порядок и состав колонок в `*` не закреплён: добавление колонки в таблицу молча меняет результат. Код, читающий результат позиционно (`rs.getString(2)`, маппинг по индексу), ломается без единой ошибки компиляции.
4. **Неявность.** По запросу `SELECT *` не видно, что реально нужно вызывающему коду, — рефакторить схему и оценивать нагрузку сложнее.

Вьюхи и подзапросы с `*` внутри — та же проблема плюс риск конфликта имён при JOIN. Легитимное место для `*` — `EXISTS (SELECT * ...)` (СУБД всё равно игнорирует список) и исследовательские запросы руками.

**count(*) / count(1) / count(column) — классический вопрос-ловушка:**

- `COUNT(*)` — считает **строки**, включая строки с NULL во всех колонках.
- `COUNT(1)` — то же самое, что `COUNT(*)`: константа не NULL, значит считаются все строки. Во всех современных СУБД оптимизатор обрабатывает их одинаково, «count(1) быстрее» — миф из девяностых.
- `COUNT(column)` — считает только строки, где колонка **не NULL**. На колонке с NULL-ами результат будет меньше, чем `COUNT(*)`.
- `COUNT(DISTINCT column)` — уникальные не-NULL значения.

```sql
SELECT COUNT(*),            -- 5 строк всего
       COUNT(middle_name),  -- 3 (у двух сотрудников NULL)
       COUNT(DISTINCT dept) -- 2 отдела
FROM employees;
```

Нюанс для follow-up: `COUNT(*)` без `WHERE` на большой таблице — это всегда скан (в PostgreSQL — полного индекса/таблицы, в MySQL InnoDB — наименьшего вторичного индекса). Для «примерно сколько строк» используют статистику: в PostgreSQL `SELECT reltuples FROM pg_class WHERE relname = 'orders'`.

---

### 11. Нормализация (1NF, 2NF, 3NF) и денормализация

**Нормализация** — процесс организации схемы для устранения избыточности данных и аномалий вставки/обновления/удаления.

Исходная денормализованная таблица заказов:

| order_id | customer | phones | product | product_price | category | category_discount |
|---|---|---|---|---|---|---|
| 1 | Иван | 111, 222 | Мышь | 1000 | Электроника | 5% |
| 1 | Иван | 111, 222 | Кабель | 500 | Электроника | 5% |
| 2 | Иван | 111, 222 | Кружка | 300 | Дом | 0% |

**1NF (первая нормальная форма)**: каждая ячейка содержит атомарное (неделимое) значение, нет повторяющихся групп. Нарушение выше: `phones = '111, 222'` — список в одной ячейке; одна строка заказа содержит несколько товаров. Решение: одна строка = одна позиция заказа, телефоны — в отдельной таблице.

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
    IF NEW.salary <> OLD.salary THEN
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
- `EXPLAIN ANALYZE` — реально выполняет запрос и показывает фактические времена и число строк (осторожно с DML — изменения применятся; оборачивайте в транзакцию с откатом).

Ключевые узлы плана (PostgreSQL):

- **Seq Scan** — последовательное чтение всей таблицы. Нормально для маленьких таблиц и низкоселективных условий; тревожный сигнал на большой таблице с селективным фильтром (вероятно, нет индекса).
- **Index Scan** — поиск по индексу + чтение нужных страниц таблицы (heap fetches). Эффективен при высокой селективности.
- **Index Only Scan** — все данные взяты из индекса, таблица не читается (покрывающий индекс).
- **Bitmap Heap Scan + Bitmap Index Scan** — компромисс: по индексу строится битовая карта страниц, затем таблица читается последовательно. Типичен при средней селективности.
- **Методы соединения**: **Nested Loop** (для малого внешнего набора + индекс по внутреннему), **Hash Join** (для больших несортированных наборов), **Merge Join** (для предварительно отсортированных наборов).

Как искать узкие места:

1. Сравнить **estimated rows vs actual rows** — расхождение на порядки означает устаревшую статистику (`ANALYZE table`) или сложную для оценки конструкцию.
2. Найти самые «горячие» узлы по **actual time** в `EXPLAIN ANALYZE`.
3. Смотреть **Buffers** (`shared hit/read`): много `read` — данные не в кэше; большой `hit` при малых строках — лишние чтения.
4. Проверить `Rows Removed by Filter` — большое значение указывает на отсутствие подходящего индекса.
5. Проверить сортировки: `Sort Method: external merge Disk` означает, что сортировка не влезла в `work_mem` и ушла на диск.

---

### 14. Принципы ACID

**ACID** — набор гарантий, которым должна удовлетворять транзакция (логическая единица работы, объединяющая несколько операций).

**A — Atomicity (Атомарность)**: транзакция либо выполняется целиком, либо не выполняется вовсе. При сбое или `ROLLBACK` все частичные изменения откатываются. Пример: перевод денег — списание со счёта А и зачисление на счёт B должны произойти вместе; недопустимо состояние, когда деньги списаны, но не зачислены. Реализуется через журналирование (WAL) и механизм undo.

```sql
BEGIN;
UPDATE accounts SET balance = balance - 1000 WHERE id = 1;
UPDATE accounts SET balance = balance + 1000 WHERE id = 2;
COMMIT; -- обе операции зафиксированы; при ошибке на любом шаге — ROLLBACK обеих
```

**C — Consistency (Согласованность)**: транзакция переводит БД из одного корректного состояния в другое корректное, не нарушая ограничения целостности (constraints, триггеры, каскады). Например, невозможно зафиксировать заказ, ссылающийся на несуществующего клиента, или отрицательный остаток при наличии `CHECK (balance >= 0)`.

**I — Isolation (Изолированность)**: параллельные транзакции не должны наблюдать промежуточные (незафиксированные) изменения друг друга. Уровень изоляции настраивается и представляет компромисс между корректностью и производительностью (см. следующий вопрос).

**D — Durability (Долговечность)**: после `COMMIT` изменения не будут потеряны даже при сбое питания или крахе сервера. Реализуется через **WAL (Write-Ahead Log)**: изменения сначала записываются в журнал на диск (fsync), и только потом применяются к страницам данных; после сбоя БД восстанавливается по журналу (redo).

Практические замечания: `COMMIT` может быть медленным из-за fsync (настраивается `synchronous_commit`); атомарность распространяется и на DDL в PostgreSQL (транзакционный DDL), чего нет, например, в MySQL.

Частый follow-up — противопоставление **BASE** (Basically Available, Soft state, Eventually consistent): модель распределённых систем (NoSQL, микросервисы), жертвующая немедленной согласованностью ради доступности — данные между узлами сходятся со временем. ACID — про гарантии внутри одной транзакции в одной БД, BASE — про компромисс в распределённой системе (теорема CAP: при сетевом разделении выбирают между консистентностью и доступностью).

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
- **REPEATABLE READ** в PostgreSQL также исключает фантомные чтения (snapshot на всю транзакцию). В MySQL (InnoDB) REPEATABLE READ тоже защищает от фантомов через next-key locks.
- **READ COMMITTED** — уровень по умолчанию в PostgreSQL: каждый *оператор* видит снимок данных на момент своего начала. В REPEATABLE READ снимок берётся один раз на всю транзакцию. В MySQL (InnoDB) умолчание другое — REPEATABLE READ; на собеседовании это любят спрашивать именно в связке «PostgreSQL vs MySQL».
- **SERIALIZABLE** в PostgreSQL реализован через SSI (Serializable Snapshot Isolation): конфликтующие транзакции не блокируются, а при обнаружении опасной структуры одна из них откатывается с ошибкой `could not serialize access` — приложение должно быть готово к повторным попыткам (retry).

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

### 16. Блокировки в БД: оптимистичная и пессимистичная модели, deadlock

**Пессимистичная блокировка** исходит из того, что конфликт вероятен: ресурс блокируется заранее, и конкуренты ждут.

```sql
BEGIN;
SELECT balance FROM accounts WHERE id = 1 FOR UPDATE;  -- эксклюзивная блокировка строки
-- Другие транзакции, пытающиеся заблокировать/изменить эту строку, будут ждать
UPDATE accounts SET balance = balance - 1000 WHERE id = 1;
COMMIT;  -- блокировка снята
```

Варианты: `FOR UPDATE` (блокирует и чтения FOR UPDATE, и записи), `FOR SHARE` / `FOR KEY SHARE` (разделяемая — другие могут читать, но не изменять), `NOWAIT` (не ждать, сразу ошибка `could not obtain lock`), `SKIP LOCKED` (пропустить заблокированные строки — классический приём для очередей задач на БД).

```sql
-- Паттерн "очередь задач": воркеры не мешают друг другу
SELECT id FROM jobs
WHERE status = 'PENDING'
ORDER BY id
LIMIT 1
FOR UPDATE SKIP LOCKED;
```

**Оптимистичная блокировка** исходит из того, что конфликт редок: строка не блокируется, но при записи проверяется, что она не изменилась с момента чтения. Реализуется через колонку версии:

```sql
-- Прочитали: balance = 1000, version = 3
UPDATE accounts
SET balance = 900, version = version + 1
WHERE id = 1 AND version = 3;
-- Если затронуто 0 строк — кто-то успел изменить запись, нужно перечитать и повторить
```

В JPA оптимистичная блокировка встроена через `@Version` (см. вопрос 23).

Сравнение: пессимистичная модель гарантирует успех операции после захвата, но снижает параллелизм и требует держать транзакцию открытой; оптимистичная масштабируется лучше, но требует обработки неудач (retry).

**Deadlock (взаимная блокировка)** — цикл ожидания: транзакция T1 держит ресурс A и ждёт B, транзакция T2 держит B и ждёт A. СУБД детектирует deadlock и принудительно откатывает одну из транзакций (PostgreSQL: `deadlock detected`, жертва выбирается автоматически). Профилактика:

- Захватывать ресурсы всегда в одном и том же порядке (например, по возрастанию id).
- Делать транзакции короткими, не включать в них сетевые вызовы и «раздумья».
- Использовать минимально необходимый уровень блокировки и индексы (без индекса UPDATE может блокировать лишние строки при сканировании).
- Настроить `lock_timeout` и реализовывать retry при ошибке `40P01` (deadlock).

---

### 17. MVCC: многоверсионный контроль конкурентного доступа

**MVCC (Multi-Version Concurrency Control)** — механизм, при котором СУБД хранит несколько версий одной строки, позволяя читателям и писателям **не блокировать друг друга**: читающая транзакция видит согласованный снимок (snapshot) данных на определённый момент, а пишущая создаёт новую версию строки.

Как это работает в PostgreSQL:

1. У каждой строки (tuple) есть системные поля `xmin` (xid транзакции, создавшей версию) и `xmax` (xid транзакции, удалившей/устаревшей версию).
2. `UPDATE` физически **не изменяет строку на месте**: старая версия помечается `xmax = текущая транзакция`, и вставляется новая версия с `xmin = текущая транзакция`. `DELETE` — только проставление `xmax`.
3. Каждая транзакция получает **снимок (snapshot)**: множество xid, зафиксированных на момент начала снимка. Версия строки видна транзакции, если `xmin` зафиксирован в снимке, а `xmax` — нет.
4. READ COMMITTED берёт новый снимок на каждый оператор; REPEATABLE READ — один снимок на всю транзакцию.

Последствия и цена MVCC:

- **Bloat (раздувание таблиц)**: устаревшие версии строк (dead tuples) накапливаются. Их убирает фоновый процесс **autovacuum**; агрессивные UPDATE-нагрузки требуют тюнинга vacuum. `VACUUM FULL` переписывает таблицу с блокировкой.
- **Transaction ID wraparound**: счётчик xid 32-битный, поэтому старые версии обязаны быть «заморожены» vacuum'ом; нельзя выключать autovacuum надолго.
- Индексы в PostgreSQL не хранят информации о видимости версий, поэтому обычный Index Scan вынужден идти в таблицу (heap fetch) и проверять видимость каждой строки. Index Only Scan возможен благодаря **visibility map**: если страница помечена как «вся видимая», обращение к куче пропускается — поэтому регулярный vacuum важен и для производительности чтения, а не только для очистки bloat.
- В MySQL (InnoDB) MVCC реализован иначе: старые версии хранятся в **undo log**, и актуальная строка читается «назад» по цепочке версий; длинные транзакции раздувают undo log.

Главный вывод для собеседования: MVCC даёт высокую конкурентность чтения/записи («читатели не блокируют писателей и наоборот»), но требует фоновой очистки версий и аккуратности с длинными транзакциями.

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
em.persist(user);               // Managed: INSERT произойдёт при flush/commit
user.setName("Анна Смирнова");  // setter — dirty checking, UPDATE сгенерируется сам

User found = em.find(User.class, user.getId()); // тот же объект из L1-кэша

em.getTransaction().commit();   // flush + COMMIT
em.close();                     // found становится Detached
```

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

При N = 1000 заказов выполнится 1001 запрос — типичная причина деградации производительности в проде.

Способы решения:

**1. JOIN FETCH в JPQL** — загрузить связь одним запросом с соединением:

```java
List<Order> orders = em.createQuery(
        "SELECT o FROM Order o JOIN FETCH o.customer", Order.class)
    .getResultList();
// Один запрос: SELECT o.*, c.* FROM orders o JOIN customer c ON ...
```

Внимание: `JOIN FETCH` коллекции (`JOIN FETCH o.items`) размножает строки родителя и несовместим с пагинацией на уровне БД (Hibernate делает пагинацию в памяти с предупреждением `HHH000104`). Для коллекций лучше batch fetching.

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

### 20. Стратегии загрузки LAZY vs EAGER и LazyInitializationException

**FetchType** определяет, когда загружается связанная сущность/коллекция:

- **LAZY (ленивая)** — связь не загружается вместе с родителем; Hibernate подставляет прокси (для одиночных связей) или PersistentCollection (для коллекций), а реальный SELECT выполняется при первом обращении к данным связи. По умолчанию LAZY для `@OneToMany` и `@ManyToMany`.
- **EAGER (жадная)** — связь загружается немедленно вместе с родителем (отдельным SELECT или JOIN). По умолчанию EAGER для `@ManyToOne` и `@OneToOne` — это частая скрытая причина проблем, рекомендуется явно ставить `fetch = FetchType.LAZY` на всех связях.

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
2. **Open Session in View** (`spring.jpa.open-in-view=true`, в Spring Boot включён по умолчанию) — держит сессию открытой до конца HTTP-запроса. Маскирует проблему N+1 и держит соединение с БД дольше необходимого; на Senior-проектах обычно отключают.
3. **DTO-проекции** — возвращать из сервисного слоя не сущности, а DTO с заранее выбранными полями; сущности не покидают транзакционную границу.
4. `@Transactional(readOnly = true)` на методах чтения, чтобы контекст жил на время всей обработки.

Принцип: LAZY по умолчанию + точечная жадная загрузка там, где связь реально нужна (через JOIN FETCH / EntityGraph) — безопаснее, чем EAGER, который грузит лишнее при каждом чтении сущности.

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
<!-- зависимость: org.hibernate.orm:hibernate-jcache + com.github.ben-manes.caffeine:caffeine
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

**Query Cache (кэш запросов)** — кэширует не сущности, а **результаты запросов** (набор идентификаторов по JPQL/HQL + параметрам); сами сущности всё равно берутся из L2 или БД. Включается отдельно и требует пометки каждого запроса:

```properties
spring.jpa.properties.hibernate.cache.use_query_cache=true
```

```java
List<Country> countries = em.createQuery("SELECT c FROM Country c", Country.class)
        .setHint("org.hibernate.cacheable", true)
        .getResultList();
```

Практика: L2 эффективен для read-mostly данных с доступом по первичному ключу; кэш запросов оправдан редко (малейший INSERT/UPDATE в таблицу инвалидирует все кэшированные запросы по ней) и почти всегда проигрывает внешним кэшам уровня приложения (Spring Cache + Redis).

---

### 22. Связи сущностей: @OneToMany, @ManyToOne, @ManyToMany, mappedBy, CascadeType

**@ManyToOne / @OneToOne** — «хозяин» связи (owning side): именно на этой стороне в таблице находится колонка внешнего ключа.

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

**@OneToMany** — обратная сторона. Важное понятие — **mappedBy**: указывает, каким полем на противоположной стороне владеется связь. Сторона с `mappedBy` является «зеркальной» (inverse side): Hibernate не читает из неё FK и не пишет по ней изменения — только owning side (`@ManyToOne`) управляет колонкой внешнего ключа. Поэтому при двунаправленной связи обязательно поддерживают обе стороны в консистентном состоянии вспомогательными методами:

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

Внимание: `CascadeType.REMOVE` (и `ALL`) удалит потомков при удалении родителя — не путать с `orphanRemoval`.

**orphanRemoval = true** — удаляет из БД потомка, **убранного из коллекции** родителя (сироту), даже если родитель не удалён: `customer.getOrders().remove(order)` при flush сгенерирует `DELETE` для этого заказа. Применяется только к `@OneToOne`/`@OneToMany` (у `@ManyToMany` «сирот» нет — запись может ссылаться на других родителей).

---

### 23. Оптимистичная блокировка через @Version в JPA

JPA поддерживает оптимистичную блокировку из коробки: достаточно добавить в сущность поле с аннотацией **@Version** (тип `int`, `long` или `java.time.Instant`/`Timestamp`).

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
repo.save(a);  // OK: version 3 → 4      repo.save(b);  // OptimisticLockException:
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
- **NESTED** — выполнить внутри вложенной транзакции через `SAVEPOINT`: при откате вложенной части откатывается только она (до savepoint), внешняя продолжается. Работает только с JDBC-DataSource (не с JPA-`JpaTransactionManager`).

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

3. **Приватные/внутренние методы**: прокси перехватывает только public-методы, вызываемые извне.

4. **Прокси-классы**: по умолчанию используются JDK-прокси на интерфейсах; для классов — CGLIB (в Spring Boot 2+ по умолчанию). Финальные методы/классы не проксируются.

5. Дополнительные атрибуты: `readOnly = true` (подсказка для оптимизации, в Hibernate отключает dirty checking и ставит FlushMode.MANUAL), `timeout`, `isolation` (переопределение уровня изоляции БД), `noRollbackFor`.

---

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
- **Liquibase**: декларативный формат независим от диалекта СУБД (один changelog для PostgreSQL и Oracle), поддержка rollback и генерации diff между схемами; цена — более многословный XML/YAML и дополнительная абстракция.

Общие правила: миграции неизменяемы после попадания в общие окружения, обратно-совместимые изменения схемы при zero-downtime-деплое делаются в несколько шагов (expand-and-contract: добавить колонку → переключить код → удалить старую колонку отдельной миграцией).

---

### 26. Пагинация: LIMIT/OFFSET против keyset (cursor) pagination

**OFFSET-пагинация** заставляет БД физически прочитать и отбросить N первых строк — на больших смещениях запрос деградирует линейно. **Keyset-пагинация** вместо смещения запоминает значение ключа сортировки последней строки страницы и начинает следующую сразу после него — стоимость не зависит от «глубины».

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

При индексе `(created_at, id)` это обычный index scan с seek, и время ответа одинаково для любой страницы. Обязательное условие — детерминированная сортировка: ключ должен быть уникальным, поэтому к неуникальному полю добавляют `id`.

Ограничения keyset: нельзя прыгнуть на произвольную страницу N, только «вперёд/назад»; многосортировочные кейсы с разным направлением сортировки писать неудобно. Частый follow-up: что выбрать для API с «бесконечной лентой» — keyset; для админки с номерами страниц — OFFSET допустим, смещения там маленькие.

---

### 27. Партиционирование таблиц: range, list и hash в PostgreSQL

**Партиционирование** — разбиение одной логической таблицы на физические части по значению ключа. Нужно, когда таблица выросла настолько, что индексы и autovacuum уже не спасают: обычно речь о десятках-сотнях миллионов строк.

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
- **дешёвое удаление старых данных**: `DROP TABLE events_2025_01` вместо `DELETE` на миллионы строк — мгновенно, без bloat и шторма autovacuum;
- индексы и vacuum обслуживают маленькие партиции, а не одну гигантскую таблицу.

Оговорки: PRIMARY KEY и UNIQUE обязаны включать ключ партиционирования, поэтому PK часто становится составным `(id, created_at)`; запрос без фильтра по ключу сканирует все партиции и может оказаться медленнее, чем по одной таблице. Ловушка в том, что партиционирование «на вырост» для маленькой таблицы — типичный оверинжиниринг: оно усложняет схему и миграции, не давая ничего взамен.

---

### 28. Репликация: синхронная и асинхронная, read replicas и replication lag

**Репликация** — копирование данных с primary (master) на одну или несколько реплик для отказоустойчивости и масштабирования чтения. Каждая реплика хранит полную копию данных.

- **Асинхронная** (по умолчанию): primary подтверждает commit клиенту, не дожидаясь реплик. Быстро, но при внезапном падении primary последние транзакции могут потеряться (RPO > 0).
- **Синхронная**: primary ждёт подтверждения записи WAL хотя бы на одной реплике (в PostgreSQL — `synchronous_standby_names`, `synchronous_commit = remote_apply`). Данные не теряются, цена — растёт latency записи и primary зависит от живости реплики.

**Read replicas**: чтение (`SELECT`) раздаём на реплики, запись идёт только на primary. Главная ловушка — **replication lag**: реплика отстаёт на миллисекунды, а под нагрузкой на секунды. Отсюда классическая проблема **read-your-writes**: пользователь сохранил форму (запись на primary), UI перечитал данные с реплики и не видит собственных изменений. Внешне выглядит как «запрос не сохранился».

Варианты решения:

- сразу после записи читать с primary (или помечать сессии, которые недавно писали, и направлять их чтение на primary);
- монотонное чтение: запоминать LSN позиции записи и ждать, пока реплика её догонит;
- синхронную репликацию ставить только там, где потеря недопустима (деньги, заказы), остальное — асинхронно.

Частый follow-up — чем репликация отличается от шардинга: реплика хранит полную копию данных и масштабирует только чтение; шард хранит часть данных и масштабирует объём и запись. Тема следующего вопроса.

---

### 29. Шардинг: отличие от репликации и выбор ключа шардирования

**Шардинг** — горизонтальное разбиение данных между независимыми узлами (шардами): каждый хранит свой поднабор строк, данные не дублируются. В отличие от репликации, шардинг масштабирует объём данных и нагрузку на запись, а не чтение.

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

Размер пула. Формула из документации PostgreSQL, которую приводит и HikariCP:

```text
connections = ((core_count * 2) + effective_spindle_count)
```

Для 4-ядерного сервера с SSD это `(4 * 2) + 1 ≈ 10` соединений. Контринтуитивно, но это нормально: соединение большую часть времени ждёт диск или сеть, а лишние соединения добавляют только contention и context switching. Ловушка в том, что пул из 50 соединений «потому что в Tomcat 50 потоков» обычно работает хуже, чем пул из 10.

```yaml
spring:
  datasource:
    hikari:
      maximum-pool-size: 10
      connection-timeout: 3000     # сколько ждать свободное соединение
      max-lifetime: 1800000        # должно быть меньше wait_timeout на стороне БД
```

Типичные ошибки: утечка соединений (не закрыли — лечится `leakDetectionThreshold` в Hikari), слишком большой пул, `maxLifetime` больше таймаута БД или файрвола (пул выдаёт «мёртвые» соединения), удержание соединения на время не-БД работы — например, HTTP-вызов внутри открытой транзакции.

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
    private String email;  // бизнес-ключ

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof User other)) return false;
        return email != null && email.equals(other.getEmail());
    }

    @Override
    public int hashCode() {
        return getClass().hashCode();
    }
}
```

Два нюанса, которые всплывают follow-up'ами. Первый: обращаться к полям другого объекта нужно через геттер (`other.getEmail()`, а не `other.email`) — у Hibernate-прокси поля не инициализированы, и прямой доступ к полю вернёт null до загрузки сущности. Второй: `getClass().hashCode()` — константа на класс (по дизайну hashCode сущности не должен зависеть от мутабельных полей), и если `equals` может сравнивать сущность с её прокси, надёжнее возвращать фиксированную константу, потому что класс прокси (`User$HibernateProxy$...`) отличается от `User.class`.

**2. Только id, с оговорками**: `equals` по id (объект с null-id равен только самому себе), `hashCode` — константа, например `getClass().hashCode()`, чтобы сущность не «переезжала» по бакетам при присвоении id. Работает, но до persist все новые сущности «разные», что иногда неудобно.

В обоих вариантах: `instanceof` вместо сравнения `getClass()` (иначе equals ломается на Hibernate-прокси) и никаких LAZY-полей внутри.

---

### 32. Пакетная вставка и обновление в Hibernate (batch)

По умолчанию Hibernate выполняет каждый INSERT/UPDATE отдельным round-trip. Для массовых операций включают JDBC batching — драйвер накапливает операторы и отправляет их пачкой:

```properties
spring.jpa.properties.hibernate.jdbc.batch_size=30
spring.jpa.properties.hibernate.order_inserts=true
spring.jpa.properties.hibernate.order_updates=true
```

`order_inserts`/`order_updates` сортируют операторы так, чтобы однотипные попадали в один батч.

**Ловушка №1 — IDENTITY.** `GenerationType.IDENTITY` требует выполнить INSERT немедленно, чтобы получить сгенерированный id, поэтому батчинг молча отключается. Для батчинга нужен `SEQUENCE`:

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
    if (i % 50 == 0) {
        em.flush();  // отправить накопленный батч в БД
        em.clear();  // очистить persistence context
    }
}
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

Оговорки: `StatelessSession` не выполняет каскады (дочерние сущности вставляем руками) и не обновляет L2-кэш. Для действительно больших объёмов ORM вообще не нужен: `COPY` в PostgreSQL или прямой JDBC `addBatch()/executeBatch()` быстрее на порядок.

---

### 33. Частые антипаттерны в работе с БД

Короткий список того, что спрашивают follow-up'ами и что регулярно встречается в ревью:

- **SELECT \*** — тянем все колонки, включая тяжёлые `text`/`jsonb`, хотя нужны три. Лишний трафик и память, невозможность использовать покрывающий индекс (index only scan), хрупкость к изменениям схемы. ORM-аналог — грузить всю сущность ради двух полей; лечится DTO-проекциями.
- **EAGER на всех связях** — «пусть сразу подгружается». Каждый `findById` превращается в join нескольких таблиц, а EAGER `@ManyToMany` — в декартово произведение. Правило по умолчанию: все связи LAZY, под конкретный сценарий — `JOIN FETCH` или entity graph.
- **Нет индексов под внешними ключами** — PostgreSQL (в отличие от Oracle) индекс под FK сам не создаёт. Удаление родительской строки и проверка FK идут полным сканом дочерней таблицы, join'ы по FK медленные. Индекс под каждый FK нужен почти всегда.
- **Запросы в цикле** — разновидность N+1: `for (id : ids) repo.findById(id)` даёт тысячу round-trip'ов вместо одного `findAllById(ids)` / `WHERE id IN (...)`.
- **Деньги во float/double** — двоичное представление не хранит десятичные дроби точно: `0.1 + 0.2 != 0.3`. Для денег — `BigDecimal` в Java и `NUMERIC`/`DECIMAL` в БД, никаких `FLOAT`/`DOUBLE`. Смежная ошибка — суммировать `double` в стриме и округлять «в конце».
- Бонусом, если спросят ещё: выборки без `LIMIT` на потенциально больших таблицах, индексы «на всякий случай» по каждой колонке (тормозят запись и раздувают БД), транзакция, открытая на время HTTP-вызова, `LIKE '%...%'` с ведущим wildcard — B-Tree индекс при таком шаблоне не используется.

---

[← Вернуться на главную страницу](README.md)
