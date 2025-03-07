# **📌 INSERT di SQL Server: Penjelasan Lengkap**
`INSERT` di SQL Server digunakan untuk **menambahkan data ke dalam tabel**. Perintah ini sangat fleksibel dan memiliki berbagai cara penggunaan.

---

## **📝 1. Sintaks Dasar `INSERT`**
```sql
INSERT INTO nama_tabel (kolom1, kolom2, kolom3, ...)
VALUES (nilai1, nilai2, nilai3, ...);
```
- `nama_tabel` → Nama tabel tempat data akan dimasukkan.
- `kolom1, kolom2, ...` → Nama kolom dalam tabel (opsional jika memasukkan semua kolom).
- `VALUES` → Menentukan nilai yang akan dimasukkan.

**Contoh:**
```sql
INSERT INTO Pegawai (ID, Nama, Jabatan)
VALUES (1, 'Budi', 'Manager');
```

---

## **📌 2. Kondisi & Syarat dalam `INSERT`**
### **✅ a. Jumlah Kolom dan Nilai Harus Cocok**
- Jumlah nilai dalam `VALUES` harus sama dengan jumlah kolom yang ditentukan.
- Jika tidak mencantumkan daftar kolom, maka nilai harus mencakup semua kolom dalam tabel.

```sql
INSERT INTO Pegawai VALUES (2, 'Siti', 'HRD');  -- Jika semua kolom ditentukan
```

### **✅ b. Format Data Harus Sesuai dengan Tipe Data Kolom**
- Jika kolom bertipe `INT`, maka nilai yang dimasukkan harus angka.
- Jika kolom bertipe `VARCHAR`, maka nilai harus berupa string dalam tanda kutip `' '`.
- Jika kolom bertipe `DATE`, gunakan format `'YYYY-MM-DD'`.

```sql
INSERT INTO Transaksi (ID, Tanggal, Total)
VALUES (1, '2024-02-12', 500000);
```

### **✅ c. Tidak Bisa Mengisi Kolom `IDENTITY` Secara Manual**
Jika tabel memiliki kolom `IDENTITY`, SQL Server akan mengisinya otomatis.

```sql
CREATE TABLE Pelanggan (
    ID INT IDENTITY(1,1) PRIMARY KEY,
    Nama VARCHAR(100)
);

INSERT INTO Pelanggan (Nama) VALUES ('Andi');  -- ID akan otomatis bertambah
```

**Jika ingin memasukkan nilai manual ke `IDENTITY`, gunakan:**
```sql
SET IDENTITY_INSERT Pelanggan ON;

INSERT INTO Pelanggan (ID, Nama) VALUES (10, 'Budi');

SET IDENTITY_INSERT Pelanggan OFF;
```

### **✅ d. Kolom `NOT NULL` Harus Diberi Nilai**
Jika kolom memiliki **konstraint `NOT NULL`**, maka tidak boleh kosong.

```sql
CREATE TABLE Produk (
    ID INT PRIMARY KEY,
    Nama VARCHAR(100) NOT NULL
);

INSERT INTO Produk (ID) VALUES (1);  -- ❌ ERROR karena Nama tidak boleh NULL
```

### **✅ e. `FOREIGN KEY` Harus Memiliki Referensi yang Ada**
Jika tabel memiliki **kunci asing (`FOREIGN KEY`)**, maka nilai yang dimasukkan harus sudah ada di tabel referensi.

```sql
CREATE TABLE Karyawan (
    ID INT PRIMARY KEY,
    Nama VARCHAR(100)
);

CREATE TABLE Gaji (
    ID INT PRIMARY KEY,
    Karyawan_ID INT,
    FOREIGN KEY (Karyawan_ID) REFERENCES Karyawan(ID)
);

INSERT INTO Gaji (ID, Karyawan_ID) VALUES (1, 5);  -- ❌ ERROR jika ID 5 tidak ada di Karyawan
```

---

## **📌 3. Aturan Penggunaan `INSERT`**
### **✅ a. `INSERT DEFAULT VALUES`**
- Digunakan jika semua kolom memiliki nilai default.

```sql
INSERT INTO Pelanggan DEFAULT VALUES;
```

### **✅ b. `INSERT MULTIPLE ROWS`**
- Bisa memasukkan beberapa baris sekaligus.

```sql
INSERT INTO Pegawai (ID, Nama, Jabatan)
VALUES 
    (3, 'Joko', 'IT'),
    (4, 'Ayu', 'Finance'),
    (5, 'Dewi', 'Marketing');
```

### **✅ c. `INSERT INTO SELECT`**
- Memasukkan data dari hasil query tabel lain.

```sql
INSERT INTO PegawaiBackup (ID, Nama, Jabatan)
SELECT ID, Nama, Jabatan FROM Pegawai WHERE Jabatan = 'IT';
```

---

## **📌 4. Fleksibilitas `INSERT`**
### **✅ a. Menggunakan `OUTPUT INSERTED`**
- Menampilkan data yang baru saja dimasukkan.

```sql
INSERT INTO Pegawai (ID, Nama, Jabatan)
OUTPUT INSERTED.*
VALUES (6, 'Rizky', 'Admin');
```

