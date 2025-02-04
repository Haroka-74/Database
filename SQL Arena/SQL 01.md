# SQL 01

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
Write a SQL query to generate a detailed report listing all current employees who are actively working in the organization, including their personal details such as `employee_id`, `first_name`, `last_name`, `email` , `phone_number`, `hire_date`, `job_id`, `salary`, `commission_pct`, `manager_id`, and `department_id`.
```sql
SELECT * FROM employees;
```

## Question 02
Write a SQL query to retrieve the first names of all employees from the `employees` table.
```sql
SELECT first_name FROM employees;
```

## Question 03
Write a SQL query to retrieve the full names of all employees from the `employees` table.
```sql
SELECT first_name + ' ' + last_name 'full_name' FROM employees;

SELECT CONCAT(first_name , ' ' , last_name) 'full_name' FROM employees;
```

## Question 04
Write a SQL query to find all unique department and job role pairs in the organization , ensuring that each pair represents a distinct combination of `department_id` and `job_id`.
```sql
SELECT DISTINCT department_id , job_id FROM employees;
```

## Question 05
Write a SQL query to retrieve the total number of departments that have at least one employee in the organization.
- `SELECT COUNT(department_id) FROM departments;` returns `27`, indicating that the company has a total of `27` departments, but not all of them have active employees.
```sql
SELECT COUNT(DISTINCT department_id) FROM employees;
```

## Question 06
Write a SQL query to retrieve the first names of all employees working in department `90`.
```sql
SELECT first_name FROM employees WHERE department_id = 90;
```

## Question 07
Write a SQL query to retrieve the first names and salaries of all employees whose salary is not equal to `10,000`.
- In SQL Server, inequality can be expressed using both `!=` and `<>`.
```sql
SELECT first_name , salary FROM employees WHERE salary != 10000;

SELECT first_name , salary FROM employees WHERE salary <> 10000;
```

## Question 08
Write a SQL query to retrieve the first names of all employees who were hired after `March 8, 2000`.
```sql
SELECT first_name FROM employees WHERE hire_date > '08-MAR-2000';
```

## Question 09
Write a SQL query to retrieve the first names and salaries of all employees who earn less than `$5,000`.
```sql
SELECT first_name , salary FROM employees WHERE salary < 5000;
```

## Question 10
Write a SQL query to retrieve the first names and salaries of all employees whose salary is between `$2,500` and `$3,500`.
```sql
SELECT first_name , salary FROM employees WHERE salary BETWEEN 2500 AND 3500;
```

## Question 11
Write a SQL query to retrieve the first names and salaries of all employees whose salary is not between `$2,500` and `$3,500`.
```sql
SELECT first_name , salary FROM employees WHERE salary NOT BETWEEN 2500 AND 3500;
```

## Question 12
Write a SQL query to retrieve the first names and job IDs of all employees who hold one of the following job titles: `AD_PRES`, `IT_PROG`, or `FI_ACCOUNT`.
```sql
SELECT first_name , job_id FROM employees WHERE job_id IN ('AD_PRES' , 'IT_PROG' , 'FI_ACCOUNT');
```

## Question 13
Write a SQL query to retrieve the first names and job IDs of all employees who don't hold the job titles: `AD_PRES`, `IT_PROG`, and `FI_ACCOUNT`.
```sql
SELECT first_name , job_id FROM employees WHERE job_id NOT IN ('AD_PRES' , 'IT_PROG' , 'FI_ACCOUNT');
```

## Question 14
Write a SQL query to retrieve the first names, department IDs, and salaries of all employees who either work in department `90` or have a salary greater than `$6,000`.
```sql
SELECT first_name , department_id , salary FROM employees WHERE department_id = 90 OR salary > 6000;
```

## Question 15
Write a SQL query to retrieve the first names of all employees who are not assigned to any department.
```sql
SELECT first_name FROM employees WHERE department_id IS NULL;
```

## Question 16
Write a SQL query to retrieve the first names of all employees who are assigned to a department.
```sql
SELECT first_name FROM employees WHERE department_id IS NOT NULL;
```