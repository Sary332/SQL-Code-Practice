#### Question:

Assume you're given a table Twitter tweet data, write a query to obtain a histogram of tweets posted per user in 2022. Output the tweet count 
per user as the bucket and the number of Twitter users who fall into that bucket.

In other words, group the users by the number of tweets they posted in 2022 and count the number of users in each group.

<img width="445" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/427650c2-51e2-48eb-8d5f-c2e134528aef">

#### Solution :

'''SQL
SELECT tweet_bucket, COUNT(*) AS user_num
FROM (
       SELECT user_id, COUNT(*) AS tweet_bucket
       FROM tweets
       WHERE EXTRACT('year' FROM tweet_date) = 2022
       GROUP BY user_id
     ) AS tweet_counts_per_user
GROUP BY 1
ORDER BY 1;
'''


