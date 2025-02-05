# SQL 03

The `employees` table stores detailed information about employees in the organization.

| Column Name        | Data Type        | Constraints                           | Description                                      |
|--------------------|------------------|---------------------------------------|--------------------------------------------------|
| `employee_id`      | `INT`            | `PRIMARY KEY`                         | Unique identifier for each employee              |
| `first_name`       | `VARCHAR(20)`    | `DEFAULT NULL`                        | First name of the employee                       |
| `last_name`        | `VARCHAR(25)`    | `NOT NULL`                            | Last name of the employee                        |
| `email`            | `VARCHAR(25)`    | `NOT NULL`                            | Email address of the employee                    |
| `phone_number`     | `VARCHAR(20)`    | `DEFAULT NULL`                        | Contact phone number of the employee             |
| `hire_date`        | `DATE`           | `NOT NULL`                            | Date when the employee was hired                 |
| `job_id`           | `VARCHAR(10)`    | `FOREIGN KEY`, `NOT NULL`             | Foreign key referencing the employee's job role  |
| `salary`           | `DECIMAL(8, 2)`  | `NOT NULL`                            | Salary of the employee                           |
| `commission_pct`   | `DECIMAL(2, 2)`  | `DEFAULT NULL`                        | Commission percentage (if applicable)            |
| `manager_id`       | `INT`            | `FOREIGN KEY`, `DEFAULT NULL`         | Foreign key referencing the employee's manager   |
| `department_id`    | `INT`            | `FOREIGN KEY`, `DEFAULT NULL`         | Foreign key referencing the employee's department|

## Question 01
Write a SQL query to retrieve the number of employees in each department that has at least one employee in the organization.
- `SELECT COUNT(department_id) FROM departments;` returns `27`, indicating that the company has a total of `27` departments, but not all of them have active employees.
```sql
SELECT department_id, COUNT(*) 'number_of_employees'
FROM employees
GROUP BY department_id;
```

## Question 02
Write a SQL query to retrieve the total salary, minimum salary, and maximum salary for each department in the organization.
```sql
SELECT department_id, SUM(salary) 'total_salary', MIN(salary) 'minimum_salary', MAX(salary) 'maximum_salary'
FROM employees
GROUP BY department_id;
```

## Question 03
Write a SQL query to retrieve the minimum total salary across all departments, where the total salary is calculated as the sum of salaries for each department.
- `A Common Table Expression (CTE)` is a temporary result set in SQL that you can reference within a `SELECT`, `INSERT`, `UPDATE`, or `DELETE` statement.
```sql
WITH TotalSalaries AS (
    SELECT department_id, SUM(salary) AS total_salary
    FROM employees
    GROUP BY department_id
)
SELECT MIN(total_salary)
FROM TotalSalaries;
```

The `teachers` table stores information about which teachers are assigned to teach specific subjects in different departments.

| Column Name        | Data Type        | Constraints                           | Description                                                            |
|--------------------|------------------|---------------------------------------|------------------------------------------------------------------------|
| `teacher_id`       | `INT`            | `NOT NULL`                            | Unique identifier for the teacher                                      |
| `subject_id`       | `INT`            | `NOT NULL`                            | Unique identifier for the subject                                      |
| `dept_id`          | `INT`            | `NOT NULL`                            | Unique identifier for the department <br> where the subject is taught  |

- `(subject_id, dept_id)` is the primary key (combinations of columns with unique values) of the `teachers` table.

## Question 04
Write a SQL query to calculate the number of unique subjects taught by each teacher.
> A teacher can teach the same subject in different departments.
```sql
SELECT teacher_id, COUNT(DISTINCT subject_id) AS subject_count
FROM teachers
GROUP BY teacher_id;
```

## Question 05
Write a SQL query to find the number of employees in each department whose salary is greater than `10,000`.
- The `WHERE` clause is applied before grouping the data, meaning it filters out rows that do not meet the condition `(salary > 10000)` before performing the `GROUP BY` operation.
```sql
SELECT department_id, COUNT(*) 'employee_count'
FROM employees
WHERE salary > 10000
GROUP BY department_id;
```

## Question 06
Write a SQL query to find the number of employees in each department, grouped by their respective job roles.
```sql
SELECT department_id, job_id, COUNT(*) 'employee_count'
FROM employees
GROUP BY department_id, job_id;
```

## Question 07
Write a SQL query to retrieve the department IDs and the number of employees in each department that has more than `5` employees, sorted by the number of employees in ascending order.
- The `HAVING` clause is used to filter groups after the aggregation is performed, whereas the `WHERE` clause filters rows before the grouping operation. In this case, `HAVING COUNT(*) > 5` filters out departments with `5` or fewer employees after the grouping by department.
- The `ORDER BY` clause can be used with aggregate functions, like `COUNT()`, to sort the results based on the computed aggregate. Here, `ORDER BY COUNT(*)` sorts the departments by the total number of employees in ascending order.
```sql
SELECT department_id, COUNT(*) 'employee_count'
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 5
ORDER BY COUNT(*);
```

## Question 08
Write a SQL query to retrieve the first names of all employees who hold a managerial position and oversee at least `7` employees.
```sql
WITH managers_ids AS (
    SELECT manager_id
    FROM employees
    GROUP BY manager_id
    HAVING COUNT(*) >= 7
)
SELECT first_name
FROM employees
WHERE employee_id IN (SELECT manager_id FROM managers_ids);
```

The `Employee_Departments` table stores information about employees and the departments they belong to, with an additional flag indicating the primary department for each employee.

| Column Name    | Data Type | Constraints | Description                                                    |
|----------------|-----------|-------------|----------------------------------------------------------------|
| `employee_id`  | `INT`     | `NOT NULL`  | Unique identifier for each employee                            |
| `department_id`| `INT`     | `NOT NULL`  | Identifier for the department where the employee works         |
| `primary_flag` | `VARCHAR` | `NOT NULL`  | Flag `('Y' or 'N')` indicating if the department is primary    |

- `(employee_id, department_id)` is the primary key (combinations of columns with unique values) of the `Employee_Departments` table.
- A flag `('Y' or 'N')` indicating if the department is the primary department for the employee. If `Y`, it is the primary department; if `N`, it is not the primary.
    - Note that when an employee belongs to only one department, their primary column is `N`.

## Question 09
Write a SQL query to report all employees with their primary department, and for employees who belong to only one department, report that department as their primary.
```sql
WITH SingleDepartmentEmployees AS (
    SELECT employee_id
    FROM Employee_Departments
    GROUP BY employee_id
    HAVING COUNT(*) = 1
)
SELECT employee_id, department_id
FROM Employee_Departments
WHERE primary_flag = 'Y'
    OR employee_id IN (SELECT employee_id FROM SingleDepartmentEmployees);
```

## Question 10
Write a SQL query to calculate the percentage of employees who earn more than `15,000`.
```sql
SELECT AVG(IIF(salary > 15000 , 1.0 , 0.0)) * 100 'percentage'
FROM employees;
```

## Question 11
Write a SQL query to find the department ID with the highest total salary, where the total salary is calculated as the sum of salaries for each department.
- The `TOP` clause in SQL is used to limit the number of rows returned by a query. This is especially useful when you only need a subset of the results, such as the first few records in a dataset. The `TOP` clause is typically used with `ORDER BY` to ensure the results are returned in a specific order. If no ORDER BY is provided, the result order is not guaranteed.
```sql
SELECT TOP 1 department_id, SUM(salary) AS total_salary
FROM employees
GROUP BY department_id
ORDER BY total_salary DESC;
```