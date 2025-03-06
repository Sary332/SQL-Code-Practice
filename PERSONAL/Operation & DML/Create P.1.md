## **📌 CREATE TABLE di SQL Server: Penjelasan Lengkap**
`CREATE TABLE` di SQL Server digunakan untuk **membuat tabel baru** di dalam database. Berikut adalah **kondisi, syarat, aturan, dan fleksibilitasnya** secara detail.

---

## **📝 1. Sintaks Dasar `CREATE TABLE`**
```sql
CREATE TABLE nama_tabel (
    nama_kolom1 tipe_data [KONSTRAINT],
    nama_kolom2 tipe_data [KONSTRAINT],
    ...
);
```
- `nama_tabel` → Nama tabel yang ingin dibuat.
- `nama_kolom` → Nama kolom dalam tabel.
- `tipe_data` → Jenis data yang akan disimpan dalam kolom.
- `KONSTRAINT` → Aturan tambahan untuk memastikan validitas data.

---

## **📌 2. Kondisi & Syarat dalam `CREATE TABLE`**
Saat membuat tabel, ada beberapa hal yang perlu diperhatikan:

### **✅ a. Nama Tabel Harus Unik**
- Tidak boleh ada dua tabel dengan nama yang sama dalam satu database.
- Jika tabel dengan nama yang sama sudah ada, maka harus dihapus atau diubah namanya.

### **✅ b. Nama Kolom Harus Unik dalam Satu Tabel**
- Setiap kolom dalam satu tabel harus memiliki nama yang berbeda.

### **✅ c. Menentukan Tipe Data yang Tepat**
SQL Server menyediakan berbagai **tipe data**, misalnya:
- **Numerik**: `INT`, `BIGINT`, `DECIMAL`, `FLOAT`
- **Teks**: `VARCHAR`, `NVARCHAR`, `TEXT`
- **Tanggal/Waktu**: `DATE`, `DATETIME`, `TIME`
- **Boolean**: `BIT`
- **BLOB (Binary Data)**: `VARBINARY`, `IMAGE`

### **✅ d. Menggunakan `NULL` atau `NOT NULL`**
- **`NULL`** → Kolom boleh berisi nilai kosong.
- **`NOT NULL`** → Wajib diisi, tidak boleh kosong.

```sql
CREATE TABLE Pegawai (
    ID INT NOT NULL,
    Nama VARCHAR(100) NOT NULL,
    Email VARCHAR(255) NULL
);
```

---

## **📌 3. Aturan Penggunaan Konstraint (Constraints)**
Konstraint digunakan untuk **mengontrol validitas dan integritas data**.

### **✅ a. `PRIMARY KEY` (Kunci Utama)**
- **Harus unik** untuk setiap baris.
- **Tidak boleh `NULL`**.
- **Hanya boleh satu `PRIMARY KEY` dalam satu tabel**, tetapi bisa terdiri dari beberapa kolom (`Composite Key`).

```sql
CREATE TABLE Mahasiswa (
    NIM INT PRIMARY KEY,
    Nama VARCHAR(100) NOT NULL,
    Jurusan VARCHAR(50) NOT NULL
);
```

### **✅ b. `FOREIGN KEY` (Kunci Asing)**
- Menghubungkan tabel satu dengan yang lain.
- Harus merujuk ke **kolom `PRIMARY KEY` di tabel lain**.

```sql
CREATE TABLE Transaksi (
    ID_Transaksi INT PRIMARY KEY,
    ID_Pelanggan INT,
    FOREIGN KEY (ID_Pelanggan) REFERENCES Pelanggan(ID_Pelanggan)
);
```

### **✅ c. `UNIQUE`**
- Memastikan nilai dalam kolom **tidak boleh duplikat**, tetapi boleh `NULL`.

```sql
CREATE TABLE UserAkun (
    ID INT PRIMARY KEY,
    Username VARCHAR(50) NOT NULL UNIQUE,
    Email VARCHAR(255) NOT NULL UNIQUE
);
```

### **✅ d. `CHECK`**
- Membatasi nilai yang dapat dimasukkan.

