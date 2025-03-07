# **📌 UPDATE di SQL Server: Penjelasan Lengkap**  
`UPDATE` di SQL Server digunakan untuk **mengubah data dalam tabel**. Perintah ini bisa diterapkan ke satu atau lebih baris dengan kondisi tertentu.

---

## **📝 1. Sintaks Dasar `UPDATE`**
```sql
UPDATE nama_tabel
SET kolom1 = nilai1, kolom2 = nilai2, ...
WHERE kondisi;
```
- `nama_tabel` → Nama tabel yang datanya akan diperbarui.
- `SET` → Menentukan kolom yang akan diperbarui dengan nilai baru.
- `WHERE` → Menentukan baris mana yang akan diperbarui.

**Contoh:**
```sql
UPDATE Pegawai
SET Jabatan = 'Manager'
WHERE ID = 1;
```
📌 **Jika `WHERE` tidak diberikan, semua baris dalam tabel akan diperbarui!** 🚨

---

## **📌 2. Kondisi & Syarat dalam `UPDATE`**
### **✅ a. Pastikan Format Data Sesuai dengan Tipe Data Kolom**
- `VARCHAR` harus dalam tanda kutip `' '`
- `DATE` harus dalam format `'YYYY-MM-DD'`
- `INT` tidak boleh dalam tanda kutip `' '`

```sql
UPDATE Karyawan
SET Gaji = 10000000, TanggalMasuk = '2020-01-01'
WHERE ID = 2;
```

### **✅ b. Kolom `PRIMARY KEY` atau `IDENTITY` Tidak Bisa Diperbarui**
- Jika kolom memiliki **`IDENTITY(1,1)`**, nilainya tidak bisa diubah.

```sql
UPDATE Pegawai
SET ID = 10  -- ❌ ERROR: IDENTITY tidak bisa diubah
WHERE ID = 1;
```

### **✅ c. Hanya Baris yang Memenuhi `WHERE` yang Akan Diperbarui**
- Jika `WHERE` tidak spesifik, banyak baris bisa berubah.

```sql
UPDATE Pegawai
SET Jabatan = 'Supervisor'
WHERE Departemen = 'HR';  -- Mengubah semua pegawai di departemen HR
```

### **✅ d. `FOREIGN KEY` Harus Dijaga Konsistensinya**
- Jika ada referensi **`FOREIGN KEY`**, pastikan tidak mengubah nilainya jika tabel lain masih bergantung padanya.

```sql
UPDATE Gaji
SET Karyawan_ID = 99  -- ❌ ERROR jika ID 99 tidak ada di tabel Karyawan
WHERE ID = 3;
```

---

## **📌 3. Aturan Penggunaan `UPDATE`**
### **✅ a. `UPDATE` Tanpa `WHERE` Akan Mengubah Semua Data** 🚨
```sql
UPDATE Pegawai
SET Jabatan = 'CEO';  -- Semua pegawai menjadi CEO!
```
🔥 **Solusi** → **Selalu gunakan `WHERE` untuk update spesifik.**

---

### **✅ b. `UPDATE` dengan `JOIN` (UPDATE dari Tabel Lain)**
- Mengupdate nilai dari tabel lain dengan `JOIN`.

```sql
UPDATE p
SET p.Gaji = g.GajiBaru
FROM Pegawai p
JOIN KenaikanGaji g ON p.ID = g.PegawaiID
WHERE p.Departemen = 'IT';
```
- UPDATE p: Memperbarui tabel Pegawai (diberi alias p).

- SET p.Gaji = g.GajiBaru: Mengatur nilai kolom Gaji di tabel Pegawai dengan nilai GajiBaru dari tabel KenaikanGaji.

- FROM Pegawai p: Menentukan tabel utama yang akan diperbarui (Pegawai).

- JOIN KenaikanGaji g ON p.ID = g.PegawaiID: Menggabungkan tabel Pegawai dengan tabel KenaikanGaji berdasarkan kolom ID di Pegawai dan PegawaiID di KenaikanGaji.

- WHERE p.Departemen = 'IT': Hanya memperbarui data untuk pegawai yang bekerja di departemen IT.

---

### **✅ c. `UPDATE` dengan Subquery**
- Bisa digunakan untuk mengambil nilai dari tabel lain tanpa `JOIN`.

```sql
UPDATE Pegawai
SET Gaji = (SELECT AVG(Gaji) FROM Pegawai WHERE Departemen = 'IT')
WHERE Departemen = 'Marketing';
```
- UPDATE Pegawai: Memperbarui tabel Pegawai.

- SET Gaji = (SELECT AVG(Gaji) FROM Pegawai WHERE Departemen = 'IT'): Mengatur nilai kolom Gaji dengan rata-rata gaji dari pegawai di departemen IT.

