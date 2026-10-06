# Security Data Analysis and Automation

## Purpose

Security work produces large amounts of data, and analysts need to query it and automate repetitive checks. These labs apply SQL to login, employee and machine records to answer investigative questions, such as which logins failed after business hours or which employee uses a given device. The Python labs automate everyday analyst tasks, including allow-list checks, failed-login analysis and user-to-device verification.

## Labs Completed

| Project | Lab | Objective | Tools used | Lessons learnt |
|---|---|---|---|---|
| [SQL for Security Analysis](https://github.com/Osita-Odo/SQL-and-Python-Automation/blob/main/SQL%20Lab.md)<br>*Platform: Google Skills (Qwiklabs)* | [Querying and Sorting a Database] | Retrieve and sort login and machine data. | `SELECT`, `FROM`, `ORDER BY` | Sorting by date and then time puts login events in their true order. |
| | [Filtering with WHERE, LIKE and Wildcards] | Filter records by exact values and patterns. | `WHERE`, `LIKE`, `%`, `_` | Wildcards catch variations that exact matches miss. |
| | [Filters on Numeric, Date, and Time Data] | Filter logins by date, time and ID. | Comparison operators, `BETWEEN` | `>` excludes the boundary value, while `>=` and `BETWEEN` include it. |
| | [Filters with AND, OR, and NOT] | Combine conditions to answer investigative questions. | `AND`, `OR`, `NOT` | One precise query can isolate failed logins after business hours. |
| | [Joining in SQL] | Link machines, employees and login attempts. | `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN` | Joins connect log activity to real users and devices. |
| | [Aggregate Functions] | Summarise large result sets into single values. | `COUNT`, `AVG`, `SUM` | Aggregates turn large results into one actionable number. |
| [Python Lab: Google Cybersecurity](https://github.com/Osita-Odo/Python-Lab---Google-Cybersecurity)<br>*Platform: Google Cybersecurity Certificate (Coursera)* | [Assign Python Variables](https://github.com/Osita-Odo/Python-Lab---Google-Cybersecurity/blob/main/01_Assign_Python_variables.md) | Track login information with variables. | Variables, `type()` | Knowing a value's data type prevents errors later in the code. |
| | [Create a Conditional Statement](https://github.com/Osita-Odo/Python-Lab---Google-Cybersecurity/blob/main/02_Create_a_conditional_statement.md) | Decide on OS updates and access permissions. | `if`/`elif`/`else`, `in`, `and`/`or` | The `in` operator makes allow-list checks simple. |
| | [Create Loops](https://github.com/Osita-Odo/Python-Lab---Google-Cybersecurity/blob/main/03_Create_loops.md) | Handle connection attempts, IP checks and ID generation. | `for`, `while`, `range()`, `break` | `break` stops a loop once the answer is found. |
| | [Work with Strings](https://github.com/Osita-Odo/Python-Lab---Google-Cybersecurity/blob/main/04_Work_with_strings.md) | Standardise employee IDs, device IDs and URLs. | `str()`, `len()`, slicing, `.index()` | Slicing extracts exactly the part of an ID I need. |
| | [Define and Call a Function](https://github.com/Osita-Odo/Python-Lab---Google-Cybersecurity/blob/main/05_Define_and_call_a_function.md) | Build alert functions and convert lists to strings. | `def`, function calls, concatenation | Functions let me reuse a check instead of repeating code. |
| | [Create More Functions](https://github.com/Osita-Odo/Python-Lab---Google-Cybersecurity/blob/main/06_Create_more_functions.md) | Analyse failed login attempts. | `sorted()`, `max()`, parameters, `return` | `return` lets one function's result feed the next step. |
| | [Develop an Algorithm](https://github.com/Osita-Odo/Python-Lab---Google-Cybersecurity/blob/main/07_Develop_an_algorithm.md) | Match users to their assigned devices. | `.append()`, `.remove()`, `.index()`, nested conditionals | Two lists linked by the same index can map users to devices. |
| | [Define and Call a Function (Alternate)](https://github.com/Osita-Odo/Python-Lab---Google-Cybersecurity/blob/main/09_Define_and_call_a_function_alt.md) | Compare printing inside and outside a loop. | Loops, `print()` | Where `print()` sits decides whether I get one result or many. |
