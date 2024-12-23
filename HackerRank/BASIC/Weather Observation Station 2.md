## Question : 
Query the following two values from the STATION table:

1) The sum of all values in LAT_N rounded to a scale of 2 decimal places.
2) The sum of all values in LONG_W rounded to a scale of 2 decimal places.

The STATION table is described as follows:

<img width="332" alt="image" src="https://github.com/user-attachments/assets/f5970881-6b85-48e6-9d54-a29fbaa682c4" />

Where LAT_N is the northern latitude and LONG_W is the western longitude.

## Solution :
```sql
SELECT CAST(ROUND(SUM(LAT_N),2) AS DECIMAL (10,2)) AS LAT_N,
       CAST(ROUND(SUM(LONG_W),2) AS DECIMAL (10,2)) AS LONG_N
FROM STATION
```
<img width="494" alt="image" src="https://github.com/user-attachments/assets/86c27db3-fd64-487f-a335-f1578c5c2158" />


