---
layout: post
title: "Deploy Angular CLI template to Apache2"
date: 2018-11-02T00:35:58+07:00
category: apache2
tags: 
- Angular CLI
- Apache2
- Front-End
author: Dimas Maryanto
references:
comments: true
---

![logo angular, httpd, nodejs]({{site.baseurl}}/assets/img/posts/deploy-angular-apache2/logo.png)

Sebelumnya kita udah [install nodejs, npm & angular-cli]({% post_url 2018-11-01-install-nodejs-ubuntu %}) sekarang kita akan membuat project dengan `angular-cli` kemudian mendeploy ke **Apache2**

Ada beberapa yang harus kita siapakan yaitu diantaranya:

- Template project (generate new project from angular-cli)
- Build template project (compile/compress project from angular-cli)
- Setup apache2 using documentRoot 

<!--more-->

## Create project from angular-cli

Ok, tahap pertama kita buat dulu project menggunakan angular-cli command seperti berikut:

```bash
# example
ng new belajar-angular
```

Setelah project terbuat, sekarang kita coba running development mode dengan perintah:

```bash
ng serve
```

Coba browser menggunakan browser ke alamat [localhost:4200](http://localhost:4200), untuk develop angular. menurutku lebih menyenangkan di Google Chrome / Chromium karena punya fitur yang lumayan seperti browser console, inspector, debuger. Maka hasilnya seperti berikut:

![angular sample project]({{site.baseurl}}/assets/img/posts/deploy-angular-apache2/angular-starter-project.png)

## Build project using production profile

Untuk mem-build project angular dengan template yang di create dari angular-cli. menggunakan perintah seperti berikut:

```bash
ng build --prod
```

Nah setelah itu file hasil buildnya akan di buat dalam folder `dist/{project-name}` contohnya seperti berikut:

```bash
dist
└── belajar-angular
    ├── 3rdpartylicenses.txt
    ├── favicon.ico
    ├── index.html
    ├── main.a5fc10236b5169f2cfbc.js
    ├── polyfills.c6871e56cb80756a5498.js
    ├── runtime.ec2944dd8b20ec099bf3.js
    └── styles.3bb2a9d4949b7dc120a9.css

1 directory, 7 files
```

Nah dari hasil build itu, kita akan mendapatkan file `html`, `css` dan `js` serta files resources lainya seperti gambar, ico dan lain-lain. Sekarang kita tinggal copy-paste aja ke web server contohnya ke apache2 tpi kita harus tau dulu RootDocument nya di taruh dimana supaya web servernya membaca file `index.html`

## Configure RootDocument on Apache2

Sekarang kita edit file `/etc/apache2/sites-enabled/000-default.conf` seperti berikut:

```conf
<VirtualHost *:81>
	ServerAdmin webmaster@localhost
    # kita copy-paste files angular yang telah di build tadi ke sini
	DocumentRoot /var/www/html

    ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Berikut perintahnya:

```bash
## grant access to user read, write, dan execute
sudo chmod -R 777 /var/www/html

## remove content from /var/www/html/*
rm -rf /var/www/html/*

## copy content angular into /var/www/html
cp -r path-project/belajar-angular/dist/belajar-angular/* /var/www/html
```

Setelah itu, coba reload configurasinya dengan perintah berikut:

```bash
## reload without restart
apachectl graceful

## or if not working try to restart
systemctl restart apache2.service
```

Sekarang coba check dengan browser ke alamat [localhost:81](http://localhost:81) berikut hasilnya:

![angular deployed into apache2]({{site.baseurl}}/assets/img/posts/deploy-angular-apache2/angular-deployed-apache2.png)

