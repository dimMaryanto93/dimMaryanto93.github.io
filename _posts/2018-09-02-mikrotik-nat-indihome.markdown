---
layout: post
title: "Setup NAT Mikrotik, for publish ip lokal to ip public"
date: 2018-09-02T22:01:08+07:00
category: network
tags: 
- Mikrotik
- ZTE Modem AN5506-04-F
- Port forwarding
- NAT
author: Dimas Maryanto
references:
- https://wiki.mikrotik.com/wiki/Manual:IP/Firewall/NAT#Destination_NAT
- https://en.wikipedia.org/wiki/Port_forwarding
comments: true
---

Sama seperti postingan sebelumya tentang [port forwarding]({% post_url 2018-06-10-indihome-ip-dynamic %}) di modem Indihome, tpi sekarang karena lokasi server di koneksi-kan di router Mikrotik artinya **Modem indihome can't any longer connect to server because different segment of ip addresses** (atau beda gateway). 

Karena modem indihome gak bisa konek ke server jadi [ip-dynamic.com](https://ip-dynamic.com) yang kita setting port-forwarding-nya pun otomatis gak akan bisa konek juga. Jadi solusinya...

![not connected schmeam]({{site.baseurl}}/assets/img/posts/mikrotik-nat/ip-dynamic-not-connect.png)

<!--more-->

Ada beberapa step yang harus di setting supaya ip-dynamic.com bisa membaca port-forwading yang udah di setup tapi lewat mikrotik tahap pertama saya mau setup dulu port forwarding di modem indihome.

## Setup modem indihome

Sebelumnya khan kita udah setting port-forwardingnya seperti berikut:

![ip port forwarding]({{site.baseurl}}/assets/img/posts/mikrotik-nat/indihome-ip-forwarding.png)

Nah sekarang kita ganti ip-target nya menjadi ip address mikrotik router yaitu `192.168.1.58` seperti berikut:

![ip port forwarding mikrotik]({{site.baseurl}}/assets/img/posts/mikrotik-nat/indihome-port-forward-mikrotik.png)

## Setup NAT di mikrotik

Ok, setelah mengubah ip-address, sekarang membuat NAT dengan cara buka menu **IP** -> **Firewall** -> **NAT** -> **New Nat**

![port forwarding new general]({{site.baseurl}}/assets/img/posts/mikrotik-nat/mikrotik-nat-new-general.png)

Settings menu general, seperti berikut:

- enabled: `di checklist`
- chain: `dstnat`
- Dst.Addresses: Masukan ip microtik di modem indihome yang saya setting adalah `192.168.1.58`
- protocol: Pilih `6 (tcp)`
- Dst.Port: Masukan port yang di forward dari modem indihome contohnya `80` atau `8080`

Masih scroll ke bawah di bagian menu Action:

![port forwarding new action]({{site.baseurl}}/assets/img/posts/mikrotik-nat/microtik-nat-new-action.png)

Setting menu action, seperti berikut:

- Action: pilih `dst-nat`
- To address: Masukan ip-address server yang di koneksikan ke microtik contohnya `192.168.88.100`
- To Port: Masukan port yang mau di forwarding dari ip-address server contohnya `80` atau `8080`
- Comments: isi descripsi ini dengan sejelas mungkin untuk apa (optional)

Sekarang harusnya configurasi **ip-dynamic.com** -> **indihome** -> **Mikrotik** -> **Local Server** sudah benar. Silahkan coba check di terminal dengan perintah (Microtik Terminal)

```bash
/tool tracepath your.ip-dynamic.com
```