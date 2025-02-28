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
