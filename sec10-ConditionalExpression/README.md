# Section 10: Conditional Expressions and Procedures

    Section Overview :
        CASE
        COALESCE
        NULLIF
        CAST
        Views
        Import and Export Functionality

## **CASE**

    We can use the CASE statement to only execute SQL code when certain conditions are met.

    This is very similar to IF/ELSE statements in other programming languages.    
> 
    There are two main ways to use a CASE statement, either a general CASE or a CASE expression.

    Both methods can lead to the same results.
    
    Let's first show the syntax for a "general" CASE.

> General Syntax 
```sql
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ELSE some other_result
END
```

![](Screenshot%202022-09-11%20at%208.45.52%20PM.png)

![](Screenshot%202022-09-11%20at%208.46.28%20PM.png)


> The CASE expression syntax first evaluates an expression then compares the result with each value in the WHEN clauses sequentially.
---
### **CASE Expression Syntax**
```sql
CASE expression
    WHEN valuel THEN result1
    WHEN value2 THEN result2
    ELSE some other_result
END
```

![](Screenshot%202022-09-11%20at%208.50.33%20PM.png)


> MEMBERSHIP FOR CUSTOMERS BASED ON ID

```sql
SELECT customer_id, 
CASE
    WHEN (customer_id <= 100) THEN 'Premium'
    WHEN (customer_id BETWEEN 100 and 200) THEN 'Plus' 
    ELSE 'Normal'
END AS customer_class
FROM customer
```

WHEN you are searching for a quality in a particular column which is true most of the times

This will be easier

```sql
SELECT customer_id, 
CASE customer_id
    WHEN 2 THEN 'Winner'
    WHEN 5 THEN 'Second Place'
    ELSE 'Normal'
END AS raffle_results
FROM customer
```

TO count number of rows which satisfy a property we can always use 
```sql
select count(*) where
```
but we can also use this

```sql
SELECT rental_rate, 
CASE rental_rate
    WHEN 0.99 THEN 1
    ELSE 0
END 
FROM film
```

basically printing 1 against rows which satify out condition
![](Screenshot%202022-09-11%20at%209.05.23%20PM.png)

```sql
SELECT
SUM (CASE rental_rate
    WHEN 0.99 THEN 1 
    ELSE 0
END) AS number_of_bargains 
FROM film
```

now summing that column will just give us the count ...smort :)

![](Screenshot%202022-09-11%20at%209.08.35%20PM.png)

```sql
SELECT
SUM (CASE rental_rate 
    WHEN 0.99 THEN 1 
    ELSE 0
END) AS bargains,
SUM (CASE rental_rate
    WHEN 2.99 THEN 1
    ELSE 0
END) AS regular,
SUM (CASE rental_rate 
    WHEN 4.99 THEN 1 
    ELSE 0
END) AS premium
FROM film
```

![](Screenshot%202022-09-11%20at%209.12.28%20PM.png)


## **CASE challenge**

![](Screenshot%202022-09-11%20at%209.20.48%20PM.png)


## **COALESCE**
---

The COALESCE function accepts an 
unlimited number of arguments. It returns 
the first argument that is not null. If all 
arguments are null, the COALESCE 
function will return null.

```sql
COALESCE (arg_1,arg_2,...,arg_n)
```

Example:
```sql
SELECT COALESCE (1, 2)
--> 1
SELECT COALESCE(NULL, 2, 3)
--> 2
```

lets see a smol problem to understand

![](Screenshot%202022-09-11%20at%209.26.28%20PM.png)

![](Screenshot%202022-09-11%20at%209.27.04%20PM.png)

> Item b price null bec number-null is null apparently for some reason

> To solve this problem

![](Screenshot%202022-09-11%20at%209.28.54%20PM.png)

    Keep the COALESCE function in mind in case you encounter a table with null values that you want to perform operations 76. CAST !!


## **CAST**

    The CAST operator let's you convert from one data type into another.

    Keep in mind not every instance of a data type can be CAST to another data type, it must be reasonable to convert the data, 

    for example '5' to an integer will work, 'five' to an integer will not.
> 
    Syntax for CAST function
        SELECT CAST('5' AS INTEGER)
    PostgreSQL CAST operator 
        SELECT '5'::INTEGER

> 
    Keep in mind you can then use this in a SELECT query with a column name instead of a single instance.
```sql
SELECT CAST(date AS TIMESTAMP) 
FROM table
```


