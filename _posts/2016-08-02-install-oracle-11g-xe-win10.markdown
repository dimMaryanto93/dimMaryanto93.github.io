---
layout: post
title: "How to install Oracle 11g XE di Windows 10"
date: 2016-08-02T10:31:49+07:00
category: Database
tags: 
- Database
- Oracle
- Express Edition
author: Dimas Maryanto
---

Halo nah kali ini saya mau share bagimana cara install Oracle 11g XE atau Oracle 11g Express Edition, Oracle 11g ini sebenarya ada 3 versi yaitu

* Oracle 11g XE
* Oracle 11g Standar Edition
* Oracle 11g Enterprice Edition.

Kanapa saya menggunakan yang XE? karena, karena kebutuhannya sebenarnya hanya untuk mengenal aja components dan objects apa aja yang ada dalam Database Oracle selain itu juga Oracle 11g XE ini sifatnya **Gratis** jadi gak perlu ngeluarin duit lagi heheh dan juga proses installasi nya terhitung cukup mudah berbeda dengan ke dua versi lainnya yang bisa dibilang agak ribet untuk kasus orang yang awam. Ok langsung aja kita install Oracle 11g XE.

<!--more-->

Pertama anda harus download dulu softwarenya di [website Oracle](http://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/index.html) seperti berikut:

![website]({{ site.baseurl}}/assets/img/posts/install-oracle-11g-xe-win10/oracle-download-1.png)

Setelah itu tingal pilih aja sesuia dengan platform klo saya karena pake Windows 10 dengan arsitektur 64bit jadi saya pilih yang ```Oracle Database Express Edition 11g Release 2 for Windows x64``` kemudian anda disuruh **login** kalo belum punya account Oracle, bikin aja dulu gratis kok dan gampang jadi saya skip aja ya untuk membuat accounya pasti anda semua bisalah.

![login]({{ site.baseurl}}/assets/img/posts/install-oracle-11g-xe-win10/oracle-download-2.png)

Setelah itu filenya di download dan hasilnya ada di file berupa archive ```.zip``` seperti berikut:

![download]({{ site.baseurl}}/assets/img/posts/install-oracle-11g-xe-win10/oracle-download-3.png)

Kemudian anda extract filenya menggunakan software bebas klo saya pake ```7z``` seperti berikut:

![extract]({{ site.baseurl}}/assets/img/posts/install-oracle-11g-xe-win10/oracle-download-4.png)

Hasilnya seperti berikut:

![hasil extract]({{ site.baseurl}}/assets/img/posts/install-oracle-11g-xe-win10/oracle-download-5.png)

Kemudian anda jalankan ```setup.exe``` yang ada didalam folder ```C:\Users\softw\Downloads\OracleXE112_Win64\DISK1``` seperti berikut:

![folder extract]({{ site.baseurl}}/assets/img/posts/install-oracle-11g-xe-win10/oracle-download-6.png)

Maka akan tampil form seperti berikut:

![prepared install oracle]({{ site.baseurl}}/assets/img/posts/install-oracle-11g-xe-win10/oracle-1.png)

![welcome oracle]({{ site.baseurl}}/assets/img/posts/install-oracle-11g-xe-win10/oracle-2.png)

Kemudian kita **Next** aja.

![lisences]({{ site.baseurl}}/assets/img/posts/install-oracle-11g-xe-win10/oracle-3.png)

Pilih **I accept...** lalu klik **Next**

![lokasi install]({{ site.baseurl}}/assets/img/posts/install-oracle-11g-xe-win10/oracle-4.png)

Ini tinggal pilih lokasi installasi oraclenya mau dimana, klo saya biarkan **default** saya simpan di ```C:\...``` klik **Next**

![password sys, system]({{ site.baseurl}}/assets/img/posts/install-oracle-11g-xe-win10/oracle-5.png)

Ini kita disuruh input password untuk schema ```SYS``` dan ```SYSTEM``` jadi **usahakan yang mudah diingat** karena khan pada dasarnya kita hanya pake untuk belajar kalo anda gunakan untuk perusahaan maka gunakan password yang dienskripsi supaya gak diketahui orang. jadi password ini digunakan untuk membuat user, menage database oracle dan lain-lain jadi sangat penting untuk diingat. klo saya passwordnya ```admin```. kemudian klik **Next**.

![confirm]({{ site.baseurl}}/assets/img/posts/install-oracle-11g-xe-win10/oracle-6.png)

Berikut adalah konfigurasinya klik **Next** selanjutnya adalah proses installnya:

![progress]({{ site.baseurl}}/assets/img/posts/install-oracle-11g-xe-win10/oracle-7.png)

Setelah selesai seperti berikut:

![finish]({{ site.baseurl}}/assets/img/posts/install-oracle-11g-xe-win10/oracle-8.png)

Setelah sampai sini anda telah berhasil menginstall Oracle 11g XE, tahap selanjutnya ini terserah sih mau dilakukan atau tidak tapi yang pasti sih hal berikut ini bertujuan untuk menghindari komputer menjadi **Lemot** atau berat karena service Oracle. jadi silahkan anda buka **Local Service** maka akan tampil form seperti berikut:

![service]({{ site.baseurl}}/assets/img/posts/install-oracle-11g-xe-win10/oracle-service-1.png)

Kemudian cari service namenya **OracleServiceXE** kemudian di **Klik Kanan** lalu **Properties** maka tampil form seperti berikut:

![manual service]({{ site.baseurl}}/assets/img/posts/install-oracle-11g-xe-win10/oracle-service-2.png)

Setelah itu ubah **Startup type** dari **Automatic** menjadi **Manual** supaya ketika komputer dihidupkan tidak lambat loadingnya. setelah itu maka hasilnya seperti berikut:

![result service]({{ site.baseurl}}/assets/img/posts/install-oracle-11g-xe-win10/oracle-service-3.png)

Setelah selesai konfigurasinya tahap selanjutnya anda bisa login dengan menggunakan command prompt, silahkan buka command promptnya kemudian masukan perintah berikut:

{% highlight bash %}
sqlplus SYSTEM
{% endhighlight %}

Setelah itu kita input password yang tadi pada saat install yaitu ```admin``` klo berhasil maka hasilnya seperti berikut:

![login]({{ site.baseurl}}/assets/img/posts/install-oracle-11g-xe-win10/oracle-login-1.png)

OK mungkin sekian dulu bagaimana cara install Oracle 11g XE di Windows. see you next post!.
