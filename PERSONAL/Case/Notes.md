## Q1
Pertanyaan yang sangat bagus! Mari kita bahas mengapa penggunaan `MAX(CASE ...)` diperlukan untuk menjaga urutan data, dan apa yang terjadi jika hanya menggunakan `CASE` tanpa `MAX`.

---

### **1. Peran `CASE` dalam Query**
Fungsi `CASE` digunakan untuk mengembalikan nilai berdasarkan kondisi tertentu. Misalnya:
```sql
CASE WHEN occupation = 'Doctor' THEN name END
```
- Jika `occupation = 'Doctor'`, maka `name` akan dikembalikan.
- Jika kondisi tidak terpenuhi, `CASE` akan mengembalikan `NULL`.

**Masalahnya**:
- Jika hanya menggunakan `CASE` tanpa agregasi (seperti `MAX`), maka setiap baris akan diperlakukan secara terpisah.
- Ini menyebabkan hasil query akan mengikuti urutan baris asli di tabel, yang mungkin tidak sesuai dengan kebutuhan Anda.

---

### **2. Peran `MAX` dalam Query**
Fungsi agregasi seperti `MAX` digunakan untuk mengelompokkan data dan memilih nilai tertentu dari kelompok tersebut. Ketika digunakan bersama `CASE`, `MAX` memastikan bahwa hanya satu nilai yang dipilih untuk setiap kelompok.

**Contoh**:
```sql
SELECT 
    MAX(CASE WHEN occupation = 'Doctor' THEN name END) AS Doctor
FROM Occupations
GROUP BY RowNum;
```
- `MAX` memilih nilai maksimum dari hasil `CASE` untuk setiap kelompok (`RowNum`).
- Jika `CASE` mengembalikan `NULL`, `MAX` akan mengabaikannya dan memilih nilai non-`NULL` (jika ada).

---

### **3. Mengapa Urutan Data Berantakan Tanpa `MAX`?**
Jika hanya menggunakan `CASE` tanpa `MAX`, query akan mengembalikan satu baris untuk setiap baris di tabel asli. Ini menyebabkan:
1. **Data Duplikat**:
   - Jika ada beberapa baris yang memenuhi kondisi `CASE`, semua baris tersebut akan muncul di hasil query.
   - Contoh:
     ```sql
     SELECT 
         CASE WHEN occupation = 'Doctor' THEN name END AS Doctor
     FROM Occupations;
     ```
     - Jika ada 3 dokter, hasilnya akan mengembalikan 3 baris untuk `Doctor`.

2. **Urutan Tidak Terkontrol**:
   - Hasil query akan mengikuti urutan baris asli di tabel, yang mungkin tidak sesuai dengan kebutuhan Anda.
   - Misalnya, jika Anda ingin mengelompokkan data berdasarkan `RowNum`, tanpa `MAX`, data akan tetap dalam urutan asli dan tidak dikelompokkan dengan benar.

3. **NULL Menyebar**:
   - Jika kondisi `CASE` tidak terpenuhi, `NULL` akan muncul di hasil query.
   - Tanpa `MAX`, `NULL` ini akan tersebar di seluruh hasil, membuat data sulit dibaca.

---

### **4. Contoh Perbandingan**

#### Data Contoh: `Occupations`
| Name       | Occupation | RowNum |
|------------|------------|--------|
| Alice      | Doctor     | 1      |
| Bob        | Singer     | 1      |
| Charlie    | Doctor     | 2      |
| David      | Professor  | 2      |

#### Query Tanpa `MAX`:
```sql
SELECT 
    CASE WHEN occupation = 'Doctor' THEN name END AS Doctor,
    CASE WHEN occupation = 'Singer' THEN name END AS Singer,
    CASE WHEN occupation = 'Professor' THEN name END AS Professor
FROM Occupations;
```

#### Hasil:
| Doctor  | Singer | Professor |
|---------|--------|-----------|
| Alice   | NULL   | NULL      |
| NULL    | Bob    | NULL      |
| Charlie | NULL   | NULL      |
| NULL    | NULL   | David     |

- Data berantakan karena setiap baris di tabel asli menghasilkan satu baris di hasil query.
- `NULL` tersebar di seluruh hasil.

