---
layout: post
title: "Install Apache httpd on Redhat 7.4"
date: 2018-11-07T20:38:08+07:00
category: rhel7
tags: 
- Apache httpd
- Redhat 7.4
author: Dimas Maryanto
references:
comments: true
---

Untuk menginstal Apache httpd dari subcribe repository / dvd ([how to enable yum repository from dvd]({% post_url 2018-09-11-enabled-yum-rhel7 %}))

<!--more-->

```bash
yum install httpd
```

Setelah terinstall jalankan service `httpd` dan open port untuk `80/tcp` dengan perintah berikut:

```bash
# start service httpd
systemctl start httpd.service
# service launch at startup
systemctl enable httpd.service

# to open http protocol
firewall-cmd --zone=public --add-port=80/tcp --permanent

## reload config firewall
firewall-cmd --reload
```

Setelah itu kita bisa check halamanya dengan menggunakan browser [http://{YOUR-IP-ADDRESS}](http://localhost) berikut hasilnya:

![works it]({{site.baseurl}}/assets/img/posts/httpd-ssl-rehel7/work-it.png)