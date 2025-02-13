## QUESTION :
You are given a table, Functions, containing two columns: X and Y.

<img width="281" alt="image" src="https://github.com/user-attachments/assets/579ad959-da65-4fc9-880f-f32c3c859207" />

Two pairs (X1, Y1) and (X2, Y2) are said to be symmetric pairs if X1 = Y2 and X2 = Y1.

Write a query to output all such symmetric pairs in ascending order by the value of X. List the rows such that X1 ≤ Y1.

<img width="287" alt="image" src="https://github.com/user-attachments/assets/f3a37e46-74e1-41bf-b2e7-c7c245eb02b2" />


## SOLUTION :
```SQL
SELECT F1.X, F1.Y 
FROM FUNCTIONS F1
JOIN FUNCTIONS F2
     ON F1.X = F2.Y AND F1.Y = F2.X 
WHERE F1.X < F1.Y

UNION

SELECT X, Y 
FROM FUNCTIONS
WHERE X = Y 
GROUP BY X, Y 
HAVING COUNT(*) > 1
ORDER BY X, Y;
```
<img width="277" alt="image" src="https://github.com/user-attachments/assets/efbefbcd-e11e-40ba-8403-5b23dada565b" />
