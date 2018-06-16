---
layout: post
title: "Install Oracle JDK 8 on Ubuntu 16.04 LTS"
date: 2016-08-02T11:29:50+07:00
category: ubuntu-16.04
tags: 
- Oracle
- JDK
- Ubuntu
- Java
author: Dimas Maryanto
---

Sekarang saya mau setting di Linux UBuntu. Kalo di ubuntu sih sebenernya gampang kita bisa install via ppa dengan menambahkan ```ppa:webupd8team/java``` berikut perintahnya

<!--more-->

{% highlight bash %}
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
{% endhighlight %}

Setelah itu kita update konfigurasi runtime environmentnya menggunakan perintah berikut ini:


{% highlight bash %}
sudo update-alternatives --config java
{% endhighlight %}

maka akan menampilkan output seperti berikut:


{% highlight bash %}
root@Aspire-E5-473G:~# update-alternatives --config java
There are 2 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                            Priority   Status
------------------------------------------------------------
* 0            /usr/lib/jvm/java-8-oracle/jre/bin/java          1083      auto mode
  1            /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java   1081      manual mode
  2            /usr/lib/jvm/java-8-oracle/jre/bin/java          1083      manual mode

Press <enter> to keep the current choice[*], or type selection number:
{% endhighlight %}

Nah dari output diatas kita pilih yang **java-8-oracle** jadi kita input ```0``` sebagai default runtime environmentnya. kemudian kita cek deh dengan terminal:

{% highlight bash %}
java -version && javac -version
{% endhighlight %}

berikut adalah outputnya:

{% highlight bash %}
dimmaryanto@Aspire-E5-473G:~$ java -version && javac -version
java version "1.8.0_101"
Java(TM) SE Runtime Environment (build 1.8.0_101-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.101-b13, mixed mode)
javac 1.8.0_101
{% endhighlight %}