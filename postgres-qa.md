# PostgreSQL Interview Questions & Answers

## What is PostgreSQL?
PostgreSQL is a powerful, open-source object-relational database management system (ORDBMS) known for reliability, feature robustness, and ACID compliance. It supports both SQL and JSON querying.

## What is the difference between PostgreSQL and MySQL?
PostgreSQL is an object-relational DBMS with advanced features like MVCC, CTEs, window functions, and full-text search. MySQL is a simpler, faster relational DBMS optimized for read-heavy workloads. PostgreSQL offers better concurrency and more data types.

## What is MVCC in PostgreSQL?
MVCC (Multi-Version Concurrency Control) allows multiple transactions to run concurrently without blocking. Each transaction sees a snapshot of data at a point in time, ensuring ACID properties while read operations never block writes.

## What are PostgreSQL indexes?
Indexes are data structures that improve query speed on columns. Types include B-tree (default), Hash, GiST (full-text, spatial), GIN (array, JSONB), and BRIN (sequential data). They trade write speed for read performance.

## What is a PostgreSQL CTE?
A CTE (Common Table Expression) is a temporary named result set defined before a query using WITH clause. It improves readability, supports recursion, and is evaluated once per query. Example: `WITH cte AS (SELECT * FROM users) SELECT * FROM cte`

## What is VACUUM in PostgreSQL?
VACUUM reclaims storage occupied by dead tuples (rows deleted/updated by MVCC). Without it, table bloat degrades performance. `VACUUM` single-file reuses space within pages; `VACUUM FULL` rewrites the entire table and returns disk space to the OS but blocks concurrent access. Autovacuum runs automatically based on dead-tuple thresholds.

## What is EXPLAIN used for?
`EXPLAIN` shows the query planner's execution plan, including scan type (sequential, index), join strategies, estimated row counts, and costs. `EXPLAIN ANALYZE` actually executes the query and adds actual timing/row counts. It is the primary tool for identifying performance bottlenecks.