# **📌 DELETE di SQL Server: Penjelasan Lengkap**  

## **1️⃣ Pengertian `DELETE` di SQL Server**
`DELETE` digunakan untuk **menghapus baris tertentu dalam tabel** berdasarkan kondisi yang diberikan dalam klausa **`WHERE`**. Jika `WHERE` tidak ditentukan, semua baris dalam tabel akan dihapus.

📌 **Perbedaan `DELETE` dengan `TRUNCATE`:**  
- `DELETE` dapat menghapus data **berdasarkan kondisi tertentu**.
- `TRUNCATE` akan **menghapus semua data dalam tabel tanpa pengecualian**.

---

## **2️⃣ Sintaks Dasar `DELETE`**
```sql
DELETE FROM nama_tabel
WHERE kondisi;
```
🔹 **`nama_tabel`** → Nama tabel tempat data akan dihapus.  
🔹 **`kondisi`** → Kondisi yang menentukan baris mana yang akan dihapus. Jika **tidak ada `WHERE`**, semua baris dalam tabel akan dihapus.  

---

## **3️⃣ Contoh Penggunaan `DELETE`**
### **✅ a. Menghapus Satu Baris Tertentu**
```sql
DELETE FROM Karyawan
WHERE ID_Karyawan = 5;
```
📌 **Menghapus satu baris di tabel `Karyawan` dengan `ID_Karyawan = 5`.**

---

### **✅ b. Menghapus Beberapa Baris dengan Kondisi**
```sql
DELETE FROM Karyawan
WHERE Departemen = 'IT';
```
📌 **Menghapus semua karyawan yang bekerja di departemen IT.**

---

### **✅ c. Menghapus Semua Baris dalam Tabel**
```sql
DELETE FROM Karyawan;
```
📌 **Menghapus semua data dalam tabel `Karyawan`, tetapi struktur tabel tetap ada.**

---

### **✅ d. Menghapus dengan Menggunakan `JOIN`**
```sql
DELETE Karyawan
FROM Karyawan
JOIN Departemen ON Karyawan.ID_Dept = Departemen.ID_Dept
WHERE Departemen.Nama = 'Finance';
```
- DELETE Karyawan: Menghapus baris dari tabel Karyawan.

- FROM Karyawan: Menentukan tabel utama yang akan dihapus datanya.

- JOIN Departemen ON Karyawan.ID_Dept = Departemen.ID_Dept: Menggabungkan tabel Karyawan dengan tabel Departemen berdasarkan kolom ID_Dept.

- WHERE Departemen.Nama = 'Finance': Hanya menghapus data untuk karyawan yang bekerja di departemen Finance.

📌 **Menghapus semua karyawan yang berada di departemen "Finance".**

---

### **✅ e. Menghapus dengan `TOP` (Menghapus Data Secara Bertahap)**
```sql
DELETE TOP (10) FROM Karyawan
WHERE Gaji < 5000000;
```
📌 **Menghapus 10 karyawan dengan gaji di bawah 5 juta.**

---

## **4️⃣ Aturan & Syarat Menggunakan `DELETE`**
| **Aturan** | **Penjelasan** |
|------------|---------------|
| **`WHERE` opsional** | Jika `WHERE` tidak digunakan, semua data dalam tabel akan dihapus. |
| **Bisa dikombinasikan dengan `JOIN`** | Bisa menghapus berdasarkan data dari tabel lain. |
| **Memicu `TRIGGER`** | Jika ada `AFTER DELETE TRIGGER`, maka akan dieksekusi saat data dihapus. |
| **Mendukung `ROLLBACK` dalam transaksi** | Bisa dikembalikan jika berada dalam transaksi `BEGIN TRANSACTION`. |
| **Lebih lambat dibandingkan `TRUNCATE`** | `DELETE` menghapus baris satu per satu, sehingga lebih lambat dibandingkan `TRUNCATE`. |

---

## **5️⃣ Perbedaan `DELETE` vs `TRUNCATE` vs `DROP`**
| **Fitur** | **DELETE** | **TRUNCATE** | **DROP** |
|----------|------------|-------------|---------|
| **Hapus Data Berdasarkan Kondisi** | ✅ Bisa dengan `WHERE` | ❌ Tidak bisa | ❌ Tidak bisa |
| **Hapus Semua Data** | ✅ Bisa (tanpa `WHERE`) | ✅ Bisa | ✅ Bisa |
| **Kecepatan Eksekusi** | ⏳ Lebih lambat (row by row) | ⚡ Lebih cepat (langsung) | 🚀 Sangat cepat (hapus tabel) |
| **Dapat Dibatalkan (`ROLLBACK`)** | ✅ Bisa | ❌ Tidak bisa | ❌ Tidak bisa |
| **Trigger Terpicu** | ✅ Ya | ❌ Tidak | ❌ Tidak |

---

## **6️⃣ `DELETE` dengan `OUTPUT` (Melihat Data yang Dihapus)**
Jika ingin melihat data yang akan dihapus sebelum benar-benar dihapus, gunakan **`OUTPUT`**:

```sql
DELETE FROM Karyawan
OUTPUT DELETED.*;
```
📌 **Menampilkan semua data yang telah dihapus.**

---

## **7️⃣ Transaksi dengan `DELETE` (`ROLLBACK` dan `COMMIT`)**
Jika ingin **menghapus data tetapi tetap bisa membatalkan (`ROLLBACK`) jika terjadi kesalahan**, gunakan **transaksi**:

```sql
BEGIN TRANSACTION;
DELETE FROM Karyawan WHERE ID_Karyawan = 10;

-- Jika ada kesalahan, batalkan
ROLLBACK;
-- Jika ingin menyimpan perubahan
COMMIT;
```
📌 **`ROLLBACK` akan membatalkan penghapusan jika ada kesalahan.**  
📌 **`COMMIT` akan menyimpan perubahan secara permanen.**

---

## **8️⃣ Fleksibilitas `DELETE`**
✅ **Bisa digunakan dengan `WHERE` untuk menghapus data spesifik**  
✅ **Bisa dikombinasikan dengan `JOIN` untuk menghapus berdasarkan data dari tabel lain**  
✅ **Dapat digunakan dalam transaksi (`BEGIN TRANSACTION`) untuk keamanan data**  
✅ **Memicu `TRIGGER` jika ada aturan bisnis yang harus dijalankan setelah `DELETE`**  

❌ **Tidak bisa menghapus seluruh tabel seperti `DROP TABLE`**  
❌ **Lebih lambat dibandingkan `TRUNCATE` jika ingin menghapus semua baris**  
❌ **Harus menggunakan `WHERE` untuk menghapus data tertentu, jika tidak, semua data akan terhapus**  

---

## **🚀 Kesimpulan**
✅ **`DELETE` digunakan untuk menghapus baris dalam tabel berdasarkan kondisi tertentu.**  
✅ **Jika `WHERE` tidak digunakan, semua data akan dihapus.**  
✅ **Lebih fleksibel dibandingkan `TRUNCATE`, tetapi lebih lambat.**  
✅ **Dapat dikombinasikan dengan `JOIN` dan transaksi (`ROLLBACK` dan `COMMIT`).**  
✅ **Sangat berguna untuk menghapus data tanpa menghapus struktur tabel.**  
