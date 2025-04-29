## **Problem**
Write a solution to find employees who have the highest salary in each of the departments.
<br>

## **Tables**
  Table `Employee`
| Column Name  | Type    |
|--------------|---------|
| id           | int     |
| name         | varchar |
| salary       | int     |
| departmentId (FK) | int     |
<br>

Table `Department`
| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |
<br>

 ## **Solution**
```sql
SELECT Department.name AS Department,
       e1.name AS Employee,
       e1.Salary AS Salary
FROM Department
JOIN Employee e1 ON Department.Id = e1.departmentId
WHERE
    e1.Salary = (
                  SELECT MAX(Salary)
                  FROM Employee e2
                  WHERE e2.departmentId = e1.departmentId
                )
