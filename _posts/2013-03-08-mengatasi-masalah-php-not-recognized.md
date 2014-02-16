---
layout: post
title: Mengatasi masalah `php is not recognized...`
description: "Untuk kalian yang sedang mengalami masalah ini, semoga bermanfaat."
modified: 2013-03-08
category: blog
tags: [laravel, php, tutorial, troubleshoot]
comments: true
share: true
---


Siapa tau diantara kalian ada beberapa yang pernah mengalami masalah ini, atau saat ini sedang bertemu masalah ini? Berarti Anda datang ketempat yang lumayan tepat. Sebelumnya mari kita cocokkan masalah Anda dengan masalah yang saya maksud melalui gambar biar tidak salah paham :

<center>
	<a href="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-0.jpg" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-0.jpg" width="300px"/>
	</a>
</center>

Oke langsung ke inti. Apabila Anda bertemu `php is not recognized...` ini, berarti ada beberapa kemungkinan :

- PHP belum terinstall di laptop/komputer Anda.

- *Yang kerap kali terjadi*, Anda belum men-*setting* variabel sistem Anda. Kita hanya akan bahas poin ini saja.

#### Bagaimana caranya? 

Untuk diketahui dalam studi kasus kali ini saya menggunakan OS Windows 8 dan XAMPP sebagai penyedia PHPnya.

- Masuk ke System Properties, bisa gunakan tombol pintas "WinFlag + PauseBreak" pada keyboard.

<center>
	<a href="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-5.jpg" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-5.jpg" width="300px"/>
	</a>
</center>

- Lalu pilih `Advanced system settings`.

<center>
	<a href="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-1.jpg" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-1.jpg" width="300px"/>
	</a>
</center>

- Pilih lagi `Environment Variables...`.

<center>
	<a href="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-2.jpg" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-2.jpg" width="300px"/>
	</a>
</center>

- Sekarang coba perhatikan tabel **System variables** (1), cari variabel **PATH** (2) pilih lalu tekan tombol `Edit...` (3).

<center>
	<a href="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-3.jpg" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-3.jpg" width="300px"/>
	</a>
</center>

- Terakhir pada `Variabel value`, tambahkan dibelakangnya `;C:\xampp\php` atau dimanapun Anda meletakkan direktori instalasi PHP. Jangan lupa meletakkan simbol `;` sebagai pemisah.

<center>
	<a href="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-4.jpg" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-4.jpg" width="300px"/>
	</a>
</center>

Semoga bermanfaat.