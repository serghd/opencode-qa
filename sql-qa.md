# SQL Interview Questions & Answers

## What is the difference between `INNER JOIN` and `LEFT JOIN`?

`INNER JOIN` returns only rows with matching values in both tables. `LEFT JOIN` returns all rows from the left table and matching rows from the right table, filling unmatched columns with `NULL`.

```sql
-- INNER JOIN: only users with orders
SELECT u.name, o.total
FROM users u
INNER JOIN orders o ON u.id = o.user_id;

-- LEFT JOIN: all users, NULL when no order
SELECT u.name, o.total
FROM users u
LEFT JOIN orders o ON u.id = o.user_id;
```

## What is the difference between `WHERE` and `HAVING`?

`WHERE` filters rows before grouping and aggregation. `HAVING` filters groups after `GROUP BY` and aggregation. `WHERE` cannot reference aggregate functions; `HAVING` can.

```sql
SELECT department, AVG(salary) AS avg_salary
FROM employees
WHERE status = 'active'           -- rows filtered before grouping
GROUP BY department
HAVING AVG(salary) > 70000;       -- groups filtered after aggregation
```

## What is the difference between `UNION` and `UNION ALL`?

`UNION` combines result sets and removes duplicate rows. `UNION ALL` combines result sets without deduplication, making it faster when duplicates are acceptable or impossible.

```sql
-- UNION: deduplicates 'Engineering'
SELECT department FROM employees WHERE salary > 80000
UNION
SELECT department FROM contractors WHERE rate > 500;

-- UNION ALL: keeps duplicates
SELECT department FROM employees WHERE salary > 80000
UNION ALL
SELECT department FROM contractors WHERE rate > 500;
```

## What is a window function and how does it differ from `GROUP BY`?

A window function performs calculations across a set of rows related to the current row without collapsing them into a single output row. `GROUP BY` collapses rows into groups, reducing the number of rows returned. Window functions use the `OVER()` clause and retain individual row identity.

```sql
-- GROUP BY: one row per department
SELECT department, AVG(salary)
FROM employees GROUP BY department;

-- Window function: per-row salary vs department average
SELECT name, department, salary,
       AVG(salary) OVER (PARTITION BY department) AS dept_avg
FROM employees;
```

## What is a correlated subquery?

A correlated subquery is a subquery that references columns from the outer query. It is re-executed for each row processed by the outer query, which can make it less efficient than a `JOIN` or non-correlated subquery.

```sql
-- Find employees earning more than their department average
SELECT name, salary, department
FROM employees e
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
    WHERE department = e.department  -- references outer row
);
```

## What is the difference between `DELETE` and `TRUNCATE`?

`DELETE` removes rows one at a time, logs each deletion, can include a `WHERE` clause, and can be rolled back. `TRUNCATE` removes all rows by deallocating data pages, logs only page deallocations, cannot use `WHERE`, and uses fewer locks and less transaction log space.

```sql
-- DELETE: selective, logged, can roll back
DELETE FROM orders WHERE status = 'cancelled';

-- TRUNCATE: removes all rows, minimally logged, cannot filter
TRUNCATE TABLE audit_log;
```

## What are indexes and what are their trade-offs?

Indexes are data structures (typically B-trees) that speed up `SELECT` queries by allowing rapid row lookup. Trade-offs include slower `INSERT`/`UPDATE`/`DELETE` operations (indexes must be maintained), increased disk usage, and potentially misleading the optimizer if over-indexed.

```sql
-- Creates a B-tree index on the email column
CREATE INDEX idx_users_email ON users(email);

-- Query that benefits from the index
SELECT * FROM users WHERE email = 'alice@example.com';
-- Without the index, this would scan the entire table.
```

## What is the difference between `CHAR`, `VARCHAR`, and `TEXT`?

- `CHAR(n)` is fixed-length, pads with spaces, fastest for fixed-size data.
- `VARCHAR(n)` is variable-length with a defined limit, no padding, good for most string fields.
- `TEXT` is variable-length with no practical limit, cannot be indexed fully without a prefix, best for large blocks of text.

```sql
CREATE TABLE example (
    country CHAR(2),            -- 'US', 'DE', 'JP' — always 2 chars
    email VARCHAR(255),         -- up to 255 chars, no padding
    bio TEXT                    -- long-form text, no explicit limit
);
```

## What are aggregate functions in SQL?

Aggregate functions perform a calculation on a set of values and return a single result. Common ones include `COUNT()`, `SUM()`, `AVG()`, `MIN()`, and `MAX()`. They are typically used with `GROUP BY`.

```sql
SELECT department,
       COUNT(*) AS headcount,
       AVG(salary) AS avg_salary,
       MIN(salary) AS min_salary,
       MAX(salary) AS max_salary,
       SUM(bonus) AS total_bonus
FROM employees
GROUP BY department;
```

## What is a self-join and when would you use it?

A self-join joins a table with itself, treating it as two separate instances using table aliases. It is useful for hierarchical data (e.g., employees and managers in the same table), comparing rows within the same table, or finding duplicates.

## What is a CTE (Common Table Expression) and how is it used?

A CTE is a temporary named result set defined with `WITH` that exists only within the scope of a single `SELECT`, `INSERT`, `UPDATE`, or `DELETE` statement. It improves readability, allows referencing the same subquery multiple times, and supports recursion.

```sql
-- Non-recursive CTE: find top departments by average salary
WITH dept_avg AS (
    SELECT department, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department
)
SELECT department, avg_salary
FROM dept_avg
WHERE avg_salary > 70000
ORDER BY avg_salary DESC;

-- Recursive CTE: build org hierarchy
WITH RECURSIVE org_tree AS (
    -- anchor: top-level manager
    SELECT id, name, manager_id, 1 AS level
    FROM employees
    WHERE manager_id IS NULL
    UNION ALL
    -- recursive: direct reports
    SELECT e.id, e.name, e.manager_id, t.level + 1
    FROM employees e
    JOIN org_tree t ON e.manager_id = t.id
)
SELECT name, level
FROM org_tree
ORDER BY level, name;
```

## What is the difference between `EXISTS` and `IN`?

Both check for the existence of rows in a subquery. `IN` evaluates the subquery once, builds a list of values, and matches against it; it can be slower with large result sets and behaves unexpectedly with `NULL` values. `EXISTS` uses semi-join semantics, evaluates row-by-row with short-circuiting, and typically performs better with large subquery results.

```sql
-- IN: builds a list of IDs from the subquery
SELECT name FROM employees
WHERE department_id IN (
    SELECT id FROM departments WHERE budget > 100000
);

-- EXISTS: semi-join, short-circuits on first match
SELECT name FROM employees e
WHERE EXISTS (
    SELECT 1 FROM departments d
    WHERE d.id = e.department_id AND d.budget > 100000
);
```

## What are SQL views and how are they used?

A view is a saved query that behaves like a virtual table. It encapsulates complex logic, restricts access to specific columns or rows (security), and simplifies reporting. Simple views on a single table can be updatable; views with `JOIN`, aggregation, or `DISTINCT` are read-only.

```sql
-- Create a view that hides salary data and joins tables
CREATE VIEW active_customer_orders AS
SELECT c.name, c.email, o.order_date, o.total
FROM customers c
JOIN orders o ON c.id = o.customer_id
WHERE o.status = 'shipped';

-- Query the view like a regular table
SELECT * FROM active_customer_orders
WHERE order_date >= '2025-01-01';
```
