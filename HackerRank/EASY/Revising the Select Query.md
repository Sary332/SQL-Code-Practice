## Question :

Query all columns for all American cities in the CITY table with populations larger than 100000. The CountryCode for America is USA.

The CITY table is described as follows:

<img width="334" alt="image" src="https://github.com/user-attachments/assets/24b1563b-ea90-4d35-bbf8-9ed1e8962d32" />

## Solution :
```sql
SELECT *
FROM  CITY
WHERE COUNTRYCODE = 'USA' AND POPULATION > 100000
```
<img width="476" alt="image" src="https://github.com/user-attachments/assets/ce9bb14c-3089-4adb-b9e4-3e47b6fc6391" />


