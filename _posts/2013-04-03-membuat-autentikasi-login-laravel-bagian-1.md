---
layout: post
title: Autentikasi Login Laravel - 1 Pengolahan Database
description: "Contoh penerapan pembuatan aplikasi autentikasi sederhana dengan menggunakan framework laravel 4."
modified: 2014-04-13
category: blog
tags: [laravel, php, tutorial]
comments: true
share: true
---

Sekarang saya akan mencoba membuat halaman **login** sederhana seperti yang biasa kita lihat pada website-website yang menerapkan sistem **member** pada umumnya. 

Seperti misalnya **Facebook**. Itu loh, yang masukin `username` sama `password`.

###UPDATE

Berhubung kang Taylor Otwell sang empunya Laravel telah melakukan update pada Enginenya ini, saya yakin banyak diantara kalian yang akan menemukan masalah dalam tulisan ini, kecuali yang menggunakan laravel versi 4.1.25 kebawah.

Untuk itu, tulisan dibawah telah banyak dilakukan perubahan. Selamat belajar.

Untuk gambaran, saya berikan demo aplikasi berupa gif ketika proyek tutorial ini jadi :

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2013-04-03-membuat-autentikasi-login-laravel-bagian-1.gif" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2013-04-03-membuat-autentikasi-login-laravel-bagian-1.gif" width="500px"/>
		</a>
	</center>
</figure>