- WHERE Departemen = 'Marketing': Hanya memperbarui data untuk pegawai yang bekerja di departemen Marketing.

---

### **✅ d. `UPDATE` dengan `CASE`**
- Bisa mengupdate berdasarkan kondisi dalam satu perintah.

```sql
UPDATE Pegawai
SET Gaji = 
    CASE 
        WHEN Jabatan = 'Manager' THEN Gaji * 1.10
        WHEN Jabatan = 'Staff' THEN Gaji * 1.05
        ELSE Gaji
    END;
```
- UPDATE Pegawai: Memperbarui tabel Pegawai.

- SET Gaji = CASE ... END: Mengatur nilai kolom Gaji berdasarkan kondisi yang ditentukan.

- WHEN Jabatan = 'Manager' THEN Gaji * 1.10: Jika Jabatan = 'Manager', gaji dinaikkan sebesar 10%.

- WHEN Jabatan = 'Staff' THEN Gaji * 1.05: Jika Jabatan = 'Staff', gaji dinaikkan sebesar 5%.

- ELSE Gaji: Jika Jabatan bukan 'Manager' atau 'Staff', gaji tetap tidak berubah.


---

### **✅ e. `UPDATE` dengan `TOP` (Hanya Beberapa Baris)**
- Bisa membatasi jumlah baris yang diupdate.

```sql
UPDATE TOP (5) Pegawai
SET Gaji = Gaji * 1.10
WHERE Departemen = 'Sales';
```
- UPDATE TOP (5): Memperbarui hanya 5 baris pertama yang memenuhi kondisi.

- Pegawai: Nama tabel yang akan diperbarui.

- SET Gaji = Gaji * 1.10: Menaikkan gaji sebesar 10%.

- WHERE Departemen = 'Sales': Hanya memperbarui data untuk pegawai yang bekerja di departemen Sales


---

## **📌 4. Fleksibilitas `UPDATE`**
| **Fitur**  | **Penjelasan** |
|----------------|----------------|
| **Gunakan `WHERE` untuk Update Spesifik** | Mencegah semua data berubah tidak sengaja. |
| **`JOIN` untuk Update dari Tabel Lain** | Berguna untuk update berdasarkan tabel lain. |
| **`CASE` untuk Update Berbeda dalam Satu Query** | Menghemat banyak query terpisah. |
| **`TOP` untuk Update Terbatas** | Berguna jika hanya ingin mengupdate beberapa baris. |
| **`OUTPUT` untuk Melihat Hasil Perubahan** | Berguna untuk debugging. |

**Contoh `OUTPUT INSERTED`**:
```sql
UPDATE Pegawai
SET Gaji = Gaji * 1.10
OUTPUT INSERTED.ID, INSERTED.Gaji
WHERE Departemen = 'IT';
```

---

## **📌 5. Performa & Best Practices**
### **✅ a. Selalu Gunakan `WHERE` untuk Mencegah Kesalahan**
```sql
UPDATE Pegawai
SET Jabatan = 'CEO'  -- ❌ Semua baris berubah!
WHERE ID = 1;  -- ✅ Hanya baris dengan ID = 1 yang berubah.
```

### **✅ b. Gunakan `TRANSACTION` untuk Update Banyak Data**
```sql
BEGIN TRANSACTION;
UPDATE Pegawai
SET Gaji = Gaji * 1.10
WHERE Departemen = 'IT';

-- Jika terjadi error, rollback perubahan
ROLLBACK;

-- Jika berhasil, simpan perubahan
COMMIT;
```

### **✅ c. Gunakan `INDEX` pada Kolom yang Sering Diperbarui**
- Jika sering mengupdate berdasarkan `WHERE`, gunakan `INDEX` agar pencarian lebih cepat.

```sql
CREATE INDEX idx_pegawai_departemen ON Pegawai(Departemen);
```

### **✅ d. Gunakan `WITH (NOLOCK)` untuk Performa Lebih Baik**
- Menghindari **locking** saat update pada tabel besar.

```sql
UPDATE Pegawai WITH (NOLOCK)
SET Gaji = Gaji * 1.10
WHERE Departemen = 'IT';
```
---

## **🚀 Kesimpulan**
✅ **`UPDATE` digunakan untuk mengubah data dalam tabel** dan harus digunakan dengan hati-hati.  
✅ **Selalu gunakan `WHERE`** untuk menghindari perubahan data secara tidak sengaja.  
✅ **Dapat dikombinasikan dengan `JOIN`, `CASE`, `TOP`, dan `SUBQUERY`** untuk fleksibilitas tinggi.  
✅ **Gunakan `TRANSACTION` saat update dalam jumlah besar** untuk menjaga konsistensi data.  
