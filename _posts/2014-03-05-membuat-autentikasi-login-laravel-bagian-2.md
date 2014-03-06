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
	return 'Halaman Logout';
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

####1. **as** bernilai **index**

**as** disini berfungsi sebagai **identitas** si `route` itu sendiri. Sedikit bocoran, biasanya identitas ini digunakan pada sesi **Views** dengan cara `route('index')` atau `URL::route('index')` atau pada sesi **Controller**dengan cara `return Redirect::route('index');`. Tenang, nanti kalian akan paham sendiri kok. XD

####2. **uses** bernilai **AuthController@getIndex**

Anda menggunakan **uses** berarti Anda menerima untuk menyerahkan tugas dari **route** kepada **Controllers**. Sesuai arti **uses** artinya menggunakan. Jadi bahasa gampangnya begini, **Route** ini memerintahkan **Controller** bernama **AuthController** dengan method bernama **getIndex** untuk selanjutnya menjalankan tugasnya.

- Untuk route kedua, ubah jadi seperti berikut :

{% highlight php %}
<?php
...
# Hala...
Route::get('/', array(...

# Halaman login (localhost:8000/login)
Route::get('login', array('as' => 'masuk', 'uses' => 'AuthController@getMasuk'));

# Hala...
Route::get('beranda', array()...
...
?>
{% endhighlight %}

Sama seperti sebelumnya, sekarang route ini memiliki 2 nilai, identitas dan pewaris. XD

- Route ketiga, ubah jadi seperti berikut :

{% highlight php %}
<?php
...
Halaman log...
Route::get('login...');

# Halaman beranda yg di akses setelah login (localhost:8000/beranda) 
Route::get('beranda', array('before' => 'auth', 'as' => 'admin', 'uses' => 'AuthController@getAdmin'));

# Halaman logout...
Route::get('user/logout'...
...
?>
{% endhighlight %}

- Dan untuk route terakhir, ubah jadi seperti berikut :

{% highlight php %}
<?php
...
# Halaman logout (localhost:8000/user/logout) 
Route::get('user/logout', array('before' => 'auth', 'as' => 'keluar', 'uses' => 'AuthController@getKeluar'));
...
?>
{% endhighlight %}

Masih tetap di `route`. Sekarang coba pikir, kita kemanakan inputan kita saat login?. Untuk itu, berarti kita membutuhkan 1 buah route **POST** untuk menangani inputan tersebut yang berupa `username` dan `password`.

Jadi, yang harus Anda lakukan sekarang adalah menambahkan 1 buah route dengan HTTPRequest **POST** seperti berikut :

{% highlight php %}
<?php
...
# Halaman login (localhost:8000/login)
Route::get('login', array(...

Route::post('login', array('as' => 'post-masuk', 'uses' => 'AuthController@postMasuk'));

# Halaman beranda yg di akses setelah login (localhost:8000/beranda) 
Route::get('beranda', array...
...
?>
{% endhighlight %}

**OKE**, sekarang urusan si **route** sudah selesai dan semua tanggung jawabnya telah dilimpahkan ke **Controller**.

### Controllers

**Controllers** disini akan kita gunakan sebagai wadah kita mengelola semua logika program. Dari beberapa hal yang telah kita lakukan sebelumnya, dapat kita simpulkan bahwa saat ini kita harus memiliki sebuah Controller bernama `AuthController` bila disesuaikan dengan apa yang kita tulis didalam `route` program. Dan bila kita kumpulkan hasilnya adalah seperti berikut :

- **1 Controller** = `AuthController`
- **5 Method** = `getIndex`, `getMasuk`, `postMasuk`, `getAdmin`, `getKeluar`

Sekarang buat file baru dalam `proyek-laravel/app/controllers/*` lalu beri nama `AuthController.php`. Dan isikan syntax berikut :

{% highlight php %}
// proyek-laravel/app/controllers/AuthController.php
<?php
class AuthController extends BaseController {

	# route('index') | localhost:8000/
	public function getIndex() {
		return 'Halaman Home Aplikasi';
	}
	
	# route('masuk') | localhost:8000/login
	public function getMasuk() {
		return 'Halaman Login';
	}

	# route('post-masuk') | localhost:8000/login
	public function postMasuk() {
		return 'Halaman Setelah form Login di Isi.';
	}

	# route('admin') | localhost:8000/admin
	public function getAdmin() {
		return 'Halaman Beranda.';
	}

	# route('keluar') | localhost:8000/user/logout
	public function getKeluar() {
		return 'Halaman Logout.';
	}	
}
?>
{% endhighlight %}

Sekarang coba akses masing-masing URL melalui browser kalian. Dan hasilnya kurang lebih sama dengan pada saat kita insialisasikan fungsi `return` melalui `route` sebelumnya.

####Untuk getIndex()

Disini kita akan menampilkan halaman yang memiliki tombol 'Login' didalamnya. Sekarang buat jadi seperti berikut :

{% highlight php %}
<?php
class AuthController extends BaseController {

	# route('index') | localhost:8000/
	public function getIndex() {
		return View::make('index');
	}
	
	# route('masuk') | localhost:8000/login
	public function get...	
}
?>
{% endhighlight %}

Sekarang kita memiliki `View::make('index');`. Apakah gerangan?. Secara kasat mata, View disini berarti tampilan, bisa dikatakan segala hal yang berbau tampilan dilakukan dalam view. Untuk letak, View ini terletak di `proyek-laravel/app/views/*`. `make('index');` artinya merujuk kearah file bernama `index.php` atau `index.blade.php` yang terletak persis didalam direktori `proyek-laravel/app/views/`.

####Untuk getMasuk()

getMasuk akan menampilkan halaman atau form login seperti pada umumnya. Coba perhatikan method **getMasuk** dan ubah menjadi seperti berikut

{% highlight php %}
<?php
	public function getIndex() {
		...
	}
	
	# route('masuk') | localhost:8000/login
	public function getMasuk() {
		return View::make('login');
	}

	# route('post-masuk') | localhost:8000/login
	public function postMasuk() {
		...
}
?>
{% endhighlight %}

Sama seperti tadi, dari sini pihak controller menunjuk view sebagai pewaris tugas dengan nama `login.php` atau 'login.blade.php' dalam folder `proyek-laravel/app/views/`.

####Untuk getAdmin()

getAdmin() akan menampilkan sebuah halaman yang hanya bisa dimasuki atau diakses ketika pengguna telah melakukan proses login. Bisa dibilang ini merupakan halaman terlarang bagi pengunjung biasa. Coba ubah menjadi seperti berikut :

{% highlight php %}
<?php
	...
	# route('post-masuk') | localhost:8000/login
	public function postMasuk() {
		...
	}

	# route('admin') | localhost:8000/admin
	public function getAdmin() {
		return View::make('admin.index');
	}

	# route('keluar') | localhost:8000/user/logout
	public function getKeluar() {
		...
	}
?>
{% endhighlight %}

Sekarang fungsi getAdmin() disini diserahkan ke Views dengan ketentuan file yang dimaksud bernama `index.php` atau `index.blade.php` yang berada dalam folder `admin` didalam direktori `views`. Tanda `.` sebagai pemisah antara nama file dengan direktori didepannya. Jadi akan ada folder baru kita ciptakan dalam `views`.

Mudahnya direktorinya jadi seperti ini : `proyek-laravel/app/views/admin/index.blade.php`.

####Untuk getKeluar()

{% highlight php %}
<?php
	...
	# route('admin') | localhost:8000/admin
	public function getAdmin() {
		...
	}

	# route('keluar') | localhost:8000/user/logout
	public function getKeluar() {
		# Hapus session dan cookies admin
		Auth::logout();
		# Arahkan ke view 'index' dengan session 'pesan'.
		return View::make('index')->withPesan('Anda telah keluar dari sistem.');
	}	
}
?>
{% endhighlight %}

Jadi ketika getKeluar() ini dikunjungi, dia akan menghapus *session* dan *cookie* pengguna yang membuat pengguna keluar dari sistem. Lalu menampilkan view `index.blade.php` dengan session bernama 'pesan' untuk ditampilkan di `index` sesaat setelah melakukan `logout`.

####Untuk postMasuk()

Untuk **POST** sengaja saya buat belakangan karena bagian ini akan menampung baris yang lebih banyak dari sebelumnya. Kalau dulu saya pribadi sih biasanya saya buat dulu alur programnya, jadi istilahnya kita ngomong ke postMasuk() ini dengan menggunakan bahasa kita lewat komentar seperti berikut :

{% highlight php %}
<?php
	...
	# route('masuk') | localhost:8000/login
	public function getMasuk() {
		...
	}

	# route('post-masuk') | localhost:8000/login
	public function postMasuk() {
		# Buat aturan validasi
		//
		# Bila validasi gagal
		//
			# Kembali kehalaman dan tampilkan error
			//
		# Bila sukses
		//
			# Tarik masing-masing inputan yang berasal dari Form
			//
			# Lakukan Pencocokan username dan password
			# Bila cocok
			//
				# Masuk ke Halaman Beranda Admin
				//
			# Bila tidak cocok
			//
				# Kembali kehalaman dan tampilkan error
				//
	}

	# route('admin') | localhost:8000/admin
	public function getAdmin() {
		...
	}
?>
{% endhighlight %}

Bagi yang belum terbiasa saya sarankan gunakan cara ini (gak harus sih XD). Tapikan apa salahnya. Ngahahahaha....

Baca dulu baik-baik tiap komentar, bila menurut kalian semua masuk akal, baru yuk kita lanjut.

Sekarang isi menjadi seperti berikut :

{% highlight php %}
<?php
	...
	# route('masuk') | localhost:8000/login
	public function getMasuk() {
		...
	}

	# route('post-masuk') | localhost:8000/login
	public function postMasuk() {

		# Buat aturan validasi

		/* Tarik Inputan dari form sekaligus, lalu
		masukkan kedalam variabel 'input' sekaligus */
		$input = Input::all();

		/* Buat aturannya dan tampung dalam variabel
		'aturan' */
		$aturan = array(
			'username' => 'required|min:5|max:30',
			'password' => 'required|min:5'
		);

		/* Dan lakukan validasi */
		$validasi = Validator::make($input, $aturan);

		# Bila validasi gagal

		if($validasi->fails()) {

			# Kembali kehalaman dan tampilkan error
			
			return Redirect::back()
				->withInput()
				->withErrors($validasi);
		
		# Bila sukses
		
		} else {
			
			# Tarik masing-masing inputan yang berasal dari Form
			
			$pengguna 	= Input::get('username');
			$sandi 		= Input::get('password');
			/* Jadikan sati untuk keperluan verifikasi */
			$verifikasi = compact('username', 'password');
			
			# Lakukan Pencocokan username dan password
			# Bila cocok
			
			if(Auth::attempt($verifikasi)) {
			
				# Masuk ke Halaman Beranda Admin
			
				return Redirect::route('admin');
			
			# Bila tidak cocok
			
			} else {
			
				# Kembali kehalaman dan tampilkan error
			
				return Redirect::back()
					->withPesan('Username dan Password tidak cocok.');
			}
		}

	}

	# route('admin') | localhost:8000/admin
	public function getAdmin() {
		...
	}
?>
{% endhighlight %}

Perhatikan baik-baik tiap baris dan komentar-komentarnya. Kalau ada yang mau ditanya, tanya dibawah ya.

Dan dengan ini, tugas si **Controllers** berakhir, sebelumnya juga **route** sudah memenuhi kewajibannya dan saatnya sekarang kita beralih ke **Views**.

### Views

Sebelumnya saya sempat beberapa kali menyinggung masalah view dan akan saya tekankan sekali lagi disini, tujuan utama si **Views** disini adalah untuk menampilkan halaman. Pemegang tugas akhir laravel yang menampilkannya kedalam bentuk visual untuk penggunanya.

Bila kita memeriksa ulang Controller kita, maka akan terkumpul 3 buah view yang harus kita buat, dan mereka adalah :

- index
- login
- admin/index

Sekarang masuk ke direktori `proyek-laravel/app/views/` dan buat semua view yang dibutuhkan, kurang lebih seperti gambar berikut :

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-03-05-membuat-autentikasi-login-laravel-bagian-2-1.JPG" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-03-05-membuat-autentikasi-login-laravel-bagian-2-1.JPG" width="300px"/>
		</a>
	</center>
</figure>

Mumpung masih disini saya juga akan mengenalkan kalian dengan fitur **blade** milik laravel untuk keperluan templating.

Sekarang tambahkan 1 buat view lagi bernama `utama.blade.php` lalu letakkan dalam folder `_tema` di direktori *Views*. Ya, kurang lebih Views kalian jadi seperti ini :

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-03-05-membuat-autentikasi-login-laravel-bagian-2-2.JPG" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-03-05-membuat-autentikasi-login-laravel-bagian-2-2.JPG" width="300px"/>
		</a>
	</center>
</figure>

Artinya sekarang kita memiliki 4 buah view :

- _tema/utama.blade.php
- admin/index.blade.php
- index.blade.php
- login.blade.php

####Untuk `_tema/utama.blade.php`

Akan dijadikan fondasi dasar kerangka website Anda. Tenang aja, nanti kalian bakalan paham sendiri kok.

Sekarang isi seperti berikut :

{% highlight html %}
<!DOCTYPE html>
<html>
	<head>
		<title>Autentikasi Sistem Sederhana</title>
	</head>
	<body>
		<h2>Autentikasi Sederhana Menggunakan Laravel</h2>

		<!-- Sediakan wadah untuk menampung session 'pesan', 
		ingat ketika controller pernah mengirim session
		melalui variabel 'pesan'? Kalau lupa coba cek ulang -->
		@if(Session::has('pesan'))
			<p>{{ Session::get('pesan') }}</p>
		@endif

		<!-- Disinilah nantinya yang akan kita isi 
		untuk setiap view utama -->
		@yield('konten')

	</body>
</html>
{% endhighlight %}

Seperti yang kalian lihat, Yap, mereka adalah sekumpulan syntax HTML. Tapi bila diperhatikan ada yang unik didalamnya. 

Secara kasat mata, kita tidak akan menyadari bila didalamnya terdapat syntax PHP yang biasanya dibuka dengan `<?php` dan ditutup dengan `?>`.

Yang ingin saya katakan disini adalah ini merupakan kemampuan dari si `blade` milik laravel. Bila kita kumpulkan hal-hal unik tersebut maka jadi seperti berikut :

- `@if(...) ... @endif` intinya disini berbicara masalah `kondisional if...else...`.
- `{ { ... } }` bernilai sama dengan `<?php ... ?>`
- `@yield` akan dijadikan tempat menampung isi dari view-view lain.

Catatan : Perhatikan baik-baik. Ketiga hal aneh tersebut tidak akan bisa terbaca oleh laravel tanpa ada **.blade**. Intinya terletak pada penamaan file *View*. `index.php` tidak akan mengenalinya, sedangkan `index.blade.php` lah yang bisa. Itulah salah satu alasan kita membutuhkan `.blade` dibelakangnya.

####Untuk `admin/index.blade.php`

Merupakan Halaman yang akan diakses ketika pengguna telah melakukan login, isi seeperti berikut :

{% highlight html %}
<!-- Kita jadikan sebagai tema,
file 'utama.blade.php' dalam foldder '_tema' -->
@extends('_tema.utama')

<!-- Ingat dengann @yield('konten')?...
Inilah yang akan diselipkan disana -->
@section('konten')
<!-- Ingat, '{{ ... }}' equivalen dengan '<?php ?>', 
Auth::user()->username akan menarik isi dari database pengguna,
lebih tepatnya isi field 'username' pengguna yang sedang login -->
<p>Selamat Datang, {{ Auth::user()->nama_tampilan }} ({{ Auth::user()->email }})</p>
@stop
{% endhighlight %}

Penjelasan ada dimasing-masing komentar. Dibaca baik-baik yak, sampe paham baru dilanjut.

####Untuk `index.blade.php`

Didalamnya hanya akan terdapat tombol login yang akan mengarah kehalaman login, jadi isi seperti berikut :

{% highlight html %}
@extends('_tema.utama')

@section('konten')
<!-- Ingat, '{{ ... }}' equivalen dengan '<?php ?>' 
'route('login')' mengarah ke identitas di route -->
<a href="{{ route('masuk') }}">Login Sebagai Admin</a>
@stop
{% endhighlight %}

####Untuk `login.blade.php`

Halaman ini akan menampilkan form yang akan kita inputan, isi script berikut :

{% highlight html %}
@extends('_tema.utama')

@section('konten')
<!-- Kita gunakan identitas route berikut -->
{{ Form::open(array('route' => 'post-masuk')) }}
	
	<!-- Label dan Textfield dengan id 'username' -->
	{{ Form::label('username', 'Username') }}
	{{ Form::text('username') }}

	<!-- Label dan Passwordfield dengan id 'password' -->
	{{ Form::label('password', 'Password') }}
	{{ Form::password('password') }}

	<!-- Tombol Masuk -->
	{{ Form::submit('Masuk') }}
{{ Form::close() }}
@stop
{% endhighlight %}



##Kesimpulan

Ya, sebenarnya tutorial ini dikhususkan buat mereka yang betul-betul buta tentang laravel pada khususnya (pemprograman PHP pada umumnya), tetapi memiliki hasrat juang untuk berkenalan dengan si 'laravel' itu tadi.

Oleh karenanya sangat wajar bila programmer *pro* bilang kalau tulisan ini terlalu lebay, ngahaha... Intinya pesan dari saya tersampaikan dengan baik. Dan mereka bisa memahami fungsi dari beberapa yang dijelaskan diatas.

Ingat, semua programmer memiliki ciri khas mereka masing-masing, jadi wajar bila tata penulisan syntax atau bahkan urutan dalam pembuatannya berbeda antara satu dan lainnya. Dan ini mengingatkan saya tentang semboyan *Bhenika Tunggal Ika* XD. Walaupun memiliki cara dan urutan yang berbeda, namun memiliki satu tujuan. Ngahahaha...

Akhir kata, 
Happy Coding dan Salam Olahraga.
Wassalam.