---
layout: post
title: "Install Oracle JDK 8 di Fedora"
date: 2016-08-02T11:30:50+07:00
category: fedora-23
tags: 
- Oracle
- Java
- JDK
- Fedora
author: Dimas Maryanto
references:
comments: true
---

Untuk install Oracle JDK di Fedora sebenarnya ada banyak cara salah satunya menggunakan software Fedy, Fedy adalah penyedia software-software yang digunakan untuk mempermudah proses installasi sama halnya seperti via ppa klo di UBuntu. tpi disini saya tidak akan menggunakan Fedy tapi menggunakan cara manual saja supaya apa? supaya anda tau proses mengginstallnya menggunakan package management fedora yaitu ```rpm```. OK sekarang kita download dulu file installernya sama seperti di Windows masuk ke [website oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html) kemudian pilih sesuai platform, karena saya pake Fedora version 64bit jadi saya pilih yang ```linux-x64.rpm``` pilih yang extensionnya **.rpm** ya!. seperti gambar berikut:

![Download Oracle JDK Fedora]({{site.baseurl}}/assets/img/posts/install-oracle-jdk8-windows10/download-jdk-2.png)

<!--more-->

Setelah selesai di download silahkan anda buka terminalnya kemudian arahkan ke hasil download seperti berikut:

{% highlight bash %}
[root@localhost ~]# cd /home/dimmaryanto93/Downloads/
{% endhighlight %}

Untuk melakukan cek filenya ada atau gak gunakan perintah ```ls``` berikut outputnya:

{% highlight bash %}
[root@localhost Downloads]# ls
jdk-8u101-linux-x64.rpm
{% endhighlight %}

Setelah file ditemukan sekarang kita install filenya menggunakan ```rpm``` seperti berikut:

{% highlight bash %}
[root@localhost Downloads]# rpm -Uivh jdk-8u101-linux-x64.rpm
{% endhighlight %}

Berikut outputnya:


{% highlight bash %}
Preparing...                          ################################# [100%]
Updating / installing...
   1:jdk1.8.0_101-2000:1.8.0_101-fcs  ################################# [100%]
Unpacking JAR files...
	tools.jar...
	plugin.jar...
	javaws.jar...
	deploy.jar...
	rt.jar...
	jsse.jar...
	charsets.jar...
	localedata.jar...
{% endhighlight %}

Kemudian setelah selesai kita setting default runtime environtmentnya, karena secara default Fedora sendiri udah menggunakan OpenJDK. karena saya disini bekerja dengan Java 8 maka saya update configurasi ke OracleJDK berikut ada perintahnya:

{% highlight bash %}
alternatives --config java
{% endhighlight %}

berikut outputnya:

{% highlight bash %}
[root@localhost ~]# alternatives --config java

There are 2 programs which provide 'java'.

  Selection    Command
-----------------------------------------------
*+ 1           /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.92-5.b14.fc24.x86_64/jre/bin/java
   2           /usr/java/jdk1.8.0_101/jre/bin/java

Enter to keep the current selection[+], or type selection number:
{% endhighlight %}

Kemudian kita pilih yang version Oracle yaitu yang nomor ```2``` atau ```/usr/java/jdk1.8.0_101/jre/bin/java``` sebagai default runtime environmentnya. setelah itu tekan **Enter** dan selesai deh tinggal kita cek dengan perintah berikut:

{% highlight bash %}
java -version && javac -version
{% endhighlight %}

maka berikut outputnya:


{% highlight bash %}
[dimmaryanto93@localhost ~]$ java -version && javac -version
java version "1.8.0_101"
Java(TM) SE Runtime Environment (build 1.8.0_101-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.101-b13, mixed mode)
javac 1.8.0_101
{% endhighlight %}