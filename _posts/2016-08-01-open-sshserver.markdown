---
layout: post
title: "Remote via SSH (Secure Shell)"
date: 2016-08-01T08:53:21+07:00
category: network
tags: 
- networking
- remote
- ssh
author: Dimas Maryanto
references:
comments: true
---

Halo, kali ini saya mau membahas tentang cara menggakes data dalam sebuah sistem 
operasi dengan menggunakan sebuah protocol yaitu Secure Shell atau lebih dikenal **SSH**.

<!--more-->

SSH pada dasarnya adalah protokol yang digunakan untuk mengakses data dalam sebuah jaringan 
secara aman dan pada dasarnya menggunakan perintah Unix.

## SSH topologi

ssh pada dasarnya memiliki topologi client-server artinya ssh yang digunakan secara 1 arah atau terpusat.
jadi SSH dibagi jadi 2 yaitu client-side (meremote) dan juga server-side (diremote).

### SSH Server side

SSH server side, ini biasanya kita harus menginstall package `openssh-server`.

Hal yang harus di setup, contohnya disini saya menginstall di **Ubuntu Server 16.04 LTS**:

- Install package `openssh-server`

{% highlight shell %}
sudo apt install openssh-server
{% endhighlight %}

- Membuat SSH Key

{% highlight shell %}
ssh-keygen -t rsa
{% endhighlight %}

- Membuka port 22, supaya bisa di akses dari luar

{% highlight shell %}
sudo iptables -I INPUT 1 -i eth0 -p tcp --dport 22 -j ACCEPT
{% endhighlight %}

### SSH Client side

SSH Client side, ini bisa digunakan untuk sistem operasi apa saja, secara default untuk UBuntu sudah terinstall openssh-client,
sedangkan untuk sistem operasi Windows bisa menggunakan 
[git-bash](https://git-scm.com/downloads) atau [putty](http://www.putty.org/)

setelah semuanya OK, berikut adalah cara penggunaanya:

- open terminal atau git-bash atau putty-client
- login ke sistem server `ssh username@host` example `ssh dimmaryanto93@10.1.1.48`

{% highlight shell %}
dimmaryanto93@E5-473G:~$ ssh developer@xxx.xxx.xxx.xxx
developer@xxx.xxx.xxx's password: # input password here and hit enter
Welcome to Ubuntu 16.04.2 LTS (GNU/Linux 4.4.0-63-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

0 packages can be updated.
0 updates are security updates.


*** System restart required ***
Last login: Sat Mar  4 10:44:22 2017 from 114.124.6.55
{% endhighlight %}

Nah sekarang kita udah masuk ke sistem server, sekarang kita tinggal remote dengan perintah linux ubuntu server.
Happy Working :) ....

Mungkin sekian dulu hal yg bisa di share tentang remote Server dari SSH.
