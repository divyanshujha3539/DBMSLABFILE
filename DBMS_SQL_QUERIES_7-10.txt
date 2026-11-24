-- ============================================================
-- DBMS Lab - SQL Queries (Experiments 7 to 10)
-- Normalization, Transactions, Indexing and Query Optimization (MySQL)
-- Run the experiments in order. Each experiment is self-contained
-- except where noted.
-- ============================================================

-- ============================================================
-- EXPERIMENT 7 – NORMALIZATION (UNF TO 3NF AND BCNF)
-- AIM: To identify functional dependencies in a denormalized Sales
-- table and normalize it step-by-step from UNF to 3NF and BCNF.
-- To verify lossless join and dependency preservation using SQL
-- queries.
-- ============================================================

-- ---------- Create a Denormalized Sales Table (UNF) ----------
CREATE TABLE Sales_UNF (
    order_id INT,
    order_date DATE,
    customer_id INT,
    customer_name VARCHAR(50),
    product_ids VARCHAR(100),
    product_names VARCHAR(200),
    quantities VARCHAR(100)
);

INSERT INTO Sales_UNF VALUES
(101, '2026-01-10', 1, 'Aman', 'P1,P2', 'Laptop,Mouse', '1,2'),
(102, '2026-01-11', 2, 'Riya', 'P2,P3', 'Mouse,Keyboard', '1,1'),
(103, '2026-01-12', 1, 'Aman', 'P3', 'Keyboard', '2');

-- ---------- Functional Dependencies ----------
-- The assumed functional dependencies are:
--   order_id -> order_date, customer_id
--   customer_id -> customer_name
--   product_id -> product_name
--   (order_id, product_id) -> quantity
-- The candidate key for the normalized order-details relation is
-- (order_id, product_id).

-- ---------- Convert to 1NF ----------
-- In 1NF, each column must contain a single atomic value.
CREATE TABLE Sales_1NF (
    order_id INT,
    order_date DATE,
    customer_id INT,
    customer_name VARCHAR(50),
    product_id VARCHAR(10),
    product_name VARCHAR(50),
    quantity INT,
    PRIMARY KEY (order_id, product_id)
);

INSERT INTO Sales_1NF VALUES
(101, '2026-01-10', 1, 'Aman', 'P1', 'Laptop', 1),
(101, '2026-01-10', 1, 'Aman', 'P2', 'Mouse', 2),
(102, '2026-01-11', 2, 'Riya', 'P2', 'Mouse', 1),
(102, '2026-01-11', 2, 'Riya', 'P3', 'Keyboard', 1),
(103, '2026-01-12', 1, 'Aman', 'P3', 'Keyboard', 2);

-- ---------- Convert to 2NF ----------
-- Remove partial dependencies by separating Orders, Products and
-- Order Details.
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    order_date DATE,
    customer_id INT,
    customer_name VARCHAR(50)
);

CREATE TABLE Products (
    product_id VARCHAR(10) PRIMARY KEY,
    product_name VARCHAR(50)
);

CREATE TABLE OrderDetails (
    order_id INT,
    product_id VARCHAR(10),
    quantity INT,
    PRIMARY KEY (order_id, product_id),
    FOREIGN KEY (order_id) REFERENCES Orders(order_id),
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
);

INSERT INTO Orders VALUES
(101, '2026-01-10', 1, 'Aman'),
(102, '2026-01-11', 2, 'Riya'),
(103, '2026-01-12', 1, 'Aman');

INSERT INTO Products VALUES
('P1', 'Laptop'),
('P2', 'Mouse'),
('P3', 'Keyboard');

INSERT INTO OrderDetails VALUES
(101, 'P1', 1),
(101, 'P2', 2),
(102, 'P2', 1),
(102, 'P3', 1),
(103, 'P3', 2);

-- ---------- Convert to 3NF ----------
-- Remove the transitive dependency:
-- order_id -> customer_id -> customer_name
CREATE TABLE Customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(50)
);

INSERT INTO Customers VALUES
(1, 'Aman'),
(2, 'Riya');

CREATE TABLE Orders_3NF (
    order_id INT PRIMARY KEY,
    order_date DATE,
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
);

INSERT INTO Orders_3NF VALUES
(101, '2026-01-10', 1),
(102, '2026-01-11', 2),
(103, '2026-01-12', 1);

