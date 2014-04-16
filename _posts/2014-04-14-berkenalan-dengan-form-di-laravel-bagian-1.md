---
layout: post
title: Kenalan dengan Form di Laravel Bagian 1
description: "Berkenalan dengan Form di Laravel."
modified: 2014-04-14
category: blog
tags: [php, laravel, trik]
comments: true
share: true
---

Disini saya akan mencoba mengenalkan beberapa syntax yang biasanya digunakan dalam FORM, diantaranya :

- [Form Open](#form-open)
- [Form Model](#form-model)
- [Form Close](#form-close)

##<a name="form-open"></a>Form Open

Bagi Anda yang sudah biasa developing, khususnya develop website dinamis pasti sering bertemu dengan tag `<form>`. Ya, singkatnya untuk menangani inputan dari seluruh field yang disediakan.

Di Laravel sebenarnya Anda diperbolehkan menggunakan tag-tag murni HTML untuk mengelolanya, tetapi disini saya akan sedikit memperkenalkan penggunaan `form` pada laravel yang memang menurut saya sedikit unik. Tentunya dengan memanfaatkan **Blade Template** milik laravel.

###Penggunaan Biasa

{% highlight php %}
{% raw %}
{{ Form::open() }}
{% endraw %}
{% endhighlight %}

Maka yang dihasilkan oleh HTML adalah seperti ini :

{% highlight html %}
<form method="POST" action="http://lokasi-direktori-sekarang" accept-charset="UTF-8">
<input name="_token" type="hidden" value="36stringacak">
{% endhighlight %}

Ada beberapa yang kita temukan disini :

- method yang digunakan adalah "POST"
- target URL kearah lokasi yang sama
- menambahkan `accept-charset="UTF-8"` yang berhubungan dengan penggunaan karakter font
- juga bonus `token` bertype `hidden` dengan `value` berupa 36 digit string yang teracak untuk alasan keamanan.

###Mengarahkan Ke URL yang diinginkan

Bila ingin mengarahkan ke URL yang diinginkan, Anda bisa gunakan syntax ini :

{% highlight php %}
{% raw %}
{{ Form::open(array('url' => 'http://www.apa.com/apa-gitu')) }}
{% endraw %}
{% endhighlight %}

Hasilnya akan seperti ini :

{% highlight html %}
<form method="POST" action="http://www.apa.com/apa-gitu" accept-charset="UTF-8">
<input name="_token" type="hidden" value="36stringacak">
{% endhighlight %}

###Mengarahkan ke Route

Gunakan cara ini :

{% highlight html %}
{% raw %}
{{ Form::open(array('route' => 'identitas.route')) }}
{% endraw %}
{% endhighlight %}

Hasilnya akan sama seperti diatas, tergantung dari tujuan `route` Anda.

###Mengarahkan ke Controller

Bisa gunakan cara berikut :

{% highlight html %}
{% raw %}
{{ Form::open(array('action' => 'NamaController@method')) }}
{% endraw %}
{% endhighlight %}

Pastikan `NamaController` beserta `method` yang Anda tuju tersedia agar tidak terjadi `error`. Dan form akan menuju method dalam Controller tersebut.

###Method yang Berbeda

Secara default Laravel akan selalu membuka form dengan method 'POST'. Sekarang bagaimana bila kita ingin menggunakan method lain seperti '**GET**', '**PUT**', '**PATCH**', atau '**DELETE**'?

Berikut saya jelaskan caranya :

{% highlight html %}
{% raw %}
{{ Form::open(array('method' => 'GET')) }}
{% endraw %}
{% endhighlight %}

Dan akan menghasilkan :

{% highlight html %}
<form method="GET" action="http://lokasi-direktori-sekarang" accept-charset="UTF-8">
{% endhighlight %}

Ada yang menarik disini, untuk method 'GET', laravel tidak akan menciptakan `token` karena memang tidak dibutuhkan. Coba sekarang gunakan syntax berikut :

{% highlight html %}
{% raw %}
{{ Form::open(array('method' => 'PUT')) }}
{% endraw %}
{% endhighlight %}

Dan hasilnya akan seperti ini :

{% highlight html %}
<form method="POST" action="http://lokasi-direktori-sekarang" accept-charset="UTF-8">
<input name="_method" type="hidden" value="PUT">
<input name="_token" type="hidden" value="36stringacak">
{% endhighlight %}

*Methodnya ada 2 ya?* Sebenarnya Laravel membuatnya seperti ini bukan tanpa alasan, mereka menjadikannya seperti ini karena mereka peduli. Peduli dengan browser-browser yang tidak support dengan method PUT', 'PATCH' dan 'DELETE'. Toh, tujuannya juga sama.

###Agar Form memperbolehkan proses Upload

Bila Anda ingin membuat sebuah form yang didalamnya terdapat aktifitas upload, entah itu upload gambar, file doc, dan lain sebagainya. Gunakan  `'files' => true`. Seperti berikut :

{% highlight html %}
{% raw %}
{{ Form::open(array('files' => true)) }}
{% endraw %}
{% endhighlight %}

Dan hasilnya akan seperti berikut :

{% highlight html %}
<form method="POST" action="http://lokasi-direktori" accept-charset="UTF-8" enctype="multipart/form-data">
<input name="_token" type="hidden" value="36digitacak">
{% endhighlight %}

Dengan begitu, form Anda berhak melakukan upload file kedalam server.

###Bonus

Bila Anda ingin membuat sebuah form lengkap didalamnya terdapat identitas route, diperbolehkan melakukan upload file sekaligus menambahkan *class css* didalamnya, Anda bisa gunakan seperti berikut :

{% highlight html %}
{% raw %}
{{ Form::open(array('route' => 'identitas.route', 'files' => true, 'class' => 'form-horizontal')) }}
{% endraw %}
{% endhighlight %}

##<a name="form-model"></a>Form Model

Bagian ini merupakan pengganti **Form Open** diatas, berfungsi untuk nge-*binding* data dari model. Ringkasnya, digunakan untuk melakukan perubahan pada record dalam salah satu tabel di database *(Update)*.

Caranya sebagai berikut :

{% highlight html %}
{% raw %}
{{ Form::model($item, array('route' => array('item-ganti', $item->id))) }}
{% endraw %}
{% endhighlight %}

Penjelasan :

- `$item` merupakan variabel yang diterima dari `Controller` atau `Route`. Biasanya memiliki seluruh record item yang berasal dari tabel. *( Model::find($id) )*
- Untuk parameter kedua **array()**, maksudnya semua inputan dalam form akan dirujuk ke identitas **route** sembari mengirim `id` dari record yang ingin di *update*.

##<a name="form-close"></a>Form Close

Ada yang dibuka, berarti harus ada yang ditutup. Dalam Laravel juga ada cara khusus untuk menutup form yang Anda bangun. 

Sebenarnya Anda masih bisa menggunakan cara lama seperti ini :

{% highlight html %}
</form>
{% endhighlight %}

Namun di dalam Laravel anda bisa menggunakan syntax seperti ini dengan memanfaatkan `blade`. Dan nilainya tetap sama-sama `</form>`.

{% highlight html %}
{% raw %}
`{{ Form::close }}`
{% endraw %}
{% endhighlight %}

Sekian pembahasan mengenai Form Open di Laravel. Semoga bermanfaat.