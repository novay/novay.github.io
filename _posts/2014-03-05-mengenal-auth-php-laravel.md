---
layout: post
title: Mengenal Auth.php milik Laravel
description: "Sedikit penjelasan mengenai eloquent dan databas pada Auth Laravel."
modified: 2014-03-05
category: blog
tags: [tips, laravel, php]
comments: true
share: true
---

**Auth.php** disini singkatnya bertugas menangani segala fungsi autentikasi pada aplikasi yang kita bangun.

Sedikit penerangan, file **auth.php** ini berada dalam `proyek-laravel/app/config/auth.php`

Dalam autentikasi laravel ada 2 macam driver yang didukung :

- database
- eloquent

Coba perhatikan baris `18`, secara default laravel akan tertulis seperti ini : `'driver' => 'eloquent',`. Anda bisa mengubah `eloquent` menjadi `database` bila mau.

##Database

Bila Anda menggunakan `database`, maka jenis autentikasi yang anda gunakan langsung melalui table dari database Anda.
Maksudnya, data yang digunakan untuk proses autentikasi langsung berasal dari tabel dalam database. 

Driver `database` sanggat erat kaitannya dengan `'table' => 'users',` pada baris `44` dalam file yang sama. Karena bila Anda menggunakan driver ini, maka disinilah letak penamaan tabel yang Anda gunakan sebagai pengelola aplikasi. Contoh bila anda menggunakan nama tabel `pengguna`, maka ubah `users` menjadi `pengguna`.

##Eloquent

Bila driver `database` berhubungan langsung dengan isi database tanpa melalui perantara, lain halnya untuk driver `eloquent`, diantara hubungan antara `eloquent` dengan isi database, ada *mak comblang* bernama **Models**. Jadi, logikanya begini : `isi database -ditarik-oleh-> Models -dikirim-ke-> Auth`.

Coba perhatikan syntax pada baris `31` dalam file yang sama,  disana tertulis `'model' => 'User',` yang artinya model yang Anda gunakan bernama `User`. Sekarang pertanyaannya adalah, `User` ini *anak mana*? *rumahnya dimana*?

Oke, buat yang belum tau ane kasih tau nih, rumah si `User` ada di `proyek-laravel/models/User.php`. Jadi, `User` disini berasal dari `User`nya `User.php`. Kalau nama filenya `Pengguna.php` berarti ubah `User` menjadi `Pengguna` di `Auth.php`.

Sekian dan **Happy Coding**.