-- ---------- Verify the Normalized Data ----------
SELECT
    o.order_id,
    o.order_date,
    c.customer_name,
    p.product_name,
    od.quantity
FROM Orders_3NF o
JOIN Customers c
    ON o.customer_id = c.customer_id
JOIN OrderDetails od
    ON o.order_id = od.order_id
JOIN Products p
    ON od.product_id = p.product_id
ORDER BY o.order_id;

-- ---------- Verify Lossless Join and Dependency Preservation ----------
SELECT *
FROM Orders_3NF o
JOIN Customers c
    ON o.customer_id = c.customer_id;

SELECT *
FROM OrderDetails od
JOIN Products p
    ON od.product_id = p.product_id;

-- The joins reconstruct the original order and product details
-- without introducing spurious rows. The decomposition preserves
-- the stated functional dependencies.

-- ---------- BCNF Verification ----------
-- In the final relations, every non-trivial functional dependency
-- has a determinant that is a candidate key:
--   Customers    : customer_id -> customer_name
--   Orders_3NF   : order_id -> order_date, customer_id
--   Products     : product_id -> product_name
--   OrderDetails : (order_id, product_id) -> quantity
-- Therefore, these relations satisfy BCNF under the stated
-- dependencies. No further decomposition is required.

-- ============================================================
-- EXPERIMENT 8 – TRANSACTIONS, SAVEPOINTS AND ISOLATION LEVELS
-- AIM: To implement banking-style transactions using BEGIN, COMMIT,
-- ROLLBACK and SAVEPOINT. To simulate partial failures and
-- demonstrate isolation levels and concurrency anomalies using two
-- database sessions.
-- ============================================================

-- ---------- Create the Bank Table ----------
CREATE TABLE Bank (
    account_id INT PRIMARY KEY,
    account_name VARCHAR(50),
    balance DECIMAL(10,2)
);

INSERT INTO Bank VALUES
(1, 'Yusuf', 10000),
(2, 'Amit', 5000);

-- ---------- BEGIN and COMMIT ----------
-- Transfer Rs. 1,000 from Yusuf to Amit.
START TRANSACTION;

UPDATE Bank
SET balance = balance - 1000
WHERE account_id = 1;

UPDATE Bank
SET balance = balance + 1000
WHERE account_id = 2;

COMMIT;

SELECT * FROM Bank;

-- ---------- ROLLBACK ----------
START TRANSACTION;

UPDATE Bank
SET balance = balance - 2000
WHERE account_id = 1;

ROLLBACK;

SELECT * FROM Bank;

-- ---------- SAVEPOINT ----------
START TRANSACTION;

UPDATE Bank
SET balance = balance - 500
WHERE account_id = 1;

SAVEPOINT sp1;

UPDATE Bank
SET balance = balance + 500
WHERE account_id = 2;

ROLLBACK TO sp1;

COMMIT;

SELECT * FROM Bank;

-- ---------- Simulate an Insufficient-Balance Failure ----------
-- No row is updated because the account does not have sufficient
-- funds. ROLLBACK ends the transaction without saving changes.
START TRANSACTION;

UPDATE Bank
SET balance = balance - 20000
WHERE account_id = 1
  AND balance >= 20000;

SELECT ROW_COUNT() AS rows_updated;

ROLLBACK;

-- ---------- Demonstrate Isolation Levels ----------
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
START TRANSACTION;
SELECT * FROM Bank;
COMMIT;

-- Other supported isolation levels in MySQL:
--   SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
--   SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
--   SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;
-- Run one isolation-level setting at a time before starting the
-- transaction.

-- ---------- Demonstrate Concurrency Using Two Sessions ----------
-- Session A
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
START TRANSACTION;
SELECT balance FROM Bank WHERE account_id = 1;

UPDATE Bank
SET balance = balance - 500
WHERE account_id = 1;

COMMIT;

-- Session B
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
START TRANSACTION;
SELECT balance FROM Bank WHERE account_id = 1;
COMMIT;

-- ---------- Demonstrate REPEATABLE READ ----------
-- In Session B:
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
START TRANSACTION;
SELECT balance FROM Bank WHERE account_id = 1;
SELECT balance FROM Bank WHERE account_id = 1;
COMMIT;

-- ---------- Demonstrate a Dirty Read ----------
-- In Session B:
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
START TRANSACTION;
SELECT balance FROM Bank WHERE account_id = 1;
COMMIT;

