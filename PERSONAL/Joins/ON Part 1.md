Klausa **`ON`** dalam **`JOIN`** bisa digunakan secara fleksibel, tidak hanya untuk kondisi sederhana seperti `F1.X = F2.Y`, tetapi juga untuk kondisi yang lebih kompleks. Mari kita bahas dua contoh yang Anda sebutkan:

---

### 1. **Contoh Pertama: `ON F1.X = F2.Y AND F1.Y = F2.X`**
Ini adalah contoh di mana klausa `ON` digunakan untuk menggabungkan dua tabel berdasarkan dua kondisi sekaligus.

#### Contoh Kasus:
Misalkan Anda memiliki dua tabel:
- **Tabel F1**:
  | X | Y |
  |---|---|
  | 1 | 2 |
  | 3 | 4 |

- **Tabel F2**:
  | X | Y |
  |---|---|
  | 2 | 1 |
  | 4 | 3 |

#### Query:
```sql
SELECT *
FROM F1
JOIN F2 ON F1.X = F2.Y AND F1.Y = F2.X;
```

#### Hasil:
| F1.X | F1.Y | F2.X | F2.Y |
|------|------|------|------|
| 1    | 2    | 2    | 1    |
| 3    | 4    | 4    | 3    |

#### Penjelasan:
- Baris di `F1` dan `F2` akan digabungkan jika:
  - Nilai `F1.X` sama dengan `F2.Y`, **dan**
  - Nilai `F1.Y` sama dengan `F2.X`.

---

### 2. **Contoh Kedua: `GRADES G ON S.MARKS BETWEEN G.MIN_MARK AND G.MAX_MARK`**
Ini adalah contoh di mana klausa `ON` digunakan untuk menggabungkan dua tabel berdasarkan range nilai.

#### Contoh Kasus:
Misalkan Anda memiliki dua tabel:
- **Tabel Students (S)**:
  | ID | MARKS |
  |----|-------|
  | 1  | 85    |
  | 2  | 95    |
  | 3  | 70    |

- **Tabel Grades (G)**:
  | GRADE | MIN_MARK | MAX_MARK |
  |-------|----------|----------|
  | A     | 90       | 100      |
  | B     | 80       | 89       |
  | C     | 70       | 79       |

#### Query:
```sql
SELECT S.ID, S.MARKS, G.GRADE
FROM Students S
JOIN Grades G ON S.MARKS BETWEEN G.MIN_MARK AND G.MAX_MARK;
```

#### Hasil:
| ID | MARKS | GRADE |
|----|-------|-------|
| 1  | 85    | B     |
| 2  | 95    | A     |
| 3  | 70    | C     |

#### Penjelasan:
- Setiap nilai `MARKS` di tabel `Students` akan dicocokkan dengan range `MIN_MARK` dan `MAX_MARK` di tabel `Grades`.
- Jika nilai `MARKS` berada dalam range tersebut, baris dari kedua tabel akan digabungkan.

---

### 3. **Kenapa Klausa `ON` Bisa Fleksibel?**
Klausa `ON` dalam `JOIN` sebenarnya adalah **kondisi boolean** (benar/salah). Anda bisa menggunakan operator apa pun yang menghasilkan nilai boolean, seperti:
- `=`, `!=`, `>`, `<`, `>=`, `<=`
- `BETWEEN ... AND ...`
- `LIKE` (untuk pencarian string)
- Kombinasi kondisi dengan `AND`, `OR`, `NOT`

---

### 4. **Contoh Lain Klausa `ON` yang Fleksibel**
#### a. Menggunakan `LIKE`:
```sql
SELECT *
FROM Customers C
JOIN Orders O ON C.CustomerName LIKE '%' + O.OrderName + '%';
```
### **Arti dari Klausa Tersebut**
1. **`C.CustomerName`**:
   - Ini merujuk ke kolom `CustomerName` di tabel `C` (biasanya tabel pelanggan).

2. **`O.OrderName`**:
   - Ini merujuk ke kolom `OrderName` di tabel `O` (biasanya tabel pesanan).

3. **`LIKE`**:
   - Operator `LIKE` digunakan untuk mencocokkan string (teks) berdasarkan pola tertentu.

