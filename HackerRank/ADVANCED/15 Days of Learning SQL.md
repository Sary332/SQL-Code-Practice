## QUESTION :
Julia conducted a  days of learning SQL contest. The start date of the contest was March 01, 2016 and the end date was 
March 15, 2016.

Write a query to print total number of unique hackers who made at least  submission each day (starting on the first day 
of the contest), and find the hacker_id and name of the hacker who made maximum number of submissions each day. If more 
than one such hacker has a maximum number of submissions, print the lowest hacker_id. The query should print this information
for each day of the contest, sorted by the date.

Input Format

The following tables hold contest data:

- Hackers: The hacker_id is the id of the hacker, and name is the name of the hacker.

  <img width="314" alt="image" src="https://github.com/user-attachments/assets/3cf35008-28b8-4013-8d57-c217df41d1ff" />  <img width="194" alt="image" src="https://github.com/user-attachments/assets/a25dbdf1-719d-4f24-a6b6-c9c7469b8143" />


- Submissions: The submission_date is the date of the submission, submission_id is the id of the submission, hacker_id is
  the id of the hacker who made the submission, and score is the score of the submission.

  <img width="315" alt="image" src="https://github.com/user-attachments/assets/0bc76bee-3e08-4850-b7c9-deed59178dc1" />  <img width="196" alt="image" src="https://github.com/user-attachments/assets/1b3b939b-a4f0-429e-bcf7-ff176d146dd6" />


<img width="288" alt="image" src="https://github.com/user-attachments/assets/2e1ef682-a109-43b6-9639-c9299b6193fa" />

## SOLUTION :
```SQL
WITH CTE AS (
    SELECT HACKER_ID, SUBMISSION_DATE, SUBMISSION_ID 
    FROM SUBMISSIONS S 
    WHERE (
        SELECT COUNT(DISTINCT SUBMISSION_DATE) 
        FROM SUBMISSIONS S1 
        WHERE S1.SUBMISSION_DATE <= S.SUBMISSION_DATE
    ) = (
        SELECT COUNT(DISTINCT SUBMISSION_DATE) 
        FROM SUBMISSIONS S1 
        WHERE S1.SUBMISSION_DATE <= S.SUBMISSION_DATE 
        AND S1.HACKER_ID = S.HACKER_ID
    )
),
UNIQUE_HACKERS AS (
    SELECT SUBMISSION_DATE, COUNT(DISTINCT HACKER_ID) AS UNIQUE_HACKER_COUNT 
    FROM CTE 
    GROUP BY SUBMISSION_DATE
),
MAXIMUM_SUB AS (
    SELECT HACKER_ID, SUBMISSION_DATE 
    FROM (
        SELECT HACKER_ID, SUBMISSION_DATE, ROW_NUMBER() OVER (PARTITION BY SUBMISSION_DATE ORDER BY TOTAL_SUB DESC, HACKER_ID) AS RNK 
        FROM (
            SELECT HACKER_ID, SUBMISSION_DATE, COUNT(*) AS TOTAL_SUB 
            FROM SUBMISSIONS 
            GROUP BY HACKER_ID, SUBMISSION_DATE
        ) A
    ) B 
    WHERE RNK = 1
)
SELECT 
    U.SUBMISSION_DATE AS SUBMISSION_DATE, 
    U.UNIQUE_HACKER_COUNT, 
    M.HACKER_ID, 
    H.NAME 
FROM UNIQUE_HACKERS U 
INNER JOIN MAXIMUM_SUB M ON U.SUBMISSION_DATE = M.SUBMISSION_DATE 
INNER JOIN HACKERS H ON M.HACKER_ID = H.HACKER_ID 
ORDER BY U.SUBMISSION_DATE;
```
<img width="424" alt="image" src="https://github.com/user-attachments/assets/0baffb87-d21f-47ad-844b-57d9f0b3ea43" />

## EXPLANATION :

Tentu! Mari kita jelaskan kode ini langkah demi langkah. Kode ini bertujuan untuk menganalisis data dari tabel `SUBMISSIONS` dan `HACKERS` untuk menghasilkan laporan harian yang mencakup:

