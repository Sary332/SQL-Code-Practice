## QUESTION :
You are given a table, Projects, containing three columns: Task_ID, Start_Date and End_Date. It is guaranteed that 
the difference between the End_Date and the Start_Date is equal to 1 day for each row in the table.

<img width="281" alt="image" src="https://github.com/user-attachments/assets/fdc1f31a-57d1-4482-bbbd-1ac53aeda59d" />

If the End_Date of the tasks are consecutive, then they are part of the same project. Samantha is interested in finding
the total number of different projects completed.

Write a query to output the start and end dates of projects listed by the number of days it took to complete the project 
in ascending order. If there is more than one project that have the same number of completion days, then order by the 
start date of the project.

<img width="284" alt="image" src="https://github.com/user-attachments/assets/6f828f48-648f-4bff-bce0-77378c1cf0e7" />


**Explanation**

The example describes following four projects:

- Project 1: Tasks 1, 2 and 3 are completed on consecutive days, so these are part of the project. Thus start date of 
  project is 2015-10-01 and end date is 2015-10-04, so it took 3 days to complete the project.
- Project 2: Tasks 4 and 5 are completed on consecutive days, so these are part of the project. Thus, the start date of
- project is 2015-10-13 and end date is 2015-10-15, so it took 2 days to complete the project.
- Project 3: Only task 6 is part of the project. Thus, the start date of project is 2015-10-28 and end date is 2015-10-29,
  so it took 1 day to complete the project.
- Project 4: Only task 7 is part of the project. Thus, the start date of project is 2015-10-30 and end date is 2015-10-31,
  so it took 1 day to complete the project.

  ## SOLUTION :
  
```SQL
WITH PROJECTS_GROUP AS (
    
SELECT START_DATE,
       END_DATE,
       DATEADD(DAY, - ROW_NUMBER() OVER(ORDER BY START_DATE),END_DATE) AS GROUP_ID
FROM PROJECTS
),

PROJECTS_DURATION AS (

SELECT MIN(START_DATE) AS START_DATE,
       MAX(END_DATE) AS END_DATE,
       DATEDIFF(DAY, MIN(START_DATE), MAX(END_DATE)) AS DURATION
FROM PROJECTS_GROUP
GROUP BY GROUP_ID
)

SELECT START_DATE,
       END_DATE
FROM PROJECTS_DURATION
ORDER BY DURATION,
         START_DATE 
```
<img width="350" alt="image" src="https://github.com/user-attachments/assets/af98a362-f883-43c5-bf61-9c2a6e932b9d" />

### NOTES :

**1. ProjectGroups CTE:**

- Instead of nesting window functions, we directly calculate a GroupID by using DATEADD(day, -ROW_NUMBER() OVER (ORDER BY
  End_Date), End_Date). This creates a unique identifier for consecutive End_Date values.
- For example, if End_Date values are consecutive, the GroupID will be the same for those rows.

**ProjectDurations CTE:**

- I group by the GroupID and calculate the minimum Start_Date and maximum End_Date for each group.
- I also calculate the duration of each project using DATEDIFF(day, MIN(Start_Date), MAX(End_Date)) + 1.

Pertanyaan yang bagus! Penambahan `+ 1` dalam perhitungan durasi (`DATEDIFF(day, MIN(Start_Date), MAX(End_Date)) + 1`) diperlukan karena **`DATEDIFF` menghitung perbedaan antara dua tanggal berdasarkan batas (boundary) hari**, bukan jumlah hari secara inklusif.

#### Mengapa +1 ? 

1. **Cara Kerja `DATEDIFF`**:
   - Fungsi `DATEDIFF(day, Start_Date, End_Date)` menghitung jumlah batas hari antara `Start_Date` dan `End_Date`.
   - Misalnya, jika `Start_Date = 2023-10-01` dan `End_Date = 2023-10-03`, maka `DATEDIFF(day, '2023-10-01', '2023-10-03')` akan mengembalikan **2**, karena ada 2 batas hari:
     - Dari `2023-10-01` ke `2023-10-02` (batas 1).
     - Dari `2023-10-02` ke `2023-10-03` (batas 2).

2. **Kenapa `+ 1`?**:
   - Dalam konteks proyek, kita ingin menghitung **total hari secara inklusif**, termasuk hari pertama dan hari terakhir.
   - Misalnya, jika proyek dimulai pada `2023-10-01` dan berakhir pada `2023-10-03`, maka total hari yang dihabiskan adalah **3 hari**:
     - `2023-10-01` (hari 1),
     - `2023-10-02` (hari 2),
     - `2023-10-03` (hari 3).
   - Namun, `DATEDIFF` hanya mengembalikan **2** untuk contoh ini. Oleh karena itu, kita perlu menambahkan `+ 1` untuk menghitung hari secara inklusif.

