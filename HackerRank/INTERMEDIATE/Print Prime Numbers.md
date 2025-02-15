## QUESTION :

Write a query to print all prime numbers less than or equal to 1000. Print your result on a single line, and use the ampersand (&)
character as your separator (instead of a space).

## SOLUTION :
```SQL
WITH Numbers AS (
    SELECT 2 AS num
    UNION ALL
    SELECT num + 1 FROM Numbers WHERE num < 1000
)
SELECT STRING_AGG(CAST(n1.num AS VARCHAR), '&') AS PrimeNumbers
FROM Numbers n1
WHERE NOT EXISTS (
    SELECT 1 FROM Numbers n2
    WHERE n2.num < n1.num AND n2.num > 1
    AND n1.num % n2.num = 0
)
OPTION (MAXRECURSION 1000);
```
<img width="485" alt="image" src="https://github.com/user-attachments/assets/e54b6e49-b338-4ce1-9eae-3c2eb97ffaa0" />


## NOTES :
Query ini digunakan untuk **menghasilkan bilangan prima** dari 2 hingga 1000 dan menggabungkannya dalam satu string, dipisahkan dengan karakter `&`.  

Mari kita pahami **bagian per bagian**:  

---

## **1️⃣ CTE (Common Table Expression) untuk Membuat Angka 2 hingga 1000**
```sql
WITH Numbers AS (
    SELECT 2 AS num
    UNION ALL
    SELECT num + 1 FROM Numbers WHERE num < 1000
)
```
👉 **Apa yang dilakukan bagian ini?**  
- Membuat tabel sementara bernama `Numbers` yang berisi angka dari **2 hingga 1000** secara **rekursif**.  
- **`SELECT 2 AS num`** → Mulai dari angka 2.  
- **`UNION ALL SELECT num + 1 FROM Numbers WHERE num < 1000`**  
  - Secara rekursif menambahkan angka **num + 1** hingga mencapai 1000.  
  - Artinya, tabel ini akan berisi angka: **2, 3, 4, ..., 1000**.  

**🔹 Contoh hasil sementara `Numbers`**
| num |
|----|
| 2  |
| 3  |
| 4  |
| 5  |
| ... |
| 1000 |

---

## **2️⃣ Memilih Bilangan Prima dengan `WHERE NOT EXISTS`**
```sql
WHERE NOT EXISTS (
    SELECT 1 FROM Numbers n2
    WHERE n2.num < n1.num AND n2.num > 1
    AND n1.num % n2.num = 0
)
```
👉 **Apa yang dilakukan bagian ini?**  
- Mengecek apakah **n1.num** adalah bilangan prima.  
- **Konsep Bilangan Prima:**  
  - Bilangan prima hanya bisa dibagi oleh 1 dan dirinya sendiri.  
  - Jika suatu angka bisa dibagi oleh angka lain **(lebih kecil dari dirinya sendiri dan lebih besar dari 1)**, berarti
    angka itu **bukan bilangan prima**.  
- **`NOT EXISTS`** → Menghapus angka yang bisa dibagi oleh angka lain selain 1 dan dirinya sendiri.  
- Dengan ini, hanya **bilangan prima** yang tersisa!  

**🔹 Contoh data setelah filter**
| num |
|----|
| 2  |
| 3  |
| 5  |
| 7  |
| 11 |
| ... |

---

## **3️⃣ Menggabungkan Semua Bilangan Prima Jadi Satu String**
```sql
SELECT STRING_AGG(CAST(n1.num AS VARCHAR), '&') AS PrimeNumbers
```
👉 **Apa yang dilakukan bagian ini?**  
- **`STRING_AGG()`** → Menggabungkan semua hasil menjadi satu string dengan pemisah `&`.  
- **`CAST(n1.num AS VARCHAR)`** → Mengubah angka menjadi teks agar bisa digabung.  

**🔹 Contoh hasil akhir**
```plaintext
2&3&5&7&11&13&17&19&23&29&...
```

---

## **4️⃣ `OPTION (MAXRECURSION 1000)`**
```sql
OPTION (MAXRECURSION 1000);
```
👉 **Apa fungsinya?**  
- **SQL Server memiliki batasan rekursi default sebesar 100.**  
- Karena kita butuh hingga **1000 angka**, kita harus menaikkan batasan rekursi menggunakan `OPTION (MAXRECURSION 1000)`.  

---

## **🎯 Kesimpulan**
### **Apa yang dilakukan query ini secara keseluruhan?**
1. **Membuat daftar angka dari 2 hingga 1000 secara rekursif.**  
2. **Menyaring hanya bilangan prima (tidak bisa dibagi oleh angka lain selain 1 dan dirinya sendiri).**  
3. **Menggabungkan bilangan prima menjadi satu string dengan pemisah `&`.**  

---

**📌 Hasil Akhir (PrimeNumbers)**
```plaintext
2&3&5&7&11&13&17&19&23&29&...
```
Hasilnya adalah daftar **bilangan prima** dari 2 sampai 1000 yang sudah digabung menjadi satu string.  

Semoga ini membantu! Kalau masih ada yang bingung, tanya saja ya. 😊
