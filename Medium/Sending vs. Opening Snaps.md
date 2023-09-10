## Question :
Assume you're given tables with information on Snapchat users, including their ages and time spent sending and opening snaps.
Write a query to obtain a breakdown of the time spent sending vs. opening snaps as a percentage of total time spent on these 

activities grouped by age group. Round the percentage to 2 decimal places in the output.

**Notes:**

**- Calculate the following percentages:**
      - Time spent sending / (Time spent sending + Time spent opening)
      - Time spent opening / (Time spent sending + Time spent opening)
  - To avoid integer division in percentages, multiply by 100.0 and not 100.

<img width="326" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/838e348e-12b7-40c8-b788-336c27e29f99">
<img width="218" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/08956d62-998c-45b3-afdb-3d281bced651">

**Explanation :**
Using the age bucket 26-30 as example, the time spent sending snaps was 5.67 and the time spent opening snaps was 3.

To calculate the percentage of time spent sending snaps, we divide the time spent sending snaps by the total time spent on 
sending and opening snaps, which is 5.67 + 3 = 8.67.

So, the percentage of time spent sending snaps is 5.67 / (5.67 + 3) = 65.4%, and the percentage of time spent opening snaps
is 3 / (5.67 + 3) = 34.6%.

## Solution
```sql
WITH time_spend AS (
          SELECT age.age_bucket 
                ,sum(case when act.activity_type = 'send' then act.time_spent else 0 END ) AS sending
                ,sum(case when act.activity_type = 'open' then act.time_spent else 0 END ) AS opening
                ,sum(act.time_spent) AS total_act
          FROM   activities act
          JOIN   age_breakdown age
          ON     act.user_id = age.user_id
          WHERE  act.activity_type in ('send','open')
          GROUP BY age.age_bucket
    )

SELECT age_bucket
      ,ROUND(100.0 * sending / (sending + opening),2) :: FLOAT AS sending_perc
      ,ROUND(100.0 * opening / (sending + opening),2) :: FLOAT AS opening_perc
FROM  time_spend;
```
<img width="376" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/c8e8d021-5870-4404-9c16-552d3dd5f91c">

