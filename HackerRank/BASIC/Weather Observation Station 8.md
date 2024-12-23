## Question
Query the list of CITY names from STATION which have vowels (i.e., a, e, i, o, and u)
as both their first and last characters. Your result cannot contain duplicates.

The STATION table is described as follows:

<img width="331" alt="image" src="https://github.com/user-attachments/assets/fe2d717d-4537-4b96-ae81-da3ed12d4bb9" />

## Solution
```sql
SELECT CITY
FROM STATION
WHERE CITY LIKE '[AIUEO]%' AND CITY LIKE '%[aiueo]'
```
<img width="340" alt="image" src="https://github.com/user-attachments/assets/fd40b39c-28c6-4d4f-871e-282bda84f684" />



