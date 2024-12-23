## Question
Write a query identifying the type of each record in the TRIANGLES table using its three side lengths. 
Output one of the following statements for each record in the table:

- Equilateral: It's a triangle with 3 sides of equal length.
- Isosceles: It's a triangle with 2 sides of equal length.
- Scalene: It's a triangle with 3 sides of differing lengths.
- Not A Triangle: The given values of A, B, and C don't form a triangle.

The TRIANGLES table is described as follows:

<img width="332" alt="image" src="https://github.com/user-attachments/assets/3133262c-fbc6-4919-abdf-4bc21a0cd4bb" />

<img width="325" alt="image" src="https://github.com/user-attachments/assets/29f56318-1d32-4fe1-a274-7fd7ce0e9869" />

## Solution
```sql
SELECT CASE WHEN A+B <= C OR A+C <= B OR B+C <= A THEN 'Not A Triangle'
            WHEN A = B AND B = C THEN 'Equilateral'
            WHEN A = B OR B = C OR A = C THEN 'Isosceles'
            ELSE 'Scalene'
        END AS TRIANGEL_TYPE
FROM  TRIANGLES 
```
<img width="293" alt="image" src="https://github.com/user-attachments/assets/82271fbf-0b05-4018-b623-36bb01245d32" />

