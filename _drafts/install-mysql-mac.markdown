---
layout: post
title: "Install MySQL database on macOS Mojave"
category: mac-os
tags: 
- macOS Mojave
- MySQL Server
- MySQL Workbranch
author: Dimas Maryanto
references:
comments: true
---

Database yang satu ini gak boleh ketinggalan, karena hampir semua applikasi di PT. Tabeldata mostly pake database MySQL tpi selain itu juga ada yang pake PostgreSQL dan MS SQL Server. Langung aja install MySQL server pertama Download dulu installernya dari [website oracle mysql](https://dev.mysql.com/downloads/)

Mysql database on macOS Mojave, berikut adalah cara installasinya

<!--more-->

yang saya download yaitu:

- [Mysql Server community](https://dev.mysql.com/downloads/mysql/)
- [Mysql Workbranch](https://dev.mysql.com/downloads/workbench/)

## Install MySQL Server

Setelah download double click file `mysql-8.0.12-macos10.13-x86_64.dmg` maka akan muncul tampilan seperti berikut:

![mysql-dmg]({{site.baseurl}}/assets/img/posts/mysql-on-macos/mysql-package.png)

Kemudian double clik `mysql-8.0.12-macos10.13-x86_64.pkg` berikutnya muncul tapilan installernya seperti berikut:

![mysql-package]({{site.baseurl}}/assets/img/posts/mysql-on-macos/mysql-mac-security-accept.png)

Seperti biasa kita harus **accept** supaya bisa install di mac kita, next:

![mysql-introduction]({{site.baseurl}}/assets/img/posts/mysql-on-macos/mysql-introduction.png)

Langung aja **Continues**

![mysql-lisence-acceptance]({{site.baseurl}}/assets/img/posts/mysql-on-macos/mysql-lisence-accept.png)

Langsung aja **Agree** klo males baca, heheh. Next

![mysql-location]({{site.baseurl}}/assets/img/posts/mysql-on-macos/mysql-location-install.png)

Klo mau ubah lokasi install, lakukan di sini. Klo saya biarkan semuanya default. Next:

![mysql-location]({{site.baseurl}}/assets/img/posts/mysql-on-macos/mysql-legacy-password.png)

Nah untuk ini, saya pilih yang kedua supaya client-client yang mau remote pake versi 5.x masih bisa login. Next

![mysql-root]({{site.baseurl}}/assets/img/posts/mysql-on-macos/mysql-root-password.png)

isi root password, klo saya menggunakan `passwordnya-root`. Next

![mysql-finish]({{site.baseurl}}/assets/img/posts/mysql-on-macos/mysql-summary.png)

Setelah itu installasi selesai. Sekarang kita test konek ke database Mysql. Buka **Settings** -> **Mysql Service**

![mac settings]({{site.baseurl}}/assets/img/posts/mysql-on-macos/settings.png)

Maka akan muncul tampilannya seperti berikut:

![mac settings]({{site.baseurl}}/assets/img/posts/mysql-on-macos/mysql-service.png)

Kemudian klick **Start Mysql Server** seperti berikut:

![mac service started]({{site.baseurl}}/assets/img/posts/mysql-on-macos/mysql-service-started.png)

Setelah itu kita konek menggunakan mysql-client / mysql cli di terminal dengan perintah seperti berikut:

```bash
/usr/local/mysql/bin/mysql -h localhost -u root -p
```

Hasilnya seperti berikut:

![mac service started]({{site.baseurl}}/assets/img/posts/mysql-on-macos/mysql-connect-client.png)

## Mysql Client GUI / MySQL Workbranch

Untuk bisa menggunakan mysql-client berbasis GUI kita bisa menggunakan software buatan mysql itu sendiri namanya Mysql Workbranch. Setelah installernya kita download tadi. Run file `mysql-workbench-community-8.0.12-macos-x86_64.dmg` seperti berikut:

![mysql client workbranch install]({{site.baseurl}}/assets/img/posts/mysql-on-macos/mysql-workbranch-install.png)

Kita tinggal drag java `Mysql Workbranch.app` ke folder `Application`, udah itu selesai kita tingal lauch applikasinya dari dashboard hasilnya seperti berikut:

![mysql client workbranch]({{site.baseurl}}/assets/img/posts/mysql-on-macos/mysql-workbranch.png)

Kita coba konek menggunakan user `root`, isi passwordnya dan hasilnya seperti berikut:

![mysql client workbranch]({{site.baseurl}}/assets/img/posts/mysql-on-macos/mysql-workbranch-connect.png)