---
layout: post
title: Instalasi Composer
description: "Hanya diperuntukkan buat kalian yang belum tahu dan ingin tahu cara instalasi Composer"
modified: 2012-07-24
category: blog
tags: [tips]
comments: true
share: true
---

Composer yang kita bahas kali ini bukan komposer yang ciptain musik itu loh ya. Composer yang dimaksud ialah si **juragan library** buat si PHP (kasarnya sih begitu).

Oke, Composer disini bertindak sebagai penyedia *library* untuk keperluan proyek Anda, khususnya buat Anda sebagai developer PHP. Terdapat sangat banyak *library* siap pakai yang tersedia yg dibagikan secara gratis untuk Anda *(Credit goes to each devs)*. Anda bisa mengunjungi [getcomposer.org](getcomposer.org) untuk informasi lebih lengkapnya.

Kali ini yang harus dipastikan adalah **PHP** telah terinstall di komputer/laptop Anda, entah itu yang Anda download langsung dari situs resminya, atau **PHP** paketan dari **XAMPP** dan lain-lain. Berikut langkah instalasinya : 

##Bagi pengguna Linux dan Mac

Kalau menurut saya pribadi, mungkin hanya beberapa saja pengguna ini yang bakalan nyari info tentang cara penginstalan ini di google, karena saya asumsikan pengguna keduanya pasti lebih familiar dengan jenis penggunaan syntax terminal berikut :

	curl -sS https://getcomposer.org/installer | php
	mv composer.phar /usr/local/bin/composer

##Bagi pengguna Windows

- Bisa langsung download [Composer](http://getcomposer.org/Composer-Setup.exe), ngga gede kok cuman 660kb.
- Lakukan instalasi seperti biasa **Next**, **Next**, dan *eits*. Sebelum melanjutkan, telusur dulu posisi file **php.exe** Anda. Seperti gambar berikut :

<center>
	<a href="{{ site.url }}/assets/post/2012-07-24-installasi-composer-1.JPG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2012-07-24-installasi-composer-1.JPG" width="500px"/>
	</a>
</center>

- Lalu **Next** dan **Install**. Tunggu hingga proses download selesai *(koneksi internet dibutuhkan)* sampai **Finish**.
- Setelah selesai, buktikan bila **composer** telah terinstall dengan sempurna seperti berikut

<center>
	<a href="{{ site.url }}/assets/post/2012-07-24-installasi-composer-2.JPG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2012-07-24-installasi-composer-2.JPG" width="500px"/>
	</a>
</center>

- Dan Happy Coding!