---
layout: post
title: "How to Upgrade Gitlab easy way, from 11.x.x to latest version using Docker"
date: 2021-04-09T13:04:18+07:00
category: gitlab
tags: 
- Gitlab
- Obsolate
author: Dimas Maryanto
references:
- https://docs.gitlab.com/ce/update/#upgrade-paths
- https://docs.gitlab.com/ce/raketasks/backup_restore.html#back-up-gitlab
comments: true
gist: dimMaryanto93/1b96308698d2ff5e54abc8f00240cff5
github: https://github.com/dimMaryanto93/gitlab-ce-updater-docker
image_path: /assets/img/posts/gitlab-upgrade-major-version
---


![gitlab-update-path]({{ page.image_path | prepend: site.baseurl }}/gitlab-upgrade-path.jfif)

Hai semuanya di artikel kali ini saya mau membahas tentang bagaimana cara Upgrade Gitlab Repository (on-premise) dengan cara yang paling mudah yaitu dengan menggunakan Docker.

<!--more-->

Pertama yang kita perlukan adalah software requirement:

1. Install Docker, Docker Compose
2. Backup Repository gitlab
3. Backup Credential Store Object
4. Template Docker Compose

## Install docker

Untuk instalasi docker sangat mudah, cukup download dari [official website](https://www.docker.com/) dan install sesuai platform yang digunakan. Kemudian install juga component docker yaitu Docker Compose. Download dari [official webiste](https://docs.docker.com/compose/install/) dan install sesuai dengan platform masing-masing.

## Backup Repository github

Untuk backup, disini kondisi gitlab repository yang saya gunakan masih menggunakan versi `11.10.xx`, jadi gunakan perintah berikut sesuai dengan [dokumentasi berikut](https://docs.gitlab.com/11.11/ee/raketasks/backup_restore.html#excluding-specific-directories-from-the-backup)

{% gist page.gist "gitlab-backup-init.bash" %}

Setelah itu, file backupnya biasanya akan di simpan di folder `/var/opt/gitlab/backups/` dengan prefix filename `millisond-date.version_gitlab_backup.tar` contohnya `1617497494_2021_04_04_11.10.2_gitlab_backup.tar`

## Backup Credential Object

Setelah di backup, kita ambil juga file credential seperti

- `/etc/gitlab/gitlab-secret.json`
- `/etc/gitlab/trusted-certs/your-ssl-cert`
- `/etc/gitlab/gitlab.rb`

## Setup & Preparation Upgrade

Pertama temen-temen download dulu template docker-compose.yaml dari [sini](https://github.com/dimMaryanto93/gitlab-ce-updater-docker), kemudian extract. Setelah extract

1. Simpan file backup repostiory ke dalam folder `backups`
2. Simpan file config credential ke dalam folder `gitlab`

Jika dilihat hasilnya seperti berikut:

```bash
C:\Users\dimasm93\Workspaces\Example\gitlab-ce-updater-docker>tree .
PATH listing

C:\USERS\DIMASM93\WORKSPACES\EXAMPLE\GITLAB-CE-UPDATER-DOCKER
├───backups
│   └───1617497494_2021_04_04_11.10.2_gitlab_backup.tar
├───docker-compose.yaml
└───gitlab
    └───gitlab-secret.json
```

Kemudian coba liat di file `docker-compose.yaml`, seperti berikut:

{% gist page.gist "docker-compose.yaml" %}

pada property `services.gitlab.image` ubahlah versinya sesuai dengan file backupnya, yaitu: `11.11.0` kemudian kita check di [hub.docker.com/r/gitlab](https://hub.docker.com/r/gitlab/gitlab-ce/tags?page=1&ordering=last_updated&name=11.10.2) menjadi seperti berikut

```yaml
services:
  gitlab:
    image: gitlab/gitlab-ce:11.10.2-ce.0
```

Setelah itu jalankan servicenya dengan perintah:

{% gist page.gist "docker-compose-up.bash" %}

Kemudian, kita bisa check logsnya dengan perintah berikut:

{% gist page.gist "docker-compose-logs.bash" %}

Setelah gitlabnya running & up, tahap selanjutnya adalah Restore dari backup yang sudah kita siapkan

## Restore Gitlab from Backup

Untuk melakukan restore, kita bisa menggunakan perintah `docker exec` seperti berikut:

{% gist page.gist "gitlab-backup-init.bash" %}

Setelah setelah selesai proses restore, kita reconfigure & restart seperti berikut

{% gist page.gist "docker-compose-gitlab-reconfigure.bash" %}

Dan 

{% gist page.gist "docker-compose-gitlab-restart.bash" %}

Kemudian coba di check pada web gitlabnya: [http://localhost:80](http://localhost:80) kalo sudah sesuai dengan yang ada di server kita. Tahap selanjutnya adalah upgrade

## Upgrade Path

Untuk melakukan upgrade, Gitlab tidak bisa secara langsung artinya klo versinya udah ketinggalan jauh tidak bisa langsung dari `11.11` -> `latest` tpi ada tahapannya seperti pada artikel [dokumentasi berikut](https://docs.gitlab.com/ce/update/#upgrade-paths)

Karena kita pake versi `11.10` maka pathnya adalah `11.11.8` -> `12.0.12` -> `12.1.17` -> `12.10.14` -> `13.0.14` -> `13.2.10` -> `13.5.4`. Lumayan panjang ya?? yaa lumayan lah, gak terlalu ribet dari pada migrasiin semua repository (sekarang repository ada ratusan di kantor ku :( ) jadi lebih milih upgrade pake docker ja, supaya lebih aman. 

Tahap pertama, matikan dulu servicenya docker-compose dengan perintah berikut:

{% gist page.gist "docker-compose-down.bash" %}

Setelah servicenya mati, pastikan `docker volume` jangan dihapus. Karena disitu kita simpan data repository kita. Selanjutnya adalah update versi gitlabnya di `docker-compose.yaml` ke target kita contohnya `11.11.8`

```yaml
services:
  gitlab:
    image: gitlab/gitlab-ce:11.11.8-ce.0
```

Setelah itu jalankan kembali service docker-composenya dengan perintah yang tadi yaitu seperti berikut:

{% gist page.gist "docker-compose-up.bash" %}

Maka Otomatis akan malakukan gitlab upgrade. Setelah selesai lakukan sampai versinya sesuai yang kita inginkan. Secara flow

1. set version `11.11.8` -> `docker-compose up` -> `docker-compose down`
2. set version `12.0.12` -> `docker-compose up` -> `docker-compose down`
3. Backup Gitlab Repository menggunakan docker-compose

    {% gist page.gist "docker-compose-gitlab-backup-12.1-earlier.bash" %}

4. set version `12.1.17` -> `docker-compose up` -> `docker-compose down`
5. set version `12.10.14` -> `docker-compose up` -> `docker-compose down`
5. set version `13.0.14` -> `docker-compose up` -> `docker-compose down`
6. Backup Gitlab Repository menggunakan docker-compose

    {% gist page.gist "docker-compose-gitlab-backup-12.2-later.bash" %}

7. set version `13.0.14` -> `docker-compose up` -> `docker-compose down`
8. dan seterurnya

Yang perlu diingat adalah, **jangan lupa Backup dan check gitlab web** supaya tidak terjadi hal yang tidak di inginkan seperti project repository yang hilang atau setting rusak atau tidak bisa dibuka.