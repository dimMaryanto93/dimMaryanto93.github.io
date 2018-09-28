---
layout: post
title: "Reverse Proxy and Load balancing to Backend in Apache httpd"
date: 2018-09-26T23:49:54+07:00
category: httpd
gist: dimMaryanto93/382f8793eec86ce9507e4626651dfdc4
tags: 
- Reverse Proxy
- Load Balancing
- Apache Httpd
- Apache2
author: Dimas Maryanto
references:
- https://httpd.apache.org/docs/2.4/howto/reverse_proxy.html
- https://httpd.apache.org/docs/2.4/mod/mod_proxy_balancer.html
- https://en.wikipedia.org/wiki/Monolithic_application
comments: true
---

![reverse proxy teknology]({{site.baseurl}}/assets/img/posts/proxy-to-backend/reverse-proxy.png)

Kalo ngomongin tentang teknologi "Backend / Server Side Programming" memang gak akan ada abisnya, teknologi jaringan yang satu ini penting di ketahui bagi seorang programmer backend yaitu Reverse Proxy dan Load Balancing.

Untuk Reverse proxy dan Load balancing dapat di-implementasi-kan di web-server sabagai berikut:

- Nginx
- Apache Httpd
- Apache apache2
- Apache Tomcat
- Dan masih banyak lagi, hampir semua web server memiliki fitur reverse proxy.

<!--more-->

Jadi misalnya saya punya aplikasi dengan mapping seperti berikut:

- `/auth` aplikasi untuk authentication ini running single instance pada host `tcp 192.168.2.100:8080`
- `/registration` aplikasi untuk pendaftar ini running di beberapa host dan menggunakan load balancer karena di module ini yang paling banyak request
    - `tcp 192.168.1.100:8080`
    - `tcp 192.168.1.101:8080`
    - `tcp 192.168.1.102:8080`
    - `tcp 192.168.1.103:8080`
    - `tcp 192.168.1.104:8080` 
- `/resource` aplikasi untuk resource management ini running di 2 host karena requestnya tidak telalu banyak seperti module `registration` dan menggunakan load balancer seperti berikut:
    - `tcp 192.168.3.100:8080`
    - `tcp 192.168.3.101:8080`

Nah gambaran akhirnya seperti ini kurang lebih:

![arch application with proxy]({{site.baseurl}}/assets/img/posts/proxy-to-backend/arch-proxy-backend.png)

Jadi reverse proxy / load balancer nya sebagai firewall untuk interaksi aplikasi client atau istilah lainnya frontend dengan aplikasi backend-nya.

Ok sekarang kita setup dulu environtment-nya misalnya networkingnya seperti berikut:

![network plan]({{site.baseurl}}/assets/img/posts/proxy-to-backend/networking-proxy.png)

Jadi setiap server harus membuka port `8080` supaya bisa di remote access oleh web-server yang akan melakukan reverse-proxy.

**Check network is tcp bukan tcp6**

```bash
netstat -na | grep "8080"
## output
# tcp   0   0 127.0.0.1:8080   0.0.0.0:*   LISTEN
```

**Menggunakan iptables**

```bash
sudo iptables -I INPUT 1 -i eth0 -p tcp --dport 8080 -j ACCEPT
```

**Menggunakan firewalld**

```bash
sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent
sudo firewall-cmd --reload
```

Setelah di buka portnya sekarang kita install `httpd` / `apache2` di server.

## Install apache2

Web server apache2 adalah package untuk Debian, Ubuntu dan turunanya berikut adalah cara installnya:

```bash
sudo apt-get install apache2
```

dan berikut adalah configurasi reverse proxy, modif file `/etc/apache2/sites-enabled/001-default.conf` tambahkan seperti berikut:

{% gist page.gist 001-default.conf %}

Nah sekarang tinggal restart aja web-server apache2 dengan perintah berikut:

```bash
## reload configuration without restart
/etc/init.d/apache2 reload

## if not working then restart apache2
/etc/init.d/apache2 restart
```

## Install httpd

Web server httpd adalah bawaan package di Redhat, Fedora, Centos dan turunannya, berikut cara installnya:

```bash
# install on centos / rhel7
yum install httpd

# install on fedora
dnf install httpd
```

dan berikut adalah configurasi reverse proxy, modif file `/etc/httpd/conf/httpd.conf` tambahkan di akhir baris dengan script seperti berikut:

{% gist page.gist httpd.conf %}

Nah sekarang tinggal restart aja web-server httpd servicenya dengan perintah berikut:

```bash
sudo apachectl restart
```
