**Bisa menggunakan subquery di klausa `HAVING`**! Subquery (query di dalam query) dapat digunakan dalam klausa `HAVING` untuk membuat kondisi yang lebih kompleks atau dinamis. Ini sangat berguna ketika Anda perlu memfilter hasil agregasi berdasarkan hasil dari query lain.

---

### 1. **Contoh Penggunaan Subquery di `HAVING`**
Misalkan Anda memiliki dua tabel:
- **Tabel `Orders`**:
  | OrderID | CustomerID | Amount |
  |---------|------------|--------|
  | 1       | 101        | 100    |
  | 2       | 102        | 200    |
  | 3       | 101        | 150    |
  | 4       | 103        | 300    |
  | 5       | 102        | 250    |

- **Tabel `Customers`**:
  | CustomerID | CustomerName | Country |
  |------------|--------------|---------|
  | 101        | Andi         | USA     |
  | 102        | Budi         | UK      |
  | 103        | Cici         | USA     |

#### Query:
Anda ingin menampilkan pelanggan yang total pembeliannya (`Amount`) lebih besar dari rata-rata total pembelian semua pelanggan.

```sql
SELECT CustomerID, SUM(Amount) AS TotalAmount
FROM Orders
GROUP BY CustomerID
HAVING SUM(Amount) > (SELECT AVG(TotalAmount) 
                      FROM (SELECT SUM(Amount) AS TotalAmount
                            FROM Orders
                            GROUP BY CustomerID) AS AvgTable);
```

#### Hasil:
| CustomerID | TotalAmount |
|------------|-------------|
| 102        | 450         |
| 103        | 300         |

#### Penjelasan:
1. **Subquery di `HAVING`**:
   - Subquery dalam `HAVING` menghitung rata-rata (`AVG`) dari total pembelian (`TotalAmount`) semua pelanggan.
   - Subquery ini menghasilkan satu nilai, yaitu rata-rata total pembelian.

2. **Kondisi `HAVING`**:
   - Klausa `HAVING` membandingkan total pembelian setiap pelanggan (`SUM(Amount)`) dengan rata-rata total pembelian yang dihasilkan oleh subquery.
   - Hanya pelanggan dengan total pembelian lebih besar dari rata-rata yang akan ditampilkan.

---

### 2. **Contoh Lain: Menggunakan Subquery dengan Tabel Lain**
Misalkan Anda ingin menampilkan pelanggan yang total pembeliannya lebih besar dari rata-rata total pembelian pelanggan dari negara tertentu (misalnya, USA).

#### Query:
```sql
SELECT CustomerID, SUM(Amount) AS TotalAmount
FROM Orders
GROUP BY CustomerID
HAVING SUM(Amount) > (SELECT AVG(TotalAmount) 
                      FROM (SELECT SUM(Amount) AS TotalAmount
                            FROM Orders O
                            JOIN Customers C ON O.CustomerID = C.CustomerID
                            WHERE C.Country = 'USA'
                            GROUP BY O.CustomerID) AS AvgTable);
```

#### Hasil:
| CustomerID | TotalAmount |
|------------|-------------|
| 102        | 450         |
| 103        | 300         |

#### Penjelasan:
1. **Subquery di `HAVING`**:
   - Subquery ini menghitung rata-rata total pembelian (`TotalAmount`) untuk pelanggan dari negara USA.
   - Subquery ini menghasilkan satu nilai, yaitu rata-rata total pembelian pelanggan USA.

2. **Kondisi `HAVING`**:
   - Klausa `HAVING` membandingkan total pembelian setiap pelanggan (`SUM(Amount)`) dengan rata-rata total pembelian pelanggan USA.
   - Hanya pelanggan dengan total pembelian lebih besar dari rata-rata USA yang akan ditampilkan.

---

### 3. **Aturan Penggunaan Subquery di `HAVING`**
1. **Subquery Harus Mengembalikan Satu Nilai**:  
   Subquery yang digunakan di `HAVING` harus mengembalikan satu nilai (skalar). Jika subquery mengembalikan banyak baris atau kolom, query akan gagal.

2. **Subquery Bisa Menggunakan Tabel yang Sama atau Berbeda**:  
   Subquery bisa mengacu ke tabel yang sama dengan query utama atau ke tabel yang berbeda.

3. **Subquery Bisa Menggunakan Fungsi Agregasi**:  
   Subquery di `HAVING` sering menggunakan fungsi agregasi seperti `SUM`, `AVG`, `COUNT`, dll.

4. **Subquery Bisa Digabungkan dengan Operator Lain**:  
   Anda bisa menggabungkan subquery dengan operator seperti `>`, `<`, `=`, `IN`, dll.

---

### 4. **Contoh Kasus Lain**
#### a. **Menampilkan Pelanggan dengan Total Pembelian Tertinggi**
```sql
SELECT CustomerID, SUM(Amount) AS TotalAmount
FROM Orders
GROUP BY CustomerID
HAVING SUM(Amount) = (SELECT MAX(TotalAmount) 
                      FROM (SELECT SUM(Amount) AS TotalAmount
                            FROM Orders
                            GROUP BY CustomerID) AS MaxTable);
```

#### b. **Menampilkan Departemen dengan Rata-Rata Gaji di Atas Rata-Rata Perusahaan**
```sql
SELECT Department, AVG(Salary) AS AvgSalary
FROM Employees
GROUP BY Department
HAVING AVG(Salary) > (SELECT AVG(Salary) FROM Employees);
```

---

### 5. **Tips Menggunakan Subquery di `HAVING`**
1. **Pastikan Subquery Mengembalikan Satu Nilai**:  
   Subquery di `HAVING` harus mengembalikan satu nilai (skalar). Jika tidak, query akan error.

2. **Optimalkan Performa**:  
   Subquery bisa memengaruhi performa query, terutama jika data besar. Pastikan subquery dioptimalkan dengan baik.

3. **Gunakan Alias untuk Subquery**:  
   Berikan alias pada subquery untuk memudahkan pembacaan dan pemeliharaan kode.

4. **Hindari Subquery yang Terlalu Kompleks**:  
   Jika subquery terlalu kompleks, pertimbangkan untuk memecahnya menjadi beberapa query atau menggunakan prosedur tersimpan.

