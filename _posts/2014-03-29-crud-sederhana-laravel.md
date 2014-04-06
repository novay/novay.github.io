---
layout: post
title: CRUD Sederhana Laravel
description: "Tutorial pembuatan aktivitas CRUD sederhana menggunakan framework Laravel."
modified: 2014-03-29
category: blog
tags: [tips, laravel, php]
comments: true
share: true
---

Saya sarankan sebelum mengikuti tutorial ini, sebaiknya Anda telah menguasai tutorial saya sebelumnya tentang [Pembuatan Autentikasi Sederhana]({{site.url}}/blog/2013/04/03/membuat-autentikasi-login-laravel-bagian-1/). Karena ada beberapa yang tidak akan saya definisikan dengan jelas disini, seperti:

- Instalasi Laravel
- *Generate Key* untuk alasan *Security*
- Pengaturan Database
- dan mungkin ada lagi yang lainnya.

Jadi, saya asumsikan kalian memahami beberapa poin diatas sebelum meneruskan bacaan ini.

Oke, mula-mula saya ingin pastikan kalian tahu dulu maksud dari **CRUD** disini. **CRUD** merupakan singkatan dari **Create**, **Read**, **Update** dan **Delete**. Bila diartikan berarti *Menciptakan, Membaca, Memperbarui* dan *Menghapus*.

Hampir semua jenis merk aplikasi pasti terdapat aktifitas CRUD didalamnya. Sekarang pertanyaannya adalah:

- *Apa yang diciptakan?*
- *Apa yang dibaca?*
- *Apa yang diperbarui?* dan
- *Apa yang akan dihapus?*

Jawabannya **TERGANTUNG**. Tergantung dari apa yang menjadi isi dari aplikasi yang ingin Anda bangun. Sebagai contoh sederhana mari kita perhatikan bagaimana cara kerja sebuah *blog*?

- *Apakah tulisan dalam blog itu muncul dengan sendirinya?* **AJAIB**. Tentu saja ada yang mengolah kata-katanya. Setelah kata-kata diolah, mau tidak mau si *blogger* harus terlibat langsung dengan aplikasi miLiknya. Dengan cara men-**CIPTA**-kan tulisan baru berisi kata-kata yang diolah tadi agar bisa masuk kedalam database *aplikasi blog* miliknya.

- Untuk diketahui bahwa isi tulisan si *blogger* tadi ditampung didalam database, artinya si *aplikasi*-lah yang kemudian akan mem-**BACA**-kan isinya dari database untuk ditampilkan kembali kepada si *blogger*.

- Sekarang bagaimana bila si *blogger* melakukan kesalahan dalam penulisan kata? Atau mungkin ia ingin menambahkan beberapa kata dalam tulisannya? Tentunya *aplikasi* harus berkemampuan untuk mem-**PERBARUI** tulisan. Dimana proses kerjanya aplikasi akan mengambil isi dari database, dan si penulis melakukan perubahan, dan diakhiri dengan aplikasi mengubah isi tulisan lama dengan tulisan terbaru.

- Dan bila si *blogger* merasa bila tulisan miliknya tidak layak untuk ditampilkan atau bahkan disimpan lagi, maka aplikasi seharusnya bisa membantunya meng-**HAPUS** tulisan tersebut dengan hanya sekali tekan.

---

Jadi jawaban untuk studi kasus diatas terkait ke-empat pertanyaan tadi adalah **TULISAN**.

- **BLOG** -> Menciptakan **TULISAN** 
- **BLOG** -> Membaca **TULISAN** 
- **BLOG** -> Memperbarui **TULISAN**
- **BLOG** -> Menghapus **TULISAN**

---

Dan untuk itulah, saya akan mencoba memperkenalkan **CRUD** pada Laravel dan bagaimana sistem kerjanya serta cara membuatnya bagi mereka yang ingin membuat proyek menggunakan laravel.

