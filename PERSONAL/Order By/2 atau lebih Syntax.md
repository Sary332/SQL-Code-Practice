Bagaimana klausa **`ORDER BY`** bekerja ketika ada **dua atau lebih kolom/syntax** yang digunakan untuk pengurutan. Klausa `ORDER BY` memungkinkan Anda mengurutkan hasil query berdasarkan satu atau lebih kolom, baik secara ascending (naik) atau descending (turun).

---

### 1. **Cara Kerja `ORDER BY` dengan Dua atau Lebih Kolom**
Ketika Anda menggunakan dua atau lebih kolom dalam klausa `ORDER BY`, SQL akan mengurutkan data berdasarkan **prioritas kolom** dari kiri ke kanan. Artinya:
- Data pertama-tama diurutkan berdasarkan kolom pertama.
- Jika ada nilai yang sama di kolom pertama, data tersebut akan diurutkan berdasarkan kolom kedua.
- Jika ada nilai yang sama di kolom kedua, data tersebut akan diurutkan berdasarkan kolom ketiga, dan seterusnya.

---

### 2. **Contoh Penggunaan `ORDER BY` dengan Dua Kolom**
Misalkan Anda memiliki tabel `Students` dengan data berikut:

| ID  | Nama    | Nilai | Kelas |
|-----|---------|-------|-------|
| 1   | Andi    | 85    | A     |
| 2   | Budi    | 90    | B     |
| 3   | Cici    | 85    | A     |
| 4   | Dedi    | 80    | B     |
| 5   | Eka     | 90    | A     |

#### Query:
```sql
SELECT *
FROM Students
ORDER BY Nilai DESC, Kelas ASC;
```

#### Hasil:
| ID  | Nama    | Nilai | Kelas |
|-----|---------|-------|-------|
| 2   | Budi    | 90    | B     |
| 5   | Eka     | 90    | A     |
| 1   | Andi    | 85    | A     |
| 3   | Cici    | 85    | A     |
| 4   | Dedi    | 80    | B     |

#### Penjelasan:
1. Data diurutkan pertama kali berdasarkan **`Nilai`** secara **descending** (`DESC`), sehingga nilai tertinggi (90) muncul di atas.
2. Karena ada dua siswa dengan nilai 90 (Budi dan Eka), data tersebut diurutkan berdasarkan **`Kelas`** secara **ascending** (`ASC`). Kelas `A` (Eka) muncul sebelum kelas `B` (Budi).

---

### 3. **Contoh Lain dengan Tiga Kolom**
Misalkan Anda memiliki tabel `Employees` dengan data berikut:

| ID  | Nama    | Departemen | Gaji  | Jabatan     |
|-----|---------|------------|-------|-------------|
| 1   | Andi    | IT         | 5000  | Programmer  |
| 2   | Budi    | HR         | 6000  | Manager     |
| 3   | Cici    | IT         | 5000  | Analyst     |
| 4   | Dedi    | HR         | 6000  | Supervisor  |
| 5   | Eka     | IT         | 5500  | Programmer  |

#### Query:
```sql
SELECT *
FROM Employees
ORDER BY Departemen ASC, Gaji DESC, Jabatan ASC;
```

#### Hasil:
| ID  | Nama    | Departemen | Gaji  | Jabatan     |
|-----|---------|------------|-------|-------------|
| 2   | Budi    | HR         | 6000  | Manager     |
| 4   | Dedi    | HR         | 6000  | Supervisor  |
| 5   | Eka     | IT         | 5500  | Programmer  |
| 1   | Andi    | IT         | 5000  | Programmer  |
| 3   | Cici    | IT         | 5000  | Analyst     |

#### Penjelasan:
1. Data diurutkan pertama kali berdasarkan **`Departemen`** secara **ascending** (`ASC`), sehingga departemen `HR` muncul sebelum `IT`.
2. Dalam departemen yang sama, data diurutkan berdasarkan **`Gaji`** secara **descending** (`DESC`), sehingga gaji tertinggi muncul di atas.
3. Jika ada gaji yang sama (misalnya, Budi dan Dedi di departemen HR), data diurutkan berdasarkan **`Jabatan`** secara **ascending** (`ASC`), sehingga `Manager` muncul sebelum `Supervisor`.

---

### 4. **Aturan Penggunaan `ORDER BY`**
1. **Urutan Prioritas**:  
   Kolom yang ditulis pertama dalam `ORDER BY` memiliki prioritas tertinggi. Kolom berikutnya hanya digunakan jika ada nilai yang sama di kolom sebelumnya.

2. **Ascending (`ASC`) vs Descending (`DESC`)**:  
   - `ASC` (default): Mengurutkan dari nilai terkecil ke terbesar.
   - `DESC`: Mengurutkan dari nilai terbesar ke terkecil.

3. **Bisa Menggunakan Ekspresi atau Fungsi**:  
   Anda bisa menggunakan ekspresi atau fungsi dalam `ORDER BY`.  
   Contoh:
   ```sql
   SELECT *
   FROM Employees
   ORDER BY LENGTH(Nama) DESC;
   ```

4. **Bisa Menggunakan Nomor Kolom**:  
   Anda bisa menggunakan nomor kolom (berdasarkan urutan di `SELECT`) sebagai referensi.  
   Contoh:
   ```sql
   SELECT Nama, Gaji
   FROM Employees
   ORDER BY 2 DESC; -- Mengurutkan berdasarkan kolom ke-2 (Gaji)
   ```

---

### 5. **Contoh Kombinasi `ORDER BY` dengan `CASE`**
Anda bisa menggunakan ekspresi `CASE` dalam `ORDER BY` untuk membuat pengurutan yang lebih dinamis.  
Contoh:
```sql
SELECT *
FROM Employees
ORDER BY 
    CASE 
        WHEN Departemen = 'IT' THEN 1
        WHEN Departemen = 'HR' THEN 2
        ELSE 3
    END,
    Gaji DESC;
```

#### Hasil:
| ID  | Nama    | Departemen | Gaji  | Jabatan     |
|-----|---------|------------|-------|-------------|
| 5   | Eka     | IT         | 5500  | Programmer  |
| 1   | Andi    | IT         | 5000  | Programmer  |
| 3   | Cici    | IT         | 5000  | Analyst     |
| 2   | Budi    | HR         | 6000  | Manager     |
| 4   | Dedi    | HR         | 6000  | Supervisor  |

#### Penjelasan:
- Departemen `IT` diurutkan terlebih dahulu karena memiliki nilai `1` di ekspresi `CASE`.
- Kemudian, data diurutkan berdasarkan `Gaji` secara descending.

---

### 6. **Tips Menggunakan `ORDER BY` dengan Banyak Kolom**
1. **Tentukan Prioritas**:  
   Pikirkan kolom mana yang paling penting untuk diurutkan terlebih dahulu.

2. **Gunakan `ASC` atau `DESC` Secara Tepat**:  
   Pastikan Anda memilih urutan yang sesuai dengan kebutuhan.

3. **Hindari Pengurutan yang Tidak Perlu**:  
   Jika pengurutan tidak diperlukan, hindari menggunakan `ORDER BY` karena bisa memengaruhi performa query.

