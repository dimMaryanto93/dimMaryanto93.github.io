---
layout: post
title: "Indihome fiber set IP Forward and public IP"
date: 2018-06-10T14:30:00+07:00
category: other
tags: 
- TCP/IP
- zte modem
author: Dimas Maryanto
references:
comments: true
---

![ip-dynamic.com]({{site.baseurl}}/assets/img/posts/indihome-ip-public/ip-dynamic.png)

Membuat PC, atau komputer bisa di remote dari internet bukanlah hal yang mahal dan ribet ngurusnya (astinet). Dengan menggunakan fasilitas Indihome rumahan (`300rb-an / bulan`) dan account [ip-dynamic.com](https://ip-dynamic.com/). 

<!--more-->

## Kebutuhan

- PC / Komputer
- Modem indihome

## Konfigurasi Network

Ada beberapa yang harus di setting seperti ip address harus di setting static, supaya tidak banyak perubahan ketika modem di restart atau keadaan mati lampu. selain itu juga kita harus mendaftarkan ip static tersebut menggunakan ip forwarding di modem Indihome.

### Konfigurasi IP Forwarding

Masuk ke default gateway modem indihome dengan alamat [http://192.168.1.1](http://192.168.1.1/login.asp) maka akan muncul halaman seperti berikut:

![login modem indihome]({{site.baseurl}}/assets/img/posts/indihome-ip-public/modem-login.png)

**note** biasanya informasi halaman mulai dari username, password, default gateway admin console tertera di bagian bawah modem.

Setelah itu masuk ke menu **Application** -> **Port Forwarding** kemudian tambahkan konfigurasi ip-address komputer yang telah anda setting seperti berikut contohnya:

![modem ip-forwarding]({{site.baseurl}}/assets/img/posts/indihome-ip-public/modem-ip-forwarding.png)

### Konfigurasi ip-dynamic.com

Setelah konfigurasi ip forwarding di modem indohome, tahap selanjutnya adalah mendaftarkan nomor indohome dan nomor telphone ke [ip-dynamic.com](https://ip-dynamic.com) dengan tujuan membuat DNS (Domain Names Server) jadi contohnya kita punya ip address di local (192.168.1.50:80) akan di baca di nomor-indihome.ip-dynamic.com atau temen-teman juga bisa membuat custome domain dengan berlangganan mulai dari regular (Rp. 10,000,- / bulan) dan pro (Rp. 15.000,- / bulan).

![ip-dynamic billing info]({{site.baseurl}}/assets/img/posts/indihome-ip-public/ip-dynamic billing.png)

Setelah itu tunggu kurang lebih 15 menit sampai konfigurasi modem di tangkap oleh ip-dynamic.com maka komputer anda sudah bisa online.