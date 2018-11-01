---
layout: post
title: "Install apache2 on Ubuntu 18.04"
category: ubuntu-18.04
tags: 
- Ubuntu 18.04
- Web Server
- Apache
author: Dimas Maryanto
references:
comments: true
---

![logo apache2]({{site.baseurl}}/assets/img/posts/install-apache2-ubuntu/logo.jpg)

Web Server yang paling commons di kantor kami selain Apache Tomcat, yaitu [Apache2](https://help.ubuntu.com/lts/serverguide/httpd.html) / [Httpd](https://httpd.apache.org) biasanya digunakan untuk mendeploy frontend seperti Angular based atau server side yaitu PHP based.

<!--more--> 

## Installation 

Untuk proses installasi cukup mudah karena apache2/httpd sudah tersedia di repository ubuntu dengan menggunakan perintah seperti berikut:

```bash
apt-get install apache2
```

Secara default, apache2 akan start tetapi belum otomatis ketika startup. klo mau aktifin service ketika startup dengan menggunakan perintah berikut:

```bash
# enable startup 
systemctl enable apache2.service

# start service
systemctl start apache2.service
```

Setelah servicenya aktif, secara default httpd akan mengaktifikan port `80/tcp` untuk protocol http dan `443/tcp` untuk protocol https

- [http://{HOST}](http://localhost)
- [https://{HOST}](https://localhost)

## Changed port

Ada kalanya kita mau ganti port, karena port default `80` udah digunakan oleh aplikasi lain. Edit file `/etc/apache2/ports.conf` ubah `Listen 80` menjadi port ingin digunakan contohnya seperti berikut:

```conf
# If you just change the port or add more ports here, you will likely also
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf

Listen 81

<IfModule ssl_module>
	Listen 8443
</IfModule>

<IfModule mod_gnutls.c>
	Listen 8443
</IfModule>
```

Setelah itu coba restart, dengan perintah berikut:

```bash
apachectl restart
```

Maka hasilnya seperti berikut:

![changed port to 81]({{site.baseurl}}/assets/img/posts/install-apache2-ubuntu/apache2-home.png)