## Question :
Query the two cities in STATION with the shortest and longest CITY names, as well as their respective lengths (i.e.: number 
of characters in the name). If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically.

The STATION table is described as follows:

<img width="329" alt="image" src="https://github.com/user-attachments/assets/2bf652e1-4d36-4f3c-ae6b-bfbd04138c94" />

**Explanation :**
When ordered alphabetically, the CITY names are listed as ABC, DEF, PQRS, and WXY, with lengths 3,3,4 and 3 . The longest name is PQRS,
but there are 3 options for shortest named city. Choose ABC, because it comes first alphabetically.

**Note :**
You can write two separate queries to get the desired output. It need not be a single query.

## Solution :
```sql
/* CARA 1 */

SELECT * 
FROM (SELECT TOP 1 CITY, LEN(CITY) AS LENGTH 
      FROM STATION 
      ORDER BY LEN(CITY) ASC, CITY) T 

UNION 

SELECT * 
FROM (SELECT TOP 1 CITY, LEN(CITY) AS LENGTH 
      FROM STATION ORDER BY LEN(CITY) DESC, CITY) T2
```
<img width="379" alt="image" src="https://github.com/user-attachments/assets/87732753-a183-4f41-a960-e256ff205043" />

```SQL
/* CARA 2 */

-- Longest city name

SELECT TOP 1 
    CONCAT(CITY, ' ', LEN(CITY)) AS RESULT
FROM 
    STATION
WHERE 
    LEN(CITY) = (
        SELECT TOP 1 LEN(CITY) 
        FROM STATION 
        ORDER BY LEN(CITY) DESC
    )
ORDER BY 
    CITY;

-- Shorter city name

SELECT TOP 1 
    CONCAT(CITY, ' ', LEN(CITY)) AS RESULT
FROM 
    STATION
WHERE 
    LEN(CITY) = (
        SELECT TOP 1 LEN(CITY) 
        FROM STATION 
        ORDER BY LEN(CITY)
    )
ORDER BY 
    CITY;
```
<img width="372" alt="image" src="https://github.com/user-attachments/assets/76b3ac5a-2bfa-40ac-851b-9b947c053581" />
