## Question :
Write a query to identify the top 2 Power Users who sent the highest number of messages on Microsoft Teams in August 2022.
Display the IDs of these 2 users along with the total number of messages they sent. Output the results in descending order 
based on the count of the messages.

**Assumption:**
No two users have sent the same number of messages in August 2022.

<img width="364" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/b0fc43d3-e16b-468d-8490-66be2d42a1f9">

## Solution :
```sql
SELECT  x.sender_id 
       ,MAX(x.mesagge_count) as paling_bnyak
FROM (
        SELECT 
              sender_id
             ,COUNT(sender_id) as mesagge_count
        FROM messages
        WHERE date_part('month', sent_date :: date ) = 8 
              AND date_part('year', sent_date :: date ) = 2022
        GROUP BY sender_id
     ) x
GROUP BY sender_id
ORDER BY MAX(x.mesagge_count) DESC
LIMIT 2;
```
<img width="269" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/64ce0058-067e-4db6-9b12-5d85e74ff349">