Biasanya, aktifitas **CRUD** terjadi dalam **Admin Panel** yang secara tak langsung akan kita bangun. Sebagai contoh penerapannya, saya akan membuat aplikasi **Biodata** sederhana. Mari kita mulai....

##Persiapan Awal

###Siapkan Database

Langsung saja eksekusi perintah berikut melalui terminal atau cmd :

`php artisan migrate:make buat_tabel_biodata`

Anda bisa menemukan file migration yang dihasilkan di `app/database/migrations`. 

Selanjutnya tambahkan beberapa syntax didalamnya.

{% highlight php %}
// app/database/migrations/####_##_##_######_buat_tabel_biodata.php
<?php

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class BuatTabelBiodata extends Migration {

	/**
	 * Run the migrations.
	 *
	 * @return void
	 */
	public function up()
	{
		Schema::create('biodata', function($tabel) {
			$tabel->increments('id');
			$tabel->string('nama');
			$tabel->integer('usia');
			$tabel->string('jenis_kelamin');
			$tabel->string('telepon');
			$tabel->string('email');
			$tabel->timestamps();
		});
	}

	/**
	 * Reverse the migrations.
	 *
	 * @return void
	 */
	public function down()
	{
		Schema::drop('biodata');
	}

}


?>
{% endhighlight %}
 
Pastikan Anda telah melakukan pengaturan database yang digunakan. Kemudian lakukan perintah berikut :

`php artisan migrate`

Dan sekarang kita memiliki tabel **Biodata** yang nantinya akan kita jadikan korban **CRUD**.


###Model

Profesi si **Model** ini simpel, yaitu sebagai perantara antara database dengan aplikasi.

Sekarang coba masuk ke direktori `app/models`. Lalu buat model baru dengan nama `Biodata.php`.

{% highlight php %}
// app/models/Biodata.php
<?php
class Biodata extends Eloquent {
	# Penamaan tabel yang digunakan
	protected $table = 'biodata';
	# MASS ASSIGNMENT (maksudnya buatkan field-field yang diperbolehkan menerima inputan)
	protected $fillable = array('nama', 'usia', 'jenis_kelamin', 'telepon', 'email');
}
?>
{% endhighlight %}

Tugas model selesai.

###Route

*Berapa jumlah route yang kira-kira dibutuhkan?* Untuk mengetahuinya Anda bisa membayangkan alur programnya. Mulai dari tampilan awal, hingga proses *delete*.

Untuk mempersingkat waktu, saya buatkan 7 buah route yang dibutuhkan untuk pembuatan aplikasi ini :

- Menampilkan semua data biodata
- Menampilkan form pembuatan biodata baru
- Melakukan proses pembuatan biodata baru
- Menampilkan biodata perorangan
- Menampilkan form perubahan biodata baru
- Melakukan proses perubahan biodata
- Hapus biodata berdasarkan id

Sekarang coba buka `app/routes.php`, dan buat ke-7-nya jadi seperti berikut :

{% highlight php %}
// app/routes.php
<?php
# Halaman muka, untuk menampilkan semua data biodata yang ada. [localhost:8000/]
Route::get('/', function(){ return 'halaman index'; });

# Halaman yang berisi Form inputan Biodata baru [localhost:8000/buat]
Route::get('buat', function(){ return 'Halaman Tambah Biodata'; });

# Memproses Form lalu mengirimnya kedalam database [localhost:8000/buat]
Route::post('buat', function(){ return 'Proses Tambah Biodata'; });

# Menampilkan Biodata perorangan [localhost:8000/lihat/{id}]
Route::get('lihat/{id}', function(){ return 'Halaman Biodata Perorangan'; });

# Form untuk mengubah isi Biodata dalam database [localhost:8000/ubah/{id}]
Route::get('ubah/{id}', function(){ return 'Halaman Ubah Biodata'; });

# Memproses Form lalu mengirim yang baru kedalam database [localhost:8000/ubah/{id}]
Route::put('ubah/{id}', function(){ return 'Proses Perubahan Biodata'; });

