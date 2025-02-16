## QUESTION :
Julia conducted a  days of learning SQL contest. The start date of the contest was March 01, 2016 and the end date was 
March 15, 2016.

Write a query to print total number of unique hackers who made at least  submission each day (starting on the first day 
of the contest), and find the hacker_id and name of the hacker who made maximum number of submissions each day. If more 
than one such hacker has a maximum number of submissions, print the lowest hacker_id. The query should print this information
for each day of the contest, sorted by the date.

Input Format

The following tables hold contest data:

- Hackers: The hacker_id is the id of the hacker, and name is the name of the hacker.

  <img width="314" alt="image" src="https://github.com/user-attachments/assets/3cf35008-28b8-4013-8d57-c217df41d1ff" />  <img width="194" alt="image" src="https://github.com/user-attachments/assets/a25dbdf1-719d-4f24-a6b6-c9c7469b8143" />


- Submissions: The submission_date is the date of the submission, submission_id is the id of the submission, hacker_id is
  the id of the hacker who made the submission, and score is the score of the submission.

  <img width="315" alt="image" src="https://github.com/user-attachments/assets/0bc76bee-3e08-4850-b7c9-deed59178dc1" />  <img width="196" alt="image" src="https://github.com/user-attachments/assets/1b3b939b-a4f0-429e-bcf7-ff176d146dd6" />


<img width="288" alt="image" src="https://github.com/user-attachments/assets/2e1ef682-a109-43b6-9639-c9299b6193fa" />

## SOLUTION :
```SQL
WITH CTE AS (
    SELECT HACKER_ID, SUBMISSION_DATE, SUBMISSION_ID 
    FROM SUBMISSIONS S 
    WHERE (
        SELECT COUNT(DISTINCT SUBMISSION_DATE) 
        FROM SUBMISSIONS S1 
        WHERE S1.SUBMISSION_DATE <= S.SUBMISSION_DATE
    ) = (
        SELECT COUNT(DISTINCT SUBMISSION_DATE) 
        FROM SUBMISSIONS S1 
        WHERE S1.SUBMISSION_DATE <= S.SUBMISSION_DATE 
        AND S1.HACKER_ID = S.HACKER_ID
    )
),
UNIQUE_HACKERS AS (
    SELECT SUBMISSION_DATE, COUNT(DISTINCT HACKER_ID) AS UNIQUE_HACKER_COUNT 
    FROM CTE 
    GROUP BY SUBMISSION_DATE
),
MAXIMUM_SUB AS (
    SELECT HACKER_ID, SUBMISSION_DATE 
    FROM (
        SELECT HACKER_ID, SUBMISSION_DATE, ROW_NUMBER() OVER (PARTITION BY SUBMISSION_DATE ORDER BY TOTAL_SUB DESC, HACKER_ID) AS RNK 
        FROM (
            SELECT HACKER_ID, SUBMISSION_DATE, COUNT(*) AS TOTAL_SUB 
            FROM SUBMISSIONS 
            GROUP BY HACKER_ID, SUBMISSION_DATE
        ) A
    ) B 
    WHERE RNK = 1
)
SELECT 
    U.SUBMISSION_DATE AS SUBMISSION_DATE, 
    U.UNIQUE_HACKER_COUNT, 
    M.HACKER_ID, 
    H.NAME 
FROM UNIQUE_HACKERS U 
INNER JOIN MAXIMUM_SUB M ON U.SUBMISSION_DATE = M.SUBMISSION_DATE 
INNER JOIN HACKERS H ON M.HACKER_ID = H.HACKER_ID 
ORDER BY U.SUBMISSION_DATE;
```
<img width="424" alt="image" src="https://github.com/user-attachments/assets/0baffb87-d21f-47ad-844b-57d9f0b3ea43" />