#### Query dengan `MAX`:
```sql
SELECT 
    MAX(CASE WHEN occupation = 'Doctor' THEN name END) AS Doctor,
    MAX(CASE WHEN occupation = 'Singer' THEN name END) AS Singer,
    MAX(CASE WHEN occupation = 'Professor' THEN name END) AS Professor
FROM Occupations
GROUP BY RowNum;
```

#### Hasil:
| Doctor  | Singer | Professor |
|---------|--------|-----------|
| Alice   | Bob    | NULL      |
| Charlie | NULL   | David     |

- Data dikelompokkan berdasarkan `RowNum`.
- Hanya satu nilai non-`NULL` yang dipilih untuk setiap kolom dan kelompok.

---

### **5. Mengapa `MAX` Membantu Menjaga Urutan?**
- **Pengelompokan (`GROUP BY`)**:
  - `MAX` digunakan bersama `GROUP BY` untuk mengelompokkan data berdasarkan kolom tertentu (misalnya, `RowNum`).
  - Ini memastikan bahwa setiap kelompok hanya menghasilkan satu baris di hasil query.

- **Pemilihan Nilai**:
  - `MAX` memilih nilai maksimum dari hasil `CASE` untuk setiap kelompok.
  - Jika ada beberapa nilai non-`NULL`, `MAX` akan memilih salah satunya (biasanya yang pertama sesuai urutan).

- **Penanganan `NULL`**:
  - `MAX` mengabaikan `NULL`, sehingga hanya nilai non-`NULL` yang dipilih.

---

### **6. Kapan Hanya `CASE` Cukup?**
Anda hanya perlu menggunakan `CASE` tanpa `MAX` jika:
- Anda tidak perlu mengelompokkan data.
- Anda ingin mempertahankan setiap baris asli di tabel.
- Anda tidak peduli dengan duplikasi atau `NULL` yang tersebar.

Contoh:
```sql
SELECT 
    CASE WHEN occupation = 'Doctor' THEN name END AS Doctor
FROM Occupations;
```

---

### **Kesimpulan**
- **`MAX(CASE ...)`** digunakan untuk mengelompokkan data dan memilih satu nilai non-`NULL` untuk setiap kelompok.
- Tanpa `MAX`, hasil query akan mengikuti urutan baris asli di tabel, menyebabkan data berantakan, duplikasi, dan `NULL` yang tersebar.
- Dengan `MAX`, data dikelompokkan dengan benar, dan hanya nilai yang relevan yang dipilih.






## Q2

Maksud dari "`MAX` memilih nilai maksimum dari hasil `CASE` untuk setiap kelompok (`RowNum`)" adalah bahwa fungsi agregasi `MAX` digunakan untuk **memilih satu nilai** dari sekumpulan nilai yang dihasilkan oleh ekspresi `CASE` dalam setiap kelompok data (yang dikelompokkan berdasarkan `RowNum`).

Mari kita bahas lebih detail:
---
### **1. Konteks Penggunaan `MAX(CASE ...)`**
Dalam query yang kita bahas, `MAX(CASE ...)` digunakan untuk:
- Mengelompokkan data berdasarkan `RowNum`.
- Memilih satu nilai dari hasil `CASE` untuk setiap kelompok (`RowNum`).

Contoh:
```sql
SELECT 
    MAX(CASE WHEN occupation = 'Doctor' THEN name END) AS Doctor
FROM Occupations
GROUP BY RowNum;
```

---

### **2. Cara Kerja `MAX(CASE ...)`**
#### a. **Ekspresi `CASE`**:
- `CASE` digunakan untuk mengembalikan nilai berdasarkan kondisi tertentu.
- Misalnya:
  ```sql
  CASE WHEN occupation = 'Doctor' THEN name END
  ```
  - Jika `occupation = 'Doctor'`, maka `name` akan dikembalikan.
  - Jika kondisi tidak terpenuhi, `CASE` mengembalikan `NULL`.

#### b. **Fungsi Agregasi `MAX`**:
- `MAX` adalah fungsi agregasi yang memilih nilai **maksimum** dari sekumpulan nilai.
- Dalam konteks ini, `MAX` tidak hanya memilih nilai terbesar secara numerik, tetapi juga memilih **nilai non-`NULL`** jika ada.

