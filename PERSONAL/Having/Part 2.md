**Bisa** menggunakan **subquery di `HAVING`**, selama subquery tersebut mengembalikan nilai yang valid untuk digunakan dalam kondisi **filtering**.

---

## **📌 Contoh 1: Menggunakan Subquery di `HAVING` untuk Perbandingan**
Misalkan ada tabel **Employees** dengan kolom:
- `id`
- `department`
- `salary`

Kita ingin menampilkan departemen yang memiliki **rata-rata gaji lebih tinggi daripada rata-rata semua karyawan**.

```sql
SELECT department, AVG(salary) AS avg_salary
FROM Employees
GROUP BY department
HAVING AVG(salary) > (SELECT AVG(salary) FROM Employees);
```
**Penjelasan:**
- `GROUP BY department` → Mengelompokkan berdasarkan departemen.
- `AVG(salary)` → Menghitung rata-rata gaji setiap departemen.
- `HAVING AVG(salary) > (SELECT AVG(salary) FROM Employees);`  
  - Menggunakan **subquery** di `HAVING` untuk membandingkan rata-rata gaji departemen dengan rata-rata semua karyawan.

---

## **📌 Contoh 2: Menggunakan Subquery di `HAVING` dengan `COUNT`**
Misalkan ada tabel **Orders** dengan kolom:
- `customer_id`
- `order_id`

Kita ingin menampilkan **customer yang jumlah pesanannya lebih besar dari jumlah pesanan customer dengan ID = 5**.

```sql
SELECT customer_id, COUNT(order_id) AS total_orders
FROM Orders
GROUP BY customer_id
HAVING COUNT(order_id) > (SELECT COUNT(order_id) FROM Orders WHERE customer_id = 5);
```

**Penjelasan:**
- `COUNT(order_id)` → Menghitung jumlah pesanan setiap customer.
- `HAVING COUNT(order_id) > (SELECT COUNT(order_id) FROM Orders WHERE customer_id = 5);`  
  - Memeriksa apakah jumlah pesanan **lebih besar** dari jumlah pesanan customer **ID = 5**.

---

## **🔴 Kapan Subquery di `HAVING` Tidak Bisa Digunakan?**
1. **Jika subquery mengembalikan lebih dari satu baris**  
   - `HAVING` hanya bisa dibandingkan dengan satu nilai (`<`, `>`, `=`, dll.).
   - Jika subquery mengembalikan banyak baris, harus menggunakan `IN()` atau `ANY()`.

   ❌ **Salah**
   ```sql
   SELECT department, COUNT(*) 
   FROM Employees 
   GROUP BY department
   HAVING COUNT(*) > (SELECT COUNT(*) FROM Employees GROUP BY department);
   ```
   - Subquery mengembalikan lebih dari satu nilai (karena `GROUP BY` di dalamnya).

   ✅ **Benar (gunakan `ANY`)**
   ```sql
   HAVING COUNT(*) > ANY (SELECT COUNT(*) FROM Employees GROUP BY department)
   ```

2. **Jika subquery tidak mengembalikan hasil yang valid**  
   - Misalnya, subquery mengembalikan `NULL`, maka perbandingan di `HAVING` bisa gagal.

---

## **Kesimpulan**
✔ **BISA** menggunakan **subquery di `HAVING`**, terutama untuk perbandingan agregat seperti `AVG()`, `COUNT()`, `SUM()`, dll.  
✔ Berguna untuk **memfilter data setelah `GROUP BY`** berdasarkan nilai yang berasal dari **subquery**.  
❌ **Tidak bisa jika subquery mengembalikan banyak nilai tanpa `IN()` atau `ANY()`**.
