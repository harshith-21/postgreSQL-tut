

### *ASSESSMENT TEST 1*

COMPLETE THE FOLLOWING TASKS!

1. Return the customer IDs of customers who have spent at least $110 with the staff member who has an ID of 2.

    > The answer should be customers 187 and 148.
    ```sql
    SELECT customer_id ,SUM(amount) FROM payment
    WHERE staff_id = 2
    GROUP BY customer_id
    HAVING SUM(amount) > 110;

    ----- this is also totally fine
    SELECT customer_id FROM payment
    WHERE staff_id = 2
    GROUP BY customer_id
    HAVING SUM(amount) > 110;
    ```
2. How many films begin with the letter J?

    > The answer should be 20.
    ```sql
    SELECT COUNT(title) FROM film
    WHERE title ILIKE 'J%';
    ```

3. What customer has the highest customer ID number whose name starts with an 'E' and has an address ID lower than 500?

    > The answer is Eddie Tomlin
    ```sql
    SELECT first_name,last_name,address_id FROM customer
    WHERE first_name ILIKE 'E%'
    AND address_id < 500
    ORDER BY customer_id DESC
    LIMIT 1
    --- ffs got me for a while
    --- feeeellinnnn ittt 🙂
    ```