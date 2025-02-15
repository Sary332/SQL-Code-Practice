## QUESTION :

P(R) represents a pattern drawn by Julia in R rows. The following pattern represents P(5):

<img width="143" alt="image" src="https://github.com/user-attachments/assets/1eae8df2-abaf-4998-8850-7cef9673d623" />

Write a query to print the pattern P(20).

## SOLUTION :
```SQL
DECLARE @n INT = 20;  -- Jumlah baris
DECLARE @i INT = 1;

WHILE @i <= @n
BEGIN
    PRINT REPLICATE('* ', @i);  -- Cetak bintang
    SET @i = @i + 1;
END
```
<img width="401" alt="image" src="https://github.com/user-attachments/assets/741aae88-d680-4c68-868d-8f9a015a9953" />

