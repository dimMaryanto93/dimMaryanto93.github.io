---
layout: post
title: "Setup DNS Server Mikrotik"
date: 2018-09-04T14:45:58+07:00
category: network
tags: 
- Mikrotik
- ip-dynamic
- Indihome
author: Dimas Maryanto
references:
- https://www.youtube.com/watch?v=y9FbvcsP4b4
- https://wiki.mikrotik.com/wiki/Manual:IP/DNS
comments: true
---

Setelah kita setup untuk connection ke intenet, port forwarding dari indihome ke mikrotik menggunakan NAT di Mikrotik.
Sebenarnya problem ini udah ada semenjak dulu yaitu misalnya klo kita menggunakan [ip-dynamic.com](http://ip-dynamic.com) jadi misalnya kita punya ip public yaitu [repository.dimas-maryanto.com](http://repository.dimas-maryanto.com) tpi klo saya akses pake wifi rumah jadi klo mau akses ke server gak bisa pake itu jadi harus pake ip-lokal. Jadi kurang lebih seperti ini schemanya:

![mikrotik server dns arsitektur]({{site.baseurl}}/assets/img/posts/mikrotik-dns/mikrotik-dns-arsitektur.png)

<!--more-->

Ada beberapa settingan untuk mengaktifkan DNS server di mikrotik, yaitu 

- Menambahkan dns server
- Configurasi NAT untuk redirect -> tcp -> udp

## Menambahkan DNS Server di Mikrotik

- Pertama ke menu **IP** -> **DNS**

![list dns server]({{site.baseurl}}/assets/img/posts/mikrotik-dns/mikrotik-dns.png)

Configurasi yang di modifikasi:

- Jika dynamic server udah tersedia seperti gambar di atas, kita cukup tambahkan server dns yang lain jika mau contohnya dns google yaitu `8.8.8.8`
- Jika belum ada, tambahkan default gateway modem indihome seperti `192.168.1.1`
- Atribut yang lain biarkan default seperti gambar di atas.
- Tahap selanjutnya, klick button **Static** seperti berikut:

![dns static server]({{site.baseurl}}/assets/img/posts/mikrotik-dns/mikrotik-dns-static.png)

- Kemudian **Add New**

![dns static add new]({{site.baseurl}}/assets/img/posts/mikrotik-dns/mikrotik-dns-new.png)

Configurasinya seperti berikut:

- name: isikan sesuai dengan domain anda contohnya `repository.dimas-maryanto.com` atau `www.dimas-maryanto.com`
- address: dns name yang kita isikan di atas akan di arahkan ke ip brapa contohnya klo saya `192.168.88.50`
- Atribute sisanya biarkan default seperti digambar.


## Konfigurasi NAT di Mikrotik

Selanjutnya kita harus configurasi NAT untuk `redirect -> tcp <--> udp` di Mikrotik supaya klo kita masukan domain yang kita input tadi di dns server. Maka semua request akan di lewatkan melalui Mikrotik terlebih dahulu baru di forward ke dns yang lainnya contohnya `8.8.8.8`. Berikut configurasinya:

- Tambahkan NAT baru di Menu **IP** -> **Firewall** -> **NAT** -> **New Nat**

![dns nat tcp 1]({{site.baseurl}}/assets/img/posts/mikrotik-dns/mikrotik-dns-firewall-tcp-1.png)

![dns nat tcp 1]({{site.baseurl}}/assets/img/posts/mikrotik-dns/mikrotik-dns-firewall-tcp-2.png)

Configurasi NAT untuk redirect dari tcp ke redirect
- General tab
    - action: `dstnat`
    - protokol: `6 (tcp)`
    - dst-port: `53`
- Action tab
    - action: `redirect`
    - To Port: `53`

- Tambahkan NAT baru di Menu **IP** -> **Firewall** -> **NAT** -> **New Nat**

![dns nat udp 1]({{site.baseurl}}/assets/img/posts/mikrotik-dns/mikrotik-dns-firewall-udp-1.png)

![dns nat udp 2]({{site.baseurl}}/assets/img/posts/mikrotik-dns/mikrotik-dns-firewall-udp-2.png)

Configurasi NAT untuk redirect dari tcp ke redirect
- General tab
    - action: `dstnat`
    - protokol: `17 (udp)`
    - dst-port: `53`
- Action tab
    - action: `redirect`
    - To Port: `53`

- Dan tahap yang terakhir liat cache di Menu **IP** -> **DNS** -> **Cache** seperti berikut:

![dns static cache]({{site.baseurl}}/assets/img/posts/mikrotik-dns/mikrotik-dns-cache.png)

Sekarang coba **Flush cache** kemudian akses domain anda [repository.dimas-maryanto.com](http://repository.dimas-maryanto.com) dari jaringan lokal dan jaringan external jika sukses maka akan ada response dan request di cachenya seperti gambar di atas.

## Summary

** Keuntungan menggunakan DNS Server sendiri**

- Kita bisa mengakses dengan satu url tanpa harus mengetahui IP server berapa pun itu. cukup di akases dengan domain yang kita buat atau beli.
- Jika tidak ada internet, yang bisa akses hanya jaringan lokal tetapi tetap menggunakan url domain tanpa harus menggunakan ip-address.

** Kekurangan menggunakan DNS Server sendiri**

- Jika Mikrotik down, tidak bisa koneksi server
- Jika Mikrotik down, Tidak ada koneksi internet, harus pindah connection ke modem indihome secara langsung.