> payment transactions greater than $5
```sql
SELECT COUNT(*) FROM payment
WHERE amount > 5;
```

> num of actors whose first name starts with 'p' 
```sql
SELECT COUNT(*) FROM actor
WHERE first_name ILIKE 'p%';
```

> num of unique districts our customers are from
```sql
SELECT COUNT(DISTINCT address_id) FROM customer;
```

> list those unique districts
```sql
SELECT DISTINCT address_id FROM customer
ORDER BY address_id
```

> number of films 
```sql
SELECT COUNT(*) FROM film
WHERE rating = 'R' AND replacement_cost between 5 and 15
```

>  films whose title has truman in the title somewhere
```sql
SELECT COUNT(*) FROM film
WHERE title ILIKE '%Truman%' 
```