```sql
CREATE TABLE Produk (
    ID INT PRIMARY KEY,
    Nama VARCHAR(100) NOT NULL,
    Harga DECIMAL(10,2) CHECK (Harga > 0)
);
```

### **✅ e. `DEFAULT`**
- Memberikan **nilai default** jika tidak diisi.

```sql
CREATE TABLE Pelanggan (
    ID INT PRIMARY KEY,
    Nama VARCHAR(100) NOT NULL,
    Status VARCHAR(20) DEFAULT 'Aktif'
);
```

### **✅ f. `IDENTITY`**
- Digunakan untuk **auto-increment** kolom angka.

```sql
CREATE TABLE Karyawan (
    ID INT IDENTITY(1,1) PRIMARY KEY,
    Nama VARCHAR(100) NOT NULL
);
```
> `IDENTITY(1,1)` → Nilai dimulai dari **1** dan bertambah **1 setiap insert**.

---

## **📌 4. Fleksibilitas `CREATE TABLE`**
SQL Server mendukung beberapa fitur fleksibel dalam `CREATE TABLE`.

### **✅ a. Membuat Tabel dengan `IF NOT EXISTS`**
Mencegah error jika tabel sudah ada.

```sql
IF NOT EXISTS (SELECT * FROM sys.tables WHERE name = 'Produk')
CREATE TABLE Produk (
    ID INT PRIMARY KEY,
    Nama VARCHAR(100) NOT NULL
);
```

### **✅ b. Membuat Tabel dengan `SELECT INTO`**
Membuat tabel baru berdasarkan hasil query.

```sql
SELECT * INTO PegawaiBackup FROM Pegawai;
```

### **✅ c. Membuat `TEMP TABLE` (Tabel Sementara)**
Digunakan dalam session tertentu dan akan otomatis terhapus setelah session berakhir.

```sql
CREATE TABLE #TempPegawai (
    ID INT PRIMARY KEY,
    Nama VARCHAR(100) NOT NULL
);
```

### **✅ d. Membuat `GLOBAL TEMP TABLE`**
- Bisa diakses oleh semua session.
- Tabel akan tetap ada sampai semua session yang menggunakannya selesai.

```sql
CREATE TABLE ##GlobalTemp (
    ID INT PRIMARY KEY,
    Nama VARCHAR(100) NOT NULL
);
```

### **✅ e. Menggunakan `PARTITION` untuk Tabel Besar**
Digunakan untuk **memisahkan data dalam beberapa partisi** berdasarkan nilai tertentu.

```sql
CREATE PARTITION FUNCTION partFunction (INT)
AS RANGE LEFT FOR VALUES (100, 200, 300);
```

---

## **📌 5. Aturan Tambahan**
| **Aturan**  | **Penjelasan** |
|-------------|---------------|
| **Nama Kolom Tidak Boleh Sama** | Tidak bisa membuat dua kolom dengan nama yang sama dalam satu tabel. |
| **Nama Tabel Harus Unik** | Tidak bisa ada dua tabel dengan nama yang sama dalam satu database. |
| **Setiap Tabel Harus Punya Setidaknya Satu Kolom** | Tabel tidak bisa dibuat tanpa kolom. |
| **Kolom `PRIMARY KEY` Tidak Boleh `NULL`** | Wajib memiliki nilai unik dan tidak boleh kosong. |
| **Kolom `FOREIGN KEY` Harus Merujuk ke `PRIMARY KEY` di Tabel Lain** | Tidak bisa merujuk ke kolom yang bukan `PRIMARY KEY`. |

---

## **🚀 Kesimpulan**
✅ `CREATE TABLE` sangat **fleksibel** dalam SQL Server.  
✅ Mendukung **konstraint (`PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`, `CHECK`, `DEFAULT`, `IDENTITY`)**.  
✅ Bisa membuat **`TEMP TABLE`**, **`GLOBAL TEMP TABLE`**, dan **partisi tabel** untuk performa yang lebih baik.  
✅ Bisa digabungkan dengan **`IF NOT EXISTS`** atau **`SELECT INTO`** untuk kemudahan manipulasi data.
