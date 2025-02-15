##  QUESTION :
Pivot the Occupation column in OCCUPATIONS so that each Name is sorted alphabetically and displayed underneath its corresponding
Occupation. The output should consist of four columns (Doctor, Professor, Singer, and Actor) in that specific order, with their 
respective names listed alphabetically under each column.

Note: Print NULL when there are no more names corresponding to an occupation.

**Input Format**

The OCCUPATIONS table is described as follows:

<img width="313" alt="image" src="https://github.com/user-attachments/assets/9eb65725-4e36-4902-b325-7db3bc1d685a" />

Occupation will only contain one of the following values: Doctor, Professor, Singer or Actor.

**Sample Input**

<img width="227" alt="image" src="https://github.com/user-attachments/assets/2f34304a-78c3-4685-bde2-05435ecc5198" />

<img width="258" alt="image" src="https://github.com/user-attachments/assets/6f8a7f82-60bb-4b52-85e4-fd37078b0ff4" />

## SOLUTION 
```SQL
WITH PIVOT_OCCUPATIONS AS (
SELECT NAME,
       OCCUPATION,
       ROW_NUMBER () OVER (PARTITION BY OCCUPATION ORDER BY NAME) AS RN
FROM OCCUPATIONS
)

SELECT MAX(CASE WHEN OCCUPATION = 'DOCTOR' THEN NAME  END) AS DOCTOR,
       MAX(CASE WHEN OCCUPATION = 'PROFESSOR' THEN NAME END) AS PROFESSOR,
       MAX(CASE WHEN OCCUPATION = 'SINGER' THEN NAME END) AS SINGER,
       MAX(CASE WHEN OCCUPATION = 'ACTOR' THEN NAME END) AS ACTOR
FROM PIVOT_OCCUPATIONS
GROUP BY RN
ORDER BY RN
```
<img width="404" alt="image" src="https://github.com/user-attachments/assets/feffa282-cd4e-4872-aa14-9f23fdcbd0bb" />





