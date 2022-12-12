# password: password harshith@21 for user
## port:  5432

----

<!-- > apple

    1.asmfdk
    2.san
    
<!-- ```c++
for (i=0;i<n;i++){
    int k=0;
    k=k++;
    cout << k;
}         
``` -->

<!-- > apple

    1.asmfdk
    2.san --> 
----
----
## **SELECT**
    
    SELECT <column-name(s)> FROM <table-name>;
    
    eg:
    let column names be c1 c2 and so on
    --> SELECT c1,c2 FROM table_1;               -- for c1,c2 columns
    --> SELECT * FROM table_1;               -- for all columns
    --> SELECT actor_id,last_name FROM actor;
    --> SELECT first_name,last_name,email FROM customer

### **SELECT DISTINCT**
    
    SELECT DISTINCT <column-name> FROM <table-name>;

    eg:
    --> SELECT DISTINCT release_year FROM film;
    --> SELECT DISTINCT rental_rates FROM film;
    --> SELECT DISTINCT rating FROM film;

## **COUNT**
```sql
    SELECT COUNT (column-name) FROM <table-name>
```
eg:
```sql
    SELECT COUNT ( name ) FROM table ;
    SELECT COUNT ( choice ) FROM table ;
    SELECT COUNT ( * ) FROM table ;            -- gives number of rows present in the table, so does the above 2  
```
All return the same thing , since the original
table had 4 rows .



for count of unique names:
```sql
SELECT COUNT (DISTINCT name) FROM table;
```

for number of unique ratings present:
```sql
SELECT COUNT(DISTINCT rating) FROM film;
```

for finding number of unique amounts in payments
```sql
SELECT COUNT(DISTINCT amount) FROM payment
```

understand this
```sql
SELECT DISTINCT COUNT (amount) FROM payment
```
prints number of rows
bec count ( amount ) gives number of amounts which are total number of rows
and distinct as 1,2,3,.....number of rows are distinct

*below gives the same result*
```sql
SELECT COUNT (amount) FROM payment
```

---

## **WHERE**

    SELECT c1,c2...
    FROM table_1
    WHERE conditions; 

    operators
    =,<,>,<=,>= are same
    <>,!= means not equal

    eg:
    SELECT name,choice FROM table
    WHERE name='David'

    gives name and choice columns for entrys where name is david


This searches for Jennifer in customers table and and prints all columns of that Jennifer row: 
```sql
SELECT * FROM customer
WHERE first_name='Jennifer';
```

**Few more examples**
```sql
SELECT * FROM FILM
WHERE rental_rate > 4 AND replacement_cost >= 19.99;
```

```sql
SELECT * FROM FILM
WHERE rental_rate > 4 AND replacement_cost >= 19.99
AND rating='R';
```

```sql
SELECT * FROM FILM
WHERE rental_rate > 4 AND replacement_cost >= 19.99;
AND rating='R' OR rating='PG-13'
```

```sql
SELECT COUNT(*) FROM FILM
WHERE rental_rate > 4 AND replacement_cost >= 19.99;
AND rating='R' OR rating='PG-13'
```

```sql
SELECT * FROM FILM
WHERE rating != 'R'
```

```sql
SELECT email FROM customer
WHERE first_name='Nancy' AND last_name='Thomas';
```

```sql
SELECT description FROM film
WHERE title = 'Outlaw Hanky'
```

the following give phone number of person who lives at address 259 Ipoh Drive
```sql
SELECT phone FROM address
WHERE address='259 Ipoh Drive' 
```

> postgres sql gives different order sometime for same query (efficiency something) so  to keep a constant order we can add a sort / **'ORDER BY'**


## **ORDER BY**

```sql
    SELECT col1,col2
    FROM table_1
    ORDER BY col1 ASC / DESC;

    -- imp:
    ORDER BY col1,col2,col3
    -- gives the result in order according to col1 and if its same for col1 then sorts acc to col2 and so on

    ORDER BY col1 ASC,col2 DESC, col3 DESC, col4 ASC
    -- u can do this too
```
> ASC by default

    eg:
    SELECT * FROM customer
    ORDER BY first_name ASC

    SELECT * FROM customer
    ORDER BY first_name DESC

## **LIMIT**
    limits the rows printed by the query


gives a decent idea of whats in the table 
kinda like head 1 in jupyter notebooks
```sql
SELECT * FROM payment
LIMIT 1;
```


gives the 5 most recent non zero payments
```sql
SELECT * FROM payment
WHERE amount != 0.00
ORDER BY payment_date DESC
LIMIT 5;
```


first 10 customers' id's
```sql
SELECT customer_id FROM payment
-- WHERE amount !=0
ORDER BY payment_date ASC
LIMIT 10;
```

to get 5 short movies acending order of length
```sql
SELECT title,lemgth FROM film
ORDER BY length ASC
LIMIT 5;
```


to find number of movies which have duration less than or equal to 50mins
```sql
SELECT COUNT(title) FROM film
WHERE length<=50
```

## **BETWEEN**
```sql
    value BETWEEN <low> AND <high>
    -- same as:
    value >= low AND value <= high
    -- low and high both inluded
```

```sql
    value NOT BETWEEN <low> AND <high>
    -- same as:
    value < low OR value > high
    -- low and high both excluded 
```

* with time stamp info if u want data till 14th date enter till 15th date because of 00:00 some ascii syntax something those will get ignored  
* 14th means till 14th 00:00
* 14th 01:00,02:00 etc are not counted
* so in these cases use a next date in upper limit

```sql
SELECT * FROM payment
-- to get rows from 01 to 14 write 01-15
WHERE payment_date BETWEEN '2007-02-01' AND '2007-02-15'
```

## **IN**
---
eg:
```sql
SELECT color FORM table-1
WHERE color IN ('red','yellow','green')
```

gives count of number of payments whose amount in the list
```sql
SELECT COUNT(*) FROM payment
WHERE amount IN (0.99,1.99,1.98);
```


prints rows which have amount mentioned in list
```sql
SELECT * FROM payment
WHERE amount IN (0.99,1.99,1.98);
```


prints rows which not have amount mentioned in list
```sql
SELECT * FROM payment
WHERE amount NOT IN (0.99,1.99,1.98);
```


## **LIKE** and **ILIKE**
> % matches any sequence of characters be it be nothing ( like '' ) / simply means it can be zero characters
> _ matches any single character

    like is case sensitive
    ilike is case insensitive

---
* POSTGRES SUPPORTS FULL REGEX CAPABILITIES

* *LEARN THIS SHIT*
---

> to get first_names which start with 'J' 
```sql
SELECT first_name FROM customer
WHERE first_name LIKE 'J%'
```


> for names which first name start with 'J' and second_name start with 'S' 
```sql
SELECT first_name,last_name FROM customer
WHERE first_name LIKE 'J%' AND last_name LIKE 'S%'
```

#### **can use ILIKE in above senarios jus be careful what you are searching for, ILIKE jus ignores the case mostly ideal for real world senarios** 


```sql
SELECT * FROM customer
WHERE first_name ILIKE 'B%' AND last_name NOT ILIKE 'B%'
ORDER BY last_name
```