# Tindakan untuk menghapus Biodata [localhost:8000/{id}/hapus]
Route::get('hapus/{id}', function(){ return 'Halaman Tambah Biodata'; });

?>
{% endhighlight %}

Sebelum lanjut, coba akses semua **URL** yang ada pada *route* diatas. Bila berhasil dan tidak ada yang error kita lanjut ke **Controller**.

###Controller

Jujur saja, sebenarnya pihak Laravel sendiri telah menyediakan fitur *resource controller* dimana sebenarnya akan sangat membantu terutama dalam pengolahan CRUD ini, karena sifatnya yang otomatis. Mungkin akan saya membuatkan tutorial tersendiri nanti untuk yang satu ini.

Berhubung disini konteksnya saya ingin agar si pembaca mengerti alur programnya, maka saya akan jelaskan pembuatan semuanya secara manual saja.

Sebelumnya kita sempat membuat 7 buah route. Dan semua itu nantinya akan kita alih fungsikan menuju ke Controller. 

Untuk itu, kita butuh 7 buah fungsi pada controller untuk menangani setiap route.

Sekarang masuk ke direktori `app/controllers/`, dan buat file baru bernama `BiodataController.php`. Isi seperti berikut :

{% highlight php %}
// app/controllers/BiodataController.php
<?php
class BiodataController extends BaseController {
	# GET localhost:8000/
	public function index() {
		#
	}

	# GET localhost:8000/buat
	public function baru() {
		#
	}

	# POST localhost:8000/buat
	public function buat() {
		#
	}

	# GET localhost:8000/lihat/{id}
	public function lihat($id) {
		#
	}

	# GET localhost:8000/ubah/{id}
	public function ubah($id) {
		#
	}

	# PUT localhost:8000/ubah/{id}
	public function ganti($id) {
		#
	}

	# DELETE localhost:8000/hapus/{id}
	public function hapus($id) {
		#
	}
}

?>
{% endhighlight %}

**Controller** dengan 7 fungsi telah disiapkan, sekarang kita siapkan **Views** untuk tampilannya.

###View

Sekarang untuk menentukan halaman yang kita butuhkan, sepetinya kita lagi-lagi harus ditekankan untuk berkhayal. 

Mari perhatikan perumpamaan dari saya, saya ambil dari route yang telah kita buat sebelumnya :

- Menampilkan semua data biodata -> **Butuh View** -> index.blade.php
- Menampilkan form pembuatan biodata baru -> **Butuh View** -> buat.blade.php
- Melakukan proses pembuatan biodata baru
- Menampilkan biodata perorangan -> **Butuh View** -> lihat.blade.php
- Menampilkan form perubahan biodata baru -> **Butuh View** -> ubah.blade.php
- Melakukan proses perubahan biodata
- Hapus biodata berdasarkan id

Bener tidak?

Bila diperhatikan kembali, kita butuh 4 buah view untuk memenuhi standar aplikasi CRUD yang ingin kita bangun.

Sekarang masuk ke direktori `app/views/` dan ciptakan folder bernama `biodata` lalu buat 4 buah view baru disana. Strukturnya kurang lebih seperti berikut :

---

	apps/
	|___views/
	|__|  biodata/
	|__|__| buat.blade.php
	|__|__| index.blade.php
	|__|__| lihat.blade.php
	|__|__| ubah.blade.php

---

Dengan ini persiapan awal kita selesai. Sekarang saatnya untuk membuat **Model, Controller, Route** dan **View** yang kita siapkan tadi agar bisa saling bekerja sama.

## Penerapan Program

Bila diingat-ingat lagi, sekarang kita memiliki :

