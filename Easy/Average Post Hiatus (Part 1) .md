## Question :
Given a table of Facebook posts, for each user who posted at least twice in 2021, write a query to find the number of days between 
each user’s first post of the year and last post of the year in the year 2021. Output the user and number of the days between each 
user's first and last post.
<img width="134" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/94d120e5-cdd5-45b2-bcd2-795c58fb8a17">
<img width="432" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/39556280-0341-4693-941f-3aa680c64641">

## Solution :
```sql
SELECT
     user_id
    ,max(post_date :: date) - min(post_date :: date) as days_between
FROM posts
WHERE  date_part('year', post_date :: date) = 2021
GROUP BY user_id
HAVING count(user_id) >1
ODER BY 2 desc;
```
<img width="265" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/55a2f96c-2ee4-4ef4-b788-8c0a147ff0dc">
