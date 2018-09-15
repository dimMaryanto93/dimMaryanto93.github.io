---
layout: post
title: "Local yum repository for Rhel7 / Fedora / Centos"
category: yum
tags: 
- yum
- repository
- package manager
- rhe7
- fedora
- contos
author: Dimas Maryanto
references:
- https://access.redhat.com/solutions/9892
comments: true
---

Setelah kita meng-enable [local-repo menggunakan dvd / iso]({% post_url 2018-09-11-enabled-yum-rhel7 %}) ada kalanya kita ingin menginstall package yang belum tersedia di dvd / iso contohnya seperti Mysql Community Edtion, atau Oracle Database dan lain-lain.

Kita bisa membuat repository kita sendiri menggunakan `createrepo` dengan bantuan

- yum
- httpd
- yum-utils

<!--more-->

Tahap pertama kita install dulu package dari `yum` yang telah kita enabled dan menggunakan local dvd/iso, berikut perintahnya:

```bash
yum install createrepo httpd yum-utils
```

Setelah terinstall bikin folder (optinal) dalam `/var/www/html` contohnya `repos/redhat/7/os/x86_64`

```bash
sudo mkdir -p /var/www/html/repos/redhat/7/os/x86_64
```

Kemudian Copy file `rpm` atau package yang telah di download sebelumnya. contohnya saya telah nge-download `mysql-5.7-community.bundle.tar` yang di [download sini](https://dev.mysql.com/downloads/mysql/5.7.html#downloads) kemudian saya extract filenya dengan perintah:

```bash
## make folder
mkdir mysql-bundle
## move to folder
mv mysql-5.7.23-1.el7.x86_64.rpm-bundle.tar mysql-bundle
## extract into folder
tar -xvf mysql-5.7.23-1.el7.x86_64.rpm-bundle.tar

## read-write-execute permision granted
sudo chmod -R 777 /var/www/html/
## copy to folder /var/www/html/repos/redhat/7/os/x86_64
cp -r mysql-commutity* /var/www/html/repos/redhat/7/os/x86_64
```

Lalu setelah `rpm` di copy ke folder httpd, kita buat lah repositorynya dengan perintah berikut:

```bash
createrepo /var/www/html/repos/redhat/7/os/x86_64
```

```bash
# update repository jika udah buat
# reposync -p /var/www/html/repo -r <REPOID> -l
# createrepo /var/www/html/repo    
```

Sekarang kita aktifkan dulu service `httpd` dengan perintah:

```bash
## enabled service to start at system start
systemctl enable httpd.service

## start service now
systemctl start httpd.service
```

Nah sekarang kita buat file `my-repo.repo` di folder `/etc/yum.repos.d/` isinya seperti berikut:

```ini
[my-yum-repository]

name=MySQL 8.0.12 Community
baseurl=http://localhost/repos/redhat/7/os/$basearch/
gpgcheck=0
enabled=1
```

Setelah itu kita update `yum` dengan perintah:

```bash
# clean yum repository
sudo yum clean all && sudo rm -rf /var/cache/yum/

# update list repository
sudo yum --noplugin update && sudo yum --noplugin list
```

Nah sekarang kita tinggal install package yang mau kita install

```bash
sudo yum install mysql-community-server-5.7.23-1.el7.x86_64
```