| Keterangan 										| Request | URL 					  | Method     |
|:--------------------------------------------------|:-------:|:-------------------------:|-----------:|
| Halaman Index, menampilkan semua biodata 			| GET 	  | localhost:8000 	 		  | index()    |
| Halaman yg berisi Form inputan Biodata Baru 		| GET     | localhost:8000/buat 	  | baru()     |
| Memproses Form lalu menginputnya kedalam database | POST    | localhost:8000/buat 	  | buat()     |
| Menampilkan Biodata perorangan 					| GET     | localhost:8000/lihat/{id} | lihat($id) |
| Form untuk mengubah isi Biodata perorangan 		| GET     | localhost:8000/ubah/{id}  | ubah($id)  |
| Proses untuk mengubah data lama menjadi baru 		| PUT     | localhost:8000/ubah/{id}  | ganti($id) |
| Tindakan untuk menghapus Biodata 					| GET     | localhost:8000/hapus/{id} | hapus($id) |
{: rules="groups"}

Nanti kita akan coba membuatnya berurutan dari **Create**, **Read**, lalu **Update** dan **Delete**. Namun sebelum itu semua, kita akan membuat INDEX-nya terlebih dahulu.

###INDEX

Index disini akan menampilkan informasi atau daftar semua isi yang ada dalam database Anda yang dikemas kedalam sebuah *table*, *dengan ketentuan database telah memiliki isi*. Sedangkan bila database belum memiliki isi, maka **INDEX** akan menampilkan sebuah komentar yang berisi: "Anda belum memiliki isi pada **tabel terkait**", beserta tombol **Tambah**. Seperti pada tampilan berikut :

<figure><center>
	<a href="{{ site.url }}/assets/post/2014-03-29-crud-sederhana-laravel-1.PNG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2014-03-29-crud-sederhana-laravel-1.PNG" width="500px"/>
	</a>
</center></figure>

Lakukan perubahan pada tahap-tahapan berikut :

####Route

{% highlight php %}
// app/routes.php
<?php
# Halaman muka, untuk menampilkan semua data biodata yang ada. [localhost:8000]
Route::get('/', array('as' => 'beranda', 'uses' => 'BiodataController@index'));

...
?>
{% endhighlight %}

####Controller

{% highlight php %}
// app/controllers/BiodataController.php
<?php
class BiodataController extends BaseController {
	# GET localhost:8000
	public function index() {
		# Tarik semua isi tabel biodata kedalam variabel
		$biodata = Biodata::all();
		# Tampilkan View
		return View::make('biodata.index', compact('biodata'));
	}
	
	...
?>
{% endhighlight %}

####View

{% highlight html %}
{% raw %}
// app/views/biodata/index.blade.php
<html>
	<head>
		<title>CRUD Biodata Sederhana</title>
	</head>
	<body>
		<h3>Daftar Biodata</h3>
		<!-- Siapkan variabel pesan untuk menampilkan isinya bila diterima -->
		@if(Session::has('pesan'))
			{{ Session::get('pesan') }}
		@endif
		<!-- Jika tabel biodata memiliki isi, tampilkan isi berikut -->
		@if($biodata->count())
		<!-- Siapkan tombol untuk membuat biodata baru -->
		<p><a href="{{ route('baru') }}">Tambah</a></p>
		<table>
			<thead>
				<tr>
					<th>Nama</th>
					<th>Usia</th>
					<th>Jenis Kelamin</th>
					<th>Telepon</th>
					<th>Email</th>
					<th>Aksi</th>
				</tr>
			</thead>
			<tbody>
				<!-- Lakukan Perulangan untuk menampilkan seluruh isi tabel -->
				@foreach($biodata as $data)
				<tr>					
					<td>{{ $data->nama }}</td>
					<td>{{ $data->usia }}</td>
					<td>{{ $data->jenis_kelamin }}</td>
					<td>{{ $data->telepon }}</td>
					<td>{{ $data->email }}</td>
					<!-- Siapkan tombol untuk edit dan hapus item tertentu -->
					<td>
						<a href="{{ route('lihat', $data->id) }}">Lihat</a>
						<a href="#">Edit</a>
						<a href="#">Hapus</a>
					</td>
				</tr>
				@endforeach
			</tbody>
		</table>
		<!-- Sedangkan, bila tidak ada isinya, tampilkan isi berikut -->
		@else
		<p>Anda belum memiliki isi pada tabel biodata.</p>
		<p><a href="{{ route('baru') }}">Tambah</a></p>
		@endif
	</body>
</html>
{% endraw %}
{% endhighlight %}

###CREATE

Create disini tujuannya untuk menciptakan atau menambah data biodata kedalam database. Yang nantinya akan ditampilkan dalam daftar **INDEX** dan tentu saja dalam **READ** dipembahasan selanjutnya.

Setelah Anda menyelesaikan **CREATE** pastikan hasilnya seperti ini :

<figure><center>
	<a href="{{ site.url }}/assets/post/2014-03-29-crud-sederhana-laravel-2.PNG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2014-03-29-crud-sederhana-laravel-2.PNG" width="500px"/>
	</a>
</center></figure>

####Route

{% highlight php %}
// app/routes.php
<?php
...

# Halaman yang berisi Form inputan Biodata baru [localhost:8000/buat]
Route::get('buat', array('as' => 'baru', 'uses' => 'BiodataController@baru'));

# Memproses Form lalu mengirimnya kedalam database [localhost:8000/buat]
Route::post('buat', array('as' => 'buat', 'uses' => 'BiodataController@buat'));

...
?>
{% endhighlight %}

####Controller

{% highlight php %}
// app/controllers/BiodataController.php
<?php
class BiodataController extends BaseController {
	...

