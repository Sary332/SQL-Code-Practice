## Question
Query the sum of Northern Latitudes (LAT_N) from STATION having values greater than 38.7880  and less than 137.2345. 
Truncate your answer to  decimal places.

The STATION table is described as follows:

<img width="332" alt="image" src="https://github.com/user-attachments/assets/e2186e09-1c42-423f-9d16-f03abe40c6c2" />

## Solution
```sql
SELECT CAST(ROUND(SUM(LAT_N),4) AS DECIMAL (10,4)) AS LAT_N_SUM
FROM STATION
WHERE LAT_N > 38.7880 AND LAT_N < 137.2345
```
<img width="286" alt="image" src="https://github.com/user-attachments/assets/21f7d5b2-8dec-4532-9b2f-d93d9adc7629" />
