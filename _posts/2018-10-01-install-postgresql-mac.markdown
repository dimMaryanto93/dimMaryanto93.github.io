---
layout: post
title: "Install Postgresql versi EnterpriceDB on masOS"
date: 2018-10-01T06:33:04+07:00
category: mac-os
tags: 
- masOS
- Postgresql
- EnterpriceDB
author: Dimas Maryanto
references:
- https://www.enterprisedb.com/docs/en/9.3/pginstguide/PostgreSQL_Installation_Guide-07.htm#TopOfPage
- https://www.enterprisedb.com/docs/en/9.5/pg/app-pg-ctl.html
- https://www.enterprisedb.com/docs/en/9.5/pg/server-start.html
comments: true
---

![EnterpriceDB]({{site.baseurl}}/assets/img/posts/postgresql-on-macos/edb-logo.jpeg)

Selain, MySQL database kita juga bisa meng-install PostgreSQL dengan mudan yaitu menggunakan EnterpriceDB dengan bantuan Wizard Installer-nya jadi dapat dengan mudah di install. Download dulu [disini](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads) untuk yang free. Berikut cara installnya

<!--more-->

Untuk proses installasinya sebenarnya sama seperti di platform Windows, yaitu tinggal di Next2 aja tpi gpp berikut configurasi saya gunakan.

- Step pertama run file `postgresql-10.5-2-osx.dmg` maka akan muncul tampilan seperti berikut:

    ![Postgresql instal]({{site.baseurl}}/assets/img/posts/postgresql-on-macos/postgresql-package.png)

- Kemudian **Double click** file `postgresql-10-5-osx.app`, berikut

    ![Postgresql instal]({{site.baseurl}}/assets/img/posts/postgresql-on-macos/postgresql-setup.png)

- Click **Next** untuk melanjutkan installasi

    ![Postgresql instal]({{site.baseurl}}/assets/img/posts/postgresql-on-macos/postgresql-set-components.png)

- Untuk saya, saya cuman mau install commponentnya `PostgreSQL Server`, sama `Command line tool` jadi yang lainnya gak perlu di checklist. Setelah itu **Next**

    ![Postgresql instal]({{site.baseurl}}/assets/img/posts/postgresql-on-macos/postgresql-set-location.png)

- Untuk lokasi install, klo saya biarkan semuanya default jadi lokasinya di `/Library/Postgresql/[versi]`, Kemudian click **Next**

    ![Postgresql instal]({{site.baseurl}}/assets/img/posts/postgresql-on-macos/postgresql-set-data-location.png)

- Untuk lokasi data, saya seperti lokasi yaitu biarkan default, jadi lokasinya di `/Library/Postgresql/[versi]/data`, Lalu click **Next**

    ![Postgresql instal]({{site.baseurl}}/assets/img/posts/postgresql-on-macos/postgresql-set-postgres-password.png)

- Untuk postgres password, saya menggunakan yang paling mudah aja yaitu `admin` biar gak lupa heheh, Click **Next** lagi

    ![Postgresql instal]({{site.baseurl}}/assets/img/posts/postgresql-on-macos/postgresql-set-port.png)

- Untuk port biarkan default yaitu `5432`, Kemudian click **Next**

    ![Postgresql instal]({{site.baseurl}}/assets/img/posts/postgresql-on-macos/postgrasql-set-locale.png)

- Untuk locale, biarkan default juga yaitu `locale default system`, lalu click **Next** 

    ![Postgresql instal]({{site.baseurl}}/assets/img/posts/postgresql-on-macos/postgresql-install-summary.png)

- Berikut adalah configurasi yang udah kita setup, click **Next** aja untuk konfirmasi installasi EnterpriceDB.

    ![Postgresql instal]({{site.baseurl}}/assets/img/posts/postgresql-on-macos/postgresql-install-ready.png)

- Click **Next** untuk intall

    ![Postgresql instal]({{site.baseurl}}/assets/img/posts/postgresql-on-macos/postgresql-install-loading.png)

- Installasi sedang berjalan, kita tunggu ja sampe selesai. Setelah itu ada muncul halaman berikut:

    ![Postgresql instal]({{site.baseurl}}/assets/img/posts/postgresql-on-macos/postgresql-install-finish.png)

- Untuk proses installasi telah selesai, Untuk `Stackbuilder` itu optional jadi saya gak di checklist setelah itu click **Finish** 

<br/>


## Configurasi service

Untuk meng-konfigurasi service bisa start/stop service kita modif file `~/.bash_profile` seperti berikut:

```bash
## add this script at the begining
export POSTGRESQL_HOME=/Library/PostgreSQL/10
alias service_stop_postgres='sudo -u postgres /Library/PostgreSQL/10/bin/pg_ctl -D /Library/PostgreSQL/10/data stop'
alias service_start_postgres='sudo -u postgres /Library/PostgreSQL/10/bin/pg_ctl -D /Library/PostgreSQL/10/data start'

## add this line at the end
export PATH=$PATH:$POSTGRESQL_HOME/bin/
```

Nah sekarang, klo mau matikan servicenya ketika startup. Jalankan script di terminal berikut:

```bash
sudo rm /Library/LaunchDaemons/com.edb.launchd.postgresql-[version].plist
```

<br/>

## Test connect

Untuk ngetest, servicenya berjalan dengan baik kita test dulu menggunakan `psql` tapi pertama jalankan dulu servicenya dengan perintah berikut:

```bash
## menggunakan alias yang telah kita declarasi di file .bash_profile
service_start_postgres

## connect menggunakan psql
psql -h localhost -U postgres
```

<br/>

