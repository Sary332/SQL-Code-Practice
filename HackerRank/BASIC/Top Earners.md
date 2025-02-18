## Question :
We define an employee's total earnings to be their monthly salary x months worked, and the maximum total 
earnings to be the maximum total earnings for any employee in the Employee table. Write a query to find 
the maximum total earnings for all employees as well as the total number of employees who have maximum total 
earnings. Then print these values as 2 space-separated integers.

The Employee table containing employee data for a company is described as follows:

<img width="171" alt="image" src="https://github.com/user-attachments/assets/a97677a2-16a6-4122-8f5a-ea6f5b273af7" />

## Solution :
```sql
SELECT TOP 1 (SALARY * MONTHS),COUNT(*) AS Total_Employees
FROM EMPLOYEE
GROUP BY (SALARY * MONTHS)
ORDER BY (SALARY * MONTHS) DESC
```
<img width="281" alt="image" src="https://github.com/user-attachments/assets/d612810e-96c2-45a4-b871-3398b6bc6f58" />



