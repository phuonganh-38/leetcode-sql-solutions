## **Table**
`Queries`
| query_name | result           | position | rating |
|------------|------------------|----------|--------|
| Dog        | Golden Retriever | 1        | 5      |
| Dog        | German Shepherd  | 2        | 5      |
| Dog        | Mule             | 200      | 1      |
| Cat        | Shirazi          | 5        | 2      |
| Cat        | Siamese          | 3        | 3      |
| Cat        | Sphynx           | 7        | 4      |
<br>

## **Solution**
```sql
SELECT query_name,
    ROUND(AVG(rating/position), 2) AS quality,
    ROUND(SUM(IF(rating < 3,1,0)) / COUNT(*) * 100, 2) AS poor_query_percentage
FROM 
    Queries
GROUP BY
    query_name;
