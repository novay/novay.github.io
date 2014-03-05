---
layout: post
title: Tutorial Grid pada Bootstrap 3
description: "Buat kalian yang ingin memperdalam ilmu dari salah satu fungsi dalam bootstrap, dan buat kalian yang sering buat web menggunakan template jadi buatan orang lain."
modified: 2014-03-04
category: blog
tags: [tips, bootstrap, tutorial]
comments: true
share: true
---

Tutorial ini sebenarnya dikhususkan bagi kalian yang ingin memperdalam ilmu tentang penggunaan Bootstrap, dalam hal ini Bootstrap versi 3. 

Disesuaikan dengan judul yang saya ambil kali ini, maka saya akan coba menjelaskan sedikit mengenai penggunaan sistem grid terbaru pada *engine* ini.

Bootstrap 3 menggunakan sistem Grid yang terbilang baru, dalam arti penggunaannya berbeda dari pendahulunya *(bootstrap 2)*, dimana  didalamnya terdapat 4 macam penggunaan namun memiliki klasifikasi yang berbeda-beda. 

Berikut urutannya berdasarkan ukuran terkecil *gadget* yang digunakan :

1. `.col-xs-*` berguna untuk Handphone
2. `.col-sm-*` berguna untuk Tablet
3. `.col-md-*` berguna untuk Laptop/Desktop
4. `.col-lg-*` berguna untuk Komputer Besar

Note: Tanda * menggantikan jumlah kolom yang digunakan.

Sekarang lihat [DEMO]() untuk melihat perbedaan setiap penggunaannya, tentunya dengan cara menggecilkan ukuran browser Anda secara manual.

##Aturan Main

- Karena Bootstrap 3 menerapkan HTML5, maka letakkan `<!DOCTYPE html>` agar browser mengenali fungsi-fungsi pada HTML5
- Selalu tambahkan *meta tag* berikut `<meta name="viewport" content="width=device-width, initial-scale=1.0">`. Alasannya agar browser tahu kalau website yang kita kelola ini bersifat responsif.

Coba buat kurang Lebih syntax HTMLnya seperti ini :

	<!-- Untuk mengenali HTML5 -->
	<!DOCTYPE html>
	<html>
	<head>
		<!-- Agar browser tau kalau web yang kita kelola bersifat responsif -->
		<meta name="viewport" content="width=device-width, initial-scale=1.0">
		<title>Contoh Grid Bootstrap 3</title>
		<!-- Karena kita hanya butuh 1 css ini untuk contoh -->
		<link rel="stylesheet" type="text/css" href="bootstrap.css">
		<!-- Dan ini CSS tambahan dari saya -->
		<link rel="stylesheet" type="text/css" href="contoh-grid.css">
	</head>
	<body>
		<!-- Isikan Syntax-syntax konten disini -->


	</body>
	</html>

Sebelum kita bahas klasifikasi penggunaannya satu per satu, saya akan coba menjelaskan struktur dasar penggunaan Sistem Grid pada Bootstrap. Pada contoh berikut kita akan menggunakan class **container** dimana **container** itu sendiri memiliki total lebar 12 kolom. Jadi, pada dasarnya setiap baris atau **.row** selalu memiliki 12 kolom.

Sehingga :

	<div class="container">
		<div class="row">
			<div class="col-xs-12">
				<p>Sesuatu</p>
			</div><!-- .col-xs-12 -->
		</div><!-- .row -->
	</div><!-- .container -->

Hasilnya akan terlihat seperti 1 kolom selebar browser

###Sekarang bagaimana agar hasilnya terbagi menjadi 2 kolom ?

Gunakan `.col-*-6` karena 6 + 6 = 12.

###Dan untuk hasil 3 kolom

Gunakan `.col-*-4` karena 4 + 4 + 4 = 12.

Intinya, total dari setiap penjumlahan kolom harus selalu 12. Gimana? Paham? 

Biar cepat paham coba lihat gambaran berikut :

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-03-01-bootstrap-3-grid-tutorial-1.jpg" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-03-01-bootstrap-3-grid-tutorial-1.jpg" width="500px"/>
		</a>
	</center>
</figure>


##Contoh Penerapan Setiap Klasifikasi

###Extra Small (xs)

Pada klasifikasi ini, browser tidak akan melakukan perubahan pada tata'an kolom, dan akan menampilkannya apa adanya, berlaku untuk semua *device* atau *gadget*

```

###Small (sm)

Untuk klasifikasi ini, browser akan mengubah semua struktur grid menjadi 1 kolom walaupun tadinya ada banyak kolom, berlaku ketika layar berukuran smartphone. **Misalnya**, Anda saat ini memiliki 3 kolom dalam 1 baris, 

namun setelah Anda melakukan pengecilan browser sampai pada ukuran layar smartphone pada umumnya, maka yang tadinya ada 3 kolom akan berubah menjadi hanya 1 kolom dengan menjadikannya berurutan dan terlihat menjadi 3 baris.

###Medium (md)

Sama seperti penjelasan **Small (sm)**, bedanya perubahan akan langsung terjadi ketika browser diperkecil seukuran tablet dan seterusnya hingga seukuran smartphone.

###Large (lg)

Untuk klasifikasi ini, untuk kalian yang kodingan menggunakan laptop tidak akan melihat perbedaan yang signifikan dengan klasifikasi **Medium**, .
11:34 04/03/2014