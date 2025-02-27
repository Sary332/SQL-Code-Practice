
### 1. **Apa Itu Klausa `HAVING`?**
Klausa `HAVING` digunakan untuk memfilter hasil query yang melibatkan fungsi agregasi (seperti `SUM`, `COUNT`, `AVG`, dll.) dan pengelompokan (`GROUP BY`). Ini memungkinkan Anda untuk menyaring grup data berdasarkan kondisi tertentu.

---

### 2. **Aturan Dasar `HAVING`**
Berikut adalah aturan dasar penggunaan klausa `HAVING`:
1. **Harus Digunakan dengan `GROUP BY`**:  
   Klausa `HAVING` biasanya digunakan bersama dengan `GROUP BY`. Jika tidak ada `GROUP BY`, `HAVING` akan berlaku untuk seluruh hasil query.

2. **Bekerja pada Hasil Agregasi**:  
   `HAVING` digunakan untuk memfilter hasil agregasi, seperti `SUM`, `COUNT`, `AVG`, dll.

3. **Bisa Menggunakan Fungsi Agregasi**:  
   Anda bisa menggunakan fungsi agregasi dalam kondisi `HAVING`.

4. **Urutan dalam Query**:  
   Klausa `HAVING` harus ditempatkan setelah `GROUP BY` dan sebelum `ORDER BY`.

5. **Bisa Menggunakan Operator Logika**:  
   Anda bisa menggunakan operator logika seperti `AND`, `OR`, dan `NOT` dalam klausa `HAVING`.

---

### 3. **Fleksibilitas `HAVING`**
Klausa `HAVING` sangat fleksibel dan bisa digunakan dalam berbagai skenario. Berikut adalah contoh fleksibilitasnya:

#### a. **Memfilter Hasil Agregasi**
Misalkan Anda memiliki tabel `Orders` dengan data berikut:

| OrderID | CustomerID | Amount |
|---------|------------|--------|
| 1       | 101        | 100    |
| 2       | 102        | 200    |
| 3       | 101        | 150    |
| 4       | 103        | 300    |
| 5       | 102        | 250    |

#### Query:
```sql
SELECT CustomerID, SUM(Amount) AS TotalAmount
FROM Orders
GROUP BY CustomerID
HAVING SUM(Amount) > 300;
```

#### Hasil:
| CustomerID | TotalAmount |
|------------|-------------|
| 102        | 450         |
| 103        | 300         |

#### Penjelasan:
- Query ini mengelompokkan data berdasarkan `CustomerID` dan menghitung total `Amount` untuk setiap pelanggan.
- Klausa `HAVING` memfilter hasil agregasi, sehingga hanya menampilkan pelanggan dengan `TotalAmount` lebih dari 300.

---

#### b. **Menggunakan Fungsi Agregasi dalam `HAVING`**
Anda bisa menggunakan fungsi agregasi seperti `COUNT`, `AVG`, `MIN`, `MAX`, dll. dalam klausa `HAVING`.

#### Query:
```sql
SELECT CustomerID, COUNT(OrderID) AS OrderCount
FROM Orders
GROUP BY CustomerID
HAVING COUNT(OrderID) > 1;
```

#### Hasil:
| CustomerID | OrderCount |
|------------|------------|
| 101        | 2          |
| 102        | 2          |

#### Penjelasan:
- Query ini mengelompokkan data berdasarkan `CustomerID` dan menghitung jumlah pesanan (`OrderCount`) untuk setiap pelanggan.
- Klausa `HAVING` memfilter hasil agregasi, sehingga hanya menampilkan pelanggan dengan lebih dari 1 pesanan.

---

#### c. **Menggunakan Operator Logika dalam `HAVING`**
Anda bisa menggabungkan beberapa kondisi menggunakan operator logika seperti `AND`, `OR`, dan `NOT`.

#### Query:
```sql
SELECT CustomerID, SUM(Amount) AS TotalAmount
FROM Orders
GROUP BY CustomerID
HAVING SUM(Amount) > 200 AND COUNT(OrderID) > 1;
```

#### Hasil:
| CustomerID | TotalAmount |
|------------|-------------|
| 102        | 450         |

#### Penjelasan:
- Query ini mengelompokkan data berdasarkan `CustomerID` dan menghitung total `Amount` untuk setiap pelanggan.
- Klausa `HAVING` memfilter hasil agregasi, sehingga hanya menampilkan pelanggan dengan `TotalAmount` lebih dari 200 **dan** lebih dari 1 pesanan.

---

#### d. **Menggunakan `HAVING` Tanpa `GROUP BY`**
Jika tidak ada `GROUP BY`, `HAVING` akan berlaku untuk seluruh hasil query.

#### Query:
```sql
SELECT SUM(Amount) AS TotalAmount
FROM Orders
HAVING SUM(Amount) > 500;
```

#### Hasil:
| TotalAmount |
|-------------|
| 1000        |

#### Penjelasan:
- Query ini menghitung total `Amount` dari semua pesanan.
- Klausa `HAVING` memfilter hasil agregasi, sehingga hanya menampilkan hasil jika `TotalAmount` lebih dari 500.

---

### 4. **Perbedaan `HAVING` dan `WHERE`**
| **Aspek**              | **`HAVING`**                          | **`WHERE`**                          |
|-------------------------|--------------------------------------|--------------------------------------|
| **Penggunaan**          | Digunakan untuk memfilter hasil agregasi. | Digunakan untuk memfilter baris individu. |
| **Fungsi Agregasi**     | Bisa menggunakan fungsi agregasi.    | Tidak bisa menggunakan fungsi agregasi. |
| **Urutan Eksekusi**     | Dieksekusi setelah `GROUP BY`.       | Dieksekusi sebelum `GROUP BY`.       |

---

### 5. **Contoh Kasus Fleksibilitas `HAVING`**
#### a. **Menampilkan Rata-Rata di Atas Nilai Tertentu**
```sql
SELECT Department, AVG(Salary) AS AvgSalary
FROM Employees
GROUP BY Department
HAVING AVG(Salary) > 5000;
```

#### b. **Menampilkan Jumlah Pesanan per Pelanggan**
```sql
SELECT CustomerID, COUNT(OrderID) AS OrderCount
FROM Orders
GROUP BY CustomerID
HAVING COUNT(OrderID) >= 3;
```

#### c. **Menggunakan `HAVING` dengan `ORDER BY`**
```sql
SELECT CustomerID, SUM(Amount) AS TotalAmount
FROM Orders
GROUP BY CustomerID
HAVING SUM(Amount) > 200
ORDER BY TotalAmount DESC;
```

---

### 6. **Tips Menggunakan `HAVING`**
1. **Gunakan untuk Hasil Agregasi**:  
   Pastikan Anda menggunakan `HAVING` hanya ketika perlu memfilter hasil agregasi.

2. **Hindari Penggunaan yang Tidak Perlu**:  
   Jika Anda hanya perlu memfilter baris individu, gunakan `WHERE` saja.

3. **Perhatikan Urutan Klausa**:  
   Pastikan `HAVING` ditempatkan setelah `GROUP BY` dan sebelum `ORDER BY`.

4. **Gunakan Operator Logika dengan Bijak**:  
   Kombinasikan kondisi dengan operator logika (`AND`, `OR`, `NOT`) untuk hasil yang lebih spesifik.

