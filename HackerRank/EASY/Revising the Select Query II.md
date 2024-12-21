## Question :
Query the NAME field for all American cities in the CITY table with populations larger than 120000. The CountryCode 
for America is USA.

The CITY table is described as follows:

<img width="332" alt="image" src="https://github.com/user-attachments/assets/3b788b44-e3dd-4ed7-9875-220e77f8987d" />

## Solution
```SQL
SELECT NAME
FROM CITY 
WHERE COUNTRYCODE = 'USA' AND POPULATION > 120000
```
<img width="494" alt="image" src="https://github.com/user-attachments/assets/171bdc28-fa92-4810-be59-a633b64dbbd6" />
