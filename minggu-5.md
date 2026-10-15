1. Tujuan
Praktikum ini bertujuan menerapkan operasi CRUD (Create, Read, Update, Delete) pada resource utama melalui REST API menggunakan Laravel. Praktikum juga mencakup validasi input, penggunaan API Resource untuk membentuk response yang konsisten, penanganan error, pengujian endpoint melalui Postman, serta pembaruan dokumentasi dan collection.
2. Perubahan yang Dilakukan
Implementasi pada project menggunakan resource Book dengan endpoint berawalan /api/books.
Perubahan yang dikerjakan atau perlu diverifikasi pada repository:
- Menyiapkan endpoint untuk membuat, membaca daftar, membaca detail, memperbarui, dan menghapus buku.
- Menggunakan Form Request untuk memvalidasi data yang masuk.
- Menggunakan API Resource untuk menjaga struktur response sukses tetap konsisten.
- Menangani kondisi data tidak ditemukan dan input tidak valid dengan status HTTP yang sesuai.
- Menyiapkan pengujian request di Postman.
- Mendokumentasikan contract, hasil pengujian, dan error case pada docs/minggu-5.md.
Pastikan daftar di atas sesuai dengan file dan implementasi yang benar-benar ada di repository sebelum dikumpulkan.

3. Endpoint dan API Contract
Operasi	Method	Endpoint	Hasil yang diharapkan
Daftar buku	GET	/api/books	200 OK
Detail buku	GET	/api/books/{id}	200 OK jika ditemukan
Tambah buku	POST	/api/books	201 Created jika berhasil
Ubah sebagian data	PATCH	/api/books/{id}	Status sukses sesuai contract
Hapus buku	DELETE	/api/books/{id}	200 OK atau 204 No Content, sesuai contract


