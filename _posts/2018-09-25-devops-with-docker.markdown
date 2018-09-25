---
layout: post
title: "Devops with docker"
date: 2018-09-25T21:16:50+07:00
category: docker
tags: 
- Devops
- Docker
- Virtual machine
author: Dimas Maryanto
references:
- https://docs.docker.com/docker-for-mac/install/#what-to-know-before-you-install
- https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install
- https://www.docker.com/resources/what-container
comments: true
---

Sebagai seorang Research and Development dan sekaligus Java Software Programer, tidak jarang loh berurusan dengan product brended sekelas IBM, Oracle dan lainnya. Misalnya karena kerjaan kita adalah mendevelop aplikasi setelah itu aplikasi tersebut harus run di product IBM misalnya JBoss EAP versi 7.x.x kemudian Untuk database menggunakan product Microsoft MS-SQL Server 2017. 

Permasalahnya tidak berhenti sampai situ? Permasalahan sebenarnya product branded tersebut punya spesifikasi masing-masin contohnya MS SQL Server hanya run di Microsoft Azure, dan Linux Server sedangkan tidak semua Developer memiliki platform tersebut dan spesifikasi laptop dan komputer yang memadai untuk me-runing product tersebut.

![docker]({{site.baseurl}}/assets/img/posts/devops-docker/logo.png){: width="400px"}

Solusinya yaitu dengan virtualisasi, virtualisasi seperti apa? VMware atau Virtual box??? opppsss itu udah kuno. Perkenalkan ada teknologi yang baru-baru (2017) ini ramai di perbincangkan yaitu Docker.

<!--more-->

![what is docker]({{site.baseurl}}/assets/img/posts/devops-docker/IsDockerSecure.png)

Jadi docker itu software virtual, untuk menjalankan application, operation system, engine, database dan masih banyak lagi di dalam host kita dengan menggunakan **docker image** yang diruning dalam **docker container** contohnya

- [Ubuntu](https://hub.docker.com/r/_/ubuntu/)
- [Postgres](https://hub.docker.com/_/postgres/)
- [Mysql](https://hub.docker.com/_/mysql/)
- [Oracle Database](https://github.com/oracle/docker-images/tree/master/OracleDatabase)
- [Maven](https://hub.docker.com/_/maven/)
- [Gitlab](https://hub.docker.com/r/gitlab/gitlab-ce/)
- dan lain-lain yang tersedia di repository docker yaitu [docker-hub](https://hub.docker.com/)

## Mininum System required

- Linux
- Mac Os Sierra or lastest
    - Mac hardware must be a 2010 or newer model, with Intel’s hardware support for memory management unit (MMU) virtualization, including Extended Page Tables (EPT) and Unrestricted Mode
    - macOS El Capitan 10.11 and newer macOS releases are supported
    - At least 4GB of RAM
- your machine must have a 64-bit operating system running Windows 7 
    - Docker for Windows requires Microsoft Hyper-V to run
    - Virtualization must be enabled in BIOS and CPU SLAT-capable

## Docker Image

Docker images adalah ibaratnya installer aplikasi, aplikasi yang akan kita install kita harus pull dulu dari [docker-hub](https://hub.docker.com/) konsep nya sama seperti git jadi kita pull, contohnya kalo kita mau install postgresql dalam **docker container** jadi kita pull dulu coba ke docker hub kemudian cara official repository untuk postgresql atau kita juga bisa menggunakan yang unofficial repository. bedanya klo yang official ada mark **official** atau di urlnya seperti berikut [official postgres](https://hub.docker.com/_/postgres/) repository tapi sebelum itu kita harus login dulu klo belum punya akun daftar aja gratis kok dan tidak butuh kartu kredit :)

```git
docker pull postgres
```

Setelah tadi kita pull maka daftar aplikasi yang telah siap digunakan atau di pasang di **Docker container** seperti berikut:

```bash
docker image ls
## output
# REPOSITORY                     TAG                 IMAGE ID            CREATED             SIZE
# mysql                          5.7                 563a026a1511        2 weeks ago         372MB
# postgres                       9.6                 0178d5af9576        4 weeks ago         229MB
# microsoft/mssql-server-linux   2017-latest         e254a7681c2f        5 weeks ago         1.44GB
```

<br/>

## Docker Container

![docker container]({{site.baseurl}}/assets/img/posts/devops-docker/Containers.jpg)

Docker container, itu ibaratnya applikasi yang telah terinstall dari **Docker image**. Misalnya kita mau install postgresql, kita bisa baca2 dokumentasinya dulu properties apa aja yang harus di pasang contohnya seperti berikut:

```bash
docker container run \
    --name postgres_db \
    -p 5432:5432 \
    -v /var/lib/postgresql/data \
    -e POSTGRES_PASSWORD=postgres \
    -e POSTGRES_DB=data_db \
    -d \
    postgres:9.6
```

Artinya dari perintah tersebut:

- service_name : `postgres_db` untuk argument `--name postgres_db`
- port yang di expose : `5432` untuk argument `-p 5432:5432`
- password untuk user postgres : `postgres` untuk argument `-e POSTGRES_PASSWORD=postgres`
- database name : `data_db` untuk argument `-e POSTGRES_DB=data_db`
- postgresql version : `9.6` 
- jalankan di background, untuk argument `-d`

Nah sekarang kita bisa check containernya apakah udah run dengan cara perintah berikut:

```bash
docker container ls
## output console
#CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
#b91d16813041        postgres:9.6        "docker-entrypoint.s…"   2 minutes ago       Up 45 seconds       0.0.0.0:5432->5432/tcp   postgres_db
```

Setelah itu kita bisa connect ke postgresql dalam docker container, caranya sama seperti biasa:

```bash
psql -h 127.0.0.1 -U postgres data_db
```

Hanya bedanya kita tidak menggunakan database yang di install di host kita. Nah mungkin dari temen-teman buat apa nanti datanya hilang donk. Eitssss tenang klo datanya mau di keep ada fiturnya kok namanya **docker volume** nanti akan saya jelaskan yang pasti ini akan memudahkan developer dalam develop app apalagi untuk barang branded seperti Oracle Database, Centos os, JBoss eap dan masih banyak lagi.

## Summary

Keuntungan menggunakan docker yaitu, 

- kita **tidak perlu ribet set environment** di host kita
- Tidak perlu takut salah waktu install software sehingga **tidak menggangu performa** host kita.
- **Tidak memakan banyak memory (RAM)** seperti di VM
- Kita bisa **menggunakan application yang sama dalam beberapa instance sekaligus** tetapi dengan syarat port / enviroment yang berbeda (masih dalam host yang sama).
- Jika aplikasi udah tidak kita gunakan dapat dengan **mudah untuk di drop** dari container (ini yang paling make me impressed) tanpa ada yang berpengaruh karena di install di virtual.