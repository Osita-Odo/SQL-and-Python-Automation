# SQL for Security Analysis: Queries, Filters, Joins & Aggregate Functions

## Objective

This project applies SQL as a data-analysis tool for security work, using relational databases of login attempts, employees, and machines. The labs progress from basic queries and sorting, through filtering with patterns, comparison operators, and logical operators (AND, OR, NOT), to joining multiple tables and running aggregate functions. The security framing runs throughout: querying a log of login attempts to investigate a potential incident, isolating failed logins after business hours, returning logins from specific countries or date ranges, and mapping employees to the machines they use. The goal was to build the practical SQL fluency an analyst needs when security-relevant data is stored in a database rather than a flat log file.

**Video recordings on SQL Labs:** [Open Google Drive folder](https://drive.google.com/drive/folders/1rFz_V3YxIsxE5qrGnIsxpxaTh8bAw8lB?usp=sharing)

## Skills Learned

* Writing basic queries with `SELECT` and `FROM`, including selecting all columns with `*`.
* Sorting results with `ORDER BY`, on single and multiple columns, ascending and descending (`DESC`).
* Filtering rows with `WHERE`, using equality and pattern matching (`LIKE` with the `%` and `_` wildcards).
* Filtering numeric, date, and time data with comparison operators (`<`, `>`, `=`, `<=`, `>=`, `<>` / `!=`) and `BETWEEN` (inclusive ranges).
* Combining conditions with the logical operators `AND`, `OR`, and `NOT`, including nesting them for complex filters.
* Joining tables with `INNER JOIN`, `LEFT JOIN`, and `RIGHT JOIN` on a shared key.
* Running aggregate functions (`COUNT`, `AVG`, `SUM`) and combining them with filters.
* Understanding relational database concepts: primary keys, foreign keys, and when SQL is preferable to Linux for filtering.

## Tools Used

* MariaDB / MySQL-compatible relational database (Google Skills / Qwiklabs environment).
* SQL (Structured Query Language) executed through the Bash shell.
* Datasets: `log_in_attempts`, `employees`, `machines`, and `customers` tables.

---

## Background: Relational Databases

Relational databases store data in tables, and a database can hold multiple tables.

**Keys**
* **Primary key:** uniquely identifies each record; there is only one primary key per table.
* **Foreign key:** a column in a table that is a primary key in another table. Its values can duplicate, and it allows one table to be connected to another.

**Query:** a query is a request for data from a database table or a combination of tables. This is directly applicable to log analysis.

## Lab 1 — Querying and Sorting a Database

**Query basics**
`SELECT` and `FROM` are not case-sensitive. Columns are separated with commas (`,`), and every statement ends with a semicolon (`;`). The asterisk (`*`) selects all columns.

```sql
-- Basic query: select specific columns
SELECT employee_ID, device_ID
FROM employees;

-- Select all columns
SELECT *
FROM employee;
```

**Querying a database (SELECT ... FROM)**
```sql
SELECT customerid, city, country
FROM customers;
```

**Sorting with ORDER BY**
```sql
SELECT customerid, city, country
FROM customers
ORDER BY city;
```

**Sorting based on multiple columns**
SQL sorts the output by the first column, and for rows with the same value it sorts them by the second column (here: country first, then city).
```sql
SELECT customerid, city, country
FROM customers
ORDER BY country, city;
```

**Sorting in descending order**
```sql
SELECT customerid, city, country
FROM customers
ORDER BY city DESC;
```

### Lab Practice — Codes

```sql
SELECT *
FROM machines;

SELECT device_id, email_client
FROM machines;

SELECT device_id, operating_system, OS_patch_date
FROM machines;

SELECT event_id, country
FROM log_in_attempts;

SELECT username, login_date, login_time
FROM log_in_attempts;

SELECT *
FROM log_in_attempts;

SELECT *
FROM log_in_attempts
ORDER BY login_date;

SELECT *
FROM log_in_attempts
ORDER BY login_date, login_time;
```

*Ref 1: Activity — Perform a SQL query (retrieving and reviewing table data)*

<img width="940" height="478" alt="image" src="https://github.com/user-attachments/assets/70ce37fa-e495-4ad4-9b32-702cd37dcf92" />

*Ref 2: Ordering login attempt data by date and time*

<img width="940" height="477" alt="image" src="https://github.com/user-attachments/assets/468210e7-f182-4333-be32-b04dde8b78db" />

---

## Lab 2 — Filtering with WHERE, LIKE and Wildcards

**Filtering**
The `WHERE` clause uses the equals sign (`=`) for exact matches.
```sql
SELECT *
FROM log_in_attempts
WHERE country = 'USA';
```

For patterns, use `%` together with the `LIKE` operator instead of the `=` sign.
```sql
SELECT *
FROM log_in_attempts
WHERE country LIKE 'US%';
```

**Filtering for patterns**
Filtering for a pattern requires two elements in the `WHERE` clause: a wildcard and the `LIKE` operator.

**Wildcards**
A wildcard is a special character that can be substituted with any other character. The two most useful are:
* The percentage sign (`%`) substitutes for any number of characters.
* The underscore (`_`) substitutes for exactly one character.

These can be placed after a string, before a string, or in both locations. Applied to the string `'a'`:

| Pattern | Results that could be returned |
|---------|--------------------------------|
| `'a%'` | apple123, art, a |
| `'a_'` | as, an, a7 |
| `'a__'` | ant, add, a1c |
| `'%a'` | pizza, Z6ra, a |
| `'_a'` | ma, 1a, Ha |
| `'%a%'` | Again, back, a |
| `'_a_'` | Car, ban, ea7 |

### Practical — Code

```sql
SELECT device_id, operating_system
FROM machines;

SELECT device_id, operating_system
FROM machines
WHERE operating_system = 'OS 2';

SELECT *
FROM employees
WHERE department = 'Finance';

SELECT *
FROM employees
WHERE department = 'Sales';

SELECT *
FROM employees
WHERE office = 'South-109';

SELECT *
FROM employees
WHERE office LIKE 'South%';
```

*Ref 3: Activity — Filter a SQL query (WHERE clause on machine and employee data)*


<img width="940" height="435" alt="image" src="https://github.com/user-attachments/assets/71437e07-1247-4b03-8d04-66d8aa5526bd" />

*Ref 4: Pattern filtering with LIKE 'South%'*

<img width="940" height="426" alt="image" src="https://github.com/user-attachments/assets/4d2185e4-9547-4654-8374-c49f179e6149" />

---

## Lab 3 — Filters on Numeric, Date, and Time Data

**Comparison operators**
Filtering numeric and date/time data often involves operators, used to return only the rows you need:

| operator | use |
|----------|-----|
| `<` | less than |
| `>` | greater than |
| `=` | equal to |
| `<=` | less than or equal to |
| `>=` | greater than or equal to |
| `<>` | not equal to |

Note: `!=` can also be used as an alternative operator for "not equal to." For date and time values, use quotation marks; this is not the case for numbers.

The `>` operator is **exclusive** (it does not include the value of comparison) and `>=` is **inclusive** (it includes the value of comparison).

**BETWEEN**
`BETWEEN` filters for numbers or dates within a range. For example, to find the first and last names of all employees hired between January 1, 2002 and January 1, 2003, the `BETWEEN` operator is used. `BETWEEN` is inclusive, so records with a hire date of January 1, 2002 or January 1, 2003 are included in the results.

### Lab Practice — Codes used

```sql
SELECT *
FROM log_in_attempts
WHERE login_date > '2023-01-15';

SELECT *
FROM log_in_attempts
WHERE login_date BETWEEN '2023-02-01' AND '2023-02-07';

SELECT *
FROM log_in_attempts
WHERE login_time = '09:30:00';

SELECT *
FROM log_in_attempts
WHERE login_id = 503;
```

*Ref 5: Filtering login attempts by date with a comparison operator*

<img width="940" height="428" alt="image" src="https://github.com/user-attachments/assets/c285ab79-8e88-409a-bb12-ede1eed2ee88" />


*Ref 6: Filtering login attempts within a date range using BETWEEN*

<img width="940" height="437" alt="image" src="https://github.com/user-attachments/assets/e59425d2-1750-4ea8-ae63-5d3de5b20cd7" />


## Lab 4 — Filters with AND, OR, and NOT

**AND** — both conditions must be met simultaneously.
```sql
SELECT firstname, lastname, email, country, supportrepid
FROM customers
WHERE supportrepid = 5 AND country = 'USA';
```

**OR** — either condition can be met; returns results where the first, the second, or both are met. Even if both conditions are based on the same column, both full conditions must be written out.
```sql
SELECT firstname, lastname, email, country
FROM customers
WHERE country = 'Canada' OR country = 'USA';
```

**NOT** — works on a single condition and negates it, returning all records that don't match.
```sql
SELECT firstname, lastname, email, country
FROM customers
WHERE NOT country = 'USA';
```

**Combining logical operators**
Operators can be combined. To return customers in all countries besides the USA and Canada, `NOT` is placed before the first condition, joined to a second condition with `AND`, and `NOT` is also placed before that second condition.
```sql
SELECT firstname, lastname, email, country
FROM customers
WHERE NOT country = 'Canada' AND NOT country = 'USA';
```

### Practical Lab

```sql
-- Retrieve all failed login attempts that occurred after business hours (after 18:00)
SELECT *
FROM log_in_attempts
WHERE login_time > '18:00' AND success = FALSE;

-- Retrieve all login attempts that occurred on May 8, 2022 or May 9, 2022
SELECT *
FROM log_in_attempts
WHERE login_date = '2022-05-09' OR login_date = '2022-05-08';

-- Retrieve all login attempts that did not originate in Mexico
SELECT *
FROM log_in_attempts
WHERE NOT country LIKE 'MEX%';

-- Retrieve all employees in the Marketing department who are located in the East building
SELECT *
FROM employees
WHERE department = 'Marketing' AND office LIKE 'East%';

-- Retrieve all employees who work in either the Finance or Sales department
SELECT *
FROM employees
WHERE department = 'Finance' OR department = 'Sales';

-- Retrieve all employees who are not in the Information Technology department
SELECT *
FROM employees
WHERE NOT department = 'Information Technology';
```

*Ref 7: Activity — Filter with AND, OR, and NOT (failed after-hours login attempts)*

<img width="940" height="432" alt="image" src="https://github.com/user-attachments/assets/10d9c1b7-aa4b-4a4b-bcc7-a1c6a0b2f5ab" />


*Ref 8: Excluding a department with NOT*

<img width="940" height="433" alt="image" src="https://github.com/user-attachments/assets/a11632c0-30d4-4293-87d0-1c40c8cbada4" />

---

## Lab 5 — Joining in SQL

Joins combine rows from two tables based on a shared column, letting an analyst connect information that lives in separate tables (for example, matching machines to the employees who use them).

```sql
-- Question 1: Retrieve all records from the machines table to review available machine data.
SELECT *
FROM machines;

-- Question 2: Use an INNER JOIN to identify which employees are using which machines
-- by matching records on the shared device_id column.
SELECT *
FROM machines
INNER JOIN employees
ON machines.device_id = employees.device_id;

-- Question 3: Use a LEFT JOIN to return all machines and any employees assigned to them,
-- including machines that are not assigned to any employee.
SELECT *
FROM machines
LEFT JOIN employees
ON machines.device_id = employees.device_id;

-- Question 4: Use a RIGHT JOIN to return all employees and any machines assigned to them,
-- including employees who do not have a machine assigned.
SELECT *
FROM machines
RIGHT JOIN employees
ON machines.device_id = employees.device_id;

-- Question 5: Use an INNER JOIN to retrieve all login attempts made by employees
-- by joining the employees and log_in_attempts tables on the username column.
SELECT *
FROM employees
INNER JOIN log_in_attempts
ON employees.username = log_in_attempts.username;
```

## Lab 6 — Other Functions in SQL: Aggregate Functions

Aggregate functions perform a calculation over multiple data points and return the result of the calculation; the actual underlying data is not returned.

* **`COUNT`** returns a single number representing the number of rows returned from the query.
* **`AVG`** returns a single number representing the average of the numerical data in a column.
* **`SUM`** returns a single number representing the sum of the numerical data in a column.

**Aggregate function syntax**
Place the function keyword after `SELECT`, then indicate the target column in parentheses.
```sql
SELECT COUNT(firstname)
FROM customers;
```

To find the number of customers from a specific country, add a filter:
```sql
SELECT COUNT(firstname)
FROM customers
WHERE country = 'USA';
```

## Key Takeaways

* SQL's column structure and `JOIN` capability make it far better than flat-file Linux filtering when security data lives in a database, but Linux is still needed for plain-text logs SQL cannot read.
* `LIKE` with `%` and `_` turns exact-match filtering into flexible pattern matching, useful for country codes, office names, and log fields.
* Combining `WHERE` conditions with `AND`, `OR`, and `NOT` lets a single query answer targeted investigative questions such as "failed logins after 18:00."
* Joins are what let an analyst connect identity to activity, for example matching a username in `log_in_attempts` to an employee record.
* Aggregate functions summarize large result sets into a single actionable number.
