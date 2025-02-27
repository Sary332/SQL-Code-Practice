### 1. **Apa Itu `UNION`?**
`UNION` adalah operator SQL yang digunakan untuk menggabungkan hasil dari dua atau lebih query `SELECT` menjadi satu set hasil. Hasil gabungan ini akan **menghapus duplikat** secara default (kecuali jika menggunakan `UNION ALL`).

---

### 2. **Aturan Dasar `UNION`**
Berikut adalah aturan dasar penggunaan `UNION`:
1. **Jumlah Kolom Harus Sama**:  
   Setiap query `SELECT` yang digabungkan dengan `UNION` harus memiliki jumlah kolom yang sama.

2. **Tipe Data Kolom Harus Kompatibel**:  
   Tipe data dari kolom yang sesuai (berdasarkan urutan) harus kompatibel. Misalnya, Anda tidak bisa menggabungkan kolom bertipe `INT` dengan kolom bertipe `VARCHAR`.

3. **Nama Kolom**:  
   Nama kolom di hasil akhir akan mengikuti nama kolom dari query `SELECT` pertama.

4. **Menghapus Duplikat**:  
   Secara default, `UNION` akan menghapus baris yang duplikat dari hasil gabungan. Jika Anda ingin mempertahankan duplikat, gunakan `UNION ALL`.

5. **Urutan Kolom**:  
   Kolom di setiap query `SELECT` harus berada dalam urutan yang sama.

---

### 3. **Fleksibilitas `UNION`**
`UNION` sangat fleksibel dan bisa digunakan dalam berbagai skenario. Berikut adalah contoh fleksibilitasnya:

#### a. **Menggabungkan Data dari Dua Tabel**
Misalkan Anda memiliki dua tabel:
- **Tabel A**:
  | ID  | Nama  |
  |-----|-------|
  | 1   | Andi  |
  | 2   | Budi  |

- **Tabel B**:
  | ID  | Nama  |
  |-----|-------|
  | 3   | Cici  |
  | 4   | Dedi  |

#### Query:
```sql
SELECT Nama FROM TabelA
UNION
SELECT Nama FROM TabelB;
```

#### Hasil:
| Nama  |
|-------|
| Andi  |
| Budi  |
| Cici  |
| Dedi  |

---

#### b. **Menggabungkan Data dengan `UNION ALL`**
Jika Anda ingin mempertahankan duplikat, gunakan `UNION ALL`.

#### Query:
```sql
SELECT Nama FROM TabelA
UNION ALL
SELECT Nama FROM TabelB;
```

#### Hasil:
| Nama  |
|-------|
| Andi  |
| Budi  |
| Cici  |
| Dedi  |

---

#### c. **Menggabungkan Data dengan Kolom Berbeda**
Anda bisa menggabungkan data dari kolom yang berbeda asalkan jumlah dan tipe datanya kompatibel.

#### Query:
```sql
SELECT Nama FROM TabelA
UNION
SELECT Alamat FROM TabelB;
```

#### Hasil:
| Nama  |
|-------|
| Andi  |
| Budi  |
| Cici  |
| Dedi  |

---

#### d. **Menggabungkan Data dengan Kondisi Berbeda**
Anda bisa menggabungkan data dari query yang memiliki kondisi `WHERE` berbeda.

#### Query:
```sql
SELECT Nama FROM TabelA WHERE ID < 3
UNION
SELECT Nama FROM TabelB WHERE ID > 2;
```

#### Hasil:
| Nama  |
|-------|
| Andi  |
| Budi  |
| Cici  |
| Dedi  |

---

#### e. **Menggabungkan Data dengan `ORDER BY`**
Anda bisa mengurutkan hasil gabungan dengan `ORDER BY`. Pastikan `ORDER BY` diletakkan di akhir query.

#### Query:
```sql
SELECT Nama FROM TabelA
UNION
SELECT Nama FROM TabelB
ORDER BY Nama ASC;
```

#### Hasil:
| Nama  |
|-------|
| Andi  |
| Budi  |
| Cici  |
| Dedi  |

---

### 4. **Contoh Kasus Fleksibilitas `UNION`**
#### a. **Menggabungkan Data dari Tabel yang Sama dengan Kondisi Berbeda**
```sql
SELECT Nama FROM Employees WHERE Department = 'IT'
UNION
SELECT Nama FROM Employees WHERE Department = 'HR';
```

#### b. **Menggabungkan Data dari Tabel yang Berbeda**
```sql
SELECT Nama FROM Students
UNION
SELECT Nama FROM Teachers;
```

#### c. **Menggabungkan Data dengan `UNION ALL` untuk Mempertahankan Duplikat**
```sql
SELECT Nama FROM Customers
UNION ALL
SELECT Nama FROM Suppliers;
```

---

### 5. **Perbedaan `UNION` dan `UNION ALL`**
| **Aspek**              | **`UNION`**                          | **`UNION ALL`**                          |
|-------------------------|--------------------------------------|------------------------------------------|
| **Duplikat**            | Menghapus baris yang duplikat.       | Mempertahankan baris yang duplikat.      |
| **Performa**            | Lebih lambat karena menghapus duplikat. | Lebih cepat karena tidak menghapus duplikat. |
| **Penggunaan**          | Digunakan ketika duplikat tidak diinginkan. | Digunakan ketika duplikat dipertahankan. |

---

### 6. **Tips Menggunakan `UNION`**
1. **Pastikan Jumlah Kolom Sama**:  
   Setiap query `SELECT` yang digabungkan harus memiliki jumlah kolom yang sama.

2. **Perhatikan Tipe Data**:  
   Pastikan tipe data kolom yang digabungkan kompatibel.

3. **Gunakan `UNION ALL` untuk Performa Lebih Cepat**:  
   Jika Anda tidak perlu menghapus duplikat, gunakan `UNION ALL` untuk meningkatkan performa.

4. **Hindari Penggunaan yang Tidak Perlu**:  
   Jika data bisa diambil dengan satu query, hindari menggunakan `UNION`.

