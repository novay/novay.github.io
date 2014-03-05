---
layout: post
title: Autentikasi Login Laravel - 2 Pembuatan Aplikasi
description: "Contoh penerapan pembuatan aplikasi autentikasi sederhana dengan menggunakan framework laravel 4 Bagian ke 2."
modified: 2014-03-05
category: blog
tags: [php, laravel, tutorial]
comments: true
share: true
---

Ini merupakan lanjutan dari panduan sebelumnya, apabila Anda belum membacanya silahkan kunjungi [Bagian 1]({{site.url}}/blog/2013/04/03/membuat-autentikasi-login-laravel-bagian-1/)...

Dari hasil percobaan sebelumnya, kita telah memiliki :

- Database, menggunakan SQLITE
- Tabel, berkat *migrations*
- Pengguna, berkat *seeding*

Ketiga poin diatas sebenarnya sudah cukup untuk kita sebagai bahan dasar membangun sebuah sistem. Ada beberapa hal lagi yang kita butuhkan untuk merealisasikannya, mereka adalah *models*, *routes*, *controllers* dan *views*.

### Models

Models pada laravel sebenarnya memiliki banyak fungsi, bahkan bisa bertindak sebagai "wadah logika program". Namun pada panduan kali ini, saya hanya akan menjadikan *models* sebagai sarana meng-*koneksikan aplikasi* dengan *database*. Karena peran sebenarnya memang *itu*. 

Perhatikan file `User.php` pada direktori `app/models` di baris 13. Terdapat baris syntax `protected $table = 'users';` yang maksudnya aplikasi Anda sejak awal telah memiliki Model bernama `User` yang berisi seluruh isi tabel bernama `users`. Untuk diketahui bahwa pada tahap pembuatan "database" sebelumnya, kita memang telah membuat sebuah tabel, tetapi tabel yang kita buat bukan bernama `users` melainkan `pengguna`. Jadi, Anda boleh mengubah `users` menjadi `pengguna`. Sudah tau maksudnya kan? Nama tabel vroh, nama tabel XD

Tips : Biasanya kalau saya pribadi, pembuatan model disini menyesuaikan banyaknya tabel yang saya punya pada aplikasi yang sedang saya bangun. Jadi, setiap model memiliki fungsi menangani isi satu tabel. Misal, dalam database saya memiliki tabel pengguna, artikel dan kategori, maka dalam *models* saya buat 3 file juga, `Pengguna.php`, `Artikel.php` dan `Kategori.php`. Kurang lebih seperti itu.

### Routes

Secara garis besar, **Route** disini bertujuan untuk menangani peng-ALAMATAN atau URL website kita.

### Controllers

### Views

SAMBIL JALAN YA...