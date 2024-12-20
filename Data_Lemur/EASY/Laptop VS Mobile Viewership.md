## Question :
Assume you're given the table on user viewership categorised by device type where the three types are laptop, tablet, and phone.

Write a query that calculates the total viewership for laptops and mobile devices where mobile is defined as the sum of tablet 
and phone viewership. Output the total viewership for laptops as laptop_reviews and the total viewership for mobile devices as 
mobile_views.

<img width="215" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/c465df9c-979e-4d85-809f-7627ca43b344">

## Solution:
```sql
SELECT sum(case when device_type = 'laptop' then 1 else 0 end) as laptop_views,
       sum(case when device_type in ('phone','tablet') then 1 else 0 end) as laptop_views	
FROM   viewership
```
<img width="301" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/5f6159dd-794b-41d5-85fb-570383398ba5">
