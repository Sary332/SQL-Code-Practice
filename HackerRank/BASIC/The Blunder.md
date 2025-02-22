## Question :
Samantha was tasked with calculating the average monthly salaries for all employees in the EMPLOYEES table, 
but did not realize her keyboard's 0 key was broken until after completing the calculation. She wants your 
help finding the difference between her miscalculation (using salaries with any zeros removed), and the 
actual average salary.

Write a query calculating the amount of error (i.e.: actual - miscalculated average monthly salaries), 
and round it up to the next integer.

The EMPLOYEES table is described as follows:

<img width="335" alt="image" src="https://github.com/user-attachments/assets/faa5715c-1a21-4c15-bfff-1fed722c7b9b" />

## Solution :
```sql
SELECT
    CAST(
          CEILING(AVG(CAST(salary AS float)) --OG Salary
                         -
           AVG(CAST(REPLACE(salary, 0, '') AS float)) --Miscalculated Salary
          ) AS int
        ) AS RIGHT_CALCULATION
FROM EMPLOYEES
```
<img width="282" alt="image" src="https://github.com/user-attachments/assets/21488d6a-df63-4459-8ce0-b7f514dbf232" />
