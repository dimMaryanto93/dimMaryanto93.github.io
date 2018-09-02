---
layout: post
title: "Basic setup Mikrotik koneksi internet dari modem Indihome"
date: 2018-09-02T19:48:01+07:00
category: network
tags: 
- Mikrotik
- Indihome
author: Dimas Maryanto
references:
comments: true
---

Waktu pertama kali unboxing, produk mikrotik tipe RB931-2nd agak kaget sih. ternyata isinya simple cuman wireless router mikrotik yang sangat mugil sama charger adapter yang menggunakan mikro usb (sama seperti colokan hp) biasanya khan colokannya yang bulet klo gak gitu kayak usb type A (yang kotak atau persegi panjang). 

Ok cukup deh basa-basinya. Jadi setup pertama yaitu kita plug-in cable ethernet dari modem indihome sebagai internet connection ke port Internet di mikrotik router. Kemudian klo mau konek ke mikrotiknya bisa menggunakan wifi dengan meng-akses [192.168.88.1](http://192.168.88.1) login sebagai **admin** dan **passwordnya di kosongkan**

Nah begitu masuk, akan tampil halaman seperti berikut:

![mikrotik-welcome]({{site.baseurl}}/assets/img/posts/mikrotik-config-base/mikrotik-configs.png)

<!--more-->

Ada beberapa settingan yang wajib di configurasi seperti berikut:

- Setting internet connection
- Setting password admin
- Setting password wifi mikrotik / access point

## Setting internet connection

Settingan internet cukup dengan colokan kabel ethernet dari modem indihome atau bisa lewat wifi dengan mengkoneksikan wifi dari modem indihome ke wifi mikrotik. tapi cara wifi to wifi tidak disarangkan sih lebih baik menggunakan kabel saja ya supaya kecepatannya stabil dan tidak ada ganguan sinya.

- Pilih mode -> Router
- Address aquisition -> automatic (optional) boleh atau static juga bisa
    - automatic (no need to setup)
    - static
        - ip-address: bebas asalkan harus sesuai dengan ip-address classnya sih modem indihome contohnya klo indihomenya default gatewaynya `192.168.1.1` jadi contohnya `192.168.1.50`
        - Netmask: sesuaikan tpi biasanya pake `255.255.255.0`
        - Gateway: gunakan gateway modem indihome yaitu biasanya `192.168.1.1` atau `192.168.100.1` (untuk modem model zte yang baru)
        - DNS Server: isi aja misalnya pake dns google
            - `8.8.8.8`
            - `8.8.4.4`

![setting internet mikrotik]({{site.baseurl}}/assets/img/posts/mikrotik-config-base/mikrotik-internet-connection.png)

## Setting `admin` mikrotik

Settingan mikrotik kurang lebih seperti berikut:

- Ip Address: ip-address intuk mikrotik contohnya `192.168.88.1`
- Netmask: ini sesuaikan dengan class ip address yang digunakan, contohnya atas pilih yang `255.255.255.0/24`
- DHCP Server: di ceklik, fitur ini supaya kita tidak harus daftar ip-address satu persatu ke mikrotik
- DHCP Server Range: setting ip yang akan di ijikan untuk konek secara dhcp
- NAT: di ceklish supaya bisa ip-lokal bisa di publish ke ip-publik 

![setting admin mikrotik]({{site.baseurl}}/assets/img/posts/mikrotik-config-base/mikrotik-base-config.png)

Setelah itu bisa **apply Configuration** supaya configurasi yang telah di setup di save ke internal mikrotik. Abis itu bisa di check di modem indihome apa sudah terkoneksi dengan membuka web control panel modem. akses aja default gateway klo saya pake modem yang model lama jadi menggunakan [192.168.1.1](http://192.168.1.1) Kemudian akses **Lan State**

![modem indihome status]({{site.baseurl}}/assets/img/posts/mikrotik-config-base/indihome-status.png)

Dan make sure ip-address yand di daftarkan di mikrotik terdaftar di modem indihome dengan mengakses menu **DHCP client list**

![modem indihome ip list]({{site.baseurl}}/assets/img/posts/mikrotik-config-base/indihome-ip.png)

## Setting wifi for mikrotik

Klo setting wifi untuk mikrotik sederhana tinggal pilih sesuai security algoritma yang diinginkan contohnya seperti berikut:

![mikrotik wifi setting]({{site.baseurl}}/assets/img/posts/mikrotik-config-base/mikrotik-wifi-config.png)

Setelah semuanya di setup click **Apply Configuration** untuk mengimpan configurasi yang telah di settings.