1. **Jumlah hacker unik** yang melakukan submission setiap hari.
2. **Hacker dengan jumlah submission terbanyak** setiap hari.

Kode ini menggunakan beberapa **Common Table Expressions (CTEs)** dan **subquery** untuk mencapai tujuan tersebut. Berikut penjelasan detailnya:

---

### **Struktur Kode**
Kode ini terdiri dari 3 CTE (`CTE`, `UNIQUE_HACKERS`, dan `MAXIMUM_SUB`) dan 1 query utama. Mari kita bahas masing-masing bagian.

---

### **1. CTE: `CTE`**
```sql
WITH CTE AS (
    SELECT HACKER_ID, SUBMISSION_DATE, SUBMISSION_ID 
    FROM SUBMISSIONS S 
    WHERE (
        SELECT COUNT(DISTINCT SUBMISSION_DATE) 
        FROM SUBMISSIONS S1 
        WHERE S1.SUBMISSION_DATE <= S.SUBMISSION_DATE
    ) = (
        SELECT COUNT(DISTINCT SUBMISSION_DATE) 
        FROM SUBMISSIONS S1 
        WHERE S1.SUBMISSION_DATE <= S.SUBMISSION_DATE 
        AND S1.HACKER_ID = S.HACKER_ID
    )
)
```

#### **Tujuan**:
- Mengidentifikasi hacker yang telah melakukan submission **setiap hari** hingga tanggal tertentu (`SUBMISSION_DATE`).

#### **Cara Kerja**:
- Subquery pertama:
  ```sql
  SELECT COUNT(DISTINCT SUBMISSION_DATE) 
  FROM SUBMISSIONS S1 
  WHERE S1.SUBMISSION_DATE <= S.SUBMISSION_DATE
  ```
  - Menghitung **total hari unik** dari awal hingga `SUBMISSION_DATE`.

- Subquery kedua:
  ```sql
  SELECT COUNT(DISTINCT SUBMISSION_DATE) 
  FROM SUBMISSIONS S1 
  WHERE S1.SUBMISSION_DATE <= S.SUBMISSION_DATE 
  AND S1.HACKER_ID = S.HACKER_ID
  ```
  - Menghitung **total hari unik** di mana hacker tertentu (`S.HACKER_ID`) melakukan submission hingga `SUBMISSION_DATE`.

- **Kondisi WHERE**:
  - Jika kedua subquery menghasilkan nilai yang sama, artinya hacker tersebut telah melakukan submission **setiap hari** hingga `SUBMISSION_DATE`.

#### **Contoh**:
- Misalkan `SUBMISSION_DATE` adalah `2023-10-03`.
- Jika total hari unik hingga `2023-10-03` adalah 3, dan hacker `123` telah melakukan submission pada 3 hari tersebut, maka hacker `123` akan dimasukkan ke dalam `CTE`.

---

### **2. CTE: `UNIQUE_HACKERS`**
```sql
UNIQUE_HACKERS AS (
    SELECT SUBMISSION_DATE, COUNT(DISTINCT HACKER_ID) AS UNIQUE_HACKER_COUNT 
    FROM CTE 
    GROUP BY SUBMISSION_DATE
)
```

#### **Tujuan**:
- Menghitung **jumlah hacker unik** yang telah melakukan submission setiap hari.

#### **Cara Kerja**:
- Mengelompokkan data dari `CTE` berdasarkan `SUBMISSION_DATE`.
- Menghitung jumlah hacker unik (`COUNT(DISTINCT HACKER_ID)`) untuk setiap tanggal.

#### **Contoh**:
- Jika pada `2023-10-03` terdapat 5 hacker yang telah melakukan submission setiap hari, maka `UNIQUE_HACKER_COUNT` akan bernilai `5`.

---

