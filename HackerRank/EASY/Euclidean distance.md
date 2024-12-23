## Question
Consider P1(a,c) and P2(b,s) to be two points on a 2D plane where (a,b) are the respective minimum and maximum 
values of Northern Latitude (LAT_N) and (c,d) are the respective minimum and maximum values of 
Western Longitude (LONG_W) in STATION.

Query the Euclidean Distance between points P1 and P2 and format your answer to display 4 decimal digits.

The STATION table is described as follows:

<img width="331" alt="image" src="https://github.com/user-attachments/assets/b6142a72-ed8d-4f51-8652-604ecab16690" />

## Solution
<img width="388" alt="image" src="https://github.com/user-attachments/assets/6d61b6fd-c411-4d85-a9ed-32263711eee0" />
```sql
SELECT 
    CAST(
        ROUND(
            SQRT(
                POWER(MAX(LONG_W) - MIN(LONG_W), 2) + 
                POWER(MAX(LAT_N) - MIN(LAT_N), 2)
            ), 4
        ) AS DECIMAL(10,4)
    ) AS Euclidean_Distance
FROM 
    STATION;
```
<img width="304" alt="image" src="https://github.com/user-attachments/assets/ba93bff3-6a6f-40b8-8f53-458861435d90" />

