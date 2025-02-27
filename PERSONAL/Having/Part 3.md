**Bisa menggunakan `CASE` dalam `HAVING`** untuk membuat kondisi yang lebih fleksibel! 🎯

---

## **📌 Contoh 1: `HAVING` dengan `CASE` untuk Perbandingan Dinamis**
Misalkan ada tabel **Orders** dengan kolom:
- `customer_id`
- `order_id`
- `total_amount`

Kita ingin menampilkan **customer yang jumlah transaksinya lebih dari 5, kecuali jika `customer_id = 10`, maka batasnya hanya 3 transaksi**.

```sql
SELECT customer_id, COUNT(order_id) AS total_orders
FROM Orders
GROUP BY customer_id
HAVING COUNT(order_id) > 
       (CASE 
            WHEN customer_id = 10 THEN 3 
            ELSE 5 
        END);
```
**Penjelasan:**
- `COUNT(order_id)` menghitung jumlah pesanan setiap customer.
- **Gunakan `CASE` di `HAVING`** untuk membuat batas transaksi yang berbeda:
  - Jika **customer_id = 10**, batasnya **3**.
  - Jika **customer lain**, batasnya **5**.

---

## **📌 Contoh 2: `HAVING` dengan `CASE` + Subquery**
Misalkan ada tabel **Employees** dengan kolom:
- `id`
- `department`
- `salary`

Kita ingin menampilkan **departemen yang memiliki rata-rata gaji lebih besar dari 50000, kecuali departemen "HR", yang cukup lebih besar dari rata-rata gaji semua karyawan**.

```sql
SELECT department, AVG(salary) AS avg_salary
FROM Employees
GROUP BY department
HAVING AVG(salary) > 
       (CASE 
            WHEN department = 'HR' THEN (SELECT AVG(salary) FROM Employees) 
            ELSE 50000 
        END);
```
**Penjelasan:**
- Jika **department = 'HR'**, maka batas minimal gaji adalah **rata-rata semua karyawan** (subquery).
- Jika **department lain**, maka batasnya **50000**.
- `HAVING` dengan `CASE` memungkinkan kita **membuat aturan yang berbeda** untuk kondisi tertentu!

---

## **📌 Contoh 3: `HAVING` dengan `CASE` untuk Filtering Kustom**
Misalkan ada tabel **Sales** dengan kolom:
- `region`
- `sales_amount`

Kita ingin menampilkan **region yang memiliki total penjualan lebih dari 100000, kecuali untuk region "East", yang hanya butuh lebih dari 50000**.

```sql
SELECT region, SUM(sales_amount) AS total_sales
FROM Sales
GROUP BY region
HAVING 
    (CASE 
         WHEN region = 'East' THEN SUM(sales_amount) > 50000
         ELSE SUM(sales_amount) > 100000
     END);
```
> **Catatan:**  
> `CASE` dalam `HAVING` bisa langsung membandingkan hasil agregat **dengan kondisi berbeda berdasarkan nilai tertentu**.

---

## **🚀 Kesimpulan**
✔ **Bisa** menggunakan `CASE` dalam `HAVING` untuk kondisi **dinamis** dalam filter agregat.  
✔ Bisa **digabungkan dengan subquery** untuk batasan yang fleksibel.  
✔ Berguna untuk **membuat aturan yang berbeda untuk kategori tertentu**.
