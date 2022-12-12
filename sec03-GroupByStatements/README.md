# **GROUP BY**


## AGGREGATION FUNCTIONS
---

* AVG ()
* COUNT ()
* MAX ()
* MIN ()
* SUM ()

- WORKS WITH SELECT CLAUSE OR HAVING CLAUSE


to find min and max replacement costs
```sql
-- SELECT MAX(replacement_cost) FROM film
-- SELECT MIN(replacement_cost) FROM film
SELECT MIN(replacement_cost), MAX(replacement_cost) FROM film
```


to find rounded value of avg of replacement costs
```sql
-- SELECT AVG(replacement_cost) FROM film
SELECT ROUND(AVG(replacement_cost),2) FROM film
```


to find sum of relpacement costs
```sql
SELECT SUM(replacement_cost) FROM film;
```


## **GROUP BY**
---
general syntax

```sql
SELECT catagory_col, AGG(data_col)
FROM table-1
-- WHERE bla bla bla
-- WHERE catagory_col != 'A'
GROUP BY catagory_col
```

to find which customer spent the most amount
```sql
SELECT customer_id, SUM(amount) FROM payment
GROUP BY customer_id
ORDER BY SUM(amount) DESC
```

more details
```sql
SELECT customer_id, SUM(amount),COUNT(amount),AVG(amount) FROM payment
GROUP BY customer_id
ORDER BY SUM(amount) DESC
```

most avg amount spent per transaction
```sql
SELECT customer_id, SUM(amount),COUNT(amount),AVG(amount) FROM payment
GROUP BY customer_id
ORDER BY AVG(amount) DESC
```

amount spend by each customer with each staff
```sql
SELECT staff_id,customer_id, SUM(amount) FROM payment
GROUP BY staff_id,customer_id
ORDER BY customer_id
```

to see dates and amount spent on that in payments by customers decending order
```sql
SELECT DATE(payment_date), SUM(amount) FROM payment
GROUP BY DATE(payment_date)
ORDER BY SUM(amount) DESC
```


to see which staff member has one more payment transactions ( not amount )
```sql
SELECT staff_id, COUNT(amount) FROM payment
GROUP BY staff_id
ORDER BY COUNT(amount) DESC
-- we used COUNT(amount) this jus counts number of amounts NOT sum of amounts
```

find avg replacement cost per MPAA rating
```sql
SELECT rating, AVG(replacement cost) FROM film
GROUP BY rating 
```


find avg replacement cost per MPAA rating in PG, R, G.
```sql
SELECT rating, AVG(replacement_cost) FROM film
WHERE rating in ('PG','R','G')
GROUP BY rating
```

same but rounded to 3 decimals
```sql
SELECT rating, ROUND(AVG(replacement_cost),3) FROM film
GROUP BY rating
```


for top 5 customers whose amount spent in total is highest
```sql
SELECT customer_id, SUM(amount) FROM payment
GROUP BY customer_id
ORDER BY SUM(amount) DESC
LIMIT 5;
```

## **HAVING**
    - basically we can filter bassed in aggregate results which  wont be caliculated until group by and where should be before group by so we cant using WHERE
    - HAVING is basically WHERE but comes after GROUP BY

```sql
SELECT customer_id, SUM(amount) FROM payment
WHERE customer_id NOT IN (184,87,477)
GROUP BY customer_id
```
-- if u want to filter using where like adding
```sql
WHERE SUM(amount) > 200
```
this wont work beacuse aggregate values are caliculated after group by 
so if u place WHERE after GROUP BY it still wont work because WHERE should come before GROUP BY
instead use HAVING with same filter requirement after GROUP BY


eg:

customer ids whose total sum of amounts is greater than 100
```sql
SELECT customer_id,SUM(amount) FROM payment
GROUP BY customer_id
HAVING SUM(amount) > 100;
```

same but in decreasing order
```sql
SELECT customer_id,SUM(amount) FROM payment
GROUP BY customer_id
HAVING SUM(amount) > 100 
ORDER BY SUM(amount) DESC;
```

store_id whose total customers are greater than 300
```sql
SELECT store_id, COUNT (customer_id) FROM customer
GROUP BY store_id
HAVING COUNT (customer_id) > 300;
```

customer ids whose transactions are more than 40
```sql
SELECT customer_id,COUNT(*) FROM payment
GROUP BY customer_id
HAVING COUNT(*) >= 40
ORDER BY COUNT (*);
-- any coulmn would also work in place of * bec we are jus counting rows so
```

customer ids whose total amount spent with staff id=2 and amount greater than $100 and ( ordered desc which is not necessary ) 
```sql
SELECT customer_id, SUM(amount) FROM payment
WHERE staff_id = 2
GROUP BY customer_id
HAVING sum(amount) > 100
ORDER BY SUM(amount) DESC;
```