## **Problem**
Find the calculation rate of requests with unbanned users (both client and driver) each day between **2013-10-01** and **2013-10-03** with at least one trip.
<br>
*Round `Cancellation Rate` to two decimal points.*<br>
<br>

## **Tables**
`Trips`
| id | client_id | driver_id | city_id | status              | request_at |
|----|-----------|-----------|---------|---------------------|------------|
| 1  | 1         | 10        | 1       | completed           | 2013-10-01 |
| 2  | 2         | 11        | 1       | cancelled_by_driver | 2013-10-01 |
| 3  | 3         | 12        | 6       | completed           | 2013-10-01 |
| 4  | 4         | 13        | 6       | cancelled_by_client | 2013-10-01 |
| 5  | 1         | 10        | 1       | completed           | 2013-10-02 |
| 6  | 2         | 11        | 6       | completed           | 2013-10-02 |
| 7  | 3         | 12        | 6       | completed           | 2013-10-02 |
| 8  | 2         | 12        | 12      | completed           | 2013-10-03 |
| 9  | 3         | 10        | 12      | completed           | 2013-10-03 |
| 10 | 4         | 13        | 12      | cancelled_by_driver | 2013-10-03 |
<br>

`Users`
| users_id | banned | role   |
|----------|--------|--------|
| 1        | No     | client |
| 2        | Yes    | client |
| 3        | No     | client |
| 4        | No     | client |
| 10       | No     | driver |
| 11       | No     | driver |
| 12       | No     | driver |
| 13       | No     | driver |
<br>

## **Solution**
```sql
SELECT request_at AS Day,
       ROUND(SUM(status != "complete") / COUNT(*), 2) AS 'Cancellation Rate'
FROM Trips
WHERE request_at BETWEEN "2013-10-01" AND "2013-10-03"
AND client_id IN (
                  SELECT users_id
                  FROM Users
                  WHERE banned = "No"
)
AND driver_id IN (
                  SELECT users_id
                  FROM Users
                  WHERE banned = "No"
)
