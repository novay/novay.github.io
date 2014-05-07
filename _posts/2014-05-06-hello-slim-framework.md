---
layout: post
title: Hello Slim Framework
description: "Mari berkenalan dengan slim framework."
modified: 2014-05-06
category: blog
tags: [php, slim, micro, framework]
comments: true
share: true
---

Sejujurnya postingan ini merupakan catatan saya pribadi vroh. Ada baiknya saya coba tuangkan segalanya kedalam blog ini. Itung-itung buat amal.

#Slim Framework. **Apa itu?**

Sesuai dengan namanya, Slim yang artinya ramping. Merupakan salah satu dari banyaknya framework PHP yang dikembangkan saat ini. Mungkin beberapa diantara kita pernah mendengar beberapa framework PHP lain, sebut saja seperti CodeIgnitier, Yii, CakePHP, Laravel.

Ya, mereka semua yang saya sebutkan tadi merupakan **full stack** framework kecuali **Slim**. Dikatakan **full stack** framework karena susunan syntaxnya yang menurut saya terlihat lebih kompleks dan juga tersedianya hampir semua library-library umum yang dapat digunakan oleh si pengembang. 

Dengan kata lain, akan sangat **mubajir** bila digunakan dalam pembuatan aplikasi ringan. *Kenapa mubajir?* Karena percuma banyak fungsi tapi tidak digunakan. *Ya toh?*

Selain ke-4 yang saya sebutkan barusan, sebenarnya masih banyak lagi diluar sana yang termasuk dalam kategori **full stack**. Lah, kok malah jadi bahas **full stack**.

Oke, sebenarnya bila kita kategorikan. PHP Framework saat ini dapat dibagi kedalam 2 jenis, 

- *Full Stack* Framework 
- *Micro* Framework

Bila tadi **full stack** framework memiliki library yang tersedia lengkap. Maka **Micro** framework sebaliknya, framework ini hanya menyediakan fitur-fitur standar untuk si pengembang. Dan *Micro* Framework ini lah tempat yang tepat untuk *Slim Framework* yang akan dibahas dipostingan ini.

Sekilas tentang Slim Framework :

- Slim Framework sebenarnya terinspirasi dari [Sinatra](www.sinatrarb.com), yang juga sesama micro framework dari bahasa Ruby.
- Ukurannya yang kecil berbanding terbalik dengan dengan fungsi dan fitur yang diberikan.
- Tentu saja sangat mudah dipelajari.

Dipostingan kali ini saya hanya sebatas berbagi tentang cara :

- [Instalasi Slim](#instalasi)
- [Menampilkan **Hello Slim**](#helloworld)
- [Secuil pengenalan penggunaan *Route*](#route)

##<a name="instalasi"></a>Instalasi Slim

Untuk instalasi Slim ini sebenarnya sudah sangat detail dijelaskan dalam dokumentasi resminya. 

####Kebutuhan :

- [Composer]({{site.url}}/blog/2012/07/24/installasi-composer/)
- XAMPP

Sebagai contoh coba buat folder di desktop Anda dengan nama `hello-slim`. Lalu buat file baru dengan nama `composer.json` lalu isi seperti berikut :

{% highlight php %}
{
	"require": {
		"slim/slim": "2.*"
	}
}
{% endhighlight %}

Sekarang buka CMD, lalu eksekusi ini :

`composer install`

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-05-06-hello-slim-framework-1.PNG" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-05-06-hello-slim-framework-1.PNG" width="500px"/>
		</a>
		<figcaption>Penggunaan di Command Prompt</figcaption>
	</center>
</figure>

Instalasi selesai.

##<a name="helloworld"></a>Menampilkan **Hello Slim**

Buat lagi file baru didalam folder `hello-slim` dengan nama `index.php`. Lalu isi dengan struktur Slim :

{% highlight php %}
<?php
# Gunakan hasil instalasi
require "vendor/autoload.php";

# Inisialisasi Slim
$app = new \Slim\Slim();

# Inilah route GET Slim yang simpel
$app->get('/', function () {
    echo "<h1>Hello, Slim</h1>";
});

# Pengenalan Route nanti disini

# Eksekusi Program
$app->run();

?>
{% endhighlight %}

Untuk melihat hasilnya, coba buka lagi *command promt* tadi, di direktori yang sama dan ketikkan perintah berikut :

`php -S localhost:1000`

Buka browser lalu kunjungi `localhost:1000`.

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-05-06-hello-slim-framework-2.PNG" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-05-06-hello-slim-framework-2.PNG" width="500px"/>
		</a>
		<figcaption>Hello World ala Slim</figcaption>
	</center>
</figure>

##<a name="route"></a>Secuil pengenalan penggunaan *Route*

Sekatang buka lagi file `index.php` tadi. Lalu tambahkan syntax berikut didalamnya :

{% highlight php %}
<?php

...

# Pengenalan Route nanti disini
$app->get('/:nama/:panjang', function ($nama, $panjang) {
    echo "<h1>" . $nama . " " . $panjang . "</h1>";
});

...

?>
{% endhighlight %}

Sekarang buka lagi *command promt*, dan lakukan hal yang sama seperti diatas hingga menghasilkan gambar berikut :

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-05-06-hello-slim-framework-3.PNG" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-05-06-hello-slim-framework-3.PNG" width="500px"/>
		</a>
		<figcaption>Penggunaan Route di Slim</figcaption>
	</center>
</figure>

===

Cukup sekian secuil pengenalan tentang Slim Framework. Mungkin kedepannya saya akan berbagi pengetahuan tentang pembangunan sebuah aplikasi yang menerapkan **REST API** dengan menggunakan framework ini. Semoga bermanfaat.

Lihat di [GitHub](https://github.com/novay/hello-slim)