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

Secara garis besar, **Route** disini bertujuan untuk menangani peng-ALAMATAN atau URL website kita. Sekarang saatnya kita berkhayal. Kira-kira ada berapa halaman yang akan kita gunakan untuk aplikasi kita. 

Routes di laravel bernama `routes.php` berada di `proyek-laravel/app/routes.php`.

Untuk aplikasi autentikasi ini kita hanya butuh :

- Halaman Home **(localhost:8000/)**
- Halaman Login **(localhost:8000/login)**
- Halaman Beranda yang akan diakses setelah pengguna login **(localhost:8000/beranda)**
- Halaman Logout

Jadi, bisa disiapkan 4 route untuk menuju ke halaman tersebut seperti berikut :

{% highlight php %}
// proyek-laravel/app/routes.php

<?php 
# Halaman Home yang nantinya berisi tombol login (localhost:8000/)
Route::get('/', function() {
	return 'Halaman Home Aplikasi';
});
# Halaman login (localhost:8000/login)
Route::get('login', function() {
	return 'Halaman Login';
});
# Halaman beranda yg di akses setelah login (localhost:8000/beranda) 
Route::get('beranda', array('before' => 'auth', function() {
	return 'Halaman Beranda';
}));
# Halaman logout (localhost:8000/user/logout) 
Route::get('user/logout', array('before' => 'auth', function() {
	return 'Halaman Beranda';
}));
?>
{% endhighlight %}

Sekarang kalian bisa coba akses keempat link terlebih dahulu melalui browser. Untuk route ketiga **(localhost:8000/beranda)** dan keempat **(localhost:8000/user/logout)** kita akan secara langsung di lempar ke halaman `localhost:8000/login`, kenapa? Karena terdapat `filter` didalamnya, yaitu `'before' => 'auth'` yang artinya halaman itu hanya bisa diakses oleh member atau pengguna yang telah melalui proses login.

Semoga kalian dapat gambaran tentang fungsi route walaupun cuma sedikit disini. 

- Bila sudah, sekarang kita fokus ke Route pertama, ubah jadi seperti berikut :

{% highlight php %}
// proyek-laravel/app/routes.php

<?php 
# Halaman Home yang nantinya berisi tombol login (localhost:8000/)
Route::get('/', array('as' => 'index', 'uses' => 'AuthController@getIndex'));

# Halam...
Route::get('login', function() {
	....
?>
{% endhighlight %}

Terdapat perubahan **function()** menjadi **array** disini, dan perhatikan sekarang kita memiliki 2 nilai dalam route :

####**as** bernilai **index**

**as** disini berfungsi sebagai identitas si `route` itu sendiri. Sedikit bocoran, biasanya identitas ini digunakan pada sesi **Views** dengan cara `route('index')` atau `URL::route('index')` atau pada sesi **Controller**dengan cara `return Redirect::route('index');`. Tenang, nanti kalian akan paham sendiri kok. XD

####**uses** bernilai **AuthController@getIndex**

Anda menggunakan **uses** berarti Anda menerima untuk menyerahkan tugas dari **route** menuju **Controllers**. Sesuai arti **uses** artinya menggunakan. Jadi gampangnya bahasanya begini, **Route** ini menggunakan **Controller** bernama **AuthController** dengan method bernama **getIndex** untuk selanjutnya menjalankan tugasnya.

- Untuk route kedua, ubah jadi seperti berikut :

{% highlight php %}
...
# Hala...
Route::get('/', array(...

# Halaman login (localhost:8000/login)
Route::get('login', array('as' => 'masuk', 'uses' => 'AuthController@getMasuk'));

# Hala...
Route::get('beranda', array(...
...
{% endhighlight %}

Sama seperti sebelumnya, sekarang route ini memiliki 2 nilai, identitas dan pewaris. XD

- Route ketiga, ubah jadi seperti berikut :

{% highlight php %}
...
Halaman log...
Route::get('login...);

# Halaman beranda yg di akses setelah login (localhost:8000/beranda) 
Route::get('beranda', array('before' => 'auth', 'as' => 'admin', 'uses' => 'AuthController@getAdmin'));

# Halaman logout...
Route::get('user/logout...
...
{% endhighlight %}

- Dan untuk route terakhir, ubah jadi seperti berikut :

{% highlight php %}
...
# Halaman berand...
Route::get('beranda', ar...

# Halaman logout (localhost:8000/user/logout) 
Route::get('user/logout', array('before' => 'auth', 'as' => 'keluar', 'uses' => 'AuthController@getKeluar'));
...
{% endhighlight %}

**OKE**, sekarang urusan si **route** sudah selesai dan semua tanggung jawabnya telah dilimpahkan ke **Controller**.

### Controllers

### Views

SAMBIL JALAN YA...