Juga buat yang ingin langsung oprek *source code*-nya bisa download di [GITHUB](https://github.com/novay/autentikasi-login-laravel)

Sebelum memulainya ada beberapa hal yang perlu Anda ketahui:

- Saya menggunakan **Windows 8** *(Status ori dipertanyakan)*
- Sistem yang dibangun menggunakan [Laravel](https://github.com/laravel/laravel) 
- Pengelola database ialah [SQLITE]() untuk memudahkan proses pembelajaran.

Harapan saya dalam panduan kali ini, setelahnya kalian akan mengetahui:

- Cara menggunakan SQLITE sebagai pengelola database
- Bagaimana membuat tabel menggunakan *migrations*
- Bagaimana mengisi tabel menggunakan *seeding*
- Mengenal fungsi dari beberapa baris perintah **artisan**
- Berinteraksi dengan *models*, *routes*, *controllers*, dan *views* dalam pembuatan tampilan beserta prosesnya.
- Menangani kesalahan yang sering terjadi ketika kita login
- Bagaimana menggunakan *Eloquent ORM* untuk mem-validasi pengguna
- Berkenalan dengan **blade**

Saya asumsikan bahwa kalian sudah pernah melakukan [instalasi Laravel]() sebelumnya. Jadi sekarang kita akan langsung fokus dalam pembuatan database. 

Perhatikan dialog antara dua bocah berikut :

- Mayu = Database di sistem ini gunanya untuk apa ya? 
- Miku = Mari berkhayal, kira-kira yang akan di autentikasi itu apa? 
- Mayu = *username* dan *password*
- Miku = Oke, sekarang bagaimana agar proses autentikasi itu bisa dilakukan?
- Mayu = *lakukan proses pencocokan dengan "data yang sudah ada"*. 
- Miku = Itu artinya kita butuh *data yang sudah ada* yang selanjutnya akan kita tampung kedalam wadah yang namanya database. 

Bisa dipahami? Kalau belum baca ulang dialognya.

#### Generate Key

Seperti biasa sebelum kita memulai membuat sebuah aplikasi, tidak ada salahnya bila kita membuat sebuah *encyption key* terlebih dahulu. Ya, fungsinya semata-mata untuk membuat aplikasi kita jauh lebih aman. Data yang mungkin akan di enkripsi salah satunya seperti *cookies*. Caranya menggunakan perintah **artisan** lewat **CMD** melalui direktori laravel Anda:

`php artisan key:generate`

- `php` merupakan perintah dasar milik **PHP** yang hanya bisa dieksekusi apabila kita telah menginstall **PHP**. Untuk kalian yang bertemu istilah `php is not recognized...` bisa lihat [artikel ini]({{ site.url }}/blog/2013/03/08/mengatasi-masalah-php-not-recognized/).
- `artisan`, mudahnya coba buka direktori proyek Anda, tepat disana Anda akan bertemu file bernama **artisan**. Jadi, untuk mengakses perintah `artisan` pada `php artisan ...`, kita butuh file dengan nama sama. Sekarang sudah tau kan alasan kenapa dalam folder proyek kita ada file **artisan**? karena yang sebenarnya kita akses adalah file **artisan** tersebut. Jadi, sebelum menggunakan perintah `php artisan ...` posisi kita harus berada tepat dimana file **artisan** itu berada dalam direktori proyek kita.
- `key:generate`, merupakan satu paket perintah untuk mengeksekusi / men-*generate* 32 digit karakter secara acak pada `app/config/app.php`. Apabila Anda ingin men-*generate* ulang, tinggal lakukan perintah yang sama.

#### Atur *Database*

Sekarang kita akan mulai membangun sebuah database. Sebelum memulainya, pastikan kamu telah menentukan jenis koneksi yang akan kamu gunakan. Coba sorot `app/config/database.php` lalu perhatikan baris ke 29 seperti berikut:

`'default' => 'mysql',`

Ubah `'mysql'` menjadi `'sqlite'`. Yang artinya kita mengubah penggunaan koneksi database kita menjadi 'SQLITE', sesuai dengan yang saya rencanakan sebelumnya. Sehingga menjadi :

`'default' => 'sqlite',`

"" 
Secara *default*, laravel menyediakan file **sqlite** kosong yang siap pakai didalam folder `app/database/` dengan nama `production.sqlite`. Anda diperbolehkan mengubah nama serta memindahkan letak file yang disediakan tersebut dengan ketentuan Anda harus memperhatikan isi file `app/config/database.php` pada baris `51`.
""

#### Migrations

Ringkasnya, *migrations* disini merupakan sebuah cara dimana kita bisa memanipulasi isi dari *database* yang kita miliki namun dengan menggunakan cara yang sedikit berbeda. Artinya, kita tidak lagi diharuskan menggunakan perintah SQL untuk memanipulasi data atau bermain hati lagi dengan si *phpmyadmin*. Untuk lebih jelasnya silahkan lihat dokumentasi resminya [disini](http://laravel.com/docs/migrations).

Laravel memungkinkan Anda untuk menciptakan sebuah file *migration* dengan hanya menggunakan perintah **artisan**. Caranya cukup mudah, buka **CMD** lalu masuk ke direktori proyek dan kemudian eksekusi perintah berikut :

`php artisan migrate:make buat_tabel_pengguna --create=pengguna`

- `migrate:make` merupakan satu paket perintah untuk menciptakan file *migration*.
- `buat_tabel_pengguna` akan menjadi bagian dari nama file yang diciptakan *####_##_##_######_buat_tabel_pengguna.php*. Simbol # menggantikan angka pada waktu sekarang berurutan seperti ini : `Tahun_Bulan_Tanggal_JamMenitDetik`. Semoga paham XD.
- `--create=pengguna`, sebenarnya ini sifatnya *optional*, bisa digunakan bisa tidak, perintah ini akan sedikit mempengaruhi isi file *migration* dengan membuat `Schema::create()` dan `Schema::drop()` secara otomatis. 

Secara keseluruhan perintah tersebut secara otomatis akan menghasilkan sebuah file ke dalam folder `app/database/migrations` dengan nama *####_##_##_######_buat_tabel_pengguna.php*. Sekarang buka file yang dihasilkan tersebut lalu tambahkan beberapa baris syntax menjadi seperti berikut :

{% highlight php %}
// app/database/migrations/####_##_##_######_buat_tabel_pengguna.php
<?php

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class BuatTabelPengguna extends Migration {

	/**
	 * Run the migrations.
	 *
	 * @return void
	 */
	public function up()
	{
		Schema::create('pengguna', function(Blueprint $table)
		{
			$table->increments('id');

			$table->string('nama_tampilan');
			$table->string('username', 40);
			$table->string('password', 60);
			$table->string('email');
			$table->string('remember_token');

			$table->timestamps();
		});
	}

	/**
	 * Reverse the migrations.
	 *
	 * @return void
	 */
	public function down()
	{
		Schema::drop('pengguna');
	}

}
?>
{% endhighlight %}

Sekarang file *migration* ini memiliki tanggung jawab untuk menciptakan tabel bernama **pengguna** dan juga memiliki hak untuk menghapusnya *(rollback)*. Untuk menjalankannya cukup dengan mengetikkan perintah melalui **CMD**.

**UPDATE** Terdapat penambahan field `remember_token` disini, untuk beberapa alasan bagi mereka yang menggunakan Laravel versi baru.

`php artisan migrate`

Dengan perintah tersebut, laravel akan meng-instalasi tabel *migration* sekaligus meng-eksekusi fungsi `up()` pada file *migration* tersebut. Dan itu artinya secara otomatis sekarang Anda memiliki 2 tabel. Tapi kita hanya fokuskan ke tabel bernama "pengguna" yang berisi kolom-kolom seperti `id`, `username`, dan seterusnya.

""
Fungsi `down()`: Untuk menghapus tabel yang ada berdasarkan nama tabel. 
Caranya `php artisan migrate:rollback` atau `php artisan migrate:reset`
""

Sekarang kita sudah punya tabel, dan saatnya untuk kita isi.

#### Seeds

**Seeding** itu ibarat menuangkan *sesuatu* kedalam *gelas*. *Sesuatu* disini berupa sampel data yang digunakan untuk login kelak, sedangkan *gelas* adalah databasenya. Tentunya fungsi **seeds** ini bertujuan dan memang akan sangat memudahkan kita dalam membangun sebuah aplikasi.

Tidak seperti *migration* yang mana filenya dibuat dengan hanya menggunakan perintah **artisan**. Untuk membuat file *Seed* kita harus menggunakan cara lama, yaitu dengan membuat file baru secara manual didalam folder `app/database/seeds`, lalu beri nama, misalnya `SeederTabelPengguna.php` dengan isi sebagai berikut :

{% highlight php %}
// app/database/seeds/SeederTabelPengguna.php
<?php

class SeederTabelPengguna extends Seeder
{

	public function run()
	{
		DB::table('pengguna')->delete();

		$tampungan = (array(
				'id' 			=> 1,
				'nama_tampilan'	=> 'Noviyanto Rachmady',
				'username'		=> 'novay',
				'password'		=> Hash::make('admins'),
				'email'			=> 'novay@about.me',
				'remember_token'=> '';
				'created_at' 	=> new DateTime,
				'updated_at' 	=> new DateTime
		));

		DB::table('pengguna')->insert($tampungan);
	}

}
?>
{% endhighlight %}

Apabila file diatas dieksekusi, maka file tersebut akan melakukan setiap tugas-tugasnya *per baris*, bermula dari memilih nama tabel yang akan diubah, menghapus bila tabel sudah ada sebelumnya, lalu kemudian mengisinya berdasarkan masing-masing kolom. Untuk nilai dari `password`, kita gunakan **Laravel's Hash Class** untuk meng-enkripsi nilai password kita dengan `Bcrypt`. Tujuannya ya untuk keamanan, lebih lanjut Anda bisa kunjungi [ini](http://laravel.com/docs/security).

File *Seeder* kita telah jadi, dan sekarang tinggal bagaimana cara memerintahkan Laravel untuk mengeksekusi file tersebut. Mudahnya Anda telusur dan buka `app/database/seeds/DatabaseSeeder.php`, lalu selipkan syntax `$this->call('SeederTabelPengguna');` seperti berikut.

{% highlight php %}
// app/database/seeds/DatabaseSeeder.php
<?php

class DatabaseSeeder extends Seeder {

	/**
	 * Run the database seeds.
	 *
	 * @return void
	 */
	public function run()
	{
		Eloquent::unguard();

		// $this->call('UserTableSeeder');

		$this->call('SeederTabelPengguna');

	}

}
?>
{% endhighlight %}

File **Seeder** telah berhasil dibuat. Dan langkah tersisa hanya tinggal menumpahkan *sesuatu* ini kedalam *botol* dengan cara:

`php artisan db:seed`

Untuk tahap pembuatan Aplikasi, kita lanjut ke [bagian ke 2]({{site.url}}/blog/2014/03/05/membuat-autentikasi-login-laravel-bagian-2/). 