#### c. **Kelompok Data (`GROUP BY`)**:
- Data dikelompokkan berdasarkan `RowNum`.
- Untuk setiap kelompok (`RowNum`), `MAX` akan memilih satu nilai dari hasil `CASE`.

---

### **3. Contoh Kasus**
Misalkan kita memiliki data berikut di tabel `Occupations`:

| Name       | Occupation | RowNum |
|------------|------------|--------|
| Alice      | Doctor     | 1      |
| Bob        | Singer     | 1      |
| Charlie    | Doctor     | 2      |
| David      | Professor  | 2      |

#### Query:
```sql
SELECT 
    MAX(CASE WHEN occupation = 'Doctor' THEN name END) AS Doctor,
    MAX(CASE WHEN occupation = 'Singer' THEN name END) AS Singer,
    MAX(CASE WHEN occupation = 'Professor' THEN name END) AS Professor
FROM Occupations
GROUP BY RowNum;
```

#### Langkah-Langkah:
1. **Kelompokkan Data**:
   - Data dikelompokkan berdasarkan `RowNum`:
     - Kelompok 1: `RowNum = 1` (Alice, Bob)
     - Kelompok 2: `RowNum = 2` (Charlie, David)

2. **Terapkan `CASE`**:
   - Untuk setiap baris, `CASE` mengembalikan nilai jika kondisi terpenuhi, atau `NULL` jika tidak.
   - Kelompok 1:
     - Alice: `CASE` mengembalikan `Alice` (karena `occupation = 'Doctor'`).
     - Bob: `CASE` mengembalikan `NULL` (karena `occupation = 'Singer'`, bukan `Doctor`).
   - Kelompok 2:
     - Charlie: `CASE` mengembalikan `Charlie` (karena `occupation = 'Doctor'`).
     - David: `CASE` mengembalikan `NULL` (karena `occupation = 'Professor'`, bukan `Doctor`).

3. **Terapkan `MAX`**:
   - Untuk setiap kelompok, `MAX` memilih nilai maksimum dari hasil `CASE`.
   - Kelompok 1:
     - Nilai yang dihasilkan oleh `CASE`: `Alice`, `NULL`.
     - `MAX` memilih `Alice` (nilai non-`NULL`).
   - Kelompok 2:
     - Nilai yang dihasilkan oleh `CASE`: `Charlie`, `NULL`.
     - `MAX` memilih `Charlie` (nilai non-`NULL`).

#### Hasil:
| Doctor  | Singer | Professor |
|---------|--------|-----------|
| Alice   | Bob    | NULL      |
| Charlie | NULL   | David     |

---

### **4. Mengapa Disebut "Memilih Nilai Maksimum"?**
- Dalam konteks ini, `MAX` tidak hanya memilih nilai terbesar secara numerik, tetapi juga memilih **nilai non-`NULL`** jika ada.
- Jika ada beberapa nilai non-`NULL` dalam kelompok, `MAX` akan memilih salah satunya (biasanya yang pertama sesuai urutan).

---

### **5. Apa yang Terjadi Tanpa `MAX`?**
Jika hanya menggunakan `CASE` tanpa `MAX`, hasil query akan mengembalikan satu baris untuk setiap baris di tabel asli. Contoh:
```sql
SELECT 
    CASE WHEN occupation = 'Doctor' THEN name END AS Doctor
FROM Occupations;
```

#### Hasil:
| Doctor  |
|---------|
| Alice   |
| NULL    |
| Charlie |
| NULL    |

- Data berantakan karena setiap baris di tabel asli menghasilkan satu baris di hasil query.
- `NULL` tersebar di seluruh hasil.

---

### **6. Kesimpulan**
- **`MAX(CASE ...)`** digunakan untuk memilih satu nilai dari hasil `CASE` untuk setiap kelompok data.
- Dalam konteks ini, "memilih nilai maksimum" berarti:
  1. Memilih nilai non-`NULL` jika ada.
  2. Jika ada beberapa nilai non-`NULL`, `MAX` akan memilih salah satunya.
- Tanpa `MAX`, data akan berantakan karena setiap baris di tabel asli menghasilkan satu baris di hasil query.