	# GET localhost:8000/buat
	public function baru() {
		# Buat dropdown jenis kelamin
		$jenis_kelamin = array(
			'Laki-laki' => 'Laki-laki', 
			'Perempuan' => 'Perempuan');
		# Tampilkan halaman pembuatan biodata
		return View::make('biodata.buat', compact('jenis_kelamin'));
	}

	# POST localhost:8000/buat
	public function buat() {
		# Tarik semua inputan dari form kedalam variabel input
		$input = Input::all();
		# Buat aturan validasi
		$aturan = array(
			'nama' => 'required|min:3', 
			'usia' => 'required', 
			'telepon' => 'required', 
			'email' => 'required|email'
		);
		# Buat pesan error validasi manual
		$pesan = array(
			'nama.required' => 'Inputan Nama wajib diisi.',
			'nama.min' => 'Inputan Nama minimal 3 karakter.',
			'usia.required' => 'Inputan Usia wajib diisi.',
			'telepon.required' => 'Inputan Telepon wajib diisi.',
			'email.required' => 'Inputan Email wajib diisi.',
			'email.email' => 'Inputan harus berupa Email.'
		);
		# Validasi
		$validasi = Validator::make($input, $aturan, $pesan);
		# Bila validasi gagal
		if($validasi->fails()) {
			# Kembali kehalaman yang sama dengan pesan error
			return Redirect::back()->withErrors($validasi)->withInput();
		# Bila validasi sukses
		} else {
			# Buatkan variabel tiap inputan
			$nama = Input::get('nama');
			$usia = Input::get('usia');
			$jenis_kelamin = Input::get('jenis_kelamin');
			$telepon = Input::get('telepon');
			$email = Input::get('email');
			# Isi kedalam database
			Biodata::create(compact('nama', 'usia', 'jenis_kelamin', 'telepon', 'email'));
			# Kehalaman beranda dengan pesan sukses
			return Redirect::route('beranda')->withPesan('Biodata baru berhasil ditambahkan.');
		}
	}

