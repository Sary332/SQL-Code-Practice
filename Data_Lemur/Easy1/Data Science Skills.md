## Question :
Given a table of candidates and their skills, you're tasked with finding the candidates best suited for an open Data Science job. 
You want to find candidates who are proficient in Python, Tableau, and PostgreSQL.

Write a query to list the candidates who possess all of the required skills for the job. Sort the output by candidate ID in ascending 
order.

**Assumption:**
There are no duplicates in the candidates table.

<img width="122" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/e82a44f2-73d8-4b97-b790-d2da5f1d3002">
<img width="142" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/23d59357-ad71-4d81-b9cc-bbd8ac5d0cc9">

## Solution :
```sql
SELECT candidate_id 
FROM   candidates
WHERE  skill IN ( 'Python', 'Tableau', 'PostgreSQL' )
GROUP BY candidate_id
HAVING   COUNT(skill) = 3
ORDER BY candidate_id ;
```
<img width="113" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/416c487f-e3df-4533-9e4f-3eb8bd31e8ba">

