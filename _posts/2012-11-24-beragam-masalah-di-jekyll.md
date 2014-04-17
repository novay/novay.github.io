---
layout: post
title: Beragam Masalah di Jekyll
description: "Beragam masalah yang bisa ditemukan dalam penerapan Jekyll sebagai Engine Website"
modified: 2012-11-16
category: blog
tags: [jekyll, web, tips, troubleshoot, problem]
comments: true
share: true
---

Kali ini saya akan coba kumpulkan semua kesalahan-kesalahan yang biasa ditemukan dalam penggunaan Jekyll berdasarkan pengalaman pribadi saya sendiri :

Daftar Isi :

- [Liquid Exception: CP850 and UTF-8](#satu)
- [Missing Dependency: kramdown](#dua)

###<a name="satu"></a>Encoding Issue. Liquid Exception: incompatible character encodings: CP850 and UTF-8.

Penyelesaiannya gunakan syntax berikut lewa **CMD** :

{% highlight html %}
chcp 65001
{% endhighlight %}

###<a name="dua"></a>Missing Dependency: kramdown

Artinya Paket `kramdown` tidak terinstall di mesin Anda, gunakan cara berikut :

{% highlight html %}
$ gem install kramdown
{% endhighlight %}

---
Masalah akan selalu saya update setiap kali saya menemukan error unik baru.
---