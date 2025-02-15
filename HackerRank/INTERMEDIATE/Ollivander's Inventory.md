## QUESTION :
Harry Potter and his friends are at Ollivander's with Ron, finally replacing Charlie's old broken wand.
Hermione decides the best way to choose is by determining the minimum number of gold galleons needed to buy each non-evil 
wand of high power and age. Write a query to print the id, age, coins_needed, and power of the wands that Ron's interested 
in, sorted in order of descending power. If more than one wand has same power, sort the result in order of descending age.

**Input Format**

The following tables contain data on the wands in Ollivander's inventory:

- Wands: The id is the id of the wand, code is the code of the wand, coins_needed is the total number of gold galleons needed 
  to buy the wand, and power denotes the quality of the wand (the higher the power, the better the wand is).

  <img width="317" alt="image" src="https://github.com/user-attachments/assets/e7be2223-a6b0-4597-a2d8-dedcfe606f84" />  <img width="263" alt="image" src="https://github.com/user-attachments/assets/e5d8bde4-59b5-4f46-a4b8-83036642ea37" />


- Wands_Property: The code is the code of the wand, age is the age of the wand, and is_evil denotes whether the wand is good
  for the dark arts. If the value of is_evil is 0, it means that the wand is not evil. The mapping between code and age is
  one-one, meaning that if there are two pairs, (Code1,Age1) and (Code2,Age2) , then Code1 ≠ Code2 and Age1  ≠ Age2 .

  <img width="319" alt="image" src="https://github.com/user-attachments/assets/8fcdf666-6ce1-4304-8b33-bb77f65b9e4c" />  <img width="262" alt="image" src="https://github.com/user-attachments/assets/d5126823-cdfd-4113-967f-475df0cffb90" />

## SOLUTION :
```SQL
WITH Ollivander_Inventory AS (
SELECT W.ID,
       W.CODE,
       WP.AGE,
       W.COINS_NEEDED,
       W.[POWER] AS POWER_LEVEL,
       WP.IS_EVIL,
       ROW_NUMBER() OVER (PARTITION BY WP.AGE, W.[POWER] ORDER BY W.COINS_NEEDED ASC) AS RN
FROM WANDS W
JOIN WANDS_PROPERTY WP ON W.CODE = WP.CODE
WHERE IS_EVIL = 0
)

SELECT ID,
       AGE,
       COINS_NEEDED,
       POWER_LEVEL
FROM Ollivander_Inventory
WHERE RN = 1
ORDER BY POWER_LEVEL DESC,AGE DESC
```
<img width="340" alt="image" src="https://github.com/user-attachments/assets/2391e466-3b86-4b59-b3ee-c8e770173f8c" />



