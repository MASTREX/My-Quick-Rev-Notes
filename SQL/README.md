> Note: Mainly syntax of MySQL

> :exclamation: For revision. For whole reference use [w3schools](https://www.w3schools.com/mysql/default.asp)

<!--
DDL: data defining language			CREATE, ALTER, DROP, MODIFY
DML: data manipulating language		INSERT, UPDATE, DELETE
DQL: data query language			select
DCL: data control language

Datatypes:
varchar2(size)
number(size, base)		-- base not confirm
date
long

Some of The Most Important SQL Commands
	SELECT - extracts data from a database
	UPDATE - updates data in a database
	DELETE - deletes data from a database
	INSERT INTO - inserts new data into a database
	CREATE DATABASE - creates a new database
	ALTER DATABASE - modifies a database
	CREATE TABLE - creates a new table
	ALTER TABLE - modifies a table
	DROP TABLE - deletes a table
	CREATE INDEX - creates an index (search key)
	DROP INDEX - deletes an index
-->

# Important Points
- SQL keywords are NOT case sensitive: `select` is the same as `SELECT`
- Column heading name with space are surrounded with square brackets [Contact Person]
- SQL requires single quotes around text values (most database systems will also allow double quotes). However, numeric fields should not be enclosed in quotes
- We will have to use the `IS NULL` and `IS NOT NULL` operators for null comparison
- Input DATE as `#07/01/1996#` or `'1996-07-01'`
- Group or aggregate functions should not be combined to single values or functions or ERROR is raised
  TODO: give example
- Single `=` is used for equalTo and `<>` or `!=` for not equalTo

<br>

> ### Order of writing
`SELECT > WHERE > GROUP BY > Group by FUNCTIONS > HAVING > ORFER BY`
> ### Order of Execution
| ORDER | CLAUSE | FUNCTION |
|-|-|-|
| 1 | `FROM`		| Choose and join tables to get base data |
| 2 | `WHERE`		| Filters the base data |
| 3 | `GROUP BY`	| Aggregates the base data |
| 4 | `HAVING`		| Filters the aggregated data |
| 5 | `SELECT`		| Returns the final data |
| 6 | `ORDER BY`	| Sorts the final data |
| 7 | `LIMIT`		| Limits the returned data to a row count |

### Comments
```MySQL
--single line
/* Multiline */
```

<br>

# SQL Statements
1. ### SELECT
	The `SELECT` statement is used to select data from a database.The data returned is stored in a result table, called the `result-set`.
	```SQL
	SELECT column1, column2, ...
	FROM table_name;
	```

1. ### DISTINCT
	```SQL
	SELECT COUNT(DISTINCT Country) FROM Customers;
	```
	```SQL
	-- MS Access
	SELECT Count(*) AS DistinctCountries
	FROM (SELECT DISTINCT Country FROM Customers);
	```

1. ### WHERE
	```SQL
	SELECT * FROM Products WHERE Price BETWEEN 50 AND 60;
	SELECT * FROM Customers WHERE City LIKE 's%';
	SELECT * FROM Customers WHERE City IN ('Paris','London');
	```
	:exclamation: A `NOT IN` query will not return any rows if any `NULL`s exists in the list of `NOT IN` values.

1. ### ORDER BY
	The `ORDER BY` keyword sorts the records in ascending order by default. To sort the records in descending order, use the `DESC` keyword.
	```SQL
	SELECT column1, column2, ...
	FROM table_name
	ORDER BY column1, column2, ... ASC|DESC;
	```
	Example: The following SQL statement selects all customers from the `Customers` table, sorted by the `Country` and the `CustomerName` column. This means that it orders by Country, but if some rows have the same Country, it orders them by CustomerName:
	```SQL
	SELECT * FROM Customers
	ORDER BY Country ASC, CustomerName DESC;
	```

1. ### INSERT INTO
	```SQL
	INSERT INTO table_name (column1, column2, column3, ...)
	VALUES (value1, value2, value3, ...);
	```

1. ### UPDATE
	```SQL
	UPDATE table_name
	SET column1 = value1, column2 = value2, ...
	WHERE condition;
	```
	> :exclamation: If you omit the WHERE clause, all records in the table will be updated!

1. ### DELETE
	```SQL
	DELETE FROM table_name WHERE condition;
	```

1. ### SELECT TOP
	> :exclamation: Not all database systems support the `SELECT TOP` clause. MySQL supports the `LIMIT` clause to select a limited number of records, while Oracle uses `FETCH FIRST n ROWS ONLY` and `ROWNUM`.

1. ### ALIASES
	```SQL
	-- Alias Column Syntax
	SELECT column_name AS alias_name
	FROM table_name;

	-- Alias Table Syntax 1
	SELECT column_name(s)
	FROM table_name AS alias_name;
	-- Alias Table Syntax 2
	SELECT column_name(s)
	FROM table_name alias_name, table2_name alias_name2;
	```

1. ### BETWEEN
	The `BETWEEN` operator is inclusive that is begining and end values are included

1. ### GROUP BY
	```SQL
	SELECT column_name(s)
	FROM table_name
	WHERE condition
	GROUP BY column_name(s)		-- position is also imp after WHERE and before ORDER BY
	ORDER BY column_name(s);
	```

1. ### HAVING
	To filter subset or groups of `GROUP BY`

1. ### UNION
	The `UNION` operator is used to combine the result-set of two or more `SELECT` statements.

	- Every `SELECT` statement within `UNION` must have the same number of columns
	- The columns must also have similar data types
	- The columns in every `SELECT` statement must also be in the same order
	```SQL
	SELECT column_name(s) FROM table1
	UNION
	SELECT column_name(s) FROM table2;
	```

1. ### UNION ALL
	The `UNION` operator selects only distinct values by default. To allow duplicate values, use `UNION ALL`

1. ### EXISTS
	- The `EXISTS` operator is used to test for the existence of any record in a subquery.
	- The `EXISTS` operator returns TRUE if the subquery returns one or more records.
	```SQL
	SELECT column_name(s)
	FROM table_name
	WHERE EXISTS
	(SELECT column_name FROM table_name WHERE condition);
	```

1. ### ANY and ALL
	`ANY` means that the condition will be true if the operation is true for any of the values in the range.
	```SQL
	SELECT column_name(s)
	FROM table_name
	WHERE column_name operator ANY
	(SELECT column_name
	FROM table_name
	WHERE condition);
	```

	`ALL` means that the condition will be true only if the operation is true for all values in the range.

	Is used with `SELECT`, `WHERE` and `HAVING` statements
	```SQL
	SELECT column_name(s)
	FROM table_name
	WHERE column_name operator ALL
	(SELECT column_name
	FROM table_name
	WHERE condition);
	```

	> :exclamation: The operator must be a standard comparison operator (=, <>, !=, >, >=, <, or <=).

1. ### CASE
	```SQL
	CASE
		WHEN condition1 THEN result1
		WHEN condition2 THEN result2
		WHEN conditionN THEN resultN
		ELSE result
	END;
	```
	> :exclamation: If there is no `ELSE` part and no conditions are true, it returns `NULL`.

## Joins
#### INNER JOIN
Returns records that have matching values in both tables
```SQL
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
FROM Orders
INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID;
```
#### LEFT JOIN
Returns all records from the left table, and the matched records from the right table
#### RIGHT JOIN
Returns all records from the right table, and the matched records from the left table
#### CROSS JOIN
1. Returns all records from both tables
2. Also known as `FULL OUTER JOIN` and `FULL JOIN` in SQL (not MySQL)
3. !!! If you add a WHERE clause (if table1 and table2 has a relationship), the CROSS JOIN will produce the same result as the INNER JOIN clause

#### SELF JOIN
A self join is a regular join, but the table is joined with itself.
When row itself are inter-related
```SQL
SELECT A.CustomerName AS CustomerName1, B.CustomerName AS CustomerName2, A.City
FROM Customers A, Customers B
WHERE A.CustomerID <> B.CustomerID
AND A.City = B.City
ORDER BY A.City;
```

<br>

> ## Using multiple table
```SQL
SELECT o.OrderID, o.OrderDate, c.CustomerName
FROM Customers AS c, Orders AS o			-- in Oracle 'AS' is not used to refer tables in 'FROM'
WHERE c.CustomerName='Around the Horn' AND c.CustomerID=o.CustomerID;

SELECT Orders.OrderID, Orders.OrderDate, Customers.CustomerName
FROM Customers, Orders
WHERE Customers.CustomerName='Around the Horn' AND Customers.CustomerID=Orders.CustomerID;
```

<br>

# FUNCTION()
Function are of two type:
- scalar or single row (sqrt, round, lower, upper)
- aggregate or multi row (sum, count, max, min)

SELECT FUNC(column_ame)
FROM table_name

COUNT, AVG, SUM:
	Note: NULL values are not counted.

> ## To combine multiple columns
SQL> `SELECT CustomerName, Address + ', ' + PostalCode + ' ' + City + ', ' + Country AS Address FROM Customers;`

MY SQL> `SELECT CustomerName, CONCAT(Address,', ',PostalCode,', ',City,', ',Country) AS Address FROM Customers;`

Oracle> `SELECT CustomerName, (Address || ', ' || PostalCode || ' ' || City || ', ' || Country) AS Address FROM Customers;`

### Group Concat
GROUP_CONCAT() use of function with distinct, orderby, separator
```SQL
SELECT dept_id,
GROUP_CONCAT ( DISTINCT emp_id ORDER BY emp_id  SEPARATOR', ') as "employees ids"
FROM employee GROUP BY dept_id;
```

<br>

# Database
### CREATE
Make sure you have admin privilege before creating any database.

Once a database is created, you can check it in the list of databases with the following SQL command: `SHOW DATABASES`;
```SQL
CREATE DATABASE databasename;
```
### DROP
```SQL
DROP DATABASE databasename;
```