	...
?>
{% endhighlight %}

####View

{% highlight html %}
{% raw %}
// app/views/biodata/index.blade.php
...
Baris 14	<p><a href="{{ route('baru') }}">Tambah</a></p>
...
Baris 48	<p><a href="{{ route('baru') }}">Tambah</a></p>
...
{% endraw %}
{% endhighlight %}

{% highlight html %}
{% raw %}
// app/views/biodata/buat.blade.php
<html>
	<head>
		<title>Tambah Biodata</title>
	</head>
	<body>
		<h2>Tambah Biodata Baru</h2>
		<!-- Buka form inutan lalu ruju ke identitas route "buat" -->
		{{ Form::open(array('route' => 'buat')) }}
			{{ Form::label('nama', 'Nama') }}
			{{ Form::text('nama') }}
			<!-- Bila ada errors validasi letakkan disini 
			variabel errors ($errors) berasal dari Controller ['withErrors'] -->
			@if($errors->has('nama'))
				{{ $errors->first('nama') }}
			@endif
			<br/>
			{{ Form::label('usia', 'Usia') }}
			{{ Form::text('usia') }}
			<!-- Penjelasan sama seperti diatas -->
			@if($errors->has('usia'))
				{{ $errors->first('usia') }}
			@endif
			<br/>
			{{ Form::label('jenis_kelamin', 'Jenis Kelamin') }}
			{{ Form::select('jenis_kelamin', $jenis_kelamin) }}
			@if($errors->has('jenis_kelamin'))
				{{ $errors->first('jenis_kelamin') }}
			@endif
			<br/>
			{{ Form::label('telepon', 'Telepon') }}
			{{ Form::text('telepon') }}
			@if($errors->has('telepon'))
				{{ $errors->first('telepon') }}
			@endif
			<br/>
			{{ Form::label('email', 'Email') }}
			{{ Form::text('email') }}
			@if($errors->has('email'))
				{{ $errors->first('email') }}
			@endif
			<br/>
			{{ Form::submit('Buat') }}
		{{ Form::close() }}
	</body>
</html>
{% endraw %}
{% endhighlight %}

###READ

Bila tadi kita telah berhasil melakukan penambahan data kedalam database, sekarang saatnya untuk menampilkan informasi lengkap mengenai data yang kita masukkan pada tahap **CREATE** tadi.

<figure><center>
	<a href="{{ site.url }}/assets/post/2014-03-29-crud-sederhana-laravel-3.PNG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2014-03-29-crud-sederhana-laravel-3.PNG" width="500px"/>
	</a>
</center></figure>

Untuk penerapannya, ikuti langkah berikut :

####Route

{% highlight php %}
// app/routes.php
<?php
...

# Menampilkan Biodata perorangan [localhost:8000/lihat/{id}]
Route::get('lihat/{id}', array('as' => 'lihat', 'uses' => 'BiodataController@lihat'));

...
?>
{% endhighlight %}

####Controller

{% highlight php %}
// app/controllers/BiodataController.php
<?php
class BiodataController extends BaseController {
	...

	# GET localhost:8000/lhat/{id}
	public function lihat($id) {
		# Ambil data dalam berdasarkan berdasarkan id
		$biodata = Biodata::find($id);
		# Tampilkan view
		return View::make('biodata.lihat', compact('biodata'));
	}

