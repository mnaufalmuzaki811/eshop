# Refleksi 1

### 1. Clean Code Principles
Selama pengerjaan fitur *Edit* dan *Delete* ini, saya mencoba menerapkan beberapa prinsip *Clean Code* agar kodingan saya gak cuma "jalan", tapi juga rapi:

* **Meaningful Names (Penamaan yang Jelas):** Saya berusaha kasih nama variabel dan function yang deskriptif. Contohnya `productService`, `findById`, dan `update`. Jadi, pas baca kodenya lagi, saya gak perlu nebak-nebak ini fungsinya buat apa. Saya juga misahin `ProductController`, `ProductService`, dan `ProductRepository` supaya jelas tanggung jawab masing-masing.

* **Single Responsibility Principle (SRP):** Saya menjaga agar setiap class punya tugas yang spesifik sesuai arsitektur MVC:
    * **Controller:** Cuma buat ngatur lalu lintas *request* dari user.
    * **Service:** Tempat mikir logika bisnisnya.
    * **Repository:** Fokus urusin data (simpan/ambil dari List).
      Dengan begini, kodenya jadi lebih modular dan gampang di-maintain.

### 2. Secure Coding Practices
Untuk keamanan (walau masih dasar), saya menerapkan hal berikut:

* **UUID for IDs:** Alih-alih pakai angka urut (1, 2, 3...) untuk ID produk, saya pakai `UUID`. Ini penting banget buat ngehindarin orang iseng yang coba-coba nebak ID produk lain (IDOR). Kalau pakai UUID, ID-nya jadi acak dan susah ditebak.

### 3. Mistakes & Future Improvements
Setelah saya review ulang, jujur ada satu kebiasaan kurang aman yang saya lakuin di fitur **Delete**:

* **Kekurangan:** Saya men-trigger proses *delete* menggunakan method `GET` (lewat link `<a>` biasa di HTML dan `@GetMapping` di Controller).

    ```html
    <a href="/product/delete/{id}">Delete</a>
    ```

* **Kenapa ini bahaya?** Menurut standar HTTP, method `GET` itu harusnya cuma buat *read data* aja, bukan buat ngubah atau hapus data. Kalau pakai GET buat delete, aplikasi jadi rentan kena serangan **CSRF (Cross-Site Request Forgery)**. Bisa aja ada orang jahat bikin link jebakan, dan kalau admin ngeklik, produknya kehapus tanpa sadar.

* **Rencana Perbaikan:**
  Nantinya, saya harus ubah mekanisme ini jadi menggunakan method `POST` (atau `DELETE` kalau pakai JS). Di sisi HTML, tombol delete-nya harus dibungkus dalam `<form>`.

    ```html
    <form action="/product/delete/{id}" method="post">
       <button type="submit">Delete</button>
    </form>
    ```