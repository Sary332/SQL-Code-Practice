## Question :
Given a table of tweet data over a specified time period, calculate the 3-day rolling average of tweets for each user. 
Output the user ID, tweet date, and rolling averages rounded to 2 decimal places.

**Notes:**

- A rolling average, also known as a moving average or running mean is a time-series technique that examines trends in data over
  a specified period of time.
- In this case, we want to determine how the tweet count for each user changes over a 3-day period.

<img width="217" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/6678de90-59e3-4b03-bc68-fb14a3a9f87e">

## Solution :
#### Step 1: Calculate the average tweet count for each user
To obtain the average rolling average tweet count for each user, we use the following query which calculates the average 
tweet count for each user ID and date using AVG() window function.
```sql
SELECT user_id,
       tweet_date,
       AVG(tweet_count) OVER (PARTITION BY user_id ORDER BY tweet_date) AS rolling_avg_3d 
FROM tweets;
```
Output showing the first 5 rows:
<img width="305" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/3d36f43d-496d-490a-8a7f-cbd1258c8d8b">
The output shows the rolling average tweet count for the cumulative number of days.

**Calculating the rolling average tweet count:**
- 06/01/2022: 2 = 2.0
- 06/02/2022: 2 + 1 = 3 / 2 days = 1.5
- 06/03/2022: 2 + 1 + 3 = 6 / 3 days = 2.0
- 06/04/2022: 2 + 1 + 3 + 4 = 10 / 4 days = 2.5
- 06/05/2022: 2 + 1 + 3 + 4 + 5 = 15 / 5 days = 3.0
  
However, this is not what we need as we want to calculate the rolling average over a 3-day period. To achieve this, we add
the expression **ROWS BETWEEN 2 PRECEDING AND CURRENT ROW** to the window function.

```sql
AVG(tweet_count) OVER (
  PARTITION BY user_id     
  ORDER BY tweet_date     
  ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) -- This is the additional expression
```
#### Step 2: Calculate the rolling average tweet count for each user over a 3-day period
To calculate the rolling average tweet count by 3-day period, we modify the previous query by adding the **ROWS BETWEEN 2 PRECEDING 
AND CURRENT ROW** expression to the window function.
```sql
SELECT  user_id,    
        tweet_date,
        tweet_count,
        AVG(tweet_count) OVER ( PARTITION BY user_id ORDER BY tweet_date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW ) AS rolling_avg
FROM tweets;
```
Output showing the first 5 rows:
<img width="306" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/e3dcdb50-44d7-4717-ae14-6dea8c489ad9">

This query outputs the rolling average tweet count by 3-day period, as required by the question.

**Calculating the rolling averages using ROWS BETWEEN 2 PRECEDING AND CURRENT ROW**

- 06/01/2022: 2 = 2.0
- 06/02/2022: 2 + 1 = 3 / 2 days = 1.5
- 06/03/2022: 2 + 1 + 3 = 6 / 3 days = 2.0
- 06/04/2022: 1 + 3 + 4 = 8 / 3 days = 2.666...
- 06/05/2022: 3 + 4 + 5 = 12 / 3 days = 4.0
  
#### Step 3: Round the rolling average tweet count to 2 decimal points
Finally, we round up the rolling averages to the nearest 2 decimal points by incorporating the ROUND() function into the 
previous query.
```sql
SELECT  user_id,    
        tweet_date,   
        ROUND(AVG(tweet_count) OVER ( PARTITION BY user_id ORDER BY tweet_date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) ,2) AS rolling_avg_3d
FROM tweets;
```
<img width="377" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/efdbf0ce-9241-42d6-ad48-936ecda0c60b">
