## Question

Generate the following two result sets:

1) Query an alphabetically ordered list of all names in OCCUPATIONS, immediately followed 
   by the first letter of each profession as a parenthetical (i.e.: enclosed in parentheses). 
   For example: AnActorName(A), ADoctorName(D), AProfessorName(P), and ASingerName(S).
2) Query the number of ocurrences of each occupation in OCCUPATIONS. Sort the occurrences in ascending order, 
   and output them in the following format:

<img width="330" alt="image" src="https://github.com/user-attachments/assets/2afaa11c-ee01-4353-b96c-3cef96d7bfcc" />

Where [occupation_count] is the number of occurrences of an occupation in OCCUPATIONS and [occupation] is the lowercase occupation name. If more than one Occupation has the same [occupation_count], they should be ordered alphabetically.

**Note:** There will be at least two entries in the table for each type of occupation.

The OCCUPATIONS table is described as follows:

<img width="365" alt="image" src="https://github.com/user-attachments/assets/70e11371-e267-4ab4-a3b9-7cf9bb539763" />

## Solution
```sql
SELECT CONCAT(NAME,'(',LEFT(OCCUPATION,1),')') AS occupation_count
FROM  OCCUPATIONS 

UNION ALL

SELECT CONCAT('There are a total of ',COUNT(*),' ',LOWER(OCCUPATION),'s.') AS occupation_count
FROM OCCUPATIONS 
GROUP BY OCCUPATION
ORDER BY occupation_count  --Nama aliasanya harus sama karena pakai union all
```
<img width="431" alt="image" src="https://github.com/user-attachments/assets/8e4ba1bc-3542-4eec-b6bb-5ee86b2dd6e7" />