-- ============================================================
-- EXPERIMENT 9 – INDEXING AND QUERY PERFORMANCE (RAILWAY BOOKING)
-- AIM: To create a Railway Booking table with at least 5,000
-- records, benchmark query performance before and after creating
-- single and composite indexes, and compare execution plans using
-- EXPLAIN.
-- ============================================================

-- ---------- Create Table ----------
CREATE TABLE Railway_Booking (
    booking_id INT PRIMARY KEY,
    passenger_name VARCHAR(100),
    train_no INT,
    source_station VARCHAR(50),
    destination_station VARCHAR(50),
    journey_date DATE,
    seat_no INT,
    fare DECIMAL(8,2),
    status VARCHAR(20)
);

-- ---------- Insert 5,000 Records ----------
-- A recursive CTE is used to generate the records automatically.
SET SESSION cte_max_recursion_depth = 6000;

INSERT INTO Railway_Booking
(booking_id, passenger_name, train_no, source_station,
 destination_station, journey_date, seat_no, fare, status)
WITH RECURSIVE numbers AS (
    SELECT 1 AS n
    UNION ALL
    SELECT n + 1 FROM numbers WHERE n < 5000
)
SELECT
    n,
    CONCAT('Passenger', n),
    12000 + MOD(n, 20),
    CASE MOD(n, 5)
        WHEN 0 THEN 'Delhi'
        WHEN 1 THEN 'Mumbai'
        WHEN 2 THEN 'Chennai'
        WHEN 3 THEN 'Kolkata'
        ELSE 'Jaipur'
    END,
    CASE MOD(n, 5)
        WHEN 0 THEN 'Mumbai'
        WHEN 1 THEN 'Delhi'
        WHEN 2 THEN 'Kolkata'
        WHEN 3 THEN 'Jaipur'
        ELSE 'Chennai'
    END,
    DATE_ADD('2026-01-01', INTERVAL MOD(n, 365) DAY),
    MOD(n, 100) + 1,
    500 + MOD(n, 2000),
    CASE MOD(n, 3)
        WHEN 0 THEN 'Confirmed'
        WHEN 1 THEN 'Waiting'
        ELSE 'Cancelled'
    END
FROM numbers;

-- ---------- Verify the Record Count ----------
SELECT COUNT(*) AS total_records
FROM Railway_Booking;

-- ---------- Test Performance Before Indexing ----------
EXPLAIN
SELECT *
FROM Railway_Booking
WHERE train_no = 12005;

SET @start_time = NOW(6);

SELECT *
FROM Railway_Booking
WHERE train_no = 12005;

SELECT TIMESTAMPDIFF(MICROSECOND, @start_time, NOW(6)) / 1000
    AS execution_time_ms;

-- ---------- Create a Single-Column Index ----------
CREATE INDEX idx_train
ON Railway_Booking(train_no);

-- Run the same query again:
EXPLAIN
SELECT *
FROM Railway_Booking
WHERE train_no = 12005;

-- Measure its execution time using the same timing queries:
SET @start_time = NOW(6);

SELECT *
FROM Railway_Booking
WHERE train_no = 12005;

SELECT TIMESTAMPDIFF(MICROSECOND, @start_time, NOW(6)) / 1000
    AS execution_time_ms;

-- ---------- Create a Composite Index ----------
CREATE INDEX idx_train_date
ON Railway_Booking(train_no, journey_date);

-- Test a query using both columns:
EXPLAIN
SELECT *
FROM Railway_Booking
WHERE train_no = 12005
  AND journey_date = '2026-01-15';

-- Measure execution time:
SET @start_time = NOW(6);

SELECT *
FROM Railway_Booking
WHERE train_no = 12005
  AND journey_date = '2026-01-15';

SELECT TIMESTAMPDIFF(MICROSECOND, @start_time, NOW(6)) / 1000
    AS execution_time_ms;

-- ---------- Compare Execution Plans ----------
EXPLAIN
SELECT *
FROM Railway_Booking
WHERE train_no = 12005;

EXPLAIN
SELECT *
FROM Railway_Booking
WHERE train_no = 12005
  AND journey_date = '2026-01-15';

-- ============================================================
-- EXPERIMENT 10 – QUERY OPTIMIZATION USING INDEXING AND REWRITING
-- AIM: To generate a dataset containing at least 10,000 rows, write
-- five slow SQL queries, analyze them using EXPLAIN, optimize them
-- using indexing and query rewriting, and compare execution time
-- before and after optimization.
-- ============================================================

