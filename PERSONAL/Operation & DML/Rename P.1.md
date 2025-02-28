# **📌 RENAME di SQL Server: Penjelasan Lengkap**  

Di SQL Server, **`RENAME`** digunakan untuk mengubah nama objek dalam database, seperti tabel, kolom, dan indeks. Namun, **SQL Server tidak memiliki perintah `RENAME` langsung** seperti di beberapa sistem database lain (misalnya, MySQL). Sebagai gantinya, SQL Server menyediakan **`sp_rename`** untuk mengubah nama objek.

---

## **📝 1. Menggunakan `sp_rename` untuk RENAME di SQL Server**
### **📌 a. Sintaks Dasar `sp_rename`**
```sql
EXEC sp_rename 'nama_lama', 'nama_baru', 'TABEL/KOLOM/INDEKS';
```
- `'nama_lama'` → Nama objek yang ingin diubah.
- `'nama_baru'` → Nama baru untuk objek tersebut.
- `'TABEL/KOLOM/INDEKS'` → Jenis objek yang diubah.

---

## **📌 2. RENAME pada Berbagai Objek**
### **✅ a. Mengubah Nama Tabel**
```sql
EXEC sp_rename 'NamaTabelLama', 'NamaTabelBaru';
```
📌 **Catatan**:
- Mengubah nama tabel tidak akan mengubah nama constraint atau hubungan **`FOREIGN KEY`**.
- Jika tabel digunakan dalam **VIEW, STORED PROCEDURE, atau TRIGGER**, nama baru harus diperbarui secara manual.

---

### **✅ b. Mengubah Nama Kolom**
```sql
EXEC sp_rename 'NamaTabel.NamaKolomLama', 'NamaKolomBaru', 'COLUMN';
```
📌 **Catatan**:
- Jika ada **INDEX atau CONSTRAINT** yang bergantung pada kolom, mungkin perlu diperbarui.
- Pastikan tidak ada **VIEW atau STORED PROCEDURE** yang masih menggunakan nama kolom lama.

---

### **✅ c. Mengubah Nama Index**
```sql
EXEC sp_rename 'NamaTabel.IndexLama', 'IndexBaru', 'INDEX';
```
📌 **Catatan**:
- Tidak mengubah definisi index, hanya nama.

---

## **📌 3. Aturan & Syarat Penggunaan `sp_rename`**
| **Aturan** | **Penjelasan** |
|------------|---------------|
| **Nama Baru Harus Unik** | Nama yang digunakan tidak boleh sudah ada dalam database. |
| **Tidak Bisa Rename Primary Key** | Jika sebuah kolom adalah PRIMARY KEY, harus dihapus dulu sebelum mengganti nama. |
| **Mengubah Nama Tabel Tidak Mengubah Nama Constraint** | Constraint seperti `FOREIGN KEY` tidak ikut berubah. |
| **Tidak Bisa Digunakan untuk Rename Schema** | Tidak bisa mengganti schema dari suatu tabel, misalnya dari `dbo.Tabel1` ke `HR.Tabel1`. |
| **Tidak Bisa Rename View Langsung** | Untuk mengubah nama view, gunakan `ALTER VIEW` atau buat ulang view. |

---

## **📌 4. Fleksibilitas & Keterbatasan `RENAME`**
| **Fleksibilitas** | **Keterbatasan** |
|-------------------|-----------------|
| Bisa mengubah nama tabel, kolom, dan indeks | Tidak bisa mengubah nama **schema** |
| Tidak mengubah data yang tersimpan dalam tabel | Tidak bisa digunakan untuk **constraint** langsung |
| Bisa digunakan pada hampir semua objek database | Bisa menyebabkan error jika ada **foreign key atau trigger** yang bergantung pada nama lama |

---

## **📌 5. Best Practices dalam `RENAME`**
✅ **Periksa apakah objek sedang digunakan dalam VIEW, STORED PROCEDURE, atau TRIGGER** sebelum mengganti nama.  
✅ **Pastikan nama baru tidak bentrok dengan objek lain** dalam database.  
✅ **Gunakan `sp_rename` dengan hati-hati**, karena tidak ada perintah `UNDO`.  
✅ **Jika mengganti nama tabel atau kolom penting, pastikan untuk memperbarui dokumentasi** dan kode aplikasi yang mengakses database.  

---

## **📌 6. Contoh Penggunaan `RENAME`**
### **Contoh 1: Rename Tabel**
```sql
EXEC sp_rename 'Pegawai', 'Karyawan';
```
📌 **Mengubah nama tabel `Pegawai` menjadi `Karyawan`**.

---

### **Contoh 2: Rename Kolom**
```sql
EXEC sp_rename 'Karyawan.NamaDepan', 'NamaLengkap', 'COLUMN';
```
📌 **Mengubah nama kolom `NamaDepan` menjadi `NamaLengkap` dalam tabel `Karyawan`**.

---

### **Contoh 3: Rename Index**
```sql
EXEC sp_rename 'Karyawan.IX_NamaDepan', 'IX_NamaLengkap', 'INDEX';
```
📌 **Mengubah nama indeks `IX_NamaDepan` menjadi `IX_NamaLengkap`**.

---

## **🚀 Kesimpulan**
✅ **SQL Server tidak memiliki perintah `RENAME`, tetapi bisa menggunakan `sp_rename`**.  
✅ **Dapat digunakan untuk mengganti nama tabel, kolom, dan indeks**.  
✅ **Pastikan nama baru unik dan tidak digunakan oleh objek lain**.  
✅ **Gunakan dengan hati-hati karena tidak ada perintah `UNDO`**.  
