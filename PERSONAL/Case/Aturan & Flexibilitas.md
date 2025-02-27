
### 1. **Apa Itu Ekspresi `CASE`?**
Ekspresi `CASE` digunakan untuk mengevaluasi kondisi dan mengembalikan nilai berdasarkan hasil evaluasi tersebut. Ini sangat berguna untuk:
- Mengganti nilai berdasarkan kondisi.
- Membuat kolom baru berdasarkan logika tertentu.
- Mengelompokkan data berdasarkan kriteria.

---

### 2. **Aturan Dasar Ekspresi `CASE`**
Berikut adalah aturan dasar penggunaan ekspresi `CASE`:
1. **Struktur Dasar**:
   ```sql
   CASE
       WHEN kondisi1 THEN hasil1
       WHEN kondisi2 THEN hasil2
       ...
       ELSE hasil_default
   END
   ```
   - `WHEN`: Menentukan kondisi yang akan dievaluasi.
   - `THEN`: Menentukan nilai yang akan dikembalikan jika kondisi benar.
   - `ELSE`: Menentukan nilai default jika tidak ada kondisi yang terpenuhi (opsional).
   - `END`: Menandai akhir dari ekspresi `CASE`.

2. **Harus Mengembalikan Nilai**:  
   Setiap ekspresi `CASE` harus mengembalikan nilai. Jika tidak ada kondisi yang terpenuhi dan tidak ada `ELSE`, hasilnya adalah `NULL`.

3. **Urutan Evaluasi**:  
   Kondisi dievaluasi secara berurutan dari atas ke bawah. Jika kondisi pertama terpenuhi, nilai yang sesuai akan dikembalikan, dan evaluasi berhenti.

4. **Bisa Digunakan di Mana Saja**:  
   Ekspresi `CASE` bisa digunakan di:
   - `SELECT`: Untuk membuat kolom baru berdasarkan kondisi.
   - `WHERE`: Untuk memfilter data berdasarkan kondisi.
   - `ORDER BY`: Untuk mengurutkan data berdasarkan kondisi.
   - `GROUP BY`: Untuk mengelompokkan data berdasarkan kondisi.

---

### 3. **Fleksibilitas Ekspresi `CASE`**
Ekspresi `CASE` sangat fleksibel dan bisa digunakan dalam berbagai skenario. Berikut adalah contoh fleksibilitasnya:

#### a. **Menggunakan Kondisi Sederhana**
```sql
SELECT 
    Nama,
    CASE 
        WHEN Usia < 18 THEN 'Anak-Anak'
        WHEN Usia BETWEEN 18 AND 60 THEN 'Dewasa'
        ELSE 'Lansia'
    END AS KategoriUsia
FROM Orang;
```

#### b. **Menggunakan Operator Perbandingan**
```sql
SELECT 
    Produk,
    CASE 
        WHEN Harga > 1000 THEN 'Mahal'
        WHEN Harga BETWEEN 500 AND 1000 THEN 'Sedang'
        ELSE 'Murah'
    END AS KategoriHarga
FROM Produk;
```

#### c. **Menggunakan Fungsi**
```sql
SELECT 
    Nama,
    CASE 
        WHEN LOWER(JenisKelamin) = 'laki-laki' THEN 'Pria'
        WHEN LOWER(JenisKelamin) = 'perempuan' THEN 'Wanita'
        ELSE 'Tidak Diketahui'
    END AS Gender
FROM Pelanggan;
```

#### d. **Menggunakan `CASE` di `WHERE`**
```sql
SELECT *
FROM Orders
WHERE 
    CASE 
        WHEN TotalAmount > 1000 THEN 'High'
        WHEN TotalAmount BETWEEN 500 AND 1000 THEN 'Medium'
        ELSE 'Low'
    END = 'High';
```

#### e. **Menggunakan `CASE` di `ORDER BY`**
```sql
SELECT *
FROM Employees
ORDER BY 
    CASE 
        WHEN Department = 'IT' THEN 1
        WHEN Department = 'HR' THEN 2
        ELSE 3
    END;
```

#### f. **Menggunakan `CASE` di `GROUP BY`**
```sql
SELECT 
    CASE 
        WHEN Usia < 18 THEN 'Anak-Anak'
        WHEN Usia BETWEEN 18 AND 60 THEN 'Dewasa'
        ELSE 'Lansia'
    END AS KategoriUsia,
    COUNT(*) AS Jumlah
FROM Orang
GROUP BY 
    CASE 
        WHEN Usia < 18 THEN 'Anak-Anak'
        WHEN Usia BETWEEN 18 AND 60 THEN 'Dewasa'
        ELSE 'Lansia'
    END;
```

---

### 4. **Contoh Kasus Fleksibilitas `CASE`**
#### a. **Mengelompokkan Nilai Siswa**
```sql
SELECT 
    Nama,
    Nilai,
    CASE 
        WHEN Nilai >= 90 THEN 'A'
        WHEN Nilai >= 80 THEN 'B'
        WHEN Nilai >= 70 THEN 'C'
        ELSE 'D'
    END AS Grade
FROM Siswa;
```

#### b. **Mengubah Nilai Boolean ke Teks**
```sql
SELECT 
    Nama,
    CASE 
        WHEN Aktif = 1 THEN 'Aktif'
        ELSE 'Tidak Aktif'
    END AS Status
FROM Pengguna;
```

#### c. **Menggunakan `CASE` dengan `JOIN`**
```sql
SELECT 
    O.OrderID,
    O.TotalAmount,
    CASE 
        WHEN O.TotalAmount > 1000 THEN 'High'
        WHEN O.TotalAmount BETWEEN 500 AND 1000 THEN 'Medium'
        ELSE 'Low'
    END AS OrderCategory
FROM Orders O
JOIN Customers C ON O.CustomerID = C.CustomerID;
```

---

### 5. **Tips Menggunakan Ekspresi `CASE`**
1. **Gunakan `ELSE` untuk Menghindari `NULL`**:  
   Jika tidak ada kondisi yang terpenuhi dan tidak ada `ELSE`, hasilnya adalah `NULL`. Pastikan Anda menggunakan `ELSE` jika diperlukan.

2. **Urutkan Kondisi dengan Benar**:  
   Kondisi dievaluasi dari atas ke bawah. Pastikan kondisi yang lebih spesifik diletakkan di atas.

3. **Hindari `CASE` yang Terlalu Kompleks**:  
   Jika logika terlalu kompleks, pertimbangkan untuk memecahnya menjadi beberapa query atau menggunakan prosedur tersimpan.