	...
}
?>
{% endhighlight %}

####View

{% highlight html %}
{% raw %}
// app/views/biodata/index.blade.php
...
Baris 37	<a href="{{ route('lihat', $data->id) }}">Lihat</a>
...
{% endraw %}
{% endhighlight %}

{% highlight html %}
{% raw %}
// app/views/biodata/lihat.blade.php
<html>
	<head>
		<title>Biodata {{ $biodata->nama }}</title>
	</head>
	<body>
		<h2>Informasi Biodata</h2>
		<p>Nama : {{ $biodata->nama }}</p>
		<p>Usia : {{ $biodata->usia }}</p>
		<p>Jenis Kelamin : {{ $biodata->jenis_kelamin }}</p>
		<p>Telepon : {{ $biodata->telepon }}</p>
		<p>Email : {{ $biodata->email }}</p>
		<br/>
		<a href="{{ route('beranda') }}">Kembali ke Index</a>
	</body>
</html>
{% endraw %}
{% endhighlight %}

###UPDATE

Kita memiliki halaman index, dimana ia akan menampilkan seluruh data yang ada dalam database dalam bentuk tabel. Kita juga bisa melakukan penambahan kedalamnya, sekaligus menampilkan kembali isi yang telah kita ditambah. Nah, penting untuk kita untuk dapat melakukan perubahan data apabila ada data yang isinya kurang valid artinya kita harus menambahkan fitur **UPDATE**.

Ikuti langkah-langkahnya :

####Route

{% highlight php %}
// app/routes.php
<?php
...

# Form untuk mengubah isi Biodata dalam database [localhost:8000/ubah/{id}]
Route::get('ubah/{id}', array('as' => 'ubah', 'uses' => 'BiodataController@ubah'));

# Memproses Form lalu mengirim yang baru kedalam database [localhost:8000/ubah/{id}]
Route::put('ubah/{id}', array('as' => 'ganti', 'uses' => 'BiodataController@ganti'));

...
?>
{% endhighlight %}

####Controller

{% highlight php %}
// app/controllers/BiodataController.php
<?php
class BiodataController extends BaseController {
	...

	# GET localhost:8000/ubah/{id}
	public function ubah($id) {
		# Buat dropdown jenis kelamin
		$jenis_kelamin = array(
			'Laki-laki' => 'Laki-laki', 
			'Perempuan' => 'Perempuan');
		# Tentukan biodata yang ingin diubah berdasarkan id
		$biodata = Biodata::find($id);
		# Tampilkan view
		return View::make('biodata.ubah', compact('jenis_kelamin', 'biodata'));
	}

	# PUT localhost:8000/ubah/{id}
	public function ganti($id) {
		# Tarik semua inputan dari form kedalam variabel input
		$input = Input::all();
		# Buat aturan validasi
		$aturan = array(
			'nama' => 'required|min:3', 
			'usia' => 'required', 
			'telepon' => 'required', 
			'email' => 'required|email'
		);
		# Buat pesan error validasi manual
		$pesan = array(
			'nama.required' => 'Inputan Nama wajib diisi.',
			'nama.min' => 'Inputan Nama minimal 3 karakter.',
			'usia.required' => 'Inputan Usia wajib diisi.',
			'telepon.required' => 'Inputan Telepon wajib diisi.',
			'email.required' => 'Inputan Email wajib diisi.',
			'email.email' => 'Inputan harus berupa Email.'
		);
		# Validasi
		$validasi = Validator::make($input, $aturan, $pesan);
		# Bila validasi gagal
		if($validasi->fails()) {
			# Kembali kehalaman yang sama dengan pesan error
			return Redirect::back()->withErrors($validasi)->withInput();
		# Bila validasi sukses
		} else {
			# Ubah isi database berdasarkan id
			$ganti = Biodata::find($id);

			$ganti->nama			= Input::get('nama');
			$ganti->usia 			= Input::get('usia');
			$ganti->jenis_kelamin 	= Input::get('jenis_kelamin');
			$ganti->telepon 		= Input::get('telepon');
			$ganti->email 			= Input::get('email');
			$ganti->save();
			# Kehalaman beranda dengan pesan sukses
			return Redirect::route('beranda')->withPesan('Biodata baru berhasil ditambahkan.');
		}
	}
	
