-- ============================================================
-- DBMS Lab - SQL Queries (Experiments 3 to 6)
-- Employee-Department-Project Database (MySQL)
-- Run the experiments in order; Experiments 4-6 use the
-- database created in Experiment 3.
-- ============================================================

-- ============================================================
-- EXPERIMENT 3 – EMPLOYEE–DEPARTMENT–PROJECT DATABASE: BASIC QUERIES
-- AIM: To design and implement an Employee–Department–Project database
-- schema and perform SQL queries using Selection, Projection, Aggregate
-- Functions, GROUP BY, HAVING, CASE expressions, and ORDER BY.
-- ============================================================

-- ---------- Database Creating ----------
CREATE DATABASE EmployeeManagement;
USE EmployeeManagement;

-- ---------- Table Creating ----------
-- Department Table
CREATE TABLE Department (
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(50) NOT NULL,
    location VARCHAR(50)
);

-- Employee Table
CREATE TABLE Employee (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(50) NOT NULL,
    gender CHAR(1),
    salary DECIMAL(10,2),
    hire_date DATE,
    dept_id INT,
    FOREIGN KEY (dept_id)
        REFERENCES Department(dept_id)
);

-- Project Table
CREATE TABLE Project (
    project_id INT PRIMARY KEY,
    project_name VARCHAR(100) NOT NULL,
    budget DECIMAL(12,2),
    dept_id INT,
    FOREIGN KEY (dept_id)
        REFERENCES Department(dept_id)
);

-- Employee-Project Relationship
CREATE TABLE Employee_Project (
    emp_id INT,
    project_id INT,
    hours_worked INT,
    PRIMARY KEY (emp_id, project_id),
    FOREIGN KEY (emp_id)
        REFERENCES Employee(emp_id),
    FOREIGN KEY (project_id)
        REFERENCES Project(project_id)
);

-- ---------- Inserting Departments ----------
INSERT INTO Department (dept_id, dept_name, location)
VALUES
(1, 'IT', 'Delhi'),
(2, 'HR', 'Mumbai'),
(3, 'Finance', 'Bangalore'),
(4, 'Marketing', 'Pune'),
(5, 'Research', 'Hyderabad');

-- ---------- Inserting 30 Employees ----------
INSERT INTO Employee
(emp_id, emp_name, gender, salary, hire_date, dept_id)
VALUES
(101, 'Aarav Sharma', 'M', 75000, '2021-01-15', 1),
(102, 'Ananya Singh', 'F', 68000, '2022-03-10', 1),
(103, 'Rohan Verma', 'M', 82000, '2020-07-20', 1),
(104, 'Priya Gupta', 'F', 72000, '2021-11-05', 1),
(105, 'Karan Mehta', 'M', 95000, '2019-06-18', 1),
(106, 'Sneha Kapoor', 'F', 64000, '2023-01-12', 1),
(107, 'Rahul Kumar', 'M', 55000, '2022-05-15', 2),
(108, 'Neha Sharma', 'F', 62000, '2021-09-22', 2),
(109, 'Amit Jain', 'M', 58000, '2020-04-11', 2),
(110, 'Pooja Verma', 'F', 67000, '2023-02-19', 2),
(111, 'Vikas Singh', 'M', 60000, '2022-08-01', 2),
(112, 'Riya Agarwal', 'F', 71000, '2019-12-10', 2),
(113, 'Arjun Patel', 'M', 88000, '2020-01-25', 3),
(114, 'Meera Shah', 'F', 92000, '2019-08-14', 3),
(115, 'Suresh Rao', 'M', 76000, '2021-06-30', 3),
(116, 'Kavya Iyer', 'F', 85000, '2022-10-17', 3),
(117, 'Manish Gupta', 'M', 73000, '2023-03-21', 3),
(118, 'Nisha Reddy', 'F', 90000, '2020-11-09', 3),
(119, 'Aditya Joshi', 'M', 70000, '2021-02-28', 4),
(120, 'Simran Kaur', 'F', 66000, '2022-04-15', 4),
(121, 'Mohit Bansal', 'M', 74000, '2020-09-12', 4),
(122, 'Isha Malhotra', 'F', 81000, '2019-07-07', 4),
(123, 'Nikhil Sethi', 'M', 69000, '2023-01-05', 4),
(124, 'Tanya Arora', 'F', 77000, '2021-12-18', 4),
(125, 'Dev Malhotra', 'M', 98000, '2018-05-20', 5),
(126, 'Aditi Mishra', 'F', 91000, '2020-03-16', 5),
(127, 'Varun Nair', 'M', 87000, '2021-10-08', 5),
(128, 'Shreya Das', 'F', 93000, '2019-11-25', 5),
(129, 'Yash Thakur', 'M', 79000, '2022-07-14', 5),
(130, 'Diya Kapoor', 'F', 96000, '2023-02-02', 5);

