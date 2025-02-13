## QUESTION :

Samantha interviews many candidates from different colleges using coding challenges and contests. Write a query to print the contest_id, hacker_id, name, and the sums of total_submissions, total_accepted_submissions, total_views, and total_unique_views for each contest sorted by contest_id. Exclude the contest from the result if all four sums are .

Note: A specific contest can be used to screen candidates at more than one college, but each college only holds  screening contest.

----

**Input Format :**

The following tables hold interview data:

- Contests: The contest_id is the id of the contest, hacker_id is the id of the hacker who created the contest, and name is the name of the hacker.

  <img width="179" alt="image" src="https://github.com/user-attachments/assets/c5a370db-e86f-4427-b94c-be31563f44e6" />  <img width="271" alt="image" src="https://github.com/user-attachments/assets/3d50f34c-3d1d-4c39-8acc-2d1ed08591bf" />


- Colleges: The college_id is the id of the college, and contest_id is the id of the contest that Samantha used to screen the candidates.

  <img width="182" alt="image" src="https://github.com/user-attachments/assets/2394e85f-a677-4b86-964a-0ec0b4c7b635" />  <img width="268" alt="image" src="https://github.com/user-attachments/assets/8aef288e-9261-421b-b7bb-4179205f0603" />


- Challenges: The challenge_id is the id of the challenge that belongs to one of the contests whose contest_id Samantha forgot, and college_id is the id of the college where the challenge was given to candidates.

  <img width="179" alt="image" src="https://github.com/user-attachments/assets/aae3a1e3-e818-4430-a48c-46522caff4b0" />  <img width="269" alt="image" src="https://github.com/user-attachments/assets/14259ed3-afff-455b-9e5c-b4b7af28478d" />


-View_Stats: The challenge_id is the id of the challenge, total_views is the number of times the challenge was viewed by candidates, and total_unique_views is the number of times the challenge was viewed by unique candidates.

<img width="179" alt="image" src="https://github.com/user-attachments/assets/6f82c8fe-7716-4911-b49a-977ce389c1a6" />  <img width="270" alt="image" src="https://github.com/user-attachments/assets/a550891c-2054-475a-8a2b-8583a10c3ab1" />


- Submission_Stats: The challenge_id is the id of the challenge, total_submissions is the number of submissions for the challenge, and total_accepted_submission is the number of submissions that achieved full scores.

  <img width="179" alt="image" src="https://github.com/user-attachments/assets/61e4b0fd-52ee-4ae6-b072-e9f6b618c5d2" />  <img width="271" alt="image" src="https://github.com/user-attachments/assets/251bb0fc-a730-4a62-910b-28ea9803f0c8" />


**Explanation :**

The contest 66406 is used 11219 in the college . In this college 11219, challenges 18765  and 47127 are asked, so from the view and submission stats:

- Sum of total submissions = 27 + 56 + 28 = 111 

- Sum of total accepted submissions = 10 + 18 + 11 = 39

- Sum of total views = 43 + 72 + 26 + 15 = 156

- Sum of total unique views 10 + 13 + 19 + 14 = 56

- Similarly, we can find the sums for contests 66556 and 94828 .

## SOLUTION :

```sql
SELECT CON.CONTEST_ID, 
       CON.HACKER_ID,
       CON.NAME,
       ISNULL(SUM(S.TOTAL_SUBMISSIONS),0) AS TOTAL_SUBMISSIONS,
       ISNULL(SUM(S.TOTAL_ACCEPTED_SUBMISSIONS),0) AS TOTAL_ACCEPTED_SUBMISSIONS ,
       ISNULL(SUM(V.TOTAL_VIEWS),0) AS TOTAL_VIEWS,
       ISNULL(SUM(V.TOTAL_UNIQUE_VIEWS),0) AS TOTAL_UNIQUE_VIEWS
FROM CONTESTS CON
JOIN 
         COLLEGES COL ON CON.CONTEST_ID = COL.CONTEST_ID
LEFT JOIN 
         CHALLENGES CHA ON COL.COLLEGE_ID = CHA.COLLEGE_ID
LEFT JOIN 
         (SELECT CHALLENGE_ID,
                 SUM(TOTAL_VIEWS) AS TOTAL_VIEWS,
                 SUM(TOTAL_UNIQUE_VIEWS) AS TOTAL_UNIQUE_VIEWS
          FROM VIEW_STATS
          GROUP BY CHALLENGE_ID
         ) AS V ON CHA.CHALLENGE_ID = V.CHALLENGE_ID
LEFT JOIN 
         (SELECT CHALLENGE_ID,
                 SUM(TOTAL_SUBMISSIONS) AS TOTAL_SUBMISSIONS,
                 SUM(TOTAL_ACCEPTED_SUBMISSIONS) AS TOTAL_ACCEPTED_SUBMISSIONS
          FROM SUBMISSION_STATS
          GROUP BY CHALLENGE_ID
          ) S ON CHA.CHALLENGE_ID = S.CHALLENGE_ID
GROUP BY CON.CONTEST_ID, 
         CON.HACKER_ID,
         CON.NAME
HAVING ISNULL(SUM(S.TOTAL_SUBMISSIONS),0) 
     + ISNULL(SUM(S.TOTAL_ACCEPTED_SUBMISSIONS),0) 
     + ISNULL(SUM(V.TOTAL_VIEWS),0) 
     + ISNULL(SUM(V.TOTAL_UNIQUE_VIEWS),0) > 0
ORDER BY CON.CONTEST_ID
```

<img width="457" alt="image" src="https://github.com/user-attachments/assets/598bd1cc-cb42-499d-9cd5-db502dba8a72" />


