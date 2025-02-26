Memahami kapan harus menggunakan subquery dalam `JOIN` adalah keterampilan penting dalam menulis query SQL. Berikut adalah **panduan langkah demi langkah** untuk membantu Anda berpikir seperti itu ketika menghadapi masalah serupa di masa depan:

---

### **1. Pahami Masalah yang Dihadapi**
Sebelum menulis query, pastikan Anda benar-benar memahami apa yang ingin dicapai. Misalnya:
- Apakah Anda perlu menggabungkan data dari beberapa tabel?
- Apakah Anda perlu mengelompokkan atau mengagregasi data sebelum menggabungkannya?
- Apakah ada kondisi khusus yang perlu diterapkan sebelum menggabungkan data?

Dalam kasus Anda:
- Anda perlu menggabungkan data dari `View_Stats` dan `Submission_Stats` dengan tabel lain.
- Anda perlu mengelompokkan dan mengagregasi data (misalnya, menjumlahkan `total_views` dan `total_submissions`) sebelum menggabungkannya.

---

### **2. Identifikasi Kebutuhan Agregasi**
Jika Anda perlu mengelompokkan atau mengagregasi data sebelum menggabungkannya, **subquery** adalah alat yang tepat. Contoh kebutuhan agregasi:
- Menghitung total (`SUM`), rata-rata (`AVG`), jumlah baris (`COUNT`), dll.
- Mengelompokkan data berdasarkan kolom tertentu (`GROUP BY`).

Dalam kasus Anda:
- Anda perlu menghitung total `total_views`, `total_unique_views`, `total_submissions`, dan `total_accepted_submissions` untuk setiap `challenge_id`.

---

### **3. Pertimbangkan Performa**
Menggabungkan tabel besar secara langsung (tanpa subquery) bisa menghasilkan banyak baris sementara (intermediate rows), yang dapat memperlambat query. Subquery membantu:
- Mengurangi jumlah baris yang perlu di-join dengan mengelompokkan dan mengagregasi data terlebih dahulu.
- Memastikan bahwa hanya data yang relevan yang di-join.

Dalam kasus Anda:
- Tanpa subquery, join langsung antara `View_Stats` dan `Submission_Stats` akan menghasilkan banyak baris sementara.
- Dengan subquery, data diagregasi terlebih dahulu, sehingga join lebih efisien.

---

### **4. Pikirkan tentang NULL Handling**
Jika ada kemungkinan data yang hilang (`NULL`) dalam tabel yang di-join, subquery dapat membantu:
- Mengelompokkan dan mengagregasi data terlebih dahulu memastikan bahwa `NULL` dihandle dengan benar.
- Fungsi agregasi seperti `SUM` dan `COUNT` secara otomatis mengabaikan `NULL`.

Dalam kasus Anda:
- Jika ada `challenge_id` yang tidak memiliki data di `View_Stats` atau `Submission_Stats`, subquery memastikan bahwa `NULL` dihandle dengan benar.

---

### **5. Gunakan Subquery Ketika Data Perlu Diproses Sebelum Join**
Jika data perlu diproses (misalnya, difilter, dikelompokkan, atau diagregasi) sebelum digabungkan, **subquery** adalah solusi yang tepat. Contoh:
- Anda perlu menghitung total penjualan per produk sebelum menggabungkannya dengan tabel pelanggan.
- Anda perlu mengelompokkan data log berdasarkan pengguna sebelum menggabungkannya dengan tabel pengguna.

Dalam kasus Anda:
- Data di `View_Stats` dan `Submission_Stats` perlu diagregasi per `challenge_id` sebelum digabungkan dengan tabel lain.

---

### **6. Contoh Langkah Berpikir**
Misalkan Anda menghadapi masalah berikut:
- Anda memiliki tabel `Orders` (pesanan) dan `OrderDetails` (detail pesanan).
- Anda ingin menampilkan total pendapatan per pelanggan.

#### Langkah Berpikir:
1. **Pahami Masalah**:
   - Anda perlu menggabungkan data dari `Orders` dan `OrderDetails`.
   - Anda perlu menghitung total pendapatan (`SUM(quantity * price)`) untuk setiap pelanggan.

2. **Identifikasi Kebutuhan Agregasi**:
   - Anda perlu mengelompokkan data berdasarkan `customer_id` dan menghitung total pendapatan.

3. **Pertimbangkan Performa**:
   - Menggabungkan `Orders` dan `OrderDetails` secara langsung akan menghasilkan banyak baris sementara.
   - Lebih efisien jika Anda mengelompokkan dan mengagregasi data di `OrderDetails` terlebih dahulu.

4. **Tulis Subquery**:
   ```sql
   SELECT 
       customer_id, 
       SUM(quantity * price) AS total_revenue
   FROM OrderDetails
   GROUP BY customer_id
   ```

5. **Gabungkan dengan Tabel Lain**:
   ```sql
   SELECT 
       O.customer_id, 
       C.customer_name, 
       OD.total_revenue
   FROM Orders O
   JOIN Customers C ON O.customer_id = C.customer_id
   JOIN (
       SELECT 
           customer_id, 
           SUM(quantity * price) AS total_revenue
       FROM OrderDetails
       GROUP BY customer_id
   ) OD ON O.customer_id = OD.customer_id;
   ```

---

### **7. Kapan Tidak Perlu Subquery?**
Subquery tidak selalu diperlukan. Jika:
- Data tidak perlu diagregasi atau dikelompokkan sebelum di-join.
- Tabel yang di-join kecil dan tidak mempengaruhi performa.
- Query sudah cukup sederhana tanpa subquery.

Contoh:
```sql
SELECT 
    C.customer_name, 
    O.order_date
FROM Customers C
JOIN Orders O ON C.customer_id = O.customer_id;
```

---

### **8. Latihan dan Praktik**
Cara terbaik untuk menguasai penggunaan subquery adalah dengan **berlatih**. Cobalah untuk:
- Menganalisis masalah dan memecahnya menjadi langkah-langkah kecil.
- Menulis query sederhana terlebih dahulu, lalu menambahkan subquery jika diperlukan.
- Mengevaluasi performa query dan membandingkan hasil dengan dan tanpa subquery.

---

### **Kesimpulan**
Anda perlu menggunakan subquery dalam `JOIN` ketika:
1. Data perlu diagregasi atau dikelompokkan sebelum di-join.
2. Performa query perlu dioptimalkan dengan mengurangi jumlah baris sementara.
3. Ada kebutuhan untuk menangani `NULL` atau data yang hilang.
