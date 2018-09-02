---
layout: post
title: "Menggunakan mikrotik sebagai extended Wifi atau ethernet"
date: 2018-09-02T18:49:30+07:00
category: network
tags: 
- Indihome
- Mikrotik
- Port Forwarding
author: Dimas Maryanto
references:
- https://wiki.mikrotik.com/wiki/Manual:IP/Firewall/NAT
- https://mikrotik.com/products/group/wireless-for-home-and-office
- https://www.blibli.com/p/mikrotik-rb931-2nd-hap-mini-2nd-wireless-router/ps--TPK-10612-00623
comments: true
---

Solusi networking untuk rumah-an dan untuk kantor yang berukuran karyawan 5 s/d 20 orang, ingin memiliki jaringan network handal dan bisa meng-handle connection wifi atau lan. Mikrotik adalah solusi membuat router yang murah dan mudah (Settingan menggunakan GUI).

![mikrotik logo]({{site.baseurl}}/assets/img/posts/mikrotik-extended-ethernet/mikrotik-logo.png)

Ada beberapa masalah yang saya dapatkan menggunakan Modem Indihome diantarnya:

- Jaringan wifi dan lan ketika di ping / traceroute tidak connect
- Jaringan wifi tidak stabil dan terbatas 10+ client
- Dan masih ada beberapa masalah lainnya yang gak bisa di ceritain satu-satu.

<!--more-->

Kurang lebih gini arsitektur jaringan yang saya gunakan sebelum menggunakan mikrotik.

![arsitektur sebelum menggunakan mikrotik]({{site.baseurl}}/assets/img/posts/mikrotik-extended-ethernet/before-mikrotik.png)

Jadi semua consentrator (Switch / Hub) berada di modem tersebut. Jadi semua limitasi dari modem Indihome ya kita harus terima, dan yang keselnya lagi radius wifinya gak terlalu luas contohnya saya browsing di lantai 1 kadang sinyalnya lemah dan kadang tidak terdeteksi karena modem di letakan di lantai 2.

Nah jadi saya putuskan untuk research, Wireless Router. setelah mendapatakan beberapa merek seperti Tp-Link, D-Link, sampe vendor2 networkin yang support dengan NAT, firewall dll yang harganya masuk di kantong. Dapatlah Mikrotik yang paling murah dan compact yaitu [RB931-2nd](https://www.blibli.com/p/mikrotik-rb931-2nd-hap-mini-2nd-wireless-router/ps--TPK-10612-00623) yang harganya kurang lebih 200rb-an di blibli.com ini dia produknya

![RB931-2nd]({{site.baseurl}}/assets/img/posts/mikrotik-extended-ethernet/rb931-2nd-2.jpg){:height="400px" width="400px"}

Oh ia ini gak di sponsorin sama produk tersebut ya....

Nah jadi produk tersebut, cocok untuk kebutuhan wifi rumah-an karena lebih stabil dan fitur-fiturnya yang lumayan banyak selain itu juga didalamnya menggunakan os linux base yaitu RouterOs. Tapi wireless router ini kurang cocok untuk kantoran karena resource hardware yang terbatas seperti yang di jelaskan di [spesifikasi berikut](https://mikrotik.com/product/RB931-2nD). Saya sarankan untuk kantor yang memiliki karwayan lebih 20 disarankan menggunakan yang lebih tinggi seperti berikut:

- https://mikrotik.com/product/RB941-2nD-TC
- https://mikrotik.com/product/RB951Ui-2HnD
- https://mikrotik.com/product/hap_ac2

Setelah di terapkan berikut adalah arsitektur jaringan yang sedang digunakan saat ini:

![arsitektur jaringan sekarang]({{site.baseurl}}/assets/img/posts/mikrotik-extended-ethernet/after-mikrotik.png)

Setelah 1 minggu pengunaan produk tersebut, sekarang belum di temukan masalah sih. Seperti salah satu masalah inter-connection dari client ke client juga lancar tanpa ada kendala lagi meskipun menggunakan adapter yang berbeda (wifi antara ethernet).