---
layout: post
title: Penalaran Relasi Database
description: "Penjelasan mengenai relasi database."
modified: 2014-04-15
category: blog
tags: [trik]
comments: true
share: true
---

Sesuai dengan permintaan salah satu teman kita, kali ini saya akan coba berbagi pengetahuan tentang cara penerapan relasi dalam membangun database. 

Beberapa relasi yang akan dijelaskan diantaranya :

- [Relasi One-to-One](#one-to-one)
- [Relasi One-to-Many](#one-to-many)
- [Relasi Many-to-Many](#many-to-many)

Simak dengan **fokus** penjelasannya vroh. 

---

##<a name="one-to-one"></a>Relasi One-to-One 

Sebut saja, saat ini saya memiliki 2 buah tabel :

{% highlight html %}
- mahasiswa
- wali
{% endhighlight %}

Dimana setiap mahasiswa pasti memiliki wali atau orang tua. Yang bila dilogikakan maka : 

**"Tiap Mahasiswa memiliki Satu Wali"**. 

Artinya `id` dari tabel `mahasiswa` akan dijadikan **foreign key** didalam tabel `wali` sebagai `id_mahasiswa`, atau sebaliknya.

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-1.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-1.png" width="400px"/>
		</a>
	</center>
</figure>

---

##<a name="one-to-many"></a>Relasi One-to-Many 

Untuk tahap ini, saya coba ubah analoginya :

{% highlight html %}
- mahasiswa
- dosen
{% endhighlight %}

Kita tahu bahwa setiap **Dosen Pembimbing** pasti memiliki lebih dari satu **Mahasiswa** yang menjadi anak bimbingannya. 

Dengan kata lain, **"Tiap Dosen memiliki Banyak Mahasiswa"**.

Artinya `id` dari tabel `dosen` akan dijadikan **foreign key** didalam tabel `mahasiswa` sebagai `id_dosen`, namun tidak untuk sebaliknya.

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-2.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-2.png" width="400px"/>
		</a>
	</center>
</figure>

---

##<a name="many-to-many"></a>Relasi Many-to-Many 

Kali ini saya akan gunakan analogi lain :

{% highlight html %}
- mahasiswa
- hobi
{% endhighlight %}

Dimana akan ada lebih dari satu mahasiswa yang memiliki hobi yang lebih dari satu.

Dikatakan `many-to-many` karena memenuhi standar logika yang berbunyi :

**"Banyak Mahasiswa yang memiliki Banyak Hobi"**.

Berbeda dengan sebelumnya, kita tidak bisa dengan hanya menempatkan `id` dari salah satu tabel kedalam tabel yang lain seperti halnya untuk kedua jenis relasi sebelumnya. *Terus gimana dong?*

Untuk relasi jenis ini, kita butuh 1 buah tabel tambahan sebagai perantara *(pivot)* antara tabel `mahasiswa` dengan tabel `hobi`. Sehingga untuk saat ini kita wajib memiliki 3 buah tabel :

{% highlight html %}
- mahasiswa
- hobi
- mahasiswa_hobi
{% endhighlight %}

Dimana `id` dari tabel `mahasiswa` beserta `id` dari tabel `hobi` akan ditempatkan kedalam tabel `mahasiswa_hobi` masing-masing sebagai `id_mahasiswa` dan `id_hobi`.

<figure>
	<center>
		<a href="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-3.png" target="_blank"> 
			<img src="{{ site.url }}/assets/post/2014-04-15-penalaran-relasi-database-3.png" width="400px"/>
		</a>
	</center>
</figure>

---

Gimana? Dapat bayangannya belum?

Kalo belum coba deh baca **lebih fokus** lagi.

Pada postingan [selanjutnya]({{site.url}}/blog/2014/04/16/implementasi-relasi-di-laravel-dengan-eloquent/), saya akan coba mengimplementasikan relasi-relasi tersebut dalam pembuatan proyek Laravel menggunakan istilah yang akrab di sebut **Eloquent ORM**. Terima Kasih.