4. **`'%' + O.OrderName + '%'`**:
   - `%` adalah wildcard dalam SQL yang berarti "nol atau lebih karakter".
   - `'%' + O.OrderName + '%'` berarti mencocokkan `CustomerName` yang mengandung teks dari `OrderName` di mana pun posisinya (di awal, tengah, atau akhir).

---

### **Contoh Kasus**
Misalkan:
- Tabel `C` (Customers):
  ```
  CustomerName
  -------------
  John Doe
  Jane Smith
  Alice Johnson
  ```

- Tabel `O` (Orders):
  ```
  OrderName
  ----------
  Doe
  Smith
  ```

Query:
```sql
SELECT *
FROM Customers C
JOIN Orders O
ON C.CustomerName LIKE '%' + O.OrderName + '%'
```

**Hasilnya**:
- `John Doe` akan cocok dengan `Doe`.
- `Jane Smith` akan cocok dengan `Smith`.
- `Alice Johnson` tidak akan cocok dengan apa pun.

Jadi, hasil query akan mengembalikan baris-baris di mana `CustomerName` mengandung `OrderName`.

---

### **Kapan Klausa Ini Digunakan?**
Klausa ini biasanya digunakan ketika Anda ingin mencocokkan data berdasarkan teks yang **tidak persis sama**, tetapi memiliki kemiripan. Contoh penggunaan:
1. Mencari pelanggan yang namanya mengandung kata kunci tertentu.
2. Mencocokkan data dari dua tabel berdasarkan pola teks.

---

### **Catatan Penting**
1. **Performance**:
   - Menggunakan `LIKE` dengan wildcard (`%`) di awal dan akhir (seperti `'%value%'`) bisa **lambat** karena database harus memindai seluruh teks. Ini tidak menggunakan indeks secara efisien.
   - Jika dataset besar, pertimbangkan untuk menggunakan teknik lain seperti full-text search atau preprocessing data.

2. **Case Sensitivity**:
   - `LIKE` biasanya **case-insensitive** di sebagian besar database (seperti MySQL), tetapi di beberapa database (seperti PostgreSQL), Anda mungkin perlu menggunakan `ILIKE` untuk pencarian yang tidak memperhatikan huruf besar/kecil.

3. **Wildcard Lain**:
   - `%` cocok dengan nol atau lebih karakter.
   - `_` cocok dengan satu karakter tunggal.

---

### **Contoh Lain**
Jika Anda hanya ingin mencocokkan `CustomerName` yang **dimulai** dengan `OrderName`, Anda bisa menulis:
```sql
ON C.CustomerName LIKE O.OrderName + '%'
```

Jika Anda ingin mencocokkan `CustomerName` yang **diakhiri** dengan `OrderName`, Anda bisa menulis:
```sql
ON C.CustomerName LIKE '%' + O.OrderName
```

---


#### b. Menggunakan `OR`:
```sql
SELECT *
FROM Table1 T1
JOIN Table2 T2 ON T1.X = T2.Y OR T1.Y = T2.X;
```

#### c. Menggunakan Fungsi:
```sql
SELECT *
FROM Employees E
JOIN Departments D ON LOWER(E.DepartmentName) = LOWER(D.DepartmentName);
```
### **Arti dari Klausa Tersebut**
1. **`LOWER(E.DepartmentName)`**:
   - Fungsi `LOWER` mengubah semua karakter dalam kolom `DepartmentName` dari tabel `E` menjadi huruf kecil.
   - Contoh: `"Sales"` akan menjadi `"sales"`, dan `"HR"` akan menjadi `"hr"`.

2. **`LOWER(D.DepartmentName)`**:
   - Fungsi `LOWER` juga mengubah semua karakter dalam kolom `DepartmentName` dari tabel `D` menjadi huruf kecil.

3. **`=`**:
   - Operator `=` digunakan untuk membandingkan apakah kedua nilai tersebut sama.

---

### **Mengapa Menggunakan `LOWER`?**
- **Case Sensitivity**: Beberapa database (seperti PostgreSQL) bersifat **case-sensitive**, artinya `"Sales"` dan `"sales"` dianggap sebagai dua nilai yang berbeda.
- Dengan menggunakan `LOWER`, Anda memastikan bahwa perbandingan dilakukan tanpa memperhatikan huruf besar/kecil. Ini berguna ketika data tidak konsisten dalam penulisan huruf.

---

