## Question :
Assume you are given the table below on Uber transactions made by users. Write a query to obtain the third transaction of every user. 
Output the user id, spend and transaction date.

<img width="194" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/20d61c92-d572-450f-a6bf-83b454ba0eb0">

## Solution :
```sql
SELECT  user_id
       ,spend
       ,transaction_date
FROM (
       SELECT user_id
             ,spend
             ,transaction_date
             ,row_number() OVER(PARTITION BY user_id ORDER BY transaction_date ASC) as rn
       FROM  transactions
       ORDER BY user_id ASC 
     ) x 
WHERE x.rn = 3
```
<img width="330" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/38d2773b-f41e-4331-a56b-9ff7b4da22fb">
