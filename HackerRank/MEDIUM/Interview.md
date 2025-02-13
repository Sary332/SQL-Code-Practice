## QUESTION :

Samantha interviews many candidates from different colleges using coding challenges and contests. Write a query to print the contest_id, hacker_id, name, and the sums of total_submissions, total_accepted_submissions, total_views, and total_unique_views for each contest sorted by contest_id. Exclude the contest from the result if all four sums are .

Note: A specific contest can be used to screen candidates at more than one college, but each college only holds  screening contest.
===


```sql
SELECT CON.CONTEST_ID, 
       CON.HACKER_ID,
       CON.NAME,
       ISNULL(SUM(S.TOTAL_SUBMISSIONS),0) AS TOTAL_SUBMISSIONS,
       ISNULL(SUM(S.TOTAL_ACCEPTED_SUBMISSIONS),0) AS TOTAL_ACCEPTED_SUBMISSIONS ,
       ISNULL(SUM(V.TOTAL_VIEWS),0) AS TOTAL_VIEWS,
       ISNULL(SUM(V.TOTAL_UNIQUE_VIEWS),0) AS TOTAL_UNIQUE_VIEWS
FROM CONTESTS CON
JOIN 
         COLLEGES COL ON CON.CONTEST_ID = COL.CONTEST_ID
LEFT JOIN 
         CHALLENGES CHA ON COL.COLLEGE_ID = CHA.COLLEGE_ID
LEFT JOIN 
         (SELECT CHALLENGE_ID,
                 SUM(TOTAL_VIEWS) AS TOTAL_VIEWS,
                 SUM(TOTAL_UNIQUE_VIEWS) AS TOTAL_UNIQUE_VIEWS
          FROM VIEW_STATS
          GROUP BY CHALLENGE_ID
         ) AS V ON CHA.CHALLENGE_ID = V.CHALLENGE_ID
LEFT JOIN 
         (SELECT CHALLENGE_ID,
                 SUM(TOTAL_SUBMISSIONS) AS TOTAL_SUBMISSIONS,
                 SUM(TOTAL_ACCEPTED_SUBMISSIONS) AS TOTAL_ACCEPTED_SUBMISSIONS
          FROM SUBMISSION_STATS
          GROUP BY CHALLENGE_ID
          ) S ON CHA.CHALLENGE_ID = S.CHALLENGE_ID
GROUP BY CON.CONTEST_ID, 
         CON.HACKER_ID,
         CON.NAME
HAVING ISNULL(SUM(S.TOTAL_SUBMISSIONS),0) 
     + ISNULL(SUM(S.TOTAL_ACCEPTED_SUBMISSIONS),0) 
     + ISNULL(SUM(V.TOTAL_VIEWS),0) 
     + ISNULL(SUM(V.TOTAL_UNIQUE_VIEWS),0) > 0
ORDER BY CON.CONTEST_ID
```