### **Contoh Kasus**
Misalkan:
- Tabel `E` (Employees):
  ```
  DepartmentName
  --------------
  Sales
  HR
  IT
  ```

- Tabel `D` (Departments):
  ```
  DepartmentName
  --------------
  sales
  hr
  finance
  ```

Query:
```sql
SELECT *
FROM Employees E
JOIN Departments D
ON LOWER(E.DepartmentName) = LOWER(D.DepartmentName)
```

**Hasilnya**:
- `Sales` (dari `E`) akan cocok dengan `sales` (dari `D`).
- `HR` (dari `E`) akan cocok dengan `hr` (dari `D`).
- `IT` (dari `E`) tidak akan cocok dengan apa pun di tabel `D`.

---

### **Kapan Klausa Ini Digunakan?**
Klausa ini digunakan ketika:
1. Anda ingin membandingkan dua kolom teks tanpa memperhatikan huruf besar/kecil.
2. Data di kedua tabel tidak konsisten dalam penulisan huruf (misalnya, ada yang menggunakan huruf besar, ada yang tidak).

---

### **Keuntungan Menggunakan `LOWER`**
1. **Konsistensi**: Memastikan perbandingan teks tidak terpengaruh oleh perbedaan huruf besar/kecil.
2. **Fleksibilitas**: Berguna ketika data input tidak terstandarisasi.

---

### **Kekurangan Menggunakan `LOWER`**
1. **Performance**:
   - Menggunakan fungsi seperti `LOWER` pada kolom dalam klausa `JOIN` atau `WHERE` bisa **memperlambat query** karena database harus mengubah setiap nilai menjadi huruf kecil sebelum membandingkannya.
   - Jika kolom tersebut diindeks, indeks tidak akan digunakan secara efisien karena fungsi `LOWER` mengubah nilai asli.

2. **Alternatif yang Lebih Efisien**:
   - Jika database Anda mendukung, Anda bisa menggunakan **collation case-insensitive** saat membuat tabel atau kolom. Contoh:
     ```sql
     CREATE TABLE Departments (
         DepartmentName VARCHAR(100) COLLATE Latin1_General_CI_AI
     );
     ```
     - `CI` berarti **case-insensitive**, dan `AI` berarti **accent-insensitive** (mengabaikan aksen).
   - Dengan collation ini, Anda tidak perlu menggunakan `LOWER` karena perbandingan sudah otomatis case-insensitive.

---

### **Contoh Query Tanpa `LOWER` (Jika Collation Case-Insensitive Digunakan)**
```sql
SELECT *
FROM Employees E
JOIN Departments D
ON E.DepartmentName = D.DepartmentName;
```

---

### 5. **Tips Menggunakan Klausa `ON` Secara Fleksibel**
1. **Pahami Data**: Pastikan Anda memahami struktur dan hubungan antara tabel yang akan di-join.
2. **Gunakan Kondisi yang Relevan**: Pastikan kondisi di `ON` memang relevan dengan kebutuhan query.
3. **Performa Query**: Kondisi yang kompleks bisa memengaruhi performa query. Pastikan query di-optimalkan dengan baik.

---

### 6. **Praktik Langsung**
Coba buat tabel dan query sederhana untuk mempraktikkan klausa `ON` yang fleksibel:
```sql
-- Buat tabel Students
CREATE TABLE Students (
    ID INT,
    MARKS INT
);
INSERT INTO Students (ID, MARKS) VALUES (1, 85), (2, 95), (3, 70);

-- Buat tabel Grades
CREATE TABLE Grades (
    GRADE CHAR(1),
    MIN_MARK INT,
    MAX_MARK INT
);
INSERT INTO Grades (GRADE, MIN_MARK, MAX_MARK) VALUES ('A', 90, 100), ('B', 80, 89), ('C', 70, 79);

-- Query JOIN dengan kondisi BETWEEN
SELECT S.ID, S.MARKS, G.GRADE
FROM Students S
JOIN Grades G ON S.MARKS BETWEEN G.MIN_MARK AND G.MAX_MARK;
```

---

Dengan memahami fleksibilitas klausa `ON`, Anda bisa menulis query yang lebih powerful dan sesuai dengan kebutuhan analisis data. Jika ada pertanyaan lagi, jangan ragu untuk bertanya! 😊
