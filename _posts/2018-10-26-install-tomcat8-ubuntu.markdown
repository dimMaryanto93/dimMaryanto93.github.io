---
layout: post
title: "How to install Apache Tomcat8 & Monitoring tools"
date: 2018-10-26T02:45:31+07:00
category: web-server
tags: 
- Web Server
- Java
- Tomcat8
author: Dimas Maryanto
references:
- http://tomcat.apache.org/
comments: true
---

Apache tomcat8, salah satu Web Server yang paling sering saya gunakan untuk deploy aplikasi JavaEE berbasis Servlet dan Spring Web MVC. Apache Tomcat8 ini memiliki [spesifikasi](https://wiki.apache.org/tomcat/Specifications) seperti berikut:

- Apache Tomcat version 8.5 implements the Servlet 3.1 and 
- JavaServer Pages 2.3 specifications from the Java Community Process, and 
- includes many additional features that make it a useful platform for developing and deploying web applications and web services

<!--more-->

## How to install Apache Tomcat8

Untuk menginstall **Tomcat8** di Ubuntu server cukup mudah, dengan menggunakan perintah berikut:

```bash
apt-get install tomcat8 tomcat8-admin
```

## Setup port & users

Setelah proses install kita perlu meng-aktifkan user, supaya bisa melakukan deploy dan memonitoring application yang running di tomcat tersebut, dengan mengedit file `/etc/tomcat8/tomcat-users.xml` seperti berikut:

```xml
<tomcat-users>
    <role rolename="tomcat"/>
    <role rolename="role1"/>
    <user username="tomcat" password="tomcat" roles="tomcat,manager-gui,manager-jmx"/>
    <user username="both" password="tomcat" roles="tomcat,role1"/>
    <user username="role1" password="tomcat" roles="role1"/>
</tomcat-users>
```

Dan setelah itu, saya mau merubah tomcat portnya manjadi `8888` dengan merubah konfirasi di file `/etc/tomcat8/server.xml` seperti berikut:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Server port="8005" shutdown="SHUTDOWN">
    <Service name="Catalina">
        <!-- this changed to 8888 -->
        <Connector port="8888" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" />

        <!-- other configuration -->
    </Service>
</Server>
```

Setelah itu restart service tomcat dengan perintah berikut:

```bash
systemctl restart tomcat8.service

# allow to service startup on login
systemctl enable tomcat8.service
```

Kemudian allow tomcat access remote from client, dengan membuka port yang kita tentukan sebelumnya yaitu `8888` dengan perintah berikut:

```bash
sudo iptables -I INPUT 1 -i eth0 -p tcp --dport 8888 -j ACCEPT
```

Nah, sekarang kita bisa browse ke port `8888` ke remote address tomcat8 hasilnya seperti berikut:

![tomcat root]({{site.baseurl}}/assets/img/posts/tomcat8-ubuntu/tomcat-root.png)

Klo saya akses [http://{IP-REMOTE-TOMCAT8}:{PORT}/manager](http://localhost:8888/manager) kemudian input username `tomcat` passwordnya `tomcat` maka anda akan diarahkan ke halaman berikut:

![tomcat manager]({{site.baseurl}}/assets/img/posts/tomcat8-ubuntu/tomcat-manager.png)

## Deploy war

Untuk deploy aplikasi `war` ke tomcat8 cukup copy-paste `file.war` ke folder `/var/lib/tomcat8/webapps/` tpi sebelum itu kita harus berikan akses dulu supaya warnya bisa di copy ke folter tersebut dengan perintah berikut:

```bash
sudo chmod -R 777 /var/lib/tomcat8/webapps/
```

Setelah itu baru kita bisa deploy aplikasi ke tomcat.