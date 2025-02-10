## QUESTION :
You are given two tables: Students and Grades. Students contains three columns ID, Name and Marks.

<img width="283" alt="image" src="https://github.com/user-attachments/assets/4d58134f-5416-4b2a-aae4-e71bdc3bc7d4" />

Grades contains the following data:

<img width="249" alt="image" src="https://github.com/user-attachments/assets/33d88b80-ed23-4c30-a861-15e6754e3a5f" />

Ketty gives Eve a task to generate a report containing three columns: Name, Grade and Mark. Ketty doesn't want the NAMES
of those students who received a grade lower than 8. The report must be in descending order by grade -- i.e. higher grades
are entered first. If there is more than one student with the same grade (8-10) assigned to them, order those particular 
students by their name alphabetically. Finally, if the grade is lower than 8, use "NULL" as their name and list them by 
their grades in descending order. If there is more than one student with the same grade (1-7) assigned to them, order those
particular students by their marks in ascending order.

Write a query to help Eve.

## SOLUTION :
```SQL
SELECT CASE WHEN GRADE < 8 THEN 'NULL' ELSE NAME END AS NAME, G.GRADE, S.MARKS
FROM STUDENTS S
LEFT JOIN GRADES G ON S.MARKS BETWEEN G.MIN_MARK AND G.MAX_MARK 
ORDER BY 
        GRADE DESC,
        CASE WHEN GRADE BETWEEN 8 AND 10 THEN NAME END ASC,
        CASE WHEN GRADE < 8 THEN MARKS END ASC
```
|NAME | GRADE | MARKS |
|:-----:|:-----:|:-----:|
|Britney |10| 95|
|Heraldo |10|94|
|Julia |10|96|
|Kristeen |10| 100|
|Stuart |10| 99|
|Amina |9| 89|
|Christene |9| 88|
|Salma |9| 81|
|Samantha |9| 87|
|Scarlet |9| 80|
|Vivek |9| 84|
|Aamina |8| 77|
|Belvet |8| 78|
|Paige |8| 74|
|Priya |8| 76|
|Priyanka |8| 77|
|NULL |7| 64|
|NULL |7| 66|
|NULL |6| 55|
|NULL |4| 34|
|NULL |3| 24|

