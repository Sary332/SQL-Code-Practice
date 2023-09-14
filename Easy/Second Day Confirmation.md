## Question :
Assume you're given tables with information about TikTok user sign-ups and confirmations through email and text. New users 
on TikTok sign up using their email addresses, and upon sign-up, each user receives a text message confirmation to activate
their account.

Write a query to display the user IDs of those who did not confirm their sign-up on the first day, but confirmed on the
second day.

**Definition:**
- action_date refers to the date when users activated their accounts and confirmed their sign-up through text messages.

<img width="192" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/dc032bc5-f58d-4f59-80f8-07ec8bd79657">
<img width="263" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/521c60e2-05e5-44ea-8791-0be65446bdf0">

## Solution :
```sql
SELECT user_id
FROM   ( SELECT 
                e.user_id as user_id
               ,t.action_date
               ,t.signup_action
               ,row_number() over(partition by e.user_id order by t.action_date asc) as rn
         FROM  emails e
         JOIN  texts t
         ON    e.email_id = t.email_id
         ORDER BY  1 asc
         )x
WHERE  x.rn = 2 and x.signup_action = 'Confirmed' ;
```
<img width="118" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/6d03cd85-c674-424c-a77f-dc9a19c170f5">