-- ---------- Create Table ----------
DROP TABLE IF EXISTS Sales_Analytics;

CREATE TABLE Sales_Analytics (
    order_id INT PRIMARY KEY,
    customer_id INT,
    product_id INT,
    city VARCHAR(50),
    order_date DATE,
    amount DECIMAL(10,2),
    quantity INT,
    status VARCHAR(20)
);

-- ---------- Insert 10,000 Records ----------
SET SESSION cte_max_recursion_depth = 11000;

INSERT INTO Sales_Analytics
(order_id, customer_id, product_id, city, order_date,
 amount, quantity, status)
WITH RECURSIVE numbers AS (
    SELECT 1 AS n
    UNION ALL
    SELECT n + 1 FROM numbers WHERE n < 10000
)
SELECT
    n,
    MOD(n, 1000) + 1,
    MOD(n, 100) + 1,
    CASE MOD(n, 4)
        WHEN 0 THEN 'Delhi'
        WHEN 1 THEN 'Mumbai'
        WHEN 2 THEN 'Pune'
        ELSE 'Jaipur'
    END,
    DATE_ADD('2025-01-01', INTERVAL MOD(n, 365) DAY),
    100 + MOD(n * 17, 5000),
    MOD(n, 10) + 1,
    CASE MOD(n, 3)
        WHEN 0 THEN 'Completed'
        WHEN 1 THEN 'Pending'
        ELSE 'Cancelled'
    END
FROM numbers;

SELECT COUNT(*) AS total_records
FROM Sales_Analytics;

-- ---------- Query 1: Date Filtering ----------
-- Before optimization (function on column prevents index use)
EXPLAIN
SELECT *
FROM Sales_Analytics
WHERE YEAR(order_date) = 2025;

CREATE INDEX idx_order_date
ON Sales_Analytics(order_date);

-- After optimization (range condition on indexed column)
EXPLAIN
SELECT *
FROM Sales_Analytics
WHERE order_date >= '2025-01-01'
  AND order_date < '2026-01-01';

-- ---------- Query 2: City Filtering ----------
-- Before optimization
EXPLAIN
SELECT *
FROM Sales_Analytics
WHERE LOWER(city) = 'delhi';

CREATE INDEX idx_city
ON Sales_Analytics(city);

-- After optimization
EXPLAIN
SELECT *
FROM Sales_Analytics
WHERE city = 'Delhi';

-- ---------- Query 3: Status and Date Filtering ----------
-- Before optimization
EXPLAIN
SELECT *
FROM Sales_Analytics
WHERE status = 'Completed'
  AND order_date >= '2025-06-01';

CREATE INDEX idx_status_date
ON Sales_Analytics(status, order_date);

-- After optimization (composite index)
EXPLAIN
SELECT *
FROM Sales_Analytics
WHERE status = 'Completed'
  AND order_date >= '2025-06-01';

-- ---------- Query 4: Customer Total Amount ----------
-- Before optimization (correlated subquery)
EXPLAIN
SELECT DISTINCT s.customer_id
FROM Sales_Analytics s
WHERE (
    SELECT SUM(s2.amount)
    FROM Sales_Analytics s2
    WHERE s2.customer_id = s.customer_id
) > 30000;

CREATE INDEX idx_customer_amount
ON Sales_Analytics(customer_id, amount);

-- After optimization (GROUP BY ... HAVING rewrite)
EXPLAIN
SELECT customer_id
FROM Sales_Analytics
GROUP BY customer_id
HAVING SUM(amount) > 30000;

-- ---------- Query 5: Top 10 Orders by Amount ----------
-- Before optimization
EXPLAIN
SELECT order_id, amount
FROM Sales_Analytics
ORDER BY amount DESC
LIMIT 10;

CREATE INDEX idx_amount
ON Sales_Analytics(amount);

-- After optimization
EXPLAIN
SELECT order_id, amount
FROM Sales_Analytics
ORDER BY amount DESC
LIMIT 10;

-- ---------- Compare Execution Time ----------
SET @start_time = NOW(6);

SELECT *
FROM Sales_Analytics
WHERE order_date >= '2025-01-01'
  AND order_date < '2026-01-01';

SELECT TIMESTAMPDIFF(MICROSECOND, @start_time, NOW(6)) / 1000
    AS execution_time_ms;
