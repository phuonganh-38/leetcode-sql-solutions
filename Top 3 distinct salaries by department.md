## **Problem**
A company's executives are interested in seeing who earns the most money in each of the company's departments. A high earner is an employee who has a salary in the top 3 unique salaries for that department.
<br>

Write a solution to find the employees who are high earners in each of the departments.
<br>

*Requirement: No holes in ranking even though there are 2 equal ranks.*
<br>

## Tables
`Employee`
| id | name  | salary | departmentId |
|----|-------|--------|--------------|
| 1  | Joe   | 85000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
| 7  | Will  | 70000  | 1            |
<br>

`Department`
| id | name  |
|----|-------|
| 1  | IT    |
| 2  | Sales |
<br>

 ## Solution
 ```sql
 WITH ranks AS (
    SELECT d.name AS Department,
           e.name AS Employee,
           e.salary AS Salary,
           DENSE_RANK() OVER (PARTITION BY d.id
                              ORDER BY salary DESC) AS rank_salary
    FROM Employee e
    JOIN Department d ON d.id = e.departmentId
)
SELECT Department,
       Employee,
       Salary
FROM ranks
WHERE rank_salary <= 3;
