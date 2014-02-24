---
layout: post
title: Masalah "Windows was unable to install your Unknown Device"
description: "Bagi kalian yang kesal dengan update driver laptop atau komputer yang tak kunjung berfungsi"
modified: 2014-02-22
category: blog
tags: [tips, windows, troubleshoot]
comments: true
share: true
---

Masalah ini biasanya kita temui saat kita ingin melakukan *Update* driver komputer/laptop, dalam hal ini anggap saja kita berniat ingin menemukan driver yang cocok via online langsung dari **Device Manager**. Akan tetapi, saat ingin di **Update** malah problem seperti berikut muncul :

<center>
	<a href="{{ site.url }}/assets/post/2013-01-16-masalah-windows-was-unable-to-istall-your-unknown-device-1.JPG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2013-01-16-masalah-windows-was-unable-to-istall-your-unknown-device-1.JPG" width="500px"/>
	</a>
</center>

Sebenarnya bila bertemu masalah ini ada beberapa kemungkinan :

- Tidak terkoneksi dengan internet
- Kemungkinan driver yang dicari memang tidak diketemukan dimanapun seantero maya
- Ada settingan yang tertinggal atau terabaikan

Kali ini yang akan kita bahas adalah poin ke 3, "Ada setingan yang tertinggal". Maksudnya apa? Ya, berarti kita perlu sedikit melakukan perubahan sistem pada Windows.

###Instruksi

- Buka **System Properties**, caranya klik kanan pada **My Computer** kemudian pilih **Properties**, dilanjutkan dengan memilih **Advanced system settings**
- Pada tab **Hardware** *(1)*, pilih **Device Installation Settings** *(2)*

<center>
	<a href="{{ site.url }}/assets/post/2013-01-16-masalah-windows-was-unable-to-istall-your-unknown-device-2.JPG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2013-01-16-masalah-windows-was-unable-to-istall-your-unknown-device-2.JPG" width="500px"/>
	</a>
</center>

- Lalu pada form **Device Installation Settings**, pilih **Yes** seperti pada gambar

<center>
	<a href="{{ site.url }}/assets/post/2013-01-16-masalah-windows-was-unable-to-istall-your-unknown-device-3.JPG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2013-01-16-masalah-windows-was-unable-to-istall-your-unknown-device-3.JPG" width="500px"/>
	</a>
</center>

------------

Sekarang coba update ulang driver kamu, yang pasti harus konek internet loh ya.