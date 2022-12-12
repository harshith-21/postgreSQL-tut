# CREATING DATABASES AND TABLES

    Section Overview:
        Data Types
        Primary and Foreign Keys
        Constraints
        CREATE
        INSERT
        UPDATE
        DELETE, ALTER, DROP

1. Boolean : True or False
2. Character: char, varchar, and text 
3. Numeric: integer and floating-point number 
4. Temporal: date, time, timestamp, and interval

few others not common
5. UUID: Universally Unique Identifiers 
6. Array: Stores an array of strings, numbers, etc. 
7. JSON: 
8. Hstore key-value pair
9. Special types such as network address an Trans geometric data.

    When creating databases and tables, you should carefully consider which data types should be used for the data to be stored. 

    Review the documentation to see limitations of data types:
    
    postgresql.org/docs/current/datatype.html

![datatypes](Screenshot%202022-09-11%20at%2010.08.53%20AM.png)

    Based on the limitations, you may think it makes sense to store it as a BIGINT data type, but we should really be thinking what is best for the situation.
    Why bother with numerics at all?
    We don't perform arithmetic with numbers, so it probably makes more sense as a VARCHAR data type instead.


    In fact, searching for best practice online, you will discover
    its usually recommended to store as a text based data type due to a variety of issues
    1. No arithmetic performed
    2. Leading zeros could cause issues, 7 and 07 treated same numerically, but are not the same phone number used in country codes

    When creating a database and table, take your time to plan for long term storage 
    Remember you can always remove historical information you've decided you aren't using, but you can't go back in time to add in information!

## **PRIMARY AND FOREIGN KEYS**
---
A primary key is a column or a group of 
columns used to identify a row uniquely in a 
table.
For example, in our dvdrental database we 
saw customers had a unique, non-null 
customer_id column as their primary key.

Primary keys are also important since they 
allow us to easily discern what columns 
should be used for joining tables together.

PRIMARY KEY : UNIQUE FOR EACH ROW AND INTEGER BASED
DENOTED BY PK IN RESULT SECTION TABLE AFTER RUNNING QUERIES

A foreign key is a field or group of fields in a 
table that uniquely identifies a row in 
another table.
A foreign key is defined in a table that 
references to the primary key of the other 
table.

The table that contains the foreign key is 
called referencing table or child table. 
The table to which the foreign key 
references is called referenced table or 
parent table.
A table can have multiple foreign keys 
depending on its relationships with other 
tables.


Recall in the dvdrental database payment 
table, each payment row had its unique 
payment_id (a primary key) and identified 
the customer that made the payment 
through the customer_id (a foreign key
Justomer table's
since it referen
primary key)
SQL
When creating tables and defining column 
we can use constraints to define columns a 
being a primary key, or attaching a foreign 
key relationship to another table. 
Let's quickly explore table properties in 
pgAdmin to see how to get information on 
primary and foreign keys!




When creating tables and defining columns, 
we can use constraints to define columns as 
being a primary key, or attaching a foreign 
key relationship to another table.
Let's quickly explore table properties in 
pgAdmin to see how to get information 
primary and foreign keys!

> TO FIND PRIMARY KEYS AND FOREIGN KEYS OPEN CONSTRAINTS UNSER COLUMNS IN THE DB


---
---
## **CONSTRAINTS**

Constraints are the rules enforced on data 
columns on table.
These are used to prevent invalid data from 
being entered into the database.
This ensures the accuracy and reliability of 
the data in the database.

The most common constraints used: 
O NOT NULL Constraint
■ Ensures that a column cannot have 
NULL value.
o UNIQUE Constraint
■ Ensures that all values in a column are 
different.


The most common constraints used: 
o PRIMARY Key
■ Uniquely identifies each row/record in 
a database table.
o FOREIGN Key
■ Constrains data based on columns in 
other tables.


The most common constraints used: 
o CHECK Constraint
O
■ Ensures that all values in a column 
satisfy certain conditions.