### **✅ b. `MERGE INTO` (UPSERT)**
- Kombinasi `INSERT`, `UPDATE`, dan `DELETE`.

```sql
MERGE INTO Pegawai AS target
USING (SELECT 6 AS ID, 'Rizky' AS Nama, 'Admin' AS Jabatan) AS source
ON target.ID = source.ID
WHEN MATCHED THEN 
    UPDATE SET Nama = source.Nama, Jabatan = source.Jabatan
WHEN NOT MATCHED THEN 
    INSERT (ID, Nama, Jabatan) VALUES (source.ID, source.Nama, source.Jabatan);
```

### **✅ c. `BULK INSERT` (Memasukkan Data dalam Jumlah Besar)**
- Digunakan untuk impor data dari file `.csv`.

```sql
BULK INSERT Pegawai
FROM 'C:\data\pegawai.csv'
WITH (FIELDTERMINATOR = ',', ROWTERMINATOR = '\n');
```
Perintah **`BULK INSERT`** di SQL Server. Perintah ini digunakan untuk memuat data dalam jumlah besar dari file eksternal (seperti file CSV) ke dalam tabel di database.

---

### 1. **Apa Itu `BULK INSERT`?**
`BULK INSERT` adalah perintah SQL Server yang digunakan untuk mengimpor data dari file eksternal (seperti CSV, TXT, atau file lainnya) ke dalam tabel di database dengan cepat dan efisien. Ini sangat berguna ketika Anda perlu memuat data dalam jumlah besar.

---

### 2. **Sintaks Dasar**
```sql
BULK INSERT NamaTabel
FROM 'PathFile'
WITH (OPTIONS);
```

- **`NamaTabel`**: Nama tabel di database yang akan dimasukkan data.
- **`'PathFile'`**: Path lengkap ke file eksternal yang berisi data.
- **`WITH (OPTIONS)`**: Opsi tambahan untuk mengontrol proses impor, seperti pemisah kolom (`FIELDTERMINATOR`), pemisah baris (`ROWTERMINATOR`), dll.

---

### 3. **Kondisi dan Syarat**
1. **File Harus Ada**:  
   File eksternal yang berisi data harus ada di lokasi yang ditentukan.

2. **Struktur File Harus Sesuai**:  
   Struktur file (kolom dan baris) harus sesuai dengan struktur tabel di database.

3. **Hak Akses**:  
   Anda harus memiliki hak akses yang cukup (seperti `ADMINISTER BULK OPERATIONS` atau `INSERT` permission) untuk menjalankan perintah `BULK INSERT`.

4. **Format File**:  
   File harus dalam format yang didukung, seperti CSV, TXT, atau format lainnya.

---

### 4. **Opsi `WITH` yang Umum Digunakan**
- **`FIELDTERMINATOR`**: Menentukan karakter pemisah antara kolom (misalnya, `','` untuk CSV).
- **`ROWTERMINATOR`**: Menentukan karakter pemisah antara baris (misalnya, `'\n'` untuk baris baru).
- **`FIRSTROW`**: Menentukan baris pertama yang akan dibaca (misalnya, `2` untuk melewati header).
- **`CODEPAGE`**: Menentukan halaman kode untuk file (misalnya, `'ACP'` untuk ANSI).
- **`ERRORFILE`**: Menentukan file untuk menyimpan baris yang error.
- **`MAXERRORS`**: Menentukan jumlah maksimum error yang diizinkan sebelum proses dihentikan.

---

### 5. **Contoh Penggunaan**
Misalkan Anda memiliki file `pegawai.csv` di lokasi `C:\data\pegawai.csv` dengan konten berikut:

```
ID,Nama,Jabatan
1,Andi,Manager
2,Budi,Staff
3,Cici,Analyst
```

#### a. **Membuat Tabel `Pegawai`**
```sql
CREATE TABLE Pegawai (
    ID INT PRIMARY KEY,
    Nama VARCHAR(100) NOT NULL,
    Jabatan VARCHAR(100)
);
```

#### b. **Mengimpor Data dari File CSV**
```sql
BULK INSERT Pegawai
FROM 'C:\data\pegawai.csv'
WITH (
    FIELDTERMINATOR = ',',
    ROWTERMINATOR = '\n',
    FIRSTROW = 2  -- Melewati baris header
);
```
#### Hasil di Tabel `Pegawai`:
| ID | Nama | Jabatan  |
|----|------|----------|
| 1  | Andi | Manager  |
| 2  | Budi | Staff    |
| 3  | Cici | Analyst  |

---

### 6. **Fleksibilitas `BULK INSERT`**
Anda bisa menggunakan `BULK INSERT` untuk mengimpor data dari berbagai format file dan dengan berbagai opsi.

#### a. **Mengimpor Data dengan `FIRSTROW`**
Jika file memiliki header, Anda bisa melewati baris pertama dengan `FIRSTROW`.

```sql
BULK INSERT Pegawai
FROM 'C:\data\pegawai.csv'
WITH (
    FIELDTERMINATOR = ',',
    ROWTERMINATOR = '\n',
    FIRSTROW = 2  -- Melewati baris header, Maksutnya tidak memasukan header yg berada di baris pertama dan langsung datanya.
);
```

