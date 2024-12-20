## Question :
Assume you have an events table on Facebook app analytics. Write a query to calculate the click-through rate (CTR) for the app 
in 2022 and round the results to 2 decimal places.

**Definition and note:**
- Percentage of click-through rate (CTR) = 100.0 * Number of clicks / Number of impressions
- To avoid integer division, multiply the CTR by 100.0, not 100.

<img width="200" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/f98c1f5f-1b59-46d6-887a-cfd70a089180">

## Solution :
```sql
SELECT  app_id
       ,round((100.0 * click/impression),2) as CTR
FROM
    (
       SELECT
              app_id
             ,sum(case when event_type = 'click' then 1 end) as click
             ,sum(case when event_type = 'impression' then 1 end) as impression
             ,extract(year from timestamp) as year
      FROM   events
      GROUP BY 1,4
    )x
WHERE x.year = 2022;
```
<img width="302" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/743b1a90-b1e2-478a-897d-ffaa576188ba">
