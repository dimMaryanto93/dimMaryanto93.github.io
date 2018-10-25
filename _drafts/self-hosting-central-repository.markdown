---
layout: post
title: "Self hosting, Central GIT Repository"
category: gitlab
tags: 
- Version Control System
- git
- gitlab
- ubuntu
author: Dimas Maryanto
references:
- https://about.gitlab.com/product/
- https://about.gitlab.com/install/#ubuntu?version=ce
comments: true
---

![gitlab ce]({{site.baseurl}}/assets/img/posts/gitlab-self-hosting/logo.png){: width="400px" }

Version control system (VCS) engine yang paling easy to use, hype yaitu git. kita kali ini gak akan bahas git-core tpi bahas shared git repository atau central repository git. Diantaranya ada 3 yang populer yaitu 

- Github
- Bitbucket (JIRA)
- Gitlab

<!--more-->

Ketiga central git repository ini punya kelebihan dan kekurangan masing-masing. Dulu sekitar 3-4 tahun yang lalu saya investasi di Github dengan subcribe Github Developer dengan membayar kurang lebih `7$ / bulan` dengan fasilitas ultimate private repository, fast speed upload / download dan masih banyak lagi tetapi klo saya hitung2 pertahunya `Rp. 100,000,- * 12` saya harus bayar sekitar `Rp. 1,200,000,-` (biaya yang bukan sedikit). 

Setelah sekitar setahun saya pake subscribe github developer itu saya pikir dari pada keluar biaya segitu setahunya mending saya cari alternatif lain, kemudian saya menemukan yang lebih murah yaitu ada saingan sama Github yaitu Bitbucket yang bisa dapet free unlimited private repository tpi usernya terbatas 5 account yang bisa kontribusi dalam 1 repository private tersebut, which is fine buat ngimpen project-project private saya tpi setelah bebapa bulan repository yang jarang di push dan di akses jadi gak bisa di buka lagi. Dan saya cari-cari lagi git central repository yang bisa lebih bermanfaat dan bisa di install di komputer/server sendiri.

Dapatlah gitlab, Gitlab ini ada 2 versi Enterprice Edition (Berbayar) dan Community Edition (Free). Ya jelaslah saya pilih yang Community Edition free gitu lohhhh (dari pada byaar buat hosting mending investasi untuk beli hardware PC/Server).

Specifikasi yang dibutuhkan untuk menjalankan Gitlab-CE adalah 

- GitLab is developed for Unix operating systems. GitLab does not run on Windows and we have no plans of supporting it in the near future, (Ubuntu, Debian, Centos, Redhat, Fedora dan lain-lain)
- Fitur compare, [check thisout](https://about.gitlab.com/pricing/self-managed/feature-comparison/)
- CPU Recomendation 2+ core
- 2GB RAM is the recommended memory size and supports up to 100 users
- Database: If you want to run the database separately expect a size of about 1 MB per user (Default database using PostgreSQl)

## Install gitlab-CE 

Untuk install gitlab-ce kita bisa ikuti step yang di kasih [official websiste](https://about.gitlab.com/install/#ubuntu?version=ce) klo menggunkan distibution yang disuport oleh gitlab seperti Ubuntu, Centos, atau Debian. Dengan Perintah berikut:

```bash
# install dependency
apt-get install -y curl openssh-server ca-certificates postfix

# add gitlab-ce repository
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash

# update repository & install gitlab-ce
apt-get update && apt-get install gitlab-ce

# post install gitlab
gitlab-ctl reconfigure
```

Setelah install gitlab-ce selesai maka akan tapi console seperti berikut:

![gitlab after install setup]({{site.baseurl}}/assets/img/posts/gitlab-self-hosting/install-finish.png)

## Setup user management gitlab-ce

Setelah install selesai kita bisa coba, gitlab host di [http://localhost](http://localhost:80), maka akan muncul halaman set default `root` password seperti berikut:

![setup root password]({{site.baseurl}}/assets/img/posts/gitlab-self-hosting/setup-root-password.png)

Untuk password `root` ini disarankan jangan sampe lupa karena ini adalah halaman admin untuk mengontrol semua aktifitas di gitlab kita seperti, mematikan user register, pengaturan halaman default login dan lain-lain.

Setelah user `root` terdaftar kita bisa register user baru untuk kebutuhan kita sehari2 seperti membuat repository, push, pull dan lain lain seperti berikut:

![register new user]({{site.baseurl}}/assets/img/posts/gitlab-self-hosting/register-user-new.png)

## Gitlab configuration

Setelah kita bisa login, kita akan konfigurasi seperti host remote, backup location, dll

```ruby
## GitLab configuration settings, contohnya ip address gitlab saya adalah sebagai berikut
external_url 'http://192.168.88.253'

### Backup Settings
## lokasi backup data repository saya simpan di folder /var/opt/gitlab/backups
###! Docs: https://docs.gitlab.com/omnibus/settings/backups.html
gitlab_rails['manage_backup_path'] = false
gitlab_rails['backup_path'] = "/var/opt/gitlab/backups"
```

Setelah itu harus restart configurasinya dengan menggunakan perintah berikut:

```bash
gitlab-ctl reconfigure
```

## Gitlab backup & restore

Untuk membackup data/repository di gitlab kita bisa gunakan perintah seperti berikut:

```bash
sudo gitlab-rake gitlab:backup:create
```
 Dengan perintah berikut file backup akan disimpan di folder yang kita tentukan tadi contohnya seperti berikut:

![gitlab backup]({{site.baseurl}}/assets/img/posts/gitlab-self-hosting/gitlab-backup.png)

Sedangkan untuk restore gunakan, kita gunakan file backup kemudian simpan di folder `/var/opt/gitlab/backups` kemudian gunakan perintah berikut untuk me-restore data/repository:

```bash
## stop dulu service gitlab
sudo service gitlab stop

## restore gitlab data/repository
bundle exec rake gitlab:backup:restore RAILS_ENV=production

## restart service gitlab
sudo service gitlab restart
```