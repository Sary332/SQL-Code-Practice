## QUESTION :

P(R) represents a pattern drawn by Julia in R rows. The following pattern represents P(5):

<img width="152" alt="image" src="https://github.com/user-attachments/assets/ee31b53c-32a9-4a2c-a0bd-c572ef5d50f2" />

Write a query to print the pattern P(20).

## SOLUTION :
```SQL
DECLARE @n INT = 20;  -- Jumlah baris
DECLARE @i INT = @n;

WHILE @i > 0
BEGIN
    PRINT REPLICATE('* ', @i);  -- Cetak bintang
    SET @i = @i - 1;
END
```
<img width="466" alt="image" src="https://github.com/user-attachments/assets/8d66f19b-a6ef-48ed-a2c8-0b7e2121ae00" />

