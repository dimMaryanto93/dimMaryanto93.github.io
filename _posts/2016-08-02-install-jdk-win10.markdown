---
layout: post
title: "Install Oracle JDK 8 di Windows 10"
date: 2016-08-02T11:13:51+07:00
category: Windows10
tags: 
- Oracle
- JDK
- Java
author: Dimas Maryanto
---

kali ini saya akan membuat atau membahas hal-hal apa aja yang harus di _setup_ untuk ngoding dengan bahasa pemograman Java berikut ulasannya tetang bagaimana cara setup Java Development Environtment pada Sistem Operasi Windows 10.

Nah sekarang kita install JDK, tapi sebelum itu kita download dulu JDKnya di [Website Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

<!--more-->

seperti gambar berikut:

![Situs Oracle]({{site.baseurl}}/assets/img/posts/install-oracle-jdk8-windows10/download-jdk-1.png)

Kemudian kita pilih **Java Platform (JDK) 8u101 / 8u102** *catatan akan berubah se-waktu2 jadi pilih versi yang paling baru ya!, tapi kalo anda sudah melakukan install JDK di Operasion System anda tidak perlu download cukup update aja JDK.a dari Control Panel atau Java Updater. Setelah ikut maka akan tampil Form seperti berikut:

![Pilih JDK version dan platform]({{site.baseurl}}/assets/img/posts/install-oracle-jdk8-windows10/download-jdk-2.png "Pilih Platform sesuia OS dan Download JDK")

Kemudian pilih sesuia dengan Platform System Operasi dan arsitekturnya, karena saya pake Windows 10 yang 64bit jadi saya pilih yang **Windows x64**

## Install Oracle JDK di Windows 10/8.1/7

setelah selesai di Download langsung aja jalankan **jdk-8u101-windows-x64.exe** maka akan muncul form seperti berikut:

![Form Instalasi JDK]({{site.baseurl}}/assets/img/posts/install-oracle-jdk8-windows10/install-jdk-1.png)

Klik next untuk menanjutkan, kemudian akan tampil form untuk menentukan lokasi JDK disimpan berikutnya:

![Form Installasi JDK 2 ]({{site.baseurl}}/assets/img/posts/install-oracle-jdk8-windows10/install-jdk-2.png)

Lokasi JDKnya saya mau biarkan default saja, kemudian klik next untuk selanjutnya melakukan instalasi JRE seperti berikut:

![Form Installas JRE]({{site.baseurl}}/assets/img/posts/install-oracle-jdk8-windows10/install-jdk-3.png)

Sama dengan JDK lokasi JREnya saya mau biarkan default aja, Setelah itu klik Next lagi dan selesai deh proses extractnya.

![Form Installasi JRE]({{site.baseurl}}/assets/img/posts/install-oracle-jdk8-windows10/install-jdk-4.png)

Tahap selanjutnya adalah setup PATHnya supaya bisa dikenali oleh Sistem Operasi Windowsnya jadi bisa di akses melalui Command Prompt (CMD). berkut langkah langkahnya:

Buka **File Explorer**nya kemudian Klik Kanan pada **This PC** setelah itu pilih **Properties**

![Setting Path Command Prompt]({{site.baseurl}}/assets/img/posts/install-oracle-jdk8-windows10/setpath-jdk-1.png)

Terus klik **Advanced system settings** maka akan tampil Form seperti Berikut:

![System properties]({{site.baseurl}}/assets/img/posts/install-oracle-jdk8-windows10/setpath-jdk-2.png)

Kemudian klik **Environtment Variables...** maka akan tampil form seperti berikut:

![Setting system variable dan variable name]({{site.baseurl}}/assets/img/posts/install-oracle-jdk8-windows10/setpath-jdk-3.png)

Di menu **User variables for dimMaryanto** klik button **New** maka akan tampil form seperti berikut:

![Setting JAVA_HOME]({{site.baseurl}}/assets/img/posts/install-oracle-jdk8-windows10/setpath-jdk-4.png)

Kemudian isi **Variable name:** dengan ```JAVA_HOME``` *huruf besar semua dan untuk **Variable value:** anda arahkan ke lokasi hasil installasi **JDK** klo saya lokasinya di sini ```C:\Program Files\Java\jdk1.8.0_91``` setelah ikut Klik **OK**, maka hasilnya seperti berikut:

![Setting JAVA HOME hasil]({{site.baseurl}}/assets/img/posts/install-oracle-jdk8-windows10/setpath-jdk-5.png)

Kemudian kita pindah ke **System variables**, anda cari **variable name:** dengan nama **PATH** atau **Path**

![Setting PATH JAVA]({{site.baseurl}}/assets/img/posts/install-oracle-jdk8-windows10/setpath-jdk-6.png)

kemudian anda klik button **Edit** maka akan tampil form seperti berikut:

![Setting Path Java 2]({{site.baseurl}}/assets/img/posts/install-oracle-jdk8-windows10/setpath-jdk-7.png)

Klik button **New** kemudian arahkan ke lokasi yang installasi folder ```bin``` di hasil installasi JDK jadi klo di saya seperti ini ```C:\Program files\Java\jdk-8u101\bin``` hasilnya seperti berikut:

![Setting PATH Java]({{site.baseurl}}/assets/img/posts/install-oracle-jdk8-windows10/setpath-jdk-8.png)

Setelah itu klik **OK** semua dialog yang ada sampe keluar dari **system properties**. setelah sampe sini proses instalasi di windows udah beres. saatnya tinggal kita cek aja configurasi.a dengan menggunakan **Command Prompt**

Buka Command Prompt dengan menggunakan kombinasi key ```WIN + R``` maka akan tampil form seperti berkut:

![Running CMD]({{site.baseurl}}/assets/img/posts/install-oracle-jdk8-windows10/run-cmd-1.png)

Kemudian ketik ```cmd``` setelah itu ```Enter``` maka akan tampil Command Prompt seperti berikut:

![Command Prompt]({{site.baseurl}}/assets/img/posts/install-oracle-jdk8-windows10/run-cmd-2.png)

kemudian coba cek configurasi java dengan cara melihat versi yang telah terinstall di OS menggunakan perintah seperti berikut

{% highlight bash %}
java -version && javac -version
{% endhighlight %}

kalo berhasil maka outputnya seperti berikut:


{% highlight bash %}
C:\Users\softw>java -version && java -version
java version "1.8.0_91"
Java(TM) SE Runtime Environment (build 1.8.0_91-b15)
Java HotSpot(TM) 64-Bit Server VM (build 25.91-b15, mixed mode)
java version "1.8.0_91"
Java(TM) SE Runtime Environment (build 1.8.0_91-b15)
Java HotSpot(TM) 64-Bit Server VM (build 25.91-b15, mixed mode)

C:\Users\softw>
{% endhighlight %}

Nah untuk setting di Windows udah selesai.