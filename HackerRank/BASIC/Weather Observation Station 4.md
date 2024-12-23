## Question :
Find the difference between the total number of CITY entries in the table and the number of distinct CITY entries in the table.

The STATION table is described as follows:

<img width="331" alt="image" src="https://github.com/user-attachments/assets/a46d6643-08f5-4764-bcb5-747fed133b42" />

Where LAT_N is the northern latitude and LONG_W is the western longitude.

For example, if there are three records in the table with CITY values 'New York', 'New York', 'Bengalaru', there are 2 different 
city names: 'New York' and 'Bengalaru'. The query returns 1, because total number of records - number of uniq city names = 3 - 2 = 1.

## Solution : 
```SQL
SELECT COUNT(CITY) - COUNT(DISTINCT(CITY)) AS DIFFERENT
FROM STATION
```
<img width="495" alt="image" src="https://github.com/user-attachments/assets/54ba4056-0d23-42eb-a16d-f7e6ecf80123" />

