---
layout: post
title: Cara Mengakses Localhost Menggunakan Komputer Lain
description: "Cara Mengakses Localhost Menggunakan Komputer Lain."
modified: 2014-09-07
category: blog
tags: [jaringan, sharing]
comments: true
share: true
---

Lama tak berjumpa blog kusamku.

Kebetulan beberapa hari ini saya sering ditanya sama teman-teman, pertanyaannya kurang lebih seperti ini.

**"Bagaimana cara mengakses localhost dari laptop saya menggunakan laptop lain?"**

Oke, kali ini saya akan jelasin jawaban dari pertanyaan diatas. 

Pertama-tama, pastiin laptop yang menjadi **server** sudah terkoneksi dengan laptop-laptop lain yang bertindak sebagai **client**.

Jadi pertanyaannya sekarang adalah **Bagaimana cara mengkoneksikannya?**

Dari pertanyaan kedua sudah jelas kalau laptop yang bertindak sebagai **server** harus memberikan akses kepada **client-client** yang ingin mengakses `localhost`nya dia terlebih dahulu dengan menggunakan jaringan **Ad-Hoc**.

Secara definitif jaringan **Ad-Hoc** pada dasarnya adalah sebuah jaringan area lokal yang dapat diatur dengan sangat mudah dan spontan dalam waktu apapun yang memungkinkan komputer dan perangkat untuk berkomunikasi secara langsung satu sama lain selama masih berada dalam jangkauan *ad-hoc*. Sebenarnya teknologi ini telah ada saat Windows masih XP. Sejak saat itulah sesuatu menjadi mungkin untuk berbagi data dan koneksi internet antara perangkat nirkabel lainnya.

Untuk hari ini saya hanya akan menuangkan cara mengkoneksikannya menggunakan sistem operasi Windows 7 dulu, mungkin untuk Windows 8 besok saya buatkan, kenapa? karena caranya menurut saya lebih sulit dari Windows 7.

### Cara di Windows 7

Sekarang fokuskan ke tombol Start Windows, kemudian pada kolom *search* ketikkan “wireless”. Sampai kamu melihat opsi “Manage Wireless Networks” dan pilih.

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-01.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-01.png" width="500px"/>
		</a>
	</center>
</figure>

Panel baru akan muncul. Sekarang tekan tombol **Add**.

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-02.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-02.png" width="500px"/>
		</a>
	</center>
</figure>


Selanjutnya klik “Create an ad hoc network”.

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-03.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-03.png" width="500px"/>
		</a>
	</center>
</figure>

Sekarang kalian akan dihadapkan dengan jendela dimana berisi pesan-pesan dan keterangan terkait tentang jaringan Ad Hoc yang kita maksud disini. Abaikan dengan klik “Next”.

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-04.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-04.png" width="500px"/>
		</a>
	</center>
</figure>


Pada panel selanjutnya, kamu akan ditanya tentang apa yang akan jadi nama jaringan, tipe keamanan, dan password untuk jaringan Anda. 
Secaram umum, tipe keamanan *(Security type)* yang digunakan adalah *WPA2-Personal*.
Centang “Save Network” pabila kedepannya Anda akan selalu menggunakan jaringan ad-hoc ini. 

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-05.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-05.png" width="500px"/>
		</a>
	</center>
</figure>

Setelah selesai, tunggu sampai proses pembuatan jaringan selesai.

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-06.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-06.png" width="500px"/>
		</a>
	</center>
</figure>

Berikut panel bila jaringan ad-hoc berhasil dibuat dan siap untuk digunakan.

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-07.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-07.png" width="500px"/>
		</a>
	</center>
</figure>

Untuk memastikannya kamu bisa melihatnya pada taskbar. Bila tertulis “Waiting for users” berarti sejauh ini prosesnya lancar.

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-08.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-08.png" width="500px"/>
		</a>
	</center>
</figure>

Sekarang coba akses jaringan yang Anda buat tadi di komputer atau laptop lain yang bertindak sebagai client. Bila ada langsung coba koneksikan.

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-09.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-09.png" width="500px"/>
		</a>
	</center>
</figure>

Selesai. Sekarang server bisa diakses menggunakan komputer atau laptop lain.

Tambahan jika kamu ingin berbagi koneksi internet:

Klik kanan pada icon jaringan di taskbar, lalu pilih properties, tekan tab sharing dan centang “Allow other network users to connect through this computer’s Internet connection“.

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-10.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-10.png" width="500px"/>
		</a>
	</center>
</figure>

---

Setelah kedua laptop saling terkoneksi, selanjutnya cari tau **IP Address** milik si server, caranya :

Buka command prompt pada komputer server dengan cara `run` -> ketik `cmd` seperti gambar berikut:

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-11.PNG" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-11.PNG" width="500px"/>
		</a>
	</center>
</figure>

Selanjutnya ketik `ipaddress` lalu enter. Berikut letak IP Address si server. (Perhatikan yang diberik warna kuning).

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-12.PNG" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-12.PNG" width="500px"/>
		</a>
	</center>
</figure>


Pastikan XAMPP si **server** running. Lalu pada komputer **client** yang terkoneksi tadi akses 

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-13.PNG" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-09-07-cara-akses-localhost-di-komputer-lain-13.PNG" width="500px"/>
		</a>
	</center>
</figure>