#### b. **Mengimpor Data dengan `ERRORFILE`**
Anda bisa menyimpan baris yang error ke file tertentu.

```sql
BULK INSERT Pegawai
FROM 'C:\data\pegawai.csv'
WITH (
    FIELDTERMINATOR = ',',
    ROWTERMINATOR = '\n',
    FIRSTROW = 2,
    ERRORFILE = 'C:\data\errorfile.log'
);
```

#### c. **Mengimpor Data dengan `MAXERRORS`**
Anda bisa menentukan jumlah maksimum error yang diizinkan.

```sql
BULK INSERT Pegawai
FROM 'C:\data\pegawai.csv'
WITH (
    FIELDTERMINATOR = ',',
    ROWTERMINATOR = '\n',
    FIRSTROW = 2,
    MAXERRORS = 10  -- Maksimum 10 error
);
```

---

### 7. **Contoh Kasus Lain**
#### a. **Mengimpor Data dari File TXT**
Jika file menggunakan format TXT dengan pemisah tab (`\t`), Anda bisa menggunakan `FIELDTERMINATOR = '\t'`.

```sql
BULK INSERT Pegawai
FROM 'C:\data\pegawai.txt'
WITH (
    FIELDTERMINATOR = '\t',
    ROWTERMINATOR = '\n',
    FIRSTROW = 2 -- Jika tidak melampirkan ini, maka data harus dari awal tidak memiliki header.
);
```

#### b. **Mengimpor Data dengan `CODEPAGE`**
Jika file menggunakan encoding tertentu, Anda bisa menentukan `CODEPAGE`.

```sql
BULK INSERT Pegawai
FROM 'C:\data\pegawai.csv'
WITH (
    FIELDTERMINATOR = ',',
    ROWTERMINATOR = '\n',
    FIRSTROW = 2,
    CODEPAGE = 'ACP'  -- ANSI Code Page
);
```

---

### 8. **Tips Menggunakan `BULK INSERT`**
1. **Periksa Struktur File**:  
   Pastikan struktur file (kolom dan baris) sesuai dengan struktur tabel di database.

2. **Gunakan `FIRSTROW` untuk Melewati Header**:  
   Jika file memiliki header, gunakan `FIRSTROW` untuk melewati baris pertama.

3. **Periksa Hak Akses**:  
   Pastikan Anda memiliki hak akses yang cukup untuk menjalankan `BULK INSERT`.

4. **Hindari File yang Terlalu Besar**:  
   Jika file sangat besar, pertimbangkan untuk membagi file menjadi beberapa bagian.

---

### 9. **Contoh Query Lengkap**
```sql
-- Membuat tabel Pegawai
CREATE TABLE Pegawai (
    ID INT PRIMARY KEY,
    Nama VARCHAR(100) NOT NULL,
    Jabatan VARCHAR(100)
);

-- Mengimpor data dari file CSV
BULK INSERT Pegawai
FROM 'C:\data\pegawai.csv'
WITH (
    FIELDTERMINATOR = ',',
    ROWTERMINATOR = '\n',
    FIRSTROW = 2  -- Melewati baris header
);

-- Menampilkan data
SELECT * FROM Pegawai;
```

#### Hasil:
| ID | Nama | Jabatan  |
|----|------|----------|
| 1  | Andi | Manager  |
| 2  | Budi | Staff    |
| 3  | Cici | Analyst  |

---

Dengan memahami penggunaan, kondisi, syarat, aturan, dan fleksibilitas `BULK INSERT`, Anda bisa mengimpor data dari file eksternal ke dalam tabel dengan lebih efektif. Jika ada pertanyaan lagi, jangan ragu untuk bertanya! 😊


---

## **📌 5. Performa dan Best Practices**
| **Best Practice**  | **Penjelasan** |
|----------------|----------------|
| **Gunakan Batch Insert** | Memasukkan banyak data dalam satu query lebih efisien daripada satu per satu. |
| **Gunakan `TRANSACTION` Jika Banyak Insert** | Mencegah data korup jika ada kegagalan. |
| **Gunakan `BULK INSERT` untuk Data Besar** | Lebih cepat daripada `INSERT INTO` biasa. |
| **Gunakan `SET IDENTITY_INSERT` dengan Hati-Hati** | Harus segera dimatikan setelah digunakan. |
| **Pastikan `FOREIGN KEY` Valid Sebelum Insert** | Hindari error karena referensi yang hilang. |

---

## **🚀 Kesimpulan**
✅ **`INSERT` sangat fleksibel** dan bisa digunakan untuk **single row, multiple row, select, default values, bulk insert, hingga upsert (`MERGE`)**.  
✅ **Pastikan format data sesuai** dengan tipe data kolom.  
✅ **Perhatikan aturan `IDENTITY`, `NOT NULL`, dan `FOREIGN KEY`** agar tidak terjadi error.  
✅ **Gunakan `TRANSACTION` dan `BULK INSERT` untuk performa lebih baik** dalam memasukkan banyak data.  
