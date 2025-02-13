## QUESTION :
Julia just finished conducting a coding contest, and she needs your help assembling the leaderboard! Write a query to print the respective hacker_id and name of hackers who achieved full scores for more than one challenge. Order your output in descending order by the total number of challenges in which the hacker earned a full score. If more than one hacker received full scores in same number of challenges, then sort them by ascending hacker_id.

The following tables contain contest data:

- Hackers: The hacker_id is the id of the hacker, and name is the name of the hacker.
  
  <img width="172" alt="image" src="https://github.com/user-attachments/assets/78328cbc-6f47-410e-ae3c-6712b2868a11" />  <img width="169" alt="image" src="https://github.com/user-attachments/assets/a2d27810-f59a-4375-ae6c-1ca597c11fdd" />


- Difficulty: The difficult_level is the level of difficulty of the challenge, and score is the maximum score that can be achieved for a challenge at that difficulty level.

  <img width="172" alt="image" src="https://github.com/user-attachments/assets/c9d93c5a-4d06-48fb-9f5b-5bea46151f60" />  <img width="164" alt="image" src="https://github.com/user-attachments/assets/46674f4d-1d85-4b83-a5a9-b336fd2f5591" />


- Challenges: The challenge_id is the id of the challenge, the hacker_id is the id of the hacker who created the challenge, and difficulty_level is the level of difficulty of the challenge.

  <img width="172" alt="image" src="https://github.com/user-attachments/assets/28e89675-433a-4137-973a-0d9f288aeca7" />  <img width="204" alt="image" src="https://github.com/user-attachments/assets/184331ef-0836-473c-a31c-26d926eeb48a" />


- Submissions: The submission_id is the id of the submission, hacker_id is the id of the hacker who made the submission, challenge_id is the id of the challenge that the submission belongs to, and score is the score of the submission.

  <img width="170" alt="image" src="https://github.com/user-attachments/assets/f4d7acda-4560-424d-a55c-8c0dbd1b3a49" />  <img width="179" alt="image" src="https://github.com/user-attachments/assets/a1eccbe5-9e31-4886-af7d-d181a294bc02" />

### Explanation :

Hacker 86870 got a score of 30 for challenge 71055 with a difficulty level of 2, so 86870 earned a full score for this challenge.

Hacker 90411 got a score of 30 for challenge 71055 with a difficulty level of 2, so 90411 earned a full score for this challenge.

Hacker 90411 got a score of 100 for challenge 66730 with a difficulty level of 6, so 90411 earned a full score for this challenge.

Only hacker 90411 managed to earn a full score for more than one challenge, so we print the their hacker_id and name as  space-separated values.


## SOLUTION :
```SQL
SELECT H.HACKER_ID, 
       H.NAME     
FROM HACKERS H
JOIN 
    SUBMISSIONS S ON H.HACKER_ID = S.HACKER_ID
JOIN 
    CHALLENGES C ON S.CHALLENGE_ID = C.CHALLENGE_ID
JOIN 
    DIFFICULTY D ON C.DIFFICULTY_LEVEL = D.DIFFICULTY_LEVEL

WHERE S.SCORE = D.SCORE
GROUP BY H.HACKER_ID, H.NAME
HAVING COUNT( DISTINCT C.CHALLENGE_ID) > 1  -- /HAVING COUNT(C.CHALLENGE_ID) > 1
ORDER BY COUNT( DISTINCT C.CHALLENGE_ID) DESC, H.HACKER_ID ASC
```
<img width="325" alt="image" src="https://github.com/user-attachments/assets/0cd9e3f2-21bd-4460-afbe-b4245f6590ed" />

