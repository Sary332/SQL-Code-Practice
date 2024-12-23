## Question
Consider P1(a,b) and P2(a,b) to be two points on a 2D plane.

- a happens to equal the minimum value in Northern Latitude (LAT_N in STATION).
- b happens to equal the minimum value in Western Longitude (LONG_W in STATION).
- c happens to equal the maximum value in Northern Latitude (LAT_N in STATION).
- d happens to equal the maximum value in Western Longitude (LONG_W in STATION).
  
Query the Manhattan Distance between points P1 and P2 and round it to a scale of 4 decimal places.

<img width="332" alt="image" src="https://github.com/user-attachments/assets/1eebba16-7fbc-4533-a78b-d17ff213038e" />

## Solution
<img width="262" alt="image" src="https://github.com/user-attachments/assets/272e14af-5898-4a9d-8074-790ac55fa492" />

```sql
SELECT
    CAST(
       ROUND(
              MAX(LAT_N) - MIN(LAT_N) + (MAX(LONG_W) - MIN(LONG_W)
         ),4)
       AS DECIMAL (10,4)
    ) AS Manhattan_Distance
FROM STATION
```
<img width="287" alt="image" src="https://github.com/user-attachments/assets/6a8d1171-14b3-4215-9800-5d4c019039dd" />


