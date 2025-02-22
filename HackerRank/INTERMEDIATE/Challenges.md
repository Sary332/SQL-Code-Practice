## QUESTION:
Julia asked her students to create some coding challenges. Write a query to print the hacker_id, name, and the total number of
challenges created by each student. Sort your results by the total number of challenges in descending order. If more than one 
student created the same number of challenges, then sort the result by hacker_id. If more than one student created the same 
number of challenges and the count is less than the maximum number of challenges created, then exclude those students from the 
result. (NOTES: Hanya ambil nilai tertinggi dan nilai unik yang tidak sama, jika sama maka tidak dimasukan)

**Input Format**

The following tables contain challenge data:

- Hackers: The hacker_id is the id of the hacker, and name is the name of the hacker.
  
  <img width="223" alt="image" src="https://github.com/user-attachments/assets/74e408be-3993-48a2-aae6-9af92a4cb145" />

- Challenges: The challenge_id is the id of the challenge, and hacker_id is the id of the student who created the challenge.
  
  <img width="222" alt="image" src="https://github.com/user-attachments/assets/9c2cfafc-4128-4a00-b827-78daa597503f" />

**Sample Input :**

  Hackers & Challenges Tables :

  <img width="236" alt="image" src="https://github.com/user-attachments/assets/5f55e648-fb9b-4e81-8dc0-698ba4669349" />  <img width="238" alt="image" src="https://github.com/user-attachments/assets/6e729250-2ff7-41e6-b51f-7eb21b0e52ca" />

## SOLUTION :
```SQL
WITH CHALLENGES_BOARD AS (
SELECT H.HACKER_ID, 
       H.NAME, 
       COUNT(C.CHALLENGE_ID) AS NUM_CHALLENGES
FROM HACKERS H
JOIN 
    CHALLENGES C ON H.HACKER_ID = C.HACKER_ID
GROUP BY H.HACKER_ID, H.NAME
)
    
SELECT HACKER_ID, 
       NAME, 
       NUM_CHALLENGES 
FROM CHALLENGES_BOARD
GROUP BY HACKER_ID, NAME, NUM_CHALLENGES 
HAVING NUM_CHALLEGES = (SELECT MAX(NUM_CHALLEGES) FROM  CHALLENGES_BOARD ) 
            OR
       NUM_CHALLEGES IN 
                       (SELECT NUM_CHALLENGES 
                        FROM  CHALLENGES_BOARD 
                        GROUP BY NUM_CHALLENGES 
                        HAVING COUNT (NUM_CHALLENGES) = 1 ) 
ORDER BY NUM_CHALLENGES DESC, HACKER_ID ASC
```
<img width="360" alt="image" src="https://github.com/user-attachments/assets/61468a7a-b7a5-4310-9a8f-8a1c1e693df8" />

