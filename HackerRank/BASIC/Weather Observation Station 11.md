## QUESTION :
Query the list of CITY names from STATION that either do not start with vowels or do not end with vowels. Your result 
cannot contain duplicates.

**Input Format**

The STATION table is described as follows:

<img width="335" alt="image" src="https://github.com/user-attachments/assets/c7b02899-6a3a-4af6-9f2a-d2b14bd91750" />

where LAT_N is the northern latitude and LONG_W is the western longitude.

## SOLUTION :
```SQL
SELECT DISTINCT CITY
FROM STATION
WHERE LEFT(CITY,1) NOT LIKE '[AEIOU]%' OR RIGHT(CITY,1) NOT LIKE '%[AEIOU]'
```
