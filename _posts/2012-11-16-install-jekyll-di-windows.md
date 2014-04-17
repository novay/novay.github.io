---
layout: post
title: Install Jekyll di Windows
description: "Instalasi Jekyll di Sistem Operasi Windows"
modified: 2012-11-16
category: blog
tags: [jekyll, web, tips]
comments: true
share: true
---

Sebenarnya Jekyll selama ini hanya memanjakan para pengguna varian Linux saja, tapi beberapa bulan terakhir ada seseorang yang telah mencoba mengembangkannya agar semua *command* yang tadinya hanya bisa diakses di distro linux dapat di akses oleh Windows melalui *command prompt*-nya. 

Eits, tunggu dulu... [Jekyll itu apa ya?]({{site.url}}/blog/2012/11/12/apa-itu-jekyll/), klik untuk memperjelas apa sebenarnya Jekyll itu. Untuk instalasi mari kita lanjut pada bahan-bahan yang sebelumnnya harus kita sediakan. 

Sebenarnya apabila dilihat dari kebutuhannya, agar Jekyll ini bisa berjalan baik di Windows kita, ada beberapa paket yang harus kita install lebih dulu, mulai dari **Ruby**, **Ruby GEM**, entah itu namanya **curl**, **Jekyll** itu sendiri, kemudian ada **Python** dan ada juga **Pygments**. Kali ini saya mencoba untuk tidak terlalu menyinggung semua paket. Dan saya rasa, untuk melakukan instalasi semua paket satu per satu itu terbilang ribet, ruwet, males, kebanyakan soalnya. 

Nah, disini saya akan jelaskan instalasi secara simpelnya saja. Untuk kalian yang ingin melakukan instalasi satu persatu bisa kunjungi [link ini](http://www.madhur.co.in/blog/2011/09/01/runningjekyllwindows.html) karena disini tidak akan dibahas secara keseluruhan. Intinya Jekyll jalan. Oke, fine. Saya males XD.

Berterima kasihlah pada **[Madhur Ahuja](http://www.madhur.co.in/)** yang telah berbaik hati menyatukan semua yang kita butuhkan tersebut kedalam satu paket lengkap yang siap pakai. 

[Unduh Portable Jekyll](https://www.dropbox.com/sh/40l6mgbl1ce2kej/Uldx8d8spz/PortableJekyll%201.3.0%20x86.7z) 203.74 MB

###Instruksi Instalasi

- Buat folder bernama **Jekyll** dalam drive **C:**. *(Contoh penerapan yang saya lakukan)*
- Buka file unduhan kemudian *extract* kedalam folder **Jekyll** yang Anda buat tadi. Jangan heran bila ukuran hasil *extract*-nya bisa mencapai 1,20 GB *(Bayangin coba kalo kamu install atu-atu)* :3
- Sekarang buka **System Properties**, melalui **Advanced system settings**.

<figure><center>
	<a href="{{ site.url }}/assets/post/2012-11-16-install-jekyll-di-windows-1.JPG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2012-11-16-install-jekyll-di-windows-1.JPG" width="500px"/>
	</a>
</center></figure>

- Tekan tombol **Environtment Variables...**

<figure><center>
	<a href="{{ site.url }}/assets/post/2012-11-16-install-jekyll-di-windows-2.JPG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2012-11-16-install-jekyll-di-windows-2.JPG" width="300px"/>
	</a>
</center></figure>

- Perhatikan tabel **System Variables** *(1)*, pada variabel **Path** *(2)* tekan **Edit...** *(3)*

<figure><center>
	<a href="{{ site.url }}/assets/post/2012-11-16-install-jekyll-di-windows-3.JPG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2012-11-16-install-jekyll-di-windows-3.JPG" width="300px"/>
	</a>
</center></figure>

- Tambahkan baris berikut dibelakang :
	
`C:\Jekyll\ruby\bin;C:\Jekyll\devkit\bin;C:\Jekyll\Git\bin;C:\Jekyll\Python\App;C:\Jekyll\devkit\mingw\bin;C:\Jekyll\curl\bin`

<figure><center>
	<a href="{{ site.url }}/assets/post/2012-11-16-install-jekyll-di-windows-4.JPG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2012-11-16-install-jekyll-di-windows-4.JPG" width="300px"/>
	</a>
</center></figure>

Depannya jangan lupa dipisah. Setiap *PATH* harus dipisahkan dengan tanda **;**

- Lalu **OK**, **OK**, dan **OK**.

- Untuk mengecek semuanya berjalan dan terinstall dengan baik, Anda dapat menggunakan cara berikut :

<figure><center>
	<a href="{{ site.url }}/assets/post/2012-11-16-install-jekyll-di-windows-5.JPG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2012-11-16-install-jekyll-di-windows-5.JPG" width="500px"/>
	</a>
</center></figure>

Sekarang instalasi kebutuhan paket Jekyll di Windows sudah lengkap, sekarang tersisa bagaimana nge-*build* Jekyll memanfaatkan paket-paket yang ada.

###Build Jekyll

- Caranya sederhana, melalui *command prompt*, arahkan posisi direktori sesuka Anda. Sebagai contoh saya akan melakukan pembuatan Jekyll di **Desktop**.
- Eksekusi `jekyll new coba-jekyll` dan Jekyll akan menciptakan satu folder yang akan menjadi situs jekyll pertama Anda dengan nama **coba-jekyll** di Desktop.
- Untuk mengakses situs jekyll, kita tidak bisa hanya dengan membuka file **index.html** yang diciptakan, karena hasilnya akan nihil. Namun agar bisa di akses, kita juga membutuhkan peran perintah jekyll melalui *cmd*. 
- Sekarang masuk ke direktori **coba-jekyll** dengan `cd coba-jekyll`.
- Kemudian eksekusi `jekyll serve`.

<figure><center>
	<a href="{{ site.url }}/assets/post/2012-11-16-install-jekyll-di-windows-6.JPG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2012-11-16-install-jekyll-di-windows-6.JPG" width="500px"/>
	</a>
</center></figure>

- Buka browser lalu kunjungi `localhost:4000` dan **taraaam...**

<figure><center>
	<a href="{{ site.url }}/assets/post/2012-11-16-install-jekyll-di-windows-7.JPG" target="_blank"> 
		<img src="{{ site.url }}/assets/post/2012-11-16-install-jekyll-di-windows-7.JPG" width="500px"/>
	</a>
</center></figure>