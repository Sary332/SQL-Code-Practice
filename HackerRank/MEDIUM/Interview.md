## QUESTION :

Samantha interviews many candidates from different colleges using coding challenges and contests. Write a query to print the contest_id, hacker_id, name, and the sums of total_submissions, total_accepted_submissions, total_views, and total_unique_views for each contest sorted by contest_id. Exclude the contest from the result if all four sums are .

Note: A specific contest can be used to screen candidates at more than one college, but each college only holds  screening contest.

===

**Input Format :**

The following tables hold interview data:

- Contests: The contest_id is the id of the contest, hacker_id is the id of the hacker who created the contest, and name is the name of the hacker.

  <img width="179" alt="image" src="https://github.com/user-attachments/assets/c5a370db-e86f-4427-b94c-be31563f44e6" />  <img width="271" alt="image" src="https://github.com/user-attachments/assets/3d50f34c-3d1d-4c39-8acc-2d1ed08591bf" />


- Colleges: The college_id is the id of the college, and contest_id is the id of the contest that Samantha used to screen the candidates.

  <img width="182" alt="image" src="https://github.com/user-attachments/assets/2394e85f-a677-4b86-964a-0ec0b4c7b635" />  <img width="268" alt="image" src="https://github.com/user-attachments/assets/8aef288e-9261-421b-b7bb-4179205f0603" />


- Challenges: The challenge_id is the id of the challenge that belongs to one of the contests whose contest_id Samantha forgot, and college_id is the id of the college where the challenge was given to candidates.

  <img width="179" alt="image" src="https://github.com/user-attachments/assets/aae3a1e3-e818-4430-a48c-46522caff4b0" />  <img width="269" alt="image" src="https://github.com/user-attachments/assets/14259ed3-afff-455b-9e5c-b4b7af28478d" />


-View_Stats: The challenge_id is the id of the challenge, total_views is the number of times the challenge was viewed by candidates, and total_unique_views is the number of times the challenge was viewed by unique candidates.

<img width="179" alt="image" src="https://github.com/user-attachments/assets/6f82c8fe-7716-4911-b49a-977ce389c1a6" />  <img width="270" alt="image" src="https://github.com/user-attachments/assets/a550891c-2054-475a-8a2b-8583a10c3ab1" />


- Submission_Stats: The challenge_id is the id of the challenge, total_submissions is the number of submissions for the challenge, and total_accepted_submission is the number of submissions that achieved full scores.

  <img width="179" alt="image" src="https://github.com/user-attachments/assets/61e4b0fd-52ee-4ae6-b072-e9f6b618c5d2" />  <img width="271" alt="image" src="https://github.com/user-attachments/assets/251bb0fc-a730-4a62-910b-28ea9803f0c8" />


**Explanation :**

The contest 66406 is used 11219 in the college . In this college 11219, challenges 18765  and 47127 are asked, so from the view and submission stats:

- Sum of total submissions = 27 + 56 + 28 = 111 

- Sum of total accepted submissions = 10 + 18 + 11 = 39

- Sum of total views = 43 + 72 + 26 + 15 = 156

- Sum of total unique views 10 + 13 + 19 + 14 = 56

- Similarly, we can find the sums for contests 66556 and 94828 .

## SOLUTION :

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

<img width="457" alt="image" src="https://github.com/user-attachments/assets/598bd1cc-cb42-499d-9cd5-db502dba8a72" />


### NOTES :