	...
}
?>
{% endhighlight %}

####View

{% highlight html %}
{% raw %}
// app/views/biodata/index.blade.php
...
Baris 38	<a href="{{ route('ubah', $data->id) }}">Edit</a>
...
{% endraw %}
{% endhighlight %}

{% highlight html %}
{% raw %}
// app/views/ubah.blade.php
<html>
	<head>
		<title>Ubah Biodata {{ $biodata->nama }}</title>
	</head>
	<body>
		<h2>Ubah Informasi Biodata</h2>
		<!-- Kita gunaka model karena kita akan mengubah data yang telah ada -->
		{{ Form::model($biodata, array('route' => array('ganti', $biodata->id), 'method' => 'PUT')) }}
			{{ Form::label('nama', 'Nama') }}
			<!-- Parameter kedua merupakan value, jadi terlihat terisi dengan data nama sebelumnya -->
			{{ Form::text('nama', $biodata->nama) }}
			<!-- Bila ada errors validasi letakkan disini 
			variabel errors ($errors) berasal dari Controller ['withErrors'] -->
			@if($errors->has('nama'))
				{{ $errors->first('nama') }}
			@endif
			<br/>
			{{ Form::label('usia', 'Usia') }}
			{{ Form::text('usia', $biodata->usia) }}
			<!-- Penjelasan sama seperti diatas -->
			@if($errors->has('usia'))
				{{ $errors->first('usia') }}
			@endif
			<br/>
			{{ Form::label('jenis_kelamin', 'Jenis Kelamin') }}
			<!-- Untuk select, parameter 1 = id, parameter 2 = option, parameter 3 = value -->
			{{ Form::select('jenis_kelamin', $jenis_kelamin, $biodata->jenis_kelamin) }}
			@if($errors->has('jenis_kelamin'))
				{{ $errors->first('jenis_kelamin') }}
			@endif
			<br/>
			{{ Form::label('telepon', 'Telepon') }}
			{{ Form::text('telepon', $biodata->telepon) }}
			@if($errors->has('telepon'))
				{{ $errors->first('telepon') }}
			@endif
			<br/>
			{{ Form::label('email', 'Email') }}
			{{ Form::text('email', $biodata->email) }}
			@if($errors->has('email'))
				{{ $errors->first('email') }}
			@endif
			<br/>
			{{ Form::submit('Ubah') }}
		{{ Form::close() }}
		<a href="{{ route('beranda') }}">Kembali ke index</a>
	</body>
</html>
{% endraw %}
{% endhighlight %}

###DELETE

Dan terakhir adalah fitur untuk menghapus salah satu isi database.

####Route

{% highlight php %}
// app/routes.php
<?php
...

# Tindakan untuk menghapus Biodata [localhost:8000/{id}/hapus]
Route::get('hapus/{id}', array('as' => 'hapus', 'uses' => 'BiodataController@hapus'));

...
?>
{% endhighlight %}

####Controller

{% highlight php %}
// app/controllers/BiodataController.php
<?php
class BiodataController extends BaseController {
	...

	# DELETE localhost:8000/hapus/{id}
	public function hapus($id) {
		# Hapus biodata berdasarkan id
		Biodata::find($id)->delete();
		# Kembali kehalaman yang sama dengan pesan sukses
		return Redirect::back()->withPesan('Biodata berhasil dihapus.');
	}
}
?>
{% endhighlight %}

####View

{% highlight html %}
{% raw %}
// app/views/biodata/index.blade.php
...
Baris 39	<a href="{{ route('hapus', $data->id) }}">Hapus</a>
...
{% endraw %}
{% endhighlight %}

Untuk tahap ini tidak membutuhka view, hanya perubahan pada halaman index saja.

###Selesai

**CRUD** selesai. Coba jalankn perintah `php artisan serve` melalui **cmd** atau **terminal** lalu kunjungi `localhost:8000` melalui browser.

###KESIMPULAN

Sebenarnya bila ingin membuat sebuah aktivitas CRUD dalam aplikasi yang sedang kita bangun tata caranya sangat beragam, bahkan ada yang bersifat otomatis dengan memanfaatkan laravel generator milik Jeffrey Way. Disini sengaja dibuat sedikit lebih ribet agar supaya pembaca yang mengikuti tutorial ini mengerti dan paham, khususnya mengenai fungsi-fungsi dasar yang sering ditemui dalam membangun sebuah project dalam laravel.

Akhirnya dengan ini saya tutup tulisan ini. Terima kasih.

Untuk yang ingin melihat langsung hasil dari tutorial ini bisa :

###[Download via GITHUB](https://github.com/novay/laravel-crud-sederhana)