### **3. CTE: `MAXIMUM_SUB`**
```sql
MAXIMUM_SUB AS (
    SELECT HACKER_ID, SUBMISSION_DATE 
    FROM (
        SELECT HACKER_ID, SUBMISSION_DATE, ROW_NUMBER() OVER (PARTITION BY SUBMISSION_DATE ORDER BY TOTAL_SUB DESC, HACKER_ID) AS RNK 
        FROM (
            SELECT HACKER_ID, SUBMISSION_DATE, COUNT(*) AS TOTAL_SUB 
            FROM SUBMISSIONS 
            GROUP BY HACKER_ID, SUBMISSION_DATE
        ) A
    ) B 
    WHERE RNK = 1
)
```

#### **Tujuan**:
- Mengidentifikasi **hacker dengan jumlah submission terbanyak** setiap hari.

#### **Cara Kerja**:
- Subquery `A`:
  ```sql
  SELECT HACKER_ID, SUBMISSION_DATE, COUNT(*) AS TOTAL_SUB 
  FROM SUBMISSIONS 
  GROUP BY HACKER_ID, SUBMISSION_DATE
  ```
  - Menghitung total submission (`COUNT(*)`) untuk setiap hacker pada setiap tanggal.

- Subquery `B`:
  ```sql
  SELECT HACKER_ID, SUBMISSION_DATE, ROW_NUMBER() OVER (PARTITION BY SUBMISSION_DATE ORDER BY TOTAL_SUB DESC, HACKER_ID) AS RNK 
  FROM A
  ```
  - Mengurutkan hacker berdasarkan `TOTAL_SUB` (dari tertinggi ke terendah) untuk setiap tanggal.
  - Jika ada hacker dengan jumlah submission yang sama, diurutkan berdasarkan `HACKER_ID`.

- **Kondisi WHERE**:
  - Memilih hacker dengan `RNK = 1`, yaitu hacker dengan jumlah submission terbanyak pada tanggal tersebut.

#### **Contoh**:
- Jika pada `2023-10-03`, hacker `456` memiliki 10 submission (terbanyak), maka hacker `456` akan dipilih.

---

### **4. Query Utama**
```sql
SELECT 
    U.SUBMISSION_DATE AS SUBMISSION_DATE, 
    U.UNIQUE_HACKER_COUNT, 
    M.HACKER_ID, 
    H.NAME 
FROM UNIQUE_HACKERS U 
INNER JOIN MAXIMUM_SUB M ON U.SUBMISSION_DATE = M.SUBMISSION_DATE 
INNER JOIN HACKERS H ON M.HACKER_ID = H.HACKER_ID 
ORDER BY U.SUBMISSION_DATE;
```

#### **Tujuan**:
- Menggabungkan hasil dari `UNIQUE_HACKERS` dan `MAXIMUM_SUB` untuk menghasilkan laporan akhir.

#### **Cara Kerja**:
- **Join**:
  - `UNIQUE_HACKERS` dan `MAXIMUM_SUB` di-join berdasarkan `SUBMISSION_DATE`.
  - `MAXIMUM_SUB` dan `HACKERS` di-join berdasarkan `HACKER_ID` untuk mendapatkan nama hacker.

- **Output**:
  - `SUBMISSION_DATE`: Tanggal submission.
  - `UNIQUE_HACKER_COUNT`: Jumlah hacker unik yang melakukan submission setiap hari.
  - `HACKER_ID`: ID hacker dengan submission terbanyak.
  - `NAME`: Nama hacker dengan submission terbanyak.

- **Order By**:
  - Hasil diurutkan berdasarkan `SUBMISSION_DATE`.

#### **Contoh Output**:
| SUBMISSION_DATE | UNIQUE_HACKER_COUNT | HACKER_ID | NAME     |
|-----------------|---------------------|-----------|----------|
| 2023-10-01      | 10                  | 123       | Alice    |
| 2023-10-02      | 15                  | 456       | Bob      |
| 2023-10-03      | 20                  | 789       | Charlie  |

---

### **Kesimpulan**
Kode ini:
1. Mengidentifikasi hacker yang telah melakukan submission setiap hari (`CTE`).
2. Menghitung jumlah hacker unik setiap hari (`UNIQUE_HACKERS`).
3. Menemukan hacker dengan submission terbanyak setiap hari (`MAXIMUM_SUB`).
4. Menggabungkan hasilnya untuk menghasilkan laporan akhir.
