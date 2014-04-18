---
layout: post
title: Implementasi Relasi di Laravel dengan Eloquent
description: "Penjelasan mengenai penggunaan relasi di Laravel menggunakan Eloquent ORM."
modified: 2014-04-16
category: blog
tags: [php, laravel, trik]
comments: true
share: true
---

Lanjutan dari postingan sebelumnya, kali ini saya akan coba berbagi pengetahuan tentang cara penerapan relasi database pada Laravel menggunakan *Eloquent*. **Apa itu Eloquent?**

Oke, saya kasih dulu daftar isi postingan ini vroh : 

- [Pendahuluan](#pendahuluan)
- [Siapkan Laravel](#laravel)
- [Penggunaan Relasi One-to-One](#one-to-one)
- [Penggunaan Relasi One-to-Many](#one-to-many)
- [Penggunaan Relasi Many-to-Many](#many-to-many)
- [Implementasi Lanjutan](#implementasi)
- [CRUD dengan Eloquent](#crud)
- [Kesimpulan](#kesimpulan)

---

##<a name="pendahuluan"></a>Pendahuluan

Seperti yang telah saya tuliskan pada postingan saya sebelumnya, bahwasanya ada 3 jenis relasi yang telah saya coba jelaskan, diantaranya :

- Relasi One-to-One
- Relasi One-to-Many
- Relasi Many-to-Many

**Sfx : Eloquent apaan woy?* 

Oke oke, kalo menurut saya **Eloquent** itu calo. *Kenapa calo?* Karena melalui eloquent kita bisa berinteraksi secara langsung dengan isi Database. Setiap model *eloquent* yang kita buat akan bertanggung jawab atas satu tabel dalam database.

Misal, saat ini kita memiliki tabel `mahasiswa`. Artinya kita akan menciptakan sebuah model baru bernama `Mahasiswa` yang nantinya akan bertanggung jawab atas isi dari tabel `mahasiswa` tadi. *Bener ngga?*

Dengan demikian, bila kita ingin berinteraksi dengan tabel `mahasiswa`, kita hanya perlu melakukannya melalui model `Mahasiswa` yang kita buat barusan.

*Beneran CALO kalo?*

Nah, berhubung saat ini saya akan berbagi mengenai cara penggunaan relasi di Laravel, saya sarankan kalian paham dulu deh tentang ketiga macam relasi yang saya sebut diatas. 

Buat kalian yang masih kurang paham atau masih bingung, coba deh baca tulisan saya sebelumnya [disini]({{site.url}}/blog/2014/04/15/penalaran-relasi-database/).

Buat yang sudah paham atau nekat nih. Sekarang saya akan coba membuatnya kedalam proyek Laravel. Tentunya dengan menggunakan perumpamaan yang sama dengan postingan saya tentang [Penalaran Relasi Database]({{site.url}}/blog/2014/04/15/penalaran-relasi-database/) tadi.

Langsung saja, bila dikumpulkan kembali, maka ada 5 buah tabel yang tercipta :

{% highlight html %}
- mahasiswa
- wali
- dosen
- hobi
- mahasiswa_hobi
{% endhighlight %}


**Ya toh?** Dan bila digambarkan akan menjadi seperti ini :

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-04-16-implementasi-relasi-di-laravel-dengan-eloquent-1.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-04-16-implementasi-relasi-di-laravel-dengan-eloquent-1.png" width="500px"/>
		</a>
	</center>
</figure>

Dari semua tabel ini, kita akan coba berkenalan dengan masing-masing relasi. <small>*Gambar dibuat menggunakan [Pencil](http://pencil.evolus.vn/).*</small>

##<a name="laravel"></a>Siapkan Laravel

Kita lakukan dengan cepat :

- Buat proyek dengan `composer create-project laravel/laravel relasi-laravel --prefer-dist`
- Sediakan database yang akan digunakan dan lakukan konfigurasi di `app/config/database.php`.

*Bingung?* Berarti kamu belum [baca ini]({{site.url}}/blog/2013/04/03/membuat-autentikasi-login-laravel-bagian-1/).

---

##<a name="one-to-one"></a>Penggunaan Relasi One-to-One 

Untuk tahap awal, mari kita berkenalan dengan **Relasi One-to-One**.

Dari total 5 tabel yang telah ada, ditahap ini kita hanya butuh 2 tabel seperti apa yang ada digambar :

{% highlight html %}
- mahasiswa
- wali
{% endhighlight %}

Katakanlah tiap `mahasiswa` memiliki `nama` dan `nim`, sedangkan tiap `wali` hanya memiliki `nama` beserta `foreign key` bernama `id_mahasiswa` yang nilainya diambil dari `id` milik tabel `mahasiswa`.

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-1.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-1.png" width="400px"/>
		</a>
	</center>
</figure>

Untuk `id_dosen` abaikan dulu.

###- Migration

Pertama-tama kita buat kedua file migration yang dibutuhkan :

####Tabel Mahasiswa

{% highlight html %}
php artisan migrate:make buat_tabel_mahasiswa --create=mahasiswa
{% endhighlight %}

Isi seperti berikut :

{% highlight php %}
// app/database/migrations/####_##_##_######_buat_tabel_mahasiswa.php
<?php
...

Schema::create('mahasiswa', function(Blueprint $table)
{
	$table->increments('id');

	$table->string('nama');
	$table->string('nim');

	$table->timestamps();
});

...
?>
{% endhighlight %}

####Tabel Wali

{% highlight html %}
php artisan migrate:make buat_tabel_wali --create=wali
{% endhighlight %}

Juga isi seperti berikut :

{% highlight php %}
// app/database/migrations/####_##_##_######_buat_tabel_wali.php
<?php
...

Schema::create('wali', function(Blueprint $table)
{
	$table->increments('id');

	$table->string('nama');
	$table->unsignedInteger('id_mahasiswa');
	$table->foreign('id_mahasiswa')->references('id')->on('mahasiswa')->onDelete('CASCADE');

	$table->timestamps();
});

...
?>
{% endhighlight %}

####Migrate

{% highlight html %}
php artisan migrate
{% endhighlight %}

###- Model

Karena alasan tertentu saya sengaja mendahulukan **Model** ketimbang **Seed**. 

Sekarang coba buat 2 buah model baru didalam direktori `app/models/`. Dengan nama masing-masing sebagai `Mahasiswa` dan `Wali`.

####Model Mahasiswa

{% highlight php %}
// app/models/Mahasiswa.php
<?php

# Sesuaikan nama class dengan nama file yang dibuat
class Mahasiswa extends Eloquent {

	# Tentukan nama tabel terkait
	protected $table = 'mahasiswa';

	# MASS ASSIGNMENT
	# Untuk membatasi attribut yang boleh di isi (Untuk keamanan)
	protected $fillable = array('nama', 'nim');

	/*
	 * Relasi One-to-One
	 * =================
	 * Buat function bernama wali(), dimana model 'Mahasiswa' memiliki relasi One-to-One
	 * terhadap model 'Wali' sebagai 'id_mahasiswa'
	 */
	public function wali() {
		return $this->hasOne('Wali', 'id_mahasiswa');
	}

	# Relasi One-to-Many nanti disini...

	# Relasi Many-to-Many nanti disini...

}
?>
{% endhighlight %}

####Model Wali

{% highlight php %}
// app/models/Wali.php
<?php

# Sesuaikan nama class dengan nama file yang dibuat
class Wali extends Eloquent {

	# Tentukan nama tabel terkait
	protected $table = 'wali';

	# MASS ASSIGNMENT
	# Untuk membatasi attribut yang boleh di isi (Untuk keamanan)
	protected $fillable = array('nama', 'id_mahasiswa');

	/*
	 * Relasi One-to-One
	 * =================
	 * Sebaliknya, buat function bernama mahasiswa(), dimana model 'Wali' memiliki relasi One-to-One (belongsTo)
	 * sebagai timbal balik terhadap model 'Mahasiswa'
	 */
	public function mahasiswa() {
		return $this->belongsTo('Mahasiswa', 'id_mahasiswa');
	}

}
?>
{% endhighlight %}

###- Seed

Sekarang mari kita buat file `Seeder` baru secara manual di direktori `app/database/seeds/` dengan nama `SeederRelasi.php`.

{% highlight php %}
// app/database/seeds/SeederRelasi.php
<?php

# Sesuaikan nama class dengan nama file yang dibuat
class SeederRelasi extends Seeder {

	public function run() {

		# Kosongin isi tabel
		DB::table('mahasiswa')->delete();
		DB::table('wali')->delete();

		/***********************************
		 *** SIAPKAN SEEDER DOSEN DISINI ***
		 ***********************************/

		//

		/* Kita akan membuat 3 orang mahasiswa sebagai sampel
		 * Disinilah alasan kenapa saya membuat model terlebih dahulu
		 * Karena saya memanfaatkan model untuk mengcreate record
		*/

		# Mahasiswa Pertama bernama Noviyanto Rachmadi. Dengan NIM 1015015072.
		$novay = Mahasiswa::create(array(
			'nama' => 'Noviyanto Rachmadi',
			'nim'  => '1015015072'));

		# Mahasiswa Kedua bernama Djaffar. Dengan NIM 1015015088.
		$dije = Mahasiswa::create(array(
			'nama' => 'Djaffar',
			'nim'  => '1015015088'));

		# Mahasiswa Ketiga bernama Puji Rahayu. Dengan NIM 1015015078.
		$ayu = Mahasiswa::create(array(
			'nama' => 'Puji Rahayu',
			'nim'  => '1015015078'));

		# Informasi ketika mahasiswa telah diisi.
		$this->command->info('Mahasiswa telah diisi!');

		/*
		 * Disini kita akan menggunakan ketiga variabel yang kita
		 * deklarasikan diatas yaitu '$novay', '$dije', '$ayu'
		 * dengan cara mengambil id dari masing-masing variabel yang
		 * baru saja di isi.
		 */

		# Ciptakan wali si $novay
		Wali::create(array(
			'nama'  => 'Achmad S',
			'id_mahasiswa' => $novay->id
		));
		# Ciptakan wali si $dije
		Wali::create(array(
			'nama'  => 'Entahlah',
			'id_mahasiswa' => $dije->id
		));
		# Ciptakan wali si $ayu
		Wali::create(array(
			'nama'  => 'Arianto',
			'id_mahasiswa' => $ayu->id
		));

		# Informasi ketika semua wali telah diisi.
		$this->command->info('Data mahasiswa dan wali telah diisi!');

		/***********************************
		 *** SIAPKAN SEEDER HOBI DISINI  ***
		 ***********************************/

		//

	}
}
?>
{% endhighlight %}

Sekarang kita aktifkan seeder yang akan kita buat tadi melalui `app/database/seeds/DatabaseSeeder.php`.

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

		# Kita akan beri nama Seeder dengan 'SeederRelasi'
		$this->call('SeederRelasi');
		# Tampilkan informasi berikut bila Seeder telah dilakukan
		$this->command->info('SeederRelasi berhasil.');
	}

}
?>
{% endhighlight %}

Lakukan *Seeding* dengan `php artisan db:seed`.

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-04-16-implementasi-relasi-di-laravel-dengan-eloquent-2.PNG" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-04-16-implementasi-relasi-di-laravel-dengan-eloquent-2.PNG" width="500px"/>
		</a>
	</center>
</figure>

Sejauh ini memang masih terlihat biasa, hanya berhasil ngisi database dan belum ada yang terkesan tujuan sebenarnya dari relasi. Tapi tenang, sebentar lagi kita akan menemukan manfaat dari relasi yang kita buat barusan.

Oke, kalau begitu seharusnya dengan hanya menggunakan model `Mahasiswa`, saya bisa memanggil data dari tabel `wali` yang notabene milik model `Wali`. *Karena apa?* **BENER**.

Untuk membuktikannya mari kita lihat cara penerapannya.

###- Route

Proses bermula dari `route` dengan menyertakan variabel yang isinya mengambil 1 record mahasiswa, lalu `route` yang sekaligus bertindak sebagai `view` akan menampilkan nama wali mahasiswa yang dimaksud dengan memanfaatkan relasi yang ada.

{% highlight php %}
// app/routes.php
<?php
	
	# URL localhost:8000/relasi-1/
	Route::get('relasi-1', function() {

		# Temukan mahasiswa dengan NIM 1015015072
		$mahasiswa = Mahasiswa::where('nim', '=', '1015015072')->first();

		# Tampilkan nama wali mahasiswa
		return $mahasiswa->wali->nama;

	});

?>
{% endhighlight %}

Penjelasan untuk `return $mahasiswa->wali->nama;`:

- `$mahasiswa` berisi 1 record mahasiswa yang diambil berdasarkan ketentuan NIM
- `wali` merupakan function yang ada dalam model Mahasiswa. Coba deh buka model mahasiswa kamu, pasti ketemu maksudnya
- `nama` merupakan field dari tabel wali yang ingin ditampilkan, dalam hal ini bernama `nama`

Output :

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-04-16-implementasi-relasi-di-laravel-dengan-eloquent-3.PNG" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-04-16-implementasi-relasi-di-laravel-dengan-eloquent-3.PNG" width="500px"/>
		</a>
	</center>
</figure>

Dengan cara ini, kita bisa menampilkan `nama` wali dengan hanya memanfaatkan `id_mahasiswa` yang dihubungkan melalui relasi.

---

##<a name="one-to-many"></a>Penggunaan Relasi One-to-Many

Seperti yang terlihat dalam gambar paling atas, untuk relasi tahap ini kita juga hanya butuh 2 tabel :

{% highlight html %}
- mahasiswa
- dosen
{% endhighlight %}

Perhatikan kali ini kita akan membuat tabel baru dengan nama `dosen` dan pastinya tiap dosen memiliki `nama` dan `nipd`. Selain itu, kita juga akan menambahkan field baru kedalam tabel `mahasiswa` dengan nama `id_dosen`.

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-2.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-2.png" width="400px"/>
		</a>
	</center>
</figure>

###- Migration

Akan terjadi 2 aktifitas disini, pembuatan tabel baru dan penambahan field kedalam tabel yang sudah ada.

####Tabel Dosen <small>*Buat Baru*</small>

{% highlight html %}
php artisan migrate:make buat_tabel_dosen --create=dosen
{% endhighlight %}

Isi seperti berikut :

{% highlight php %}
// app/database/migrations/####_##_##_######_buat_tabel_dosen.php
<?php
...

	Schema::create('dosen', function(Blueprint $table)
	{
		$table->increments('id');

		$table->string('nama');
		$table->string('nipd');

		$table->timestamps();
	});

...
?>
{% endhighlight %}

####Tabel Mahasiswa <small>*Tambah Field*</small>

{% highlight html %}
php artisan migrate:make tambah_field_mahasiswa
{% endhighlight %}

Lalu isi seperti berikut :

{% highlight php %}
// app/database/migrations/####_##_##_######_tambah_field_mahasiswa.php
<?php
...
	
	public function up() {
		Schema::table('mahasiswa', function($table) {
			$table->unsignedInteger('id_dosen')->after('nim')->nullable();
			$table->foreign('id_dosen')->references('id')->on('dosen')->onDelete('CASCADE');
		});
	}

...

	public function down() {
		Schema::table('mahasiswa', function($table) {
			$table->dropColumn('id_dosen');
		});
	}

...
?>
{% endhighlight %}

####Migrate

{% highlight html %}
php artisan migrate:refresh
{% endhighlight %}

###- Model

Karena model `mahasiswa` sudah ada, maka kita hanya perlu membuat **1 model** baru dengan nama `Dosen.php`. Dan tetap akan melakukan sedikit perubahan serta penambahan pada model `Mahasiswa`.

####Model Dosen

{% highlight php %}
// app/models/Dosen.php
<?php

# Sesuaikan nama class dengan nama file yang dibuat
class Dosen extends Eloquent {

	# Tentukan nama tabel terkait
	protected $table = 'dosen';

	# MASS ASSIGNMENT
	# Untuk membatasi attribut yang boleh di isi (Untuk keamanan)
	protected $fillable = array('nama', 'nipd');

	/*
	 * Relasi One-to-Many
	 * ==================
	 * Buat function bernama mahasiswa(), dimana model 'Dosen' akan memiliki 
	 * relasi One-to-Many terhadap model 'Mahasiswa' sebagai 'id_dosen'
	 */
	public function mahasiswa() {
		return $this->hasMany('Mahasiswa', 'id_dosen');
	}

}
?>
{% endhighlight %}

####Model Mahasiswa <small>*Sedikit Perubahan*</small>

{% highlight php %}
// app/models/Mahasiswa.php
<?php
...
	# Tambahkan 'id_dosen' disini
	protected $fillable = array('nama', 'nim', 'id_dosen');

...

	# Tambahkan baris berikut setelah relasi One-to-One, (BUKAN DIGANTI!)

	/*
	 * Relasi One-to-Many
	 * =================
	 * Buat function bernama dosen(), dimana model 'Mahasiswa' memiliki 
	 * relasi One-to-Many (belongsTo) sebagai penerima 'id_dosen'
	 */
	public function dosen() {
		return $this->belongsTo('Dosen', 'id_dosen');
	}

	# Relasi Many-to-Many nanti disini...

...
?>
{% endhighlight %}

###- Seed

Sekarang fokus di direktori `app/database/seeds/`, buka lagi file yang bernama `SeederRelasi.php`. Dan lakukan perubahan dan penambahan yang diperlukan.

{% highlight php %}
// app/database/seeds/SeederRelasi.php
<?php

# Sesuaikan nama class dengan nama file yang dibuat
class SeederRelasi extends Seeder {

	public function run() {

		# Kosongin isi tabel
		DB::table('mahasiswa')->delete();
		DB::table('wali')->delete();

		/***********************************
		 *** SIAPKAN SEEDER DOSEN DISINI ***
		 ***********************************/
		DB::table('dosen')->delete();

		# Tambahkan data dosen
		$dosen = Dosen::create(array(
			'nama' => 'Yulianto',
			'nipd' => '1234567890'));

		$this->command->info('Data dosen telah diisi!');

		# Kemudian tambahkan nilai id_dosen ditiap record mahasiswa 

		/* Kita akan membuat 3 orang mahasiswa sebagai sampel
		 * Disinilah alasan kenapa saya membuat model terlebih dahulu
		 * Karena saya memanfaatkan model untuk mengcreate record
		 */

		# Mahasiswa Pertama bernama Noviyanto Rachmadi. Dengan NIM 1015015072.
		$novay = Mahasiswa::create(array(
			'nama' => 'Noviyanto Rachmadi',
			'nim'  => '1015015072',
			'id_dosen' => $dosen->id));

		# Mahasiswa Kedua bernama Djaffar. Dengan NIM 1015015088.
		$dije = Mahasiswa::create(array(
			'nama' => 'Djaffar',
			'nim'  => '1015015088',
			'id_dosen' => $dosen->id));

		# Mahasiswa Ketiga bernama Puji Rahayu. Dengan NIM 1015015078.
		$ayu = Mahasiswa::create(array(
			'nama' => 'Puji Rahayu',
			'nim'  => '1015015078',
			'id_dosen' => $dosen->id));

		# Informasi ketika mahasiswa telah diisi.
		$this->command->info('Mahasiswa telah diisi!');

		...

	}

}
?>
{% endhighlight %}

*Seeding* dan **Poofft**.

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-04-16-implementasi-relasi-di-laravel-dengan-eloquent-4.PNG" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-04-16-implementasi-relasi-di-laravel-dengan-eloquent-4.PNG" width="500px"/>
		</a>
	</center>
</figure>

###- Route

Artinya disini kita akan menampilkan data milik tabel `dosen` melalui model `Mahasiswa` yang seharusnya hanya bertanggung jawab atas tabel `mahasiswa`.

{% highlight php %}
// app/routes.php
<?php

	...
	
	# URL localhost:8000/relasi-2/
	Route::get('relasi-2', function() {

		# Temukan mahasiswa dengan NIM 1015015072
		$mahasiswa = Mahasiswa::where('nim', '=', '1015015072')->first();

		# Tampilkan nama dosen pembimbing
		return $mahasiswa->dosen->nama;

	});

?>
{% endhighlight %}

Output :

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-04-16-implementasi-relasi-di-laravel-dengan-eloquent-5.PNG" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-04-16-implementasi-relasi-di-laravel-dengan-eloquent-5.PNG" width="500px"/>
		</a>
	</center>
</figure>

Oke, saya setuju kalau ini kurang lebih sama dengan relasi sebelumnya. *Bedanya apaan?*

Coba sekarang kita ubah objeknya menjadi `Dosen`. 

Melalui `Dosen`, kita akan menampilkan semua `mahasiswa` didikan si dosen yang dimaksud. 

Mari buka route kita kembali dan tambahkan baris syntax berikut dibawahnya.

{% highlight php %}
// app/routes.php
<?php

	...
	
	# URL localhost:8000/relasi-3/
	Route::get('relasi-3', function() {

		# Temukan dosen dengan yang bernama Yulianto
		$dosen = Dosen::where('nama', '=', 'Yulianto')->first();

		# Tampilkan seluruh data mahasiswa didikannya
		foreach ($dosen->mahasiswa as $temp)
			echo '<li> Nama : ' . $temp->nama . ' <strong>' . $temp->nim . '</strong></li>';
	});

?>
{% endhighlight %}

Penjelasan :

- Saya menggunakan `foreach` untuk menampilkan data mahasiswa yang berupa `array()`.
- `$dosen->mahasiswa` maksudnya mengambil semua data mahasiswa milik dosen terkait. Untuk lebih jelasnya, coba deh buka lagi model `Dosen` kamu. Lalu temukan `function mahasiswa` disana.
- `$temp` merupakan variabel penampungan nilai sementara
- `echo ...` sebenarnya bagian ini gabungan **HTML** sama **PHP**. *Ya kamu pasti tau lah...*

Output :

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-04-16-implementasi-relasi-di-laravel-dengan-eloquent-6.PNG" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-04-16-implementasi-relasi-di-laravel-dengan-eloquent-6.PNG" width="500px"/>
		</a>
	</center>
</figure>

---

##<a name="many-to-manu"></a>Penggunaan Relasi Many-to-Many

Saatnya berkenalan dengan **Many-to-Many**. Berikut saya cuplik beberapa kata-kata saya dari postingan sebelumnya.

*Berbeda dengan sebelumnya, kita tidak bisa dengan hanya menempatkan* `id` *dari salah satu tabel kedalam tabel yang lain.*

*Untuk relasi jenis ini, kita butuh 1 buah tabel tambahan sebagai perantara* **(pivot)** *antara tabel* `mahasiswa` *dengan tabel* `hobi`. 

Kalau begitu artinya kita akan menggunakan 3 tabel pada pembahasan relasi jenis ini :

{% highlight html %}
- mahasiswa
- hobi
- mahasiswa_hobi
{% endhighlight %}

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-3.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-3.png" width="400px"/>
		</a>
	</center>
</figure>

Dimana `id` dari tabel `mahasiswa` beserta `id` dari tabel `hobi` akan ditempatkan kedalam tabel `mahasiswa_hobi` masing-masing sebagai `id_mahasiswa` dan `id_hobi`.

###- Migration

Kita sudah punya tabel `mahasiswa`, jadi kita akan buat **2 tabel** lainnya :

####Tabel Hobi

{% highlight html %}
php artisan migrate:make buat_tabel_hobi --create=hobi
{% endhighlight %}

Lalu isi :

{% highlight php %}
// app/database/migrations/####_##_##_######_buat_tabel_hobi.php
<?php
...

Schema::create('hobi', function(Blueprint $table)
{
	$table->increments('id');

	$table->string('hobi');

	$table->timestamps();
});

...
?>
{% endhighlight %}

####Tabel Mahasiswa_Hobi

{% highlight html %}
php artisan migrate:make buat_tabel_mahasiswa_hobi --create=mahasiswa_hobi
{% endhighlight %}

Lalu isi seperti berikut :

{% highlight php %}
// app/database/migrations/####_##_##_######_buat_tabel_mahasiswa_hobi.php
<?php
...

Schema::create('mahasiswa_hobi', function(Blueprint $table)
{
	$table->increments('id');

	$table->unsignedInteger('id_mahasiswa');
	$table->foreign('id_mahasiswa')->references('id')->on('mahasiswa')->onDelete('CASCADE');
	$table->unsignedInteger('id_hobi');
	$table->foreign('id_hobi')->references('id')->on('hobi')->onDelete('CASCADE');

});

...
?>
{% endhighlight %}

**Jangan lupa hapus `timestamps`nya vroh.*

####Migrate

{% highlight html %}
php artisan migrate:refresh
{% endhighlight %}

###- Model

Terlihat bahwa ada **3 tabel** yang terlibat. **2 tabel** baru, dan **1 tabel** mahasiswa. 

Tapi disini, kita hanya akan buatkan model untuk tabel `hobi` saja, selain itu tabel mahasiswa juga akan mengalami penambahan syntax nantinya. Untuk tabel *pivot* (perantara) tidak butuh `Model`.

####Model Hobi <small>*Buat Baru*</small>

{% highlight php %}
// app/models/Hobi.php
<?php

# Sesuaikan nama class dengan nama file yang dibuat
class Hobi extends Eloquent {

	# Tentukan nama tabel terkait
	protected $table = 'hobi';

	# MASS ASSIGNMENT
	# Untuk membatasi attribut yang boleh di isi (Untuk keamanan)
	protected $fillable = array('hobi');

	/*
	 * Relasi Many-to-Many
	 * ===================
	 * Buat function bernama mahasiswa(), dimana model 'Hobi' memiliki relasi
	 * Many-to-Many (belongsToMany) terhadap model 'Mahasiswa' yang terhubung oleh
	 * tabel 'mahasiswa_hobi' masing-masing sebagai 'id_hobi' dan 'id_mahasiswa' 
	 */
	public function mahasiswa() {
		return $this->belongsToMany('Mahasiswa', 'mahasiswa_hobi', 'id_hobi', 'id_mahasiswa');
	}

}
?>
{% endhighlight %}

####Model Mahasiswa <small>*Tambah Dikit*</small>

{% highlight php %}
// app/models/Mahasiswa.php
<?php

# Sesuaikan nama class dengan nama file yang dibuat
class Mahasiswa extends Eloquent {

	...

	/*
	 * Relasi Many-to-Many
	 * ===================
	 * Buat function bernama hobi(), dimana model 'Mahasiswa' memiliki relasi
	 * Many-to-Many (belongsToMany) terhadap model 'Hobi' yang terhubung oleh
	 * tabel 'mahasiswa_hobi' masing-masing sebagai 'id_mahasiswa' dan 'id_hobi' 
	 */
	public function hobi() {
		return $this->belongsToMany('Hobi', 'mahasiswa_hobi', 'id_mahasiswa', 'id_hobi');
	}

}
?>
{% endhighlight %}

###- Seed

Buka lagi file `Seeder` yang ada di direktori `app/database/seeds/` bernama `SeederRelasi.php`. Lalu paste syntax berikut di baris yang telah disediakan.

{% highlight php %}
// app/database/seeds/SeederRelasi.php
<?php

# Sesuaikan nama class dengan nama file yang dibuat
class SeederRelasi extends Seeder {

	public function run() {

		...

		/***********************************
		 *** SIAPKAN SEEDER HOBI DISINI  ***
		 ***********************************/

		# Bersihkan tabel yang dibutuhkan
		DB::table('hobi')->delete();
		DB::table('mahasiswa_hobi')->delete();

		# Isi tabel hobi
		$mandi_hujan = Hobi::create(array('hobi' => 'Mandi Hujan'));
		$baca_buku = Hobi::create(array('hobi' => 'Baca Buku'));

		# Hubungkan Mahasiswa dengan Hobinya masing-masing
		$novay->hobi()->attach($mandi_hujan->id);
		$novay->hobi()->attach($baca_buku->id);
		$dije->hobi()->attach($mandi_hujan->id);
		$ayu->hobi()->attach($baca_buku->id);

		# Tampilkan pesan ini bila berhasil diisi
		$this->command->info('Mahasiswa beserta Hobi telah diisi!');

	}
}
?>
{% endhighlight %}

{% highlight php %}
php artisan db:seed
{% endhighlight %}

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-7.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-7.png" width="400px"/>
		</a>
	</center>
</figure>

###- Route

Untuk pembuktian kalau relasi ini tidak sia-sia. Coba ikuti langkah berikut :

{% highlight php %}
// app/routes.php
<?php
	
	# URL localhost:8000/relasi-4/
	Route::get('relasi-4', function() {

		# Bila kita ingin melihat hobi saya
		$novay = Mahasiswa::where('nama', '=', 'Noviyanto Rachmadi')->first();

		# Tampilkan seluruh hobi si novay
		foreach ($novay->hobi as $temp) 
			echo '<li>' . $temp->hobi . '</li>';
	});

?>
{% endhighlight %}

Output :

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-8.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-8.png" width="400px"/>
		</a>
	</center>
</figure>

Bukan cuman itu saja, kita juga bisa menampilkan yang sebaliknya. Dimana objek yang akan kita gunakan adalah `Hobi`. Dan dengan memanfaatkan `Hobi` kita bisa menapilkan siapa saja yang memiliki `Hobi` yang dimaksud.

{% highlight php %}
// app/routes.php
<?php
	
	# URL localhost:8000/relasi-5/
	Route::get('relasi-5', function() {

		# Temukan hobi Mandi Hujan
		$mandi_hujan = Hobi::where('hobi', '=', 'Mandi Hujan')->first();

		# Tampilkan semua mahasiswa yang punya hobi mandi hujan
		foreach ($mandi_hujan->mahasiswa as $temp)
			echo '<li> Nama : ' . $temp->nama . ' <strong>' . $temp->nim . '</strong></li>';

	});

?>
{% endhighlight %}

Ini output mereka yang punya hobi mandi hujan :

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-9.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-9.png" width="400px"/>
		</a>
	</center>
</figure>

*Gimana menurut kamu?*

---

##<a name="implementasi"></a>Implementasi Lanjutan

Kita telah membahas ketiga jenis relasi serta bagaimana penerapannya dalam Laravel. Sekarang saya akan menampilkan semua dengan hanya memanfaatkan model Mahasiswa. Selain itu, disini juga akan dijelaskan bagaimana agar data yang di kirim ditampilkan di View.

Untuk itu kita butuh 2 hal, mari kita coba :

####Route

Tambahkan bumbu berikut dalam route kalian :

{% highlight php %}
// app/routes.php
<?php
...

	# URL : localhost:8000/eloquent
	Route::get('eloquent', function() {

		# Ambil semua data mahasiswa (lengkap dengan semua relasi yang ada)
		$mahasiswa = Mahasiswa::with('wali', 'dosen', 'hobi')->get();

		# Kirim variabel ke View
		return View::make('eloquent', compact('mahasiswa'));
	});

...

?>
{% endhighlight %}

####View

Disini kita akan menggunakan **Blade Engine** untuk melakukan *looping* data dan menampilkannya di View. Coba buat file baru di direktori `app/views` dengan nama `eloquent.blade.php`.

{% highlight html %}
<!-- app/views/eloquent.blade.php -->

<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Halo Eloquent</title>
		<!-- CSS -->
		<link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.1.1/css/bootstrap.min.css">
		<style type="text/css"> body { padding-top:50px; } </style>
	</head>
	<body class="container">
		<div class="col-sm-8 col-sm-offset-2">
			@foreach ($mahasiswa as $temp)
				<h3>{{ $temp->nama }} <small>[{{ $temp->nim }}]</small></h3>
				<h5>Hobi : 
					@foreach($temp->hobi as $tampung) 
						<small>{{ $tampung->hobi }}, </small> 
					@endforeach
				</h5>
				<h4>
					<li>Nama Wali : <strong>{{ $temp->wali->nama }}</strong></li>
					<li>Dosen Pembimbing : <strong>{{ $temp->dosen->nama }}</strong></li>
				</h4>
				<hr/>
			@endforeach
		</div>
	</body>
</html>

{% endhighlight %}

Sekarang coba deh akses `http://localhost:8000/eloquent` lalu lihat hasilnya.

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-10.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-10.png" width="400px"/>
		</a>
	</center>
</figure>

---

##<a name="crud"></a>CRUD dengan Eloquent

Saya kasih bonus nih, untuk tahap ini saya akan coba berbagi beberapa hal yang bisa kita gunakan dalam Laravel dengan memanfaatkan Eloquent.

###Create

Untuk Create kita bisa menggunakan beberapa cara sebagai berikut :

{% highlight php %}
<?php

	# Membuat Mahasiswa baru kedalam tabel
	Mahasiswa::create(array(
		'nama' => 'Noviyanto Rachmadi',
		'nim'  => '1015015072'
	));

?>
{% endhighlight %}

Cara pertama diatas sepertinya sudah kita temui di penerapan tutorial diatas.

{% highlight php %}
<?php

	# Alternatif lain dalam membuat Mahasiswa baru
	# Buat sebuah objek atau instansi
	$temp               = new Mahasiswa;
	# Siapkan isi
	$temp->nama         = 'Noviyanto Rachmadi';
	$temp->nim          = '1015015072';
	# Simpan kedalam database
	$bear->save();

?>
{% endhighlight %}

Cara kedua ini kamu jadikan alternatif, ya walaupun terlihat lebih ribet, sebenarnya tetep memiliki tujuan yang sama kok.

Oh iya, kita juga bisa menggunakan `firstOrCreate()` atau `firstOrNew()`. Tapi kedua cara ini jarang banget dipakai. Tapi tak apa lah, sekalian nambah catatan pribadi saya.

{% highlight php %}
<?php

	/* Ini sebenarnya tujuannya sama seperti cara pertama,
	 * tapi bila kita perhatikan lagi, prosesnya sedikit berbeda.
	 * Mula-mula syntax akan mencari mahasiswa yang disebut dalam atribut
	 * (dalam hal ini mahasiswa bernama Noviyanto Rachmadi).
	 * Bila mahasiswa yang dimaksud tidak ditemukan, barulah mahasiswa
	 * baru ini dimasukin kedalam database
	 */
	Mahasiswa::firstOrCreate(array('nama' => 'Noviyanto Rachmadi'));

	/* Untuk cara ini prosesnya sama seperti cara barusan.
	 * Yang berbeda adalah penggunaannya yang menggunakan cara kedua
	 * sebagai instansi yang di tampung dalam variabel $temp
	 */
	$temp = Mahasiswa::firstOrNew(array('nama' => 'Noviyanto Rachmadi'));

?>
{% endhighlight %}

###Read

Ada banyak cara untuk memanggil isi tabel dengan menggunakan Eloquent. Berikut beberapa contoh yang bisa kita gunakan :

{% highlight php %}
<?php

	# Ambil semua isi tabel mahasiswa
	$temp = Mahasiswa::all();

	# Ambil data mahasiswa berdasarkan id
	$temp = Mahasiswa::find(1);

	# Temukan data mahasiswa berdasarkan ketentuan atribut
	$temp = Mahasiswa::where('nim', '=', '1015015072')->first();

	# Temukan semua data mahasiswa yang NIMnya diatas 1015015072
	$temp = Mahasiswa::where('nim', '>', '1015015072')->get();

?>
{% endhighlight %}

Mungkin diantara kamu ada yang penasaran dengan `first()` dan `get()`. Oke, ringkasnya `First` hanya akan mengambil data salah satu record yang paling mendekati ketentuan **(LIMIT 1)**. Sedangkan `Get` akan mengambil banyak record kemudian menampungnya menjadi `array` yang mengharuskan kamu untuk melakukan **Perulangan** agar bisa ditampilkan secara utuh.

###Update

Untuk melakukan perubahan data, berarti logikanya pertama-tama kita harus menemukan data yang ingin kita ubah dahulu *kan?*, setelah ketemu selanjutnya kita siapkan perubahan, sebelum akhirnya kita simpan kedalam database.

Oke, contoh sederhana saat ini kita memiliki tabel mahasiswa, dimana kita akan mengubah **NIM** milik salah satu mahasiswa. Dengan ketentuan yang tadinya **1015015072** akan diubah menjadi **1015015078**.

{% highlight php %}
<?php

	# Temukan mahasiswa yang memiliki nim 1015015072
	$temp = Mahasiswa::where('nim', '=', '1015015072')->first();

	# Siapkan perubahan atribut
	$temp->nim = '1015015078';

	# Simpan kedalam database
	$temp->save();

?>
{% endhighlight %}

###Delete

Untuk menghapus record dalam tabel sangatlah mudah, kita bisa menggunakan beberapa cara berikut :

{% highlight php %}
<?php

	# Temukan mahasiswa dengan id 1 lalu hapus
	$temp = Mahasiswa::find(1);
	$temp->delete();

	# Hapus mahasiswa yang memiliki id 1
	Mahasiswa::destroy(1);

	# Untuk menghapus beberapa mahasiswa sekaligus
	Mahasiswa::destroy(1, 2, 3);

	# Menghapus dengan menggunakan kondisi
	Mahasiswa::where('nim', '=', '1015015072')->delete();

?>
{% endhighlight %}

---

##Kesimpulan

Untuk hari ini saya berkeras kalo **Eloquent** itu CALO.

Bila kamu mengikuti tutorial ini dengan baik, maka saya pastikan kamu yang sekarang bukanlah kamu yang dulu lagi. Ngahahaha... Skip skip. Sekarang kamu pasti bisa menerapkan relasi dalam Laravel sendiri. 

Kembali lagi ke saya, mungkin saja banyak teori yang berbeda dari apa yang saya sampaikan, tapi tujuannya sama kok. :3 

Oke, saya kupas kembali beberapa poin penting dari postingan hari ini :

- Diatas hanya dijelaskan 3 jenis relasi. Padahal sebenarnya masih ada yang lainnya. *Cacat nih yang punya blog.*
- Pembahasannya acak adut nyampur sana sini. *Oke, maaf*
- Pokoknya intinya begitulah. **RELASI**.

<center>W-_-W *Where is the important things?* W-_-W</center>

###<center>Source Code [Via Github](https://github.com/novay/relasi-laravel)</center>

Semoga bermanfaat sahabatku sedunia dan setanah air.

<small>*Credit: Scotch*</small>