Constraint
o EXCLUSION
■ Ensures that if any two rows are 
compared on the specified column or 
expression using the specified
operator, not all of these comparisons 
will return TRUE.



Table Constraints
o CHECK (condition)
■ to check a condition when inserting or 
updating data.
O REFERENCES
■ to constr 
column t
alue stored in the 
exist in a column in
The most common constraints used 
CHECK Constraint
Ensures that all values in a column 
satisfy certain conditions.
another table


Table Constraints
o UNIQUE (column_list)
■ forces the values stored in the columns 
listed inside the parentheses to be 
unique.
o PRIMARY KEY(column_list)
■ Allows you to define the primary key 
that consists of multiple columns.
Fullsc

---
## **CREATE DB**
    TO CREATE A DB IN POSTGRES DOUCLECLICK IN DATABASES IN PGADMIN APP AND HOT CREATE, GIVE NAME, AND RESTORE IF U HAVE ATAR FILE FOR SKIP RESTORE PART FOR CREATING AN EMPTY DB



---
---
## **CREATE**
---

    GENERAL SYNTAX:
```sql
CREATE TABLE table_name (
    column_name TYPE column_constraint, --column names
    column_name TYPE column_constraint,
    table_constraint table_constraint   --TABLE CONSTRAINTS
) INHERITS existing_table_name ;
```

    COMMON SYNTAX:
```sql
    CREATE TABLE table_name (
        column_name TYPE column_constraint,
        column_name TYPE column_constraint,
    );
```

### **SERIAL**
---
o In PostgreSQL, a sequence is a special 
kind of database object that generates a 
sequence of integers.

o A sequence is often used as the primary 
key column in a table.

o It will create a sequence object and set 
the next value generated by the 
sequence as the default value for the 
column.

o This is perfect for a primary ke 
it logs unique integer entries f 
automatically upon insertion.

If a row is later removed, the column with 
the SERIAL data type will not adjust, 
marking the fact that a row was remove 
from the sequence, for example 

■ 1,2,3,5,6,7

• You know row 4 was removed at 
some point

> eg: 
```sql
CREATE TABLE players(
player_id SERIAL PRIMARY KEY,
age SMALLINT NOT NULL
);
```


```sql
CREATE TABLE account (
user_id SERIAL PRIMARY KEY,
username VARCHAR (50) UNIQUE NOT NULL, 
password VARCHAR (50) NOT NULL,
email VARCHAR (250) UNIQUE NOT NULL,
created_on TIMESTAMP NOT NULL, 
last_login TIMESTAMP
)
```


```sql
CREATE TABLE job (
job_id SERIAL PRIMARY KEY,
job_name VARCHAR (200) UNIQUE NOT NULL
)
```

```sql
CREATE TABLE account_job( 
user_id INTEGER REFERENCES account(user_id),
job_id INTEGER REFERENCES job (job_id), 
hire_date TIMESTAMP
)
```

    serial is used when its primary
    when refering primary in other table use "integer" bec it is foreign key reference

---
---
## **INSERT**
---

INSERT allows you to add in rows to a table. 
General Syntax
o INSERT INTO table (column1, column2, ...) 
VALUES
(value1, value2, ...), 
(valuel, value2, ...),...;


INSERT allows you to add in rows to a table. 
Syntax for Inserting Values from another 
table:
table(column1,column2,...)
o 
```sql
INSERT INTO table(column1, column2)
SELECT column1,column2,...
FROM another_table 
WHERE condition;
```

Keep in mind, the inserted row values must 
match up for the table, including
constraints.
SERIAL columns do not need to be provided 
a value.
Let's use INSERT in pgAdmin!

```sql
INSERT INTO account (username, password, email, created_on) 
VALUES
('Jose', 'password','jose@mail.com',CURRENT_TIMESTAMP)

-- TO CHECK
SELECT * FROM account
```

> U DONT NEED TO ENTRY FOR ID THINGS BECAUSE THEY ARE SERIAL; AND GET ALLOTED ON NEXT FREE INTEGER

```sql
INSERT INTO job (job_name)
VALUES
('Astronaut')

--TO CHECK 
SELECT * FROM job
```


```sql
INSERT INTO job (job_name)
VALUES
('President')
```

![result](Screenshot%202022-09-11%20at%2011.20.38%20AM.png)

> observe job id is increased for us as it is a SERIAL

```sql
INSERT INTO account_job (user_id, job_id, hire_date)
VALUES
(1,1,CURRENT_TIMESTAMP)
```

![acc_job](Screenshot%202022-09-11%20at%2011.24.39%20AM.png)


this happens when u add something from pther tables which dont exist there

![](Screenshot%202022-09-11%20at%2011.42.17%20AM.png)



## **UPDATE**
---

The UPDATE keyword allows for the 
changing of values of the columns in a 
table.

    General Syntax 
```sql    
    UPDATE table 
    SET column1 = valuel, 
        column2 = value2,...
    WHERE 
        condition;
```

eg:
```sql

-- updating last_login timestamps which are NULL with current timestamp
    UPDATE account
    SET last_login = CURRENT_TIMESTAMP 
    WHERE last_login IS NULL;

---


-- set based on other column 
    UPDATE account
    SET last_login = created_on

---

-- Using another table's values (UPDATE join)
UPDATE TableA
SET original_col = TableB.new_col
FROM tableB
WHERE tableA.id = TableB.id

---

-- Return affected rows 
UPDATE account
SET last_login = created_on 
RETURNING account_id,last_login

```

![](Screenshot%202022-09-11%20at%2011.59.20%20AM.png)


![](Screenshot%202022-09-11%20at%2012.01.17%20PM.png)


> changing sata in column from data of another table

![](Screenshot%202022-09-11%20at%2012.03.22%20PM.png)
 

```sql
UPDATE account_job
SET hire_date= account.created_on
FROM account
WHERE account_job.user_id = account.user_id
```

![](Screenshot%202022-09-11%20at%2012.05.43%20PM.png)




```sql
UPDATE account
SET last_login = CURRENT_TIMESTAMP 
RETURNING email,created_on,last_login
```

![](Screenshot%202022-09-11%20at%2012.07.57%20PM.png)

---
---
## **DELETE**
---

We can use the DELETE clause to remove rows from a table. 
For example:
```sql
DELETE FROM table 
WHERE row_id = 1
```

We can delete rows based on their presence in other tables 
For example:
```sql
DELETE FROM tableA 
USING tableB
WHERE tableA.id=TableB.id
```

We can delete all rows from a table
For example:
```sql
DELETE FROM table
```

Similar to UPDATE command, you can also add in a RETURNING call to return rows that were removed.

Let's explore DELETE with pgAdmin!

add another job:
```sql
INSERT INTO job (job_name)
VALUES
('Cowboy')
```

![](Screenshot%202022-09-11%20at%2012.20.38%20PM.png)

> deleting a job
```sql
DELETE FROM job
WHERE job_name = 'Cowboy' 
RETURNING job_id, job_name
```

![](Screenshot%202022-09-11%20at%2012.22.12%20PM.png)

![](Screenshot%202022-09-11%20at%2012.23.19%20PM.png)

## **ALTER**

The ALTER clause allows for changes to an 
existing table structure, such as:
o Adding, dropping,or renaming columns 
o Changing a column's data type
o Set DEFAULT values for a column 
o Add CHECK constraints 
o Rename table


Adding Columns
```sql
ALTER TABLE table_name
ADD COLUMN new_col TYPE
```

Removing Columns
```sql
ALTER TABLE table_name 
DROP COLUMN col_name
```


Alter constraints
```sql
ALTER TABLE table_name
ALTER COLUMN col_name
SET DEFAULT value
```


Alter constraints
```sql
ALTER TABLE table_name
ALTER COLUMN col_name 
DROP DEFAULT
```


Alter constraints
```sql
ALTER TABLE table_name
ALTER COLUMN col_name 
SET NOT NULL
```


Alter constraints
```sql
ALTER TABLE table_name
ALTER COLUMN col_name
ADD CONSTRAINT constraint_name
```


> for knowing how this works jus creating a new table to experiment

```sql
CREATE TABLE information ( 
info_id SERIAL PRIMARY KEY, 
title VARCHAR (500) NOT NULL,
person VARCHAR (50) NOT NULL UNIQUE
)

-- to check everything
SELECT * FROM information


-- TO RENAME TABLE:
ALTER TABLE information
RENAME TO new_info

-- TO RENAME ROW
ALTER TABLE new_info
RENAME COLUMN person TO people
```
![](Screenshot%202022-09-11%20at%2012.37.05%20PM.png)


tryna add just title:
![](Screenshot%202022-09-11%20at%2012.39.04%20PM.png)

bec we defined people to be NOT NULL and havent given a value here its refusing to accept

we have 2 choices
1. giving people a value
2. removing the constraint NOT NULL for people

we are going with 2. drop constraint
```sql
ALTER TABLE new_info 
ALTER COLUMN people DROP NOT NULL
```

in order to set constraint (jus for info)
```sql
ALTER TABLE new_info 
ALTER COLUMN people SET NOT NULL
```

now that we removed the constraint:

```sql
INSERT INTO new_info(title)
VALUES
('some new title')
```

after that checking the table 

![](Screenshot%202022-09-11%20at%2012.44.29%20PM.png)

>> check documentation for more fun XD reg ALTER TABLE

## **DROP**

DROP allows for the complete removal of a 
column in a table.
In PostgreSQL this will also automatically 
remove all of its indexes and constraints 
involving the column.
However, it will not remove columns used 
in views, triggers, or stored procedures 
without the additional CASCADE clause.

Remove all dependencies 
```sql
ALTER TABLE table_name
DROP COLUMN col_name CASCADE
```

Drop multiple columns 
```sql
ALTER TABLE table_name 
DROP COLUMN col_one, 
DROP COLUMN col_two
```

![](Screenshot%202022-09-11%20at%2012.49.27%20PM.png)


>>> better way
```sql
    ALTER TABLE new_info
    DROP COLUMN IF EXISTS people
```
    instead of error gives a message

## **CHECK**

> General Syntax example
```sql
CREATE TABLE example(
ex_id SERIAL PRIMARY KEY, 
age SMALLINT CHECK (age > 21), 
parent_age SMALLINT CHECK ( parent_age > age)
);
```


creating a employee table with a few validity "checks"
```sql
CREATE TABLE employees ( 
emp_id SERIAL PRIMARY KEY, 
first_name VARCHAR (50) NOT NULL, 
last_name VARCHAR (50) NOT NULL,
birthdate DATE CHECK (birthdate> '1900-01-01'), 
hire_date DATE CHECK (hire_date> birthdate), 
salary INTEGER CHECK (salary > 0)
)
```

adding a row ( which should fail a check)
```sql
INSERT INTO employees (
first name,
last name,
birthdate,
hire_date, 
salary
)
VALUES
('Jose',
'Portilla',
'1899-11-03',
'2010-01-01',
100
)
```

this fails as it should be:
![](Screenshot%202022-09-11%20at%201.01.14%20PM.png)

changing it to be valid

![](Screenshot%202022-09-11%20at%201.02.24%20PM.png)

another example
![](Screenshot%202022-09-11%20at%201.03.31%20PM.png)

making it valid again
![](Screenshot%202022-09-11%20at%201.04.41%20PM.png)


```sql
SELECT * FROM employees
```

![](Screenshot%202022-09-11%20at%201.06.01%20PM.png)