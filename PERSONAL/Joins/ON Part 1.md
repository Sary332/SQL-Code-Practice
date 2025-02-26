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
