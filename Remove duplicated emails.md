**Problem**: Write a solution to delete all duplicate emails, keeping only one unique email with the smallest id.

```sql
DELETE FROM Person
WHERE
    id NOT IN (
        SELECT id
        FROM (
                SELECT
                    id,
                    ROW_NUMBER() OVER (
                        PARTITION BY email
                        ORDER BY id
                    ) AS rank
                FROM Person
            ) AS temp_table
        WHERE rank = 1
    );
