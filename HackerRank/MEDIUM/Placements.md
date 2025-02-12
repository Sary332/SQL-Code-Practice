## QUESTION :
You are given three tables: Students, Friends and Packages. Students contains two columns: ID and Name.
Friends contains two columns: ID and Friend_ID (ID of the ONLY best friend). Packages contains two columns: 
ID and Salary (offered salary in $ thousands per month).

<img width="332" alt="image" src="https://github.com/user-attachments/assets/f38d4474-3214-430e-974a-9da6a05e3871" />
<img width="335" alt="image" src="https://github.com/user-attachments/assets/cbae4dbb-137a-42ab-80ca-11c4d15d8c93" />

Write a query to output the names of those students whose best friends got offered a higher salary than them. Names must be ordered by the salary amount offered to the best friends. It is guaranteed that no two students got same salary offer.

Sample Input : 

<img width="327" alt="image" src="https://github.com/user-attachments/assets/0257b37c-74a7-4069-b59a-287123e7ec55" /> <img width="331" alt="image" src="https://github.com/user-attachments/assets/6c11ab34-b3d2-464e-8b88-42522c94ecc9" />  <img width="331" alt="image" src="https://github.com/user-attachments/assets/daf14354-e82e-4c03-ad3d-687eb99ce21a" />

<img width="338" alt="image" src="https://github.com/user-attachments/assets/608b7188-53fe-4c66-86f3-8ec09aefb183" />


Now,
- Samantha's best friend got offered a higher salary than her at 11.55
- Julia's best friend got offered a higher salary than her at 12.12
- Scarlet's best friend got offered a higher salary than her at 15.2
- Ashley's best friend did NOT get offered a higher salary than her
The name output, when ordered by the salary offered to their friends, will be:
**Samantha**
**Julia**
**Scarlet**

## SOLUTION :
```SQL
WITH SALARY_INFO AS (
SELECT S.ID,
       S.NAME AS NAME,
       P1.SALARY AS SALARY,
       F. FRIEND_ID,
       P2.SALARY AS FRIEND_SALARY
FROM
     STUDENTS S
JOIN
     FRIENDS F ON S.ID = F.ID
JOIN
     PACKAGES P1 ON S.ID = P1.ID
JOIN
     PACKAGES P2 ON F.FRIEND_ID = P2.ID 
WHERE P1.SALARY < P2.SALARY 
--ORDER BY FRIEND_SALARY DESC, SALARY ASC
)

SELECT NAME 
FROM SALARY_INFO
ORDER BY FRIEND_SALARY 
```
<img width="479" alt="image" src="https://github.com/user-attachments/assets/be019d74-9c64-4004-af95-9cdbfd12313d" />


