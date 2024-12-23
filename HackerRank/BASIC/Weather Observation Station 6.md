## Question
Query the list of CITY names starting with vowels (i.e., a, e, i, o, or u) from STATION. 
Your result cannot contain duplicates.

The STATION table is described as follows:

<img width="329" alt="image" src="https://github.com/user-attachments/assets/bbbc8e18-3059-4e14-b00a-79b9167c88c3" />

## Solution
```sql
SELECT CITY
FROM STATION
WHERE CITY LIKE '[AEIOU]%'
```
<img width="460" alt="image" src="https://github.com/user-attachments/assets/e94d3a2c-d4aa-4b13-a22b-a5e8b74fc554" />