### NOTES :
**1. Why i cant just do a simple joins ?? bcuz :**
"To anyone who is wondering why simple inner join and performing sum() using group by does not work: One of the reason is, the set of challenge_ids in View_Stats does not equal to the set of challenge_ids in Submission_Stats. Therefore, using simple inner join to join stats data with the contest table will lead to missing records. It's not your fault."

" The real issue was the duplicates in the stats tables (view_stats and submission_stats)."

===

2. Alasan utama mengapa subquery digunakan untuk tabel `View_Stats` (`V`) dan `Submission_Stats` (`S`) dalam query tersebut adalah untuk **mengoptimalkan performa** dan **memastikan agregasi data yang benar**. Berikut adalah penjelasan detailnya:

#### **1. Mengoptimalkan Performa**
- **Tanpa Subquery**:
  Jika kita langsung melakukan `LEFT JOIN` dengan tabel `View_Stats` dan `Submission_Stats` tanpa subquery, maka database akan melakukan join pada setiap baris di tabel `View_Stats` dan `Submission_Stats` yang sesuai dengan `challenge_id`.
  - Ini bisa menghasilkan banyak baris sementara (intermediate rows) yang tidak diperlukan, terutama jika ada banyak data di `View_Stats` dan `Submission_Stats`.
  - Proses join akan lebih lambat karena database harus memproses lebih banyak baris.

- **Dengan Subquery**:
  Dengan menggunakan subquery, kita **mengelompokkan dan mengagregasi data terlebih dahulu** di `View_Stats` dan `Submission_Stats` sebelum melakukan join.
  - Subquery mengurangi jumlah baris yang perlu di-join karena data sudah diagregasi (dijumlahkan) per `challenge_id`.
  - Ini membuat proses join lebih cepat dan efisien.

**Contoh**:
- Jika `View_Stats` memiliki 1000 baris untuk 100 `challenge_id`, subquery akan mengelompokkannya menjadi 100 baris (satu baris per `challenge_id`).
- Tanpa subquery, database harus memproses 1000 baris saat join.

---

#### **2. Memastikan Agregasi Data yang Benar**
- **Tanpa Subquery**:
  Jika kita langsung melakukan join dengan `View_Stats` dan `Submission_Stats`, maka saat melakukan agregasi (misalnya `SUM`), kita bisa mendapatkan hasil yang salah karena duplikasi data.
  - Misalnya, jika satu `challenge_id` memiliki beberapa baris di `View_Stats` atau `Submission_Stats`, join langsung akan menghasilkan beberapa baris untuk `challenge_id` yang sama.
  - Saat melakukan `SUM`, nilai tersebut akan dihitung berkali-kali, yang menyebabkan hasil agregasi tidak akurat.

- **Dengan Subquery**:
  Subquery memastikan bahwa data di `View_Stats` dan `Submission_Stats` sudah diagregasi per `challenge_id` sebelum di-join.
  - Ini menghindari duplikasi data dan memastikan hasil agregasi (seperti `SUM`) akurat.

**Contoh**:
- Jika `View_Stats` memiliki data berikut:
  | challenge_id | total_views | total_unique_views |
  |--------------|-------------|--------------------|
  | 1            | 100         | 50                 |
  | 1            | 200         | 100                |
  - Tanpa subquery, join akan menghasilkan dua baris untuk `challenge_id = 1`.
  - Dengan subquery, data akan diagregasi terlebih dahulu menjadi:
    | challenge_id | total_views | total_unique_views |
    |--------------|-------------|--------------------|
    | 1            | 300         | 150                |
  - Ini memastikan bahwa `SUM` dihitung dengan benar.

---

#### **3. Menghindari Masalah NULL**
- **Tanpa Subquery**:
  Jika kita langsung melakukan join dengan `View_Stats` dan `Submission_Stats`, dan ada `challenge_id` yang tidak memiliki data di salah satu tabel tersebut, maka hasil join akan mengandung `NULL`.
  - Ini bisa menyebabkan masalah saat melakukan agregasi, karena `NULL` akan diabaikan oleh fungsi agregasi seperti `SUM`.

- **Dengan Subquery**:
  Subquery memastikan bahwa setiap `challenge_id` memiliki baris di hasil subquery, bahkan jika tidak ada data di `View_Stats` atau `Submission_Stats`.
  - Jika tidak ada data, subquery akan mengembalikan `NULL` untuk kolom yang diagregasi, yang kemudian dihandle oleh `ISNULL` atau `COALESCE`.

---

#### **4. Memisahkan Logika Agregasi**
- **Dengan Subquery**:
  Subquery memisahkan logika agregasi (seperti `SUM`) dari logika join.
  - Ini membuat query lebih modular dan mudah dipahami.
  - Jika ada perubahan pada logika agregasi (misalnya, menambahkan kolom baru), kita hanya perlu mengubah subquery, bukan seluruh query.

- **Tanpa Subquery**:
  Logika agregasi dan join dicampur dalam satu query, yang bisa membuat query lebih sulit dibaca dan dipelihara.

---

#### **Kesimpulan**
Penggunaan subquery untuk `View_Stats` (`V`) dan `Submission_Stats` (`S`) dalam query tersebut memiliki beberapa keuntungan:
1. **Optimasi performa**: Mengurangi jumlah baris yang perlu di-join.
2. **Akurasi agregasi**: Menghindari duplikasi data dan memastikan hasil agregasi yang benar.
3. **Penanganan NULL**: Memastikan bahwa `NULL` dihandle dengan benar.
4. **Modularitas**: Memisahkan logika agregasi dari logika join, membuat query lebih mudah dipahami dan dipelihara.
