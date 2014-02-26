---
layout: post
title: Mengatasi masalah "php is not recognized..."
description: "Untuk kalian yang sedang mengalami masalah ini, semoga bermanfaat."
modified: 2013-03-08
category: blog
tags: [laravel, php, tutorial, troubleshoot]
comments: true
share: true
---


Siapa tau kiranya ada diantara kalian yang sudah pernah menghadapi masalah ini, atau saat ini malah sedang bertemu masalah ini? 

Kalau begitu berarti Anda datang ke tempat yang lumayan tepat. 

Sebelumnya mari kita cocokkan masalah Anda dengan masalah yang saya maksud melalui gambar :

<figure><center>
	<a href="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-0.JPG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-0.JPG" width="500px"/>
	</a>
</center></figure>

Oke langsung ke inti. Apabila Anda bertemu `php is not recognized...` ini, berarti ada beberapa kemungkinan :

- PHP belum terinstall di laptop/komputer Anda.
- *Yang kerap kali terjadi*, Anda belum men-*setting* variabel sistem Anda. 

#### Bagaimana caranya? 

Untuk diketahui dalam studi kasus kali ini saya menggunakan OS **Windows 8** dan **XAMPP** sebagai penyedia **PHP**nya.

- Sekarang masuk ke **System Properties**, bisa gunakan tombol pintas **"WinFlag + PauseBreak"** pada keyboard.

<figure><center>
	<a href="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-5.JPG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-5.JPG" width="500px"/>
	</a>
</center></figure>

- Lalu pilih `Advanced system settings`.

<figure><center>
	<a href="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-1.JPG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-1.JPG" width="500px"/>
	</a>
</center></figure>

- Pilih lagi `Environment Variables...`.

<figure><center>
	<a href="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-2.JPG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-2.JPG" width="300px"/>
	</a>
</center></figure>

- Sekarang coba perhatikan tabel **System variables** (1), cari variabel **PATH** (2) pilih lalu tekan tombol `Edit...` (3).

<figure><center>
	<a href="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-3.JPG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-3.JPG" width="300px"/>
	</a>
</center></figure>

- Terakhir pada `Variabel value`, tambahkan dibelakangnya `;C:\xampp\php` atau dimanapun Anda meletakkan direktori instalasi **PHP** Anda. 

<figure><center>
	<a href="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-4.JPG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2013-03-08-mengatasi-masalah-php-not-recognized-4.JPG" width="300px"/>
	</a>
</center></figure>

Jangan lupa meletakkan simbol `;` sebagai pemisah. Semoga bermanfaat.