-- ---------- Inserting 8 Projects ----------
INSERT INTO Project
(project_id, project_name, budget, dept_id)
VALUES
(201, 'AI Chatbot', 500000, 1),
(202, 'Cloud Migration', 750000, 1),
(203, 'Employee Training', 250000, 2),
(204, 'Financial Analysis', 600000, 3),
(205, 'Digital Marketing', 450000, 4),
(206, 'Market Research', 350000, 4),
(207, 'Machine Learning Research', 900000, 5),
(208, 'AI Security System', 1000000, 5);

-- ---------- Assigning Employees to Projects ----------
INSERT INTO Employee_Project
(emp_id, project_id, hours_worked)
VALUES
(101, 201, 120),
(102, 201, 100),
(103, 202, 150),
(104, 202, 110),
(105, 201, 130),
(106, 202, 90),
(107, 203, 100),
(108, 203, 120),
(109, 203, 80),
(110, 203, 110),
(111, 203, 90),
(112, 203, 130),
(113, 204, 140),
(114, 204, 150),
(115, 204, 120),
(116, 204, 135),
(117, 204, 100),
(118, 204, 145),
(119, 205, 110),
(120, 205, 100),
(121, 206, 120),
(122, 205, 140),
(123, 206, 90),
(124, 206, 130),
(125, 207, 160),
(126, 207, 140),
(127, 208, 150),
(128, 208, 160),
(129, 207, 120),
(130, 208, 135);

-- ---------- Selection ----------
SELECT *
FROM Employee
WHERE salary > 80000;

-- ---------- Projection ----------
SELECT emp_name, salary
FROM Employee;

-- ---------- Aggregate Functions ----------
SELECT
    COUNT(*) AS total_employees,
    SUM(salary) AS total_salary,
    AVG(salary) AS average_salary,
    MAX(salary) AS highest_salary,
    MIN(salary) AS lowest_salary
FROM Employee;

-- ---------- Group By ----------
SELECT
    dept_id,
    COUNT(*) AS employee_count
FROM Employee
GROUP BY dept_id;

-- ---------- Group By with Join ----------
SELECT
    d.dept_name,
    AVG(e.salary) AS average_salary
FROM Department d
JOIN Employee e
    ON d.dept_id = e.dept_id
GROUP BY d.dept_name;

-- ---------- Having ----------
SELECT
    d.dept_name,
    AVG(e.salary) AS average_salary
FROM Department d
JOIN Employee e
    ON d.dept_id = e.dept_id
GROUP BY d.dept_name
HAVING AVG(e.salary) > 75000;

-- ---------- Case Expressions ----------
SELECT
    emp_name,
    salary,
    CASE
        WHEN salary >= 90000 THEN 'High Salary'
        WHEN salary >= 70000 THEN 'Medium Salary'
        ELSE 'Low Salary'
    END AS salary_category
FROM Employee;

-- ---------- Order By ----------
SELECT
    emp_id,
    emp_name,
    salary
FROM Employee
ORDER BY salary DESC;

-- ---------- Complete Employee-Department Report ----------
SELECT
    e.emp_id,
    e.emp_name,
    d.dept_name,
    e.salary
FROM Employee e
JOIN Department d
    ON e.dept_id = d.dept_id
ORDER BY d.dept_name, e.salary DESC;

-- ---------- Project Details ----------
SELECT
    p.project_name,
    d.dept_name,
    p.budget
FROM Project p
JOIN Department d
    ON p.dept_id = d.dept_id
ORDER BY p.budget DESC;

-- ---------- Employees Working on Projects ----------
SELECT
    e.emp_name,
    p.project_name,
    ep.hours_worked
FROM Employee_Project ep
JOIN Employee e
    ON ep.emp_id = e.emp_id
JOIN Project p
    ON ep.project_id = p.project_id
ORDER BY ep.hours_worked DESC;

-- ============================================================
-- EXPERIMENT 4 – JOINS, SUBQUERIES, EXISTS AND EXPLAIN
-- AIM: To perform and analyze SQL queries using INNER JOIN, LEFT JOIN,
-- SELF JOIN, 3-way JOIN, correlated subqueries, EXISTS, and simulated
-- INTERSECT and EXCEPT operations on the Employee–Department–Project
-- database, and to compare their execution plans using EXPLAIN. (Uses
-- the Experiment 3 database.)
-- ============================================================

-- ---------- Inner Join ----------
SELECT
    e.emp_id,
    e.emp_name,
    d.dept_name
FROM Employee e
INNER JOIN Department d
    ON e.dept_id = d.dept_id;

-- ---------- Left Join ----------
SELECT
    d.dept_id,
    d.dept_name,
    e.emp_name
FROM Department d
LEFT JOIN Employee e
    ON d.dept_id = e.dept_id;

-- ---------- Self Join ----------
SELECT
    e1.emp_name AS Employee1,
    e2.emp_name AS Employee2,
    e1.dept_id
FROM Employee e1
JOIN Employee e2
    ON e1.dept_id = e2.dept_id
    AND e1.emp_id < e2.emp_id;

-- ---------- 3-Way Join ----------
SELECT
    e.emp_name,
    d.dept_name,
    p.project_name
FROM Employee e
JOIN Department d
    ON e.dept_id = d.dept_id
JOIN Employee_Project ep
    ON e.emp_id = ep.emp_id
JOIN Project p
    ON ep.project_id = p.project_id;

-- ---------- 3-Way Join (Employee – Department – Project via Department) ----------
SELECT
    e.emp_name,
    d.dept_name,
    e.salary
FROM Employee e
JOIN Department d
    ON e.dept_id = d.dept_id
JOIN Project p
    ON d.dept_id = p.dept_id;

-- ---------- Correlated Subquery ----------
SELECT
    e.emp_id,
    e.emp_name,
    e.salary,
    e.dept_id
FROM Employee e
WHERE e.salary >
(
    SELECT AVG(e2.salary)
    FROM Employee e2
    WHERE e2.dept_id = e.dept_id
);

-- ---------- Exists ----------
SELECT
    e.emp_id,
    e.emp_name
FROM Employee e
WHERE EXISTS
(
    SELECT 1
    FROM Employee_Project ep
    WHERE ep.emp_id = e.emp_id
);

-- ---------- Simulated Intersect ----------
SELECT e.emp_id, e.emp_name
FROM Employee e
WHERE e.dept_id = 1
AND EXISTS
(
    SELECT 1
    FROM Employee_Project ep
    WHERE ep.emp_id = e.emp_id
      AND ep.project_id = 201
);

-- ---------- Simulated Except ----------
SELECT e.emp_id, e.emp_name
FROM Employee e
WHERE e.dept_id = 1
AND NOT EXISTS
(
    SELECT 1
    FROM Employee_Project ep
    WHERE ep.emp_id = e.emp_id
      AND ep.project_id = 201
);

-- ---------- Comparing Execution Plans Using EXPLAIN – Inner Join ----------
EXPLAIN
SELECT
    e.emp_id,
    e.emp_name,
    d.dept_name
FROM Employee e
INNER JOIN Department d
    ON e.dept_id = d.dept_id;

-- ---------- EXPLAIN – Left Join ----------
EXPLAIN
SELECT
    d.dept_name,
    e.emp_name
FROM Department d
LEFT JOIN Employee e
    ON d.dept_id = e.dept_id;

-- ---------- EXPLAIN – Correlated Subquery ----------
EXPLAIN
SELECT
    e.emp_name,
    e.salary
FROM Employee e
WHERE e.salary >
(
    SELECT AVG(e2.salary)
    FROM Employee e2
    WHERE e2.dept_id = e.dept_id
);

-- ---------- EXPLAIN – Exists ----------
EXPLAIN
SELECT
    e.emp_id,
    e.emp_name
FROM Employee e
WHERE EXISTS
(
    SELECT 1
    FROM Employee_Project ep
    WHERE ep.emp_id = e.emp_id
);

-- ============================================================
-- EXPERIMENT 5 – VIEWS AND RECURSIVE CTE
-- AIM: To create and manipulate SQL views for department salary summary
-- and employee hierarchy, test the updatability of views, and implement
-- a recursive CTE to display employee reporting chains. (Uses the
-- Experiment 3 database.)
-- ============================================================

-- ---------- Department Salary Summary View ----------
CREATE VIEW Department_Salary_Summary AS
SELECT
    d.dept_id,
    d.dept_name,
    COUNT(e.emp_id) AS total_employees,
    SUM(e.salary) AS total_salary,
    AVG(e.salary) AS average_salary,
    MAX(e.salary) AS highest_salary,
    MIN(e.salary) AS lowest_salary
FROM Department d
LEFT JOIN Employee e
    ON d.dept_id = e.dept_id
GROUP BY d.dept_id, d.dept_name;

SELECT *
FROM Department_Salary_Summary;

-- ---------- Query the View ----------
-- Find departments with an average salary greater than Rs. 75,000
SELECT
    dept_name,
    average_salary
FROM Department_Salary_Summary
WHERE average_salary > 75000;

-- ---------- Employee Hierarchy ----------
ALTER TABLE Employee
ADD manager_id INT NULL;

ALTER TABLE Employee
ADD CONSTRAINT fk_employee_manager
FOREIGN KEY (manager_id)
REFERENCES Employee(emp_id);

-- ---------- Insert Employee Hierarchy ----------
-- Update Manager IDs
UPDATE Employee SET manager_id = NULL WHERE emp_id IN (105, 112, 114, 122, 125);

-- IT
UPDATE Employee
SET manager_id = 105
WHERE emp_id IN (103, 104);

UPDATE Employee
SET manager_id = 103
WHERE emp_id IN (101, 102);

UPDATE Employee
SET manager_id = 104
WHERE emp_id = 106;

-- HR
UPDATE Employee
SET manager_id = 105
WHERE emp_id IN (103, 104);

UPDATE Employee
SET manager_id = 103
WHERE emp_id IN (101, 102);

UPDATE Employee
SET manager_id = 104
WHERE emp_id = 106;

-- Finance
UPDATE Employee
SET manager_id = 112
WHERE emp_id IN (107, 108);

UPDATE Employee
SET manager_id = 107
WHERE emp_id = 109;

UPDATE Employee
SET manager_id = 108
WHERE emp_id IN (110, 111);

-- Marketing
UPDATE Employee
SET manager_id = 112
WHERE emp_id IN (107, 108);

UPDATE Employee
SET manager_id = 107
WHERE emp_id = 109;

UPDATE Employee
SET manager_id = 108
WHERE emp_id IN (110, 111);

-- Research
UPDATE Employee
SET manager_id = 125
WHERE emp_id IN (126, 127, 128);

UPDATE Employee
SET manager_id = 126
WHERE emp_id = 129;

UPDATE Employee
SET manager_id = 128
WHERE emp_id = 130;

-- ---------- Employee Hierarchy View ----------
CREATE VIEW Employee_Hierarchy AS
SELECT
    e.emp_id,
    e.emp_name AS employee_name,
    e.manager_id,
    m.emp_name AS manager_name,
    e.salary,
    e.dept_id
FROM Employee e
LEFT JOIN Employee m
    ON e.manager_id = m.emp_id;

SELECT *
FROM Employee_Hierarchy;

-- ---------- Test Updatability of a Single View ----------
CREATE VIEW Employee_Basic AS
SELECT
    emp_id,
    emp_name,
    salary,
    dept_id
FROM Employee;

SELECT *
FROM Employee_Basic
WHERE emp_id = 101;

-- ---------- Update Through the View ----------
UPDATE Employee_Basic
SET salary = 78000
WHERE emp_id = 101;

SELECT *
FROM Employee
WHERE emp_id = 101;

-- ---------- Test Non-Updatability of Department Salary View ----------
UPDATE Department_Salary_Summary
SET average_salary = 80000
WHERE dept_id = 1;

-- ---------- Recursive CTE ----------
WITH RECURSIVE EmployeeChain AS
(
    -- Anchor member
    SELECT
        emp_id,
        emp_name,
        manager_id,
        0 AS level,
        CAST(emp_name AS CHAR(500)) AS reporting_chain
    FROM Employee
    WHERE manager_id IS NULL

    UNION ALL

    -- Recursive member
    SELECT
        e.emp_id,
        e.emp_name,
        e.manager_id,
        ec.level + 1,
        CONCAT(ec.reporting_chain, ' -> ', e.emp_name)
    FROM Employee e
    INNER JOIN EmployeeChain ec
        ON e.manager_id = ec.emp_id
)
SELECT
    emp_id,
    emp_name,
    manager_id,
    level,
    reporting_chain
FROM EmployeeChain
ORDER BY reporting_chain;

-- ---------- Recursive CTE for One Employee ----------
WITH RECURSIVE ReportingChain AS
(
    SELECT
        emp_id,
        emp_name,
        manager_id,
        0 AS level
    FROM Employee
    WHERE emp_id = 101

    UNION ALL

    SELECT
        e.emp_id,
        e.emp_name,
        e.manager_id,
        rc.level + 1
    FROM Employee e
    JOIN ReportingChain rc
        ON e.emp_id = rc.manager_id
)
SELECT
    emp_id,
    emp_name,
    manager_id,
    level
FROM ReportingChain;

-- ============================================================
-- EXPERIMENT 6 – STORED PROCEDURE AND TRIGGERS
-- AIM: To create a stored procedure transfer_employee(emp_id,
-- new_dept_id) with validation and error handling, and to implement
-- database triggers for salary validation and audit logging. The
-- procedure and triggers are tested using normal and edge-case inputs.
-- (Uses the Experiment 3 database.)
-- ============================================================

-- ---------- Stored Procedure: transfer_employee ----------
-- The procedure will:
--   1. Check whether the employee exists.
--   2. Check whether the new department exists.
--   3. Check whether the employee is already in that department.
--   4. Transfer the employee.
--   5. Handle errors using SQLEXCEPTION.
--   6. Roll back the transaction if an error occurs.

DELIMITER //

CREATE PROCEDURE transfer_employee(
    IN p_emp_id INT,
    IN p_new_dept_id INT
)
BEGIN

    DECLARE v_employee_count INT DEFAULT 0;
    DECLARE v_department_count INT DEFAULT 0;
    DECLARE v_current_dept INT DEFAULT 0;

    -- Error handler
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
        SELECT 'Error occurred. Transaction rolled back.' AS message;
    END;

    START TRANSACTION;

    -- Check whether employee exists
    SELECT COUNT(*)
    INTO v_employee_count
    FROM Employee
    WHERE emp_id = p_emp_id;

    IF v_employee_count = 0 THEN
        ROLLBACK;
        SELECT 'Error: Employee does not exist.' AS message;

    ELSE

        -- Check whether department exists
        SELECT COUNT(*)
        INTO v_department_count
        FROM Department
        WHERE dept_id = p_new_dept_id;

        IF v_department_count = 0 THEN
            ROLLBACK;
            SELECT 'Error: Department does not exist.' AS message;

        ELSE

            -- Get current department
            SELECT dept_id
            INTO v_current_dept
            FROM Employee
            WHERE emp_id = p_emp_id;

            -- Check if already in requested department
            IF v_current_dept = p_new_dept_id THEN
                ROLLBACK;
                SELECT 'Error: Employee is already in this department.'
                    AS message;

            ELSE

                -- Transfer employee
                UPDATE Employee
                SET dept_id = p_new_dept_id
                WHERE emp_id = p_emp_id;

                COMMIT;

                SELECT
                    'Employee transferred successfully.'
                    AS message;

            END IF;

        END IF;

    END IF;

END //

DELIMITER ;

-- ---------- Testing the Stored Procedure ----------
SELECT
    e.emp_id,
    e.emp_name,
    d.dept_name
FROM Employee e
JOIN Department d
    ON e.dept_id = d.dept_id
WHERE e.emp_id = 101;

CALL transfer_employee(101, 3);

SELECT
    e.emp_id,
    e.emp_name,
    d.dept_name
FROM Employee e
JOIN Department d
    ON e.dept_id = d.dept_id
WHERE e.emp_id = 101;

-- ---------- Edge Case – Employee Does Not Exist ----------
CALL transfer_employee(999, 3);

-- ---------- Edge Case – Department Does Not Exist ----------
CALL transfer_employee(101, 99);

-- ---------- Edge Case – Same Department ----------
CALL transfer_employee(101, 3);

-- ---------- Salary Validation Trigger ----------
-- Salary must be >= 15000
DELIMITER //

CREATE TRIGGER validate_employee_salary
BEFORE INSERT ON Employee
FOR EACH ROW
BEGIN

    IF NEW.salary < 15000 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Salary must be at least 15000';
    END IF;

END //

DELIMITER ;

-- ---------- Test Salary Validation ----------
INSERT INTO Employee
(emp_id, emp_name, gender, salary, hire_date, dept_id)
VALUES
(131, 'Test Employee', 'M', 10000, '2026-09-25', 1);

-- ---------- Test Valid Salary ----------
INSERT INTO Employee
(emp_id, emp_name, gender, salary, hire_date, dept_id)
VALUES
(131, 'Test Employee', 'M', 30000, '2026-09-25', 1);

SELECT
    emp_id,
    emp_name,
    salary
FROM Employee
WHERE emp_id = 131;

-- ---------- Salary Validation for Update ----------
DELIMITER //

CREATE TRIGGER validate_employee_salary_update
BEFORE UPDATE ON Employee
FOR EACH ROW
BEGIN

    IF NEW.salary < 15000 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Salary must be at least 15000';
    END IF;

END //

DELIMITER ;

-- ---------- Test Invalid Salary Update ----------
UPDATE Employee
SET salary = 5000
WHERE emp_id = 131;

SELECT emp_id, emp_name, salary
FROM Employee
WHERE emp_id = 131;

-- ---------- Create Audit Table ----------
CREATE TABLE Salary_Audit (
    audit_id INT AUTO_INCREMENT PRIMARY KEY,
    emp_id INT,
    old_salary DECIMAL(10,2),
    new_salary DECIMAL(10,2),
    changed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    action VARCHAR(30)
);

-- ---------- Audit Logging Trigger ----------
DELIMITER //

CREATE TRIGGER salary_audit_trigger
AFTER UPDATE ON Employee
FOR EACH ROW
BEGIN

    IF OLD.salary <> NEW.salary THEN

        INSERT INTO Salary_Audit
        (
            emp_id,
            old_salary,
            new_salary,
            action
        )
        VALUES
        (
            NEW.emp_id,
            OLD.salary,
            NEW.salary,
            'SALARY UPDATED'
        );

    END IF;

END //

DELIMITER ;

-- ---------- Test Audit Trigger ----------
UPDATE Employee
SET salary = 35000
WHERE emp_id = 131;

SELECT *
FROM Salary_Audit;

-- ---------- Test Multiple Salary Changes ----------
UPDATE Employee
SET salary = 40000
WHERE emp_id = 131;

UPDATE Employee
SET salary = 45000
WHERE emp_id = 131;

SELECT
    audit_id,
    emp_id,
    old_salary,
    new_salary,
    action
FROM Salary_Audit
WHERE emp_id = 131;

-- ---------- Test Edge Case – Salary Unchanged ----------
-- No new audit record is created
UPDATE Employee
SET salary = 45000
WHERE emp_id = 131;

-- ---------- Test Edge Case – NULL Salary ----------
DROP TRIGGER validate_employee_salary;

DELIMITER //

CREATE TRIGGER validate_employee_salary
BEFORE INSERT ON Employee
FOR EACH ROW
BEGIN

    IF NEW.salary IS NULL OR NEW.salary < 15000 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT =
        'Salary cannot be NULL and must be at least 15000';
    END IF;

END //

DELIMITER ;

INSERT INTO Employee
(emp_id, emp_name, gender, salary, hire_date, dept_id)
VALUES
(132, 'Null Salary', 'F', NULL, '2026-09-25', 1);

-- ---------- Check All Triggers ----------
SHOW TRIGGERS;

-- ---------- Check Stored Procedure ----------
SHOW PROCEDURE STATUS
WHERE Db = 'db_454577wwy';
