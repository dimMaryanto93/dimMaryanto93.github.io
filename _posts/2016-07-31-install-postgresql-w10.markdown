---
layout: post
title: "Install PostgreSQL on Windows 10"
date: 2016-07-31T07:10:38+07:00
category: Windows10
tags: 
- PostgreSQL
- Database
- SQL
author: Dimas Maryanto
references:
comments: true
---

Kali ini saya akan membahas bagaimana cara install database PostgreSQL sama seperti [post sebelumnya]({% post_url 2016-07-29-install-postgresql-f23 %}) hanya salah kali ini di sistem operasi Windows 10. Ok langsung aja kita download softwarenya dari [website PostgreSQL](https://www.postgresql.org/download/windows/)

<!--more-->

Sebenarnya ada 2 cara untuk menginstall database PostgreSQL di Windows

![Pilihan install postgresql]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pg-download-1.png)

* Installer.
* Compile sediri dari binary source.

Nah saya akan menggunakan cara yang paling mudah ada yaitu pake yang Installer. di websitenya postgresql sendiri ada 2 pilihan untuk melakukan instalasi dengan Installer yaitu Graphical installer by BigSQL atau EnterpriceDB. Keduanya pada dasarnya sama mengediakan software postgresql tpi kayak kali ini bakalan coba yang BigSQL jadi kita buka aja [link berikut](http://www.bigsql.org/postgresql/installers.jsp)

![Installer by BigSQL]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pg-download-2.png)

Selanjutnya kita Download yang versi Windows karena system arsitektur windows saya 64bit jadi saya pilih yang ```x64```


Setelah selesai di Download langsung aja jalankan ```setup.exe```nya, maka akan muncul form seperti berikut:

![Starting install PostgreSQL]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pg-install-1.png)

Kemudian klik **Next** untuk melanjutkan, setelah itu maka akan muncul form seperti berikut:

![Select location install postgresql]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pg-install-2.png)

Kemudian kita pilih lokasi install untuk PostgreSQL, klo saya biarkan aja default seperti itu, Klik **Next** lalu maka akan muncul lagi form seperti berikut:

![Pilih sql editor]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pg-install-3.png)

Nah kita tinggal pilih SQL Editornya, klo saya udah terbiasa dengan menggunakan **pgAdmin3** jadi saya cheklist pgAdminIII seperti gambar diatas, Kemudian klik **Next** untuk menanjutkan:

![Init data]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pg-install-4.png)

Setelah itu akan muncul form untuk menyimpan data seperti gambar diatas, konfigurasi ini biarkan default aja. langsung klik **Next**. setelah ikut akan tampil form seperti berikut:

![]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pg-install-5.png)

Nah di form ini kita diminta untuk memasukan password default untuk schema ```postgres```. User ```postgres``` ini adalah superusernya yang bertujuan untuk membuat user baru, database, dan objek-objek lain di Database Management Sistemnya PostgreSQL. Jadi saran saya gunakan password yang mudah di ingat contohnya ```admin```, ```nama-anda``` atau lain-lain. klo saya passwordnya ```admin```. Setelah password di isi kita klik **Next** untuk melanjutkan dan menampilkan konfirmasi atas konfigurasi yang udah di setup barusan maka formnya seperti berikut:

![Konfirmasi konfigurasi postgresql]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pg-install-6.png)

Klo anda sudah yakin, langsung aja **Next** untuk menajukan ke proses Installasi seperti berikut:

![Progress install postgresql]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pg-install-7.png)

Setelah selesai, kita akan melihat tampilan seperti berikut:

![Finish install postgresql]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pg-install-8.png)

Klik **Finish** untuk menutup jendela dialog installasi PostgreSQL, selanjutnya kita login ke database postgresql. Ada 2 cara untuk login ke database yaitu dengan menggunakan command prompt dan juga tools yang telah kita tambahkan tadi yaitu pgAdminIII. sekarang kita coba dulu login dengan command prompt maka gunakan perintah seperti berikut:

{% highlight postgresql-console %}
psql -h localhost -U postgres
{% endhighlight %}

jika berhasil login maka tampilannya seperti berikut:

![Login command prompt]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pg-login-1.png)

Selain itu juga kita bisa menggunakan pgAdminIII berikut caranya:

buka dulu pgAdminIII, tampilanya seperti berikut:

![pgAdmin3]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pgadmin-1.png)

Karena masih belum ada setup apapun, jadi sekarang yang harus kita lakukan adalah membuat connection untuk login sebagai ```postgress```, berikut caranya:

Klik **File** kemudian **New Connection** maka akan tampil form seperti berikut:

![Form new connection]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pgadmin-2.png)

Kemudian kita isi dengan data sebagai berikut:

* Name = boleh bebas saran saya sih samakan dengan nama user atau schema yang akan digunakan klo saya namanya ```postgres```
* Host = karena saya install database ini komputer local jadi ```localhost```
* Port = port yang saya gunakan tadi adalah default ```5432```
* Service = biarkan kosongkan aja
* Username = isi dengan username yang anda ingin login klo saya karena blum membuat user atau schema lain selain ```postgres``` jadi saya pake ```postgres```
* Password = isilah dengan password dari username anda klo saya ```admin```.

Maka hasilnya seperti berikut:

![Form new connection]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pgadmin-3.png)

Jika sudah klik **OK** untuk meng-koneksikan dengan database dan user tersebut, klo sukses maka tampilanya seperti berikut:

![new connection available]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pgadmin-4.png)

Ok sampai sini kita udah melakukan Installasi software Database PosgreSQL dengan benar.

Setelah itu, ada beberapa yang harus dilakukan supaya sistem operasi anda tidak menjadi **LEMOT** waktu dihidupkan. berikut adalah caranya:

Silahkan anda masuk ke konfigurasi **View Local Services** seperti berikut:

![find local service]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pg-service-1.png)

Kemudian akan tampil konfigurasi seperti berikut:

![Local service]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pg-service-2.png)

setelah itu anda cari service namenya ```PostgreSQl 9.x Server``` seperti gambar berikut ini:

![PostgreSQL 9.x service]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pg-service-3.png)

Setelah ketemu, Klik **Kanan** di ```PostgreSQL 9.x Server``` tersebut kemudian pilih **Properties** maka akan tampil konfigurasi seperti berikut:

![PostgreSQL 9.x server configurasi default]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pg-service-4.png)

Jadi konfigurasi pada gambar diatas itu servicenya akan otomatis di jalankan ketika komputer dihidupkan nah disini lah yang membuat komputer menjadi lambat performanya pada saat di startup. Jadi solusinya adalah kita harus buat servicenya dijalan **Manual** atau **Automated with delay** klo saya sih pakenya yang **Manual** jadi saya harus jalankan servicenya sendiri setiap kali mau menggunakan database PostgreSQL jadi kurang lebih konfigurasinya seperti berikut:

![Manual service stated]({{ site.baseurl }}/assets/img/posts/install-postgresql-win10/pg-service-5.png)

Jika udah klik **OK** dan servicenya sekarang akan manual di jalankannya jadi harus buka **View Local Services** lagi klo mau menjalankan service. Ok mungkin cukup sekian dulu postingan kali ini, see you next post!.
