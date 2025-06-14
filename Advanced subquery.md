## **Problem**
<br>

Write a solution to find the percentage of **immediate** orders in the **first** orders of all customers, rounded to 2 decimal places.
<br>

Explanation:
*immediate orders: order_date = customer_pref_delivery_date*<br>
<br>

## **Table Delivery**
| delivery_id | customer_id | order_date | customer_pref_delivery_date |
|-------------|-------------|------------|------------------------------|
| 1           | 1           | 2019-08-01 | 2019-08-02                   |
| 2           | 2           | 2019-08-02 | 2019-08-02                   |
| 3           | 1           | 2019-08-11 | 2019-08-12                   |
| 4           | 3           | 2019-08-24 | 2019-08-24                   |
| 5           | 3           | 2019-08-21 | 2019-08-22                   |
| 6           | 2           | 2019-08-11 | 2019-08-13                   |
| 7           | 4           | 2019-08-09 | 2019-08-09                   |
<br>


## **Solution**
```sql
SELECT ROUND(SUM(CASE
                    WHEN order_date = customer_pref_delivery_date THEN 1
                    ELSE 0
                 END) * 100 / COUNT(delivery_id), 2) AS immediate_percentage
FROM Delivery
WHERE (customer_id, order_date) IN (SELECT customer_id,
                                           MIN(order_date)
                                    FROM Delivery
                                    GROUP BY customer_id)
