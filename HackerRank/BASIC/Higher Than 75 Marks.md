## Question
Query the Name of any student in STUDENTS who scored higher than 75 Marks. Order your 
output by the last three characters of each name. If two or more students both have 
names ending in the same last three characters (i.e.: Bobby, Robby, etc.), secondary 
sort them by ascending ID.

The STUDENTS table is described as follows:

<img width="329" alt="image" src="https://github.com/user-attachments/assets/8ad408f9-eaee-4c53-9da5-3352c8c9246e" />

## Solution
```sql
SELECT NAME
FROM STUDENTS
WHERE MARKS > 75
ORDER BY RIGHT(NAME,3),ID
```
<img width="298" alt="image" src="https://github.com/user-attachments/assets/174d0be8-2ed9-490a-9746-72ebb6c3ecb4" />
