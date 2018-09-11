---
layout: post
title: "Setup networking rhel7 (net-tools)"
date: 2018-09-11T20:04:13+07:00
category: rhel7
tags: 
- linux
- networking
- rhel7
author: Dimas Maryanto 
references:
comments: true
---

![rhel7 net-tools]({{site.baseurl}}/assets/img/posts/rhel7-setup/redhat.jpg){:height="200px" width="200px"}

Setelah fresh install os redhat (zero configuration). Configuration yang akan di setup kali ini adalah Networking diataranya untuk 

- Koneksi ke internet
- Menggunakan ssh, Untuk me-remote.
- Dan masih banyak lagi.

<!--more-->

Secara default `ifconfig` not active di redhet kita perlu install dulu...

```bash
sudo yum install net-tools
```

atau selain menggunakan `ifconfig` sebenarnya kita bisa menggunakan perintah `ip link show` cuman karena udah terbiasa dengan ifconfig jadi ya gpp lah kita install toh juga tida mempengaruhi performa system.

**configuration network**

```bash
nmtui
```

![nmtui]({{site.baseurl}}/assets/img/posts/rhel7-net-tools/nmtui.png)

Pilih edit connection -> 

![nmtui ethernet]({{site.baseurl}}/assets/img/posts/rhel7-net-tools/nmtui-ethernet.png)

Pilih interface network yang tersedia atau, network card yang terinstall klo di saya `enp0s3` ->

![mntui network interface]({{site.baseurl}}/assets/img/posts/rhel7-net-tools/nmtui-network-interface.png)

Kemudian isilah sesuia configurasi network yang diinginkan, contohnya configuration netwok saya configurasi static seperti berikut:

- ip address = `192.168.88.200`
- gateway = `192.168.88.1`
- dns server = `8.8.8.8` dan `8.8.4.4`

Nah setelah itu **apply configuration** dan **exit**

Maka setelah itu tinggal di check aja configurasinya di 

```bash
cat /etc/sysconfig/network-script/ifcfg-enp0s3
```

Berikut outputnya:

```bash
# /etc/sysconfig/network-script/ifcfg-enp0s3
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=dhcp
DEFROUTE=yes
IPV4_FAILURE_FATAL=yes
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s3
UUID=aa286e8b-6e41-4598-87ba-6178f2a5d297
DEVICE=enp0s3
ONBOOT=yes
IPADDR=192.168.88.200
PREFIX=32
GATEWAY=192.168.88.1
DNS1=8.8.8.8
DNS2=8.8.4.4
```

dan Jika di ping ke `8.8.8.8` ada reponsenya, maka selamat anda telah berhasil koneksi ke internet.