Format body untuk membuat buku mengikuti field yang digunakan project:
{
  "title": "Belajar Laravel",
  "isbn": "9780000000456",
  "available": true
}
ISBN harus memenuhi aturan validasi project dan belum pernah digunakan. Contoh di atas hanya data uji; gunakan ISBN yang valid menurut aturan project dan tidak bentrok dengan data yang sudah ada.
Konsistensi response
- Response sukses mengikuti bentuk API Resource dan API contract project.
- Input yang tidak lolos validasi mengembalikan 422 Unprocessable Entity dengan rincian pada field errors.
- Resource yang tidak ditemukan mengembalikan 404 Not Found sesuai format error yang diterapkan project.
- Jangan mengubah format response hanya agar sama dengan contoh laporan; API contract repository menjadi acuan utama.
4. Bukti Pengujian
Pengujian dilakukan melalui Postman pada server lokal Laravel (http://127.0.0.1:8000).
4.1 Create berhasil
- Request: POST /api/books
- Hasil yang sudah terlihat: 201 Created
- Data contoh: buku berjudul Refactoring, ISBN 9780000000123, available: true.
- ID yang dikembalikan pada screenshot: 4.
- Kesimpulan: operasi create berhasil pada pengujian tersebut.
Bukti: Sisipkan screenshot Postman yang memperlihatkan request dan status 201 Created.
4.2 Validasi gagal
- Request: POST /api/books
- Hasil yang sudah terlihat: 422 Unprocessable Entity
- Input uji: title kosong dan ISBN berisi 123.
- Pesan yang terlihat: title wajib diisi dan ISBN harus 13 karakter.
- Kesimpulan: validasi menolak data yang tidak memenuhi aturan.
Bukti: Sisipkan screenshot Postman yang memperlihatkan status 422 dan object errors.
4.3 Detail buku
- Request: GET /api/books/{id}
- Status terakhir pada screenshot: 404 Not Found saat meminta /api/books/1.
- Kesimpulan: pengujian detail belum dapat dinyatakan berhasil. Uji ulang menggunakan ID yang benar-benar ada, misalnya ID 4 dari response create, dan catat hasil aktualnya.
Bukti: Sisipkan screenshot setelah pengujian ulang. Jangan menyatakan 200 OK sebelum hasil itu benar-benar didapatkan.
4.4 Update sebagian
- Request: PATCH /api/books/{id}
- Body yang digunakan: {"available": false}
- Status: Belum terverifikasi berhasil dari bukti yang tersedia. Uji ulang dengan ID buku yang masih ada, lalu catat status dan response sebenarnya.
4.5 Delete
- Request: DELETE /api/books/{id}
- Status: Belum terverifikasi berhasil dari bukti yang tersedia. Uji menggunakan ID buku yang memang ada, kemudian periksa status response dan pastikan resource sudah tidak dapat ditemukan.
4.6 Daftar buku
- Request: GET /api/books
- Status: Perlu dijalankan dan dicatat dari Postman sebelum laporan dikumpulkan.
5. Error Case
Kasus	Input/skenario	Status yang diharapkan	Hasil
Validasi title	title kosong	422	Terlihat pada pengujian
Format ISBN	ISBN kurang dari 13 karakter	422	Terlihat pada pengujian
ISBN duplikat	ISBN sudah terdaftar	422	Terlihat pada pengujian
Buku tidak ditemukan	GET ID yang tidak ada	404	Terlihat untuk /api/books/1; pastikan ID memang tidak ada
Update resource tidak ada	PATCH ID yang tidak ada	404 jika sesuai contract	Perlu diverifikasi
Delete resource tidak ada	DELETE ID yang tidak ada	404 jika sesuai contract	Perlu diverifikasi


Status dan pesan akhir harus mengikuti API contract dan implementasi yang ada. Error yang disengaja pada request Validation Gagal dapat dipakai sebagai bukti pengujian error, tetapi jangan menyebutnya sebagai pengujian sukses.
6. Pembaruan Postman dan Dokumentasi
Collection Postman yang digunakan adalah Minggu 3 — API Contract, dengan request seperti:
- Collection Berhasil
- Detail Berhasil
- Create Berhasil
- Validation Gagal
- Update Sebagian
- Not Found
- Delete buku
- buku terhapus
Sebelum dikumpulkan:
1. Pastikan URL dan method setiap request sesuai contract.
2. Gunakan ID hasil create atau ID yang dipastikan ada untuk tes detail, update, dan delete.
3. Tambahkan atau perbarui assertion/test script jika diwajibkan.
4. Simpan perubahan collection dan ekspor collection terbaru jika diminta dosen.
5. Perbarui docs/minggu-5.md dengan status pengujian yang benar serta screenshot pendukung.
7. Keamanan
- Tidak menaruh API key, token, password, cookie, atau secret pada dokumentasi maupun collection.
- Periksa file .env dan pastikan tidak ikut di-commit.
- Jangan mencantumkan kredensial atau data sensitif pada screenshot.
- Pastikan file database lokal atau file lain yang mengandung data sensitif tidak ikut di-commit jika tidak diperlukan.
8. Kesimpulan
Pengujian Postman telah menunjukkan bahwa endpoint create dapat mengembalikan 201 Created dan validasi input dapat mengembalikan 422 Unprocessable Entity untuk data yang tidak valid. Pengujian juga memperlihatkan respons 404 Not Found ketika detail buku diminta dengan ID yang tidak ditemukan. Namun, keberhasilan endpoint daftar, detail dengan ID yang benar, update, dan delete masih harus diverifikasi dan dicatat sebelum tugas dinyatakan lengkap. Konsistensi akhir perlu diperiksa terhadap API contract, implementasi Laravel, dan collection Postman.
9. Referensi
1. Dokumentasi Laravel — Validation: https://laravel.com/docs/validation
2. Dokumentasi Laravel — Eloquent API Resources: https://laravel.com/docs/eloquent-resources
3. Dokumentasi Laravel — Routing: https://laravel.com/docs/routing
4. Dokumentasi Postman — Send requests: https://learning.postman.com/docs/sending-requests/requests/
5. docs/api-contract.md pada repository project.
6. Materi Minggu 5 dan Hasil Praktikum 5 dari kelas.
10. Deklarasi Penggunaan AI
Saya menggunakan ChatGPT sebagai alat bantu untuk menyusun kerangka laporan, merapikan penjelasan, dan memahami hasil pengujian HTTP. Implementasi, verifikasi endpoint, penyesuaian terhadap API contract, serta pemeriksaan akhir repository harus saya lakukan dan pastikan sendiri. Isi laporan akan saya sesuaikan dengan hasil aktual project dan aturan kelas.