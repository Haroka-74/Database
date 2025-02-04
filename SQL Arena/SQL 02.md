# SQL 02

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
Write a SQL query to retrieve the first names and salaries of all employees sorted by their `salaries` in ascending order.
- `ORDER BY` clause sorts the result set in ascending order by default, or explicitly by using `ASC`.
```sql
SELECT first_name , salary FROM employees ORDER BY salary;
```

## Question 02
Write a SQL query to retrieve the first names and hire dates of all employees sorted by their `hire dates` in descending order.
```sql
SELECT first_name , hire_date FROM employees ORDER BY hire_date DESC;
```

## Question 03
Write a SQL query to retrieve the full names of all employees sorted by their `first names` in ascending order, and in case of a tie, sorted by their `last names` in descending order.
```sql
SELECT first_name + ' ' + last_name AS 'full_name' FROM employees ORDER BY first_name ASC , last_name DESC;
```

## Question 04
Write a SQL query to retrieve the first names of all employees sorted by the length of their `first names` in descending order.
```sql
SELECT first_name FROM employees ORDER BY LEN(first_name) DESC;
```

## Question 05
Write a SQL query to retrieve the first names, salaries, and commission percentages of all employees sorted by the total value of their `salaries` plus the product of their `salaries` and `commission percentages` in ascending order.
- A user-defined function `CalculateTotalSalary(salary, commission_pct)` is created to compute the total salary by adding the salary and the commission percentage if available, and if no commission percentage is specified, the function returns only the salary.
```sql
CREATE FUNCTION CalculateTotalSalary (@salary DECIMAL(8, 2), @commission_pct DECIMAL(2, 2))
RETURNS DECIMAL(8, 2) AS
BEGIN
    RETURN CASE
        WHEN @commission_pct IS NULL THEN @salary
        ELSE @salary + (@salary * @commission_pct)
    END
END;
```
- In the following query, the `CalculateTotalSalary(salary, commission_pct)` function is called to compute the total salary for each employee, and the results are sorted in ascending order by the calculated total salary.
```sql
SELECT first_name , salary , commission_pct , dbo.CalculateTotalSalary(salary , commission_pct) 'total_salary' FROM employees ORDER BY total_salary ASC;
```