```sql
SELECT CAST ('5' AS INTEGER)
--> prints 5 as int

SELECT CAST ('5' AS INTEGER) AS new_int
--> second AS is for column name ... dont confuse

-- DONT BE A DUMBASS, DONT DO THIS FFS
SELECT CAST('five' AS INTEGER)


--
-- FOLLOWING ARE THE POSTGRES ONLI SYNTAX
--

-- DOES PRETTY MUCH SAME WORK
SELECT '10'::INTEGER


--- TO FIND LENGTH OF A NUMBER PRESENT IN A COLUMN IN INT ITS A LIL TOUGH BUT IN STRING ITS EASY JUS CHAR_LENGTH FUNCTION

SELECT CHAR_LENGTH (CAST (inventory_id AS VARCHAR)) FROM rental
```


## **NULLIF**

The NULLIF function takes in 2 inputs and returns NULL if both are equal, otherwise it returns the first argument passed. 
```sql
NULLIF(arg1,arg2) 
```
Example
```sql
NULLIF(10,12) 
--> Returns 10
```

![](Screenshot%202022-09-11%20at%2010.03.13%20PM.png)


TO SOLVE THIS CREATE TABLE AND ADD DATA
```sql

-- CREATING TABLE
CREATE TABLE depts(
first_name VARCHAR (50),
department VARCHAR (50)
)

-- ADDING DATA
INSERT INTO depts (
first_name,
department
)
VALUES
('Vinton', 'A'),
('Lauren', 'A'),
('Claire', 'B');


-- to check
SELECT * FROM dept


-- to find ratio
SELECT (
SUM (CASE WHEN department = 'A' THEN 1 ELSE 0 END)/ 
SUM (CASE WHEN department = 'B' THEN 1 ELSE 0 END) 
) AS department_ratio
FROM depts

-- deleting dept B
DELETE FROM depts 
WHERE department = 'B'

-- to check
SELECT * FROM dept -- only dept A will be there


-- USING NULLIF
SELECT (
SUM (CASE WHEN department = 'A' THEN 1 ELSE O END)/
NULLIF (SUM (CASE WHEN department = 'B' THEN 1 ELSE 0 END), 0)
) AS department_ratio
FROM depts

```


## **VIEWS**

    Often there are specific combinations of tables and conditions that you find yourself using quite often for a project.

    Instead of having to perform the same query over and over again as a starting point, you can create a VIEW to quickly see this query with a simple call.

![](Screenshot%202022-09-11%20at%2010.16.19%20PM.png)


    A view is a database object that is of a stored query.
    
    A view can be accessed as a virtual table in PostgreSQL.
    Notice that a view does not store data physically, it simply stores the query.

>

```sql
SELECT first_name, last_name, address FROM customer 
INNER JOIN address
ON customer.address_id = address.address_id
```
> this is some table which is not present already but made ... if we wanna use this again later


```sql
CREATE VIEW customer_info AS
SELECT first_name, last_name, address FROM customer 
INNER JOIN address
ON customer.address_id = address.address_id
```
and after running this we can directly call this "table not table" with the name

![](Screenshot%202022-09-11%20at%2010.32.34%20PM.png)

> editing is easy too

```sql
CREATE OR REPLACE VIEW customer_info AS
SELECT first_name, last_name, address, district FROM customer 
INNER JOIN address
ON customer.address_id = address.address_id
```

![](Screenshot%202022-09-11%20at%2010.35.10%20PM.png)

    note district is also included here


```sql
--to delete
DROP VIEW customer_info

--to rename
ALTER VIEW customer_info RENAME to c_info
```


## **IMPORT AND EXPORT**
---

    Important Note!
    
    Not every outside data file will work, variations in formatting, macros, data types, etc. may prevent the Import command from reading the file, at which point, you must edit your file to be compatible with SQL.

>

    Details of compatible file types and examples are available in the online 
    
    documentation:
    postgresql.org/docs/12/sql-copy.html


    Important Note! 
        You MUST provide the 100% correct file path to your outside file, otherwise the Import command will fail to find the file. 
        The most common mistake if failing toprovide the correct file path, confirm the file's location under its properties.

    VERY Important Note!
    The Import command DOES NOT create a table for you.
    It assumes a table is already created. 
    Currently there is no automated way within pgAdmin to create a table directly from a .csv file.

```sql
CREATE TABLE simple (
a INTEGER,
b INTEGER, 
C INTEGER
)
```

now double click simple db and select export/import select the simple csv file in the parent folder and check headers in options and hit ok

this should work
![](Screenshot%202022-09-11%20at%2010.54.08%20PM.png)


> u can export too pretty much same steps go with INTUITION



