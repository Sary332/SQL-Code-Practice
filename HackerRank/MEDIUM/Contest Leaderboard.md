## QUESTION
You did such a great job helping Julia with her last coding contest challenge that she wants you to work on this one, too!
The total score of a hacker is the sum of their maximum scores for all of the challenges. Write a query to print the hacker_id,
name, and total score of the hackers ordered by the descending score. If more than one hacker achieved the same total score, 
then sort the result by ascending hacker_id. Exclude all hackers with a total score of  from your result.

**Input Format :**

The following tables contain contest data:
- Hackers: The hacker_id is the id of the hacker, and name is the name of the hacker.

  <img width="316" alt="image" src="https://github.com/user-attachments/assets/a63ebf7f-9d24-41db-8602-f3b9ce013c17" />

- Submissions: The submission_id is the id of the submission, hacker_id is the id of the hacker who made the submission,
challenge_id is the id of the challenge for which the submission belongs to, and score is the score of the submission.

<img width="315" alt="image" src="https://github.com/user-attachments/assets/bd8235b0-7e1f-483e-8e86-7461c1a68e47" />

**Sample Input :**

Hackers Table & Submissions Table :

<img width="173" alt="image" src="https://github.com/user-attachments/assets/03e3ae0a-0e5d-46ec-9510-a12ed2cddb4a" />  <img width="191" alt="image" src="https://github.com/user-attachments/assets/364575f4-1de1-4ca0-8fd1-d4192981040f" />


**Explanation :** 

Hacker 4071 submitted solutions for challenges 19797 and 49593, so the total score .

Hacker 74842 submitted solutions for challenges 19797 and 63132, so the total score 

Hacker 84072 submitted solutions for challenges 49593 and 63132, so the total score .

The total scores for hackers 4806, 26071, 80305, and 49438 can be similarly calculated.

## SOLUTION :
```SQL
WITH LEADERBOARD AS (
SELECT H.HACKER_ID,
       H.NAME,
       S.CHALLENGE_ID,
       MAX(S.SCORE) AS MAX_SCORE
FROM HACKERS H
JOIN
    SUBMISSIONS S ON H.HACKER_ID = S.HACKER_ID
GROUP BY H.HACKER_ID, H.NAME, S.CHALLENGE_ID
)

SELECT HACKER_ID,
       NAME,
       SUM(MAX_SCORE) AS TOTAL_SCORE
FROM LEADERBOARD
GROUP BY HACKER_ID, NAME
HAVING SUM(MAX_SCORE) > 0
ORDER BY SUM(MAX_SCORE) DESC, HACKER_ID ASC
```
