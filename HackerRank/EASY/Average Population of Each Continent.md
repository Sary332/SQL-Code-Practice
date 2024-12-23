## Question
Given the CITY and COUNTRY tables, query the names of all the continents (COUNTRY.Continent)
and their respective average city populations (CITY.Population) rounded down to the nearest integer.

Note: CITY.CountryCode and COUNTRY.Code are matching key columns.

The CITY and COUNTRY tables are described as follows:

<img width="222" alt="image" src="https://github.com/user-attachments/assets/578e16fa-c0bf-4852-ba86-c2911de372a9" />
<img width="229" alt="image" src="https://github.com/user-attachments/assets/e35b3e33-7fc8-4be4-9577-a05386d8aa60" />

## Solution
```sql
SELECT 
    CO.CONTINENT,
    AVG(CI.POPULATION) AS AVG_POPULATION
FROM 
    COUNTRY CO
JOIN 
    CITY CI 
ON 
    CI.CountryCode = CO.Code
GROUP BY 
    CO.CONTINENT;
```
<img width="345" alt="image" src="https://github.com/user-attachments/assets/6ada5a48-b866-46f2-a287-aa4410b4920a" />
