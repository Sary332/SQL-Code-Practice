## Question : 
Query a list of CITY names from STATION for cities that have an even ID number. Print the results in any order, but exclude 
duplicates from the answer.

The STATION table is described as follows:

<img width="331" alt="image" src="https://github.com/user-attachments/assets/968c8947-05f6-48c5-a3cd-6e494f8964bb" />

Where LAT_N is the northern latitude and LONG_W is the western longitude.

## Solution : 
```sql
SELECT DISTINCT CITY
FROM STATION
WHERE ID %2 = 0 -- Odd :  %2 <> 0
```
<img width="478" alt="image" src="https://github.com/user-attachments/assets/504e6014-5442-45cf-b4f4-548886153566" />
