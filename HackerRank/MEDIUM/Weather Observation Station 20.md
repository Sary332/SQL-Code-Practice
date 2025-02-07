## Question :
A median is defined as a number separating the higher half of a data set from the lower half. Query the median of the Northern Latitudes (LAT_N) from STATION and round your answer to 4 decimal places.

Input Format

The STATION table is described as follows:

<img width="331" alt="image" src="https://github.com/user-attachments/assets/2af526f6-5973-4958-b479-54c33068b648" />

where LAT_N is the northern latitude and LONG_W is the western longitude.

## SOLUTION :
```SQL
SELECT DISTINCT CAST(PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY LAT_N) OVER() AS DECIMAL (18,4)) AS MEDIAN
FROM STATION
```
<img width="302" alt="image" src="https://github.com/user-attachments/assets/85b7fafe-e99c-4947-9c36-295b4fa133b4" />


