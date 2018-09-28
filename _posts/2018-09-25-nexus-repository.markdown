---
layout: post
title: "Own your repository by Sonatype Nexus Repository"
date: 2018-09-25T23:49:05+07:00
category: repository
gist: dimMaryanto93/a5820a5a4824af4723fd2096b6ca891d
tags: 
- Packages Management
- Dependency
- Maven
- npm
- yum
- docker
author: Dimas Maryanto
references:
- https://www.sonatype.com/nexus-repository-sonatype
- https://www.sonatype.com/nexus-repository-oss
- https://help.sonatype.com/repomanager3/installation
- https://help.sonatype.com/repomanager3/system-requirements
comments: true
---

![nexus repository logo]({{site.baseurl}}/assets/img/posts/sonatype-nexus/nexus-logo.png)

Nexus Sonatype repository itu Central Repository to download components. Jika temen-temen terbiasa menggunakan lib / plugin / dependency yang dibuat orang lain, artinya kita kan kita harus download tu lib-nya. Nah jika temen temen bikin libs sendiri dan mau di publikasikan tapi cuman mau di gunakan untuk internal atau bahkan publik. Kita bisa menggunakan software package management dari salah satu product buatan [Sonatype](https://www.sonatype.com/) yaitu [Nexus Sonatype OSS](https://www.sonatype.com/nexus-repository-oss)

<!--more-->

Fitur yang ditawarkan juga menarik yaitu:

- Store and distribute Maven/Java, npm, NuGet, RubyGems, Docker, P2, OBR, APT and YUM and more.
- Manage components from dev through delivery: binaries, containers, assemblies, and finished goods.
- Mengurangi brantwith yang membengkak, karena download dependency lib yang digunakan.

Ilustrasi seperti berikut Misalnya kita buat project angular, nah dependency nya kurang lebih 200mb, nah programer yang koding ada sekitar 10 orang itu artinya kita harus sediakan kurang lebih quota 200mb X 10 sekitar 2gb, Untuk perusahan yang punya developer yang banyak brandwithnya pun bakalan semakin besar. Untuk yang koneksi internetnya yang super kenceng dan unlimited itu gak masalah tpi klo yang pas-pasan mah apalah daya harus keluar cost lagi untuk itu. Perumpamaan tersebut solusinya sebagai berikut:

![brantwith management nexus]({{site.baseurl}}/assets/img/posts/sonatype-nexus/nexus-latency.jpg)

Jadi kita cukup install Nexus Sonatype Repository, jadi kita akan di sediakan proxy dari Contral Repository ke Local Repository seperti gambar diatas. Selain itu, sebanyak apapun programmernya klo di proxy-kan ke local nexus repository maka tidak akan menggunakan jaringan internet tapi **local area network** jika dependency tersedia tpi klo belum ada akan didownload dan stored di Nexus Repository kemudian baru di copy ke client workstation yang membutuhkan.

## Install Nexus Sonatype

Sebelum menginstall pastikan kebutuhan spec `PC` / `Desktop` / `Server` memenuhi syarat berikut

### System required

- Java runtime environtment
- RAM,The default JRE min and max heap size of NXRM3 is pre-configured to be 1200MB, which should be considered an absolute minimum. The codebase will consume approximately another 1GB.  So factoring in operating system overhead you will need at least 4GB of RAM on a dedicated NXRM host, assuming no other large applications are running on the machine.
- CPU, NXRM performance is primarily bounded by IO (disk and network) rather than CPU.  So any reasonably modern 4 core (or better) CPU will generally be sufficient for normal uses of NXRM. 
- Storage, Nexus Repository Manager 3 installed consumes around 500 MB.

### Run as service

Proses installasi, betulan OS yang saya gunakan adalah Linux Ubuntu Server / Centos jadi saya lebih memilih install menggunakan service systemd karena bisa di control ketika di startup auto. Stepnya seperti berikut

- Download installer dari [nexus sonatype oss](https://www.sonatype.com/download-oss-sonatype)

- Extract file 

    ```bash
    tar -zxvf nexus-version.unix.tar.gz
    ```

- Move ke folder misalnya `opt`

    ```bash
    ## create folder
    sudo mkdir -p /opt/nexus-sonatype-oss/
    
    ## move folder to /opt/nexus-sonatype-oss/
    sudo mv nexus-version /opt/nexus-sonatype-oss/nexus-version
    ```

- Create file on `/etc/systemd/service` with name `nexus.service`:
    
    {% gist page.gist nexus.service %}

- Start service nexus repository

    ```bash
    sudo systemctl daemon-reload
    sudo systemctl enable nexus.service
    sudo systemctl start nexus.service
    ```

- Check if is running goes to [localhost:8081](http://localhost:8081)

![Nexus Repository on localhost]({{site.baseurl}}/assets/img/posts/sonatype-nexus/nexus-welcome.png)

## Configuration Proxy

Untuk configuration proxy atau daftar repository central yang ingin di daftarkan di Nexus Repository kita yang ingin di proxy-kan yaitu sebagai berikut:

- Login dulu sebagai admin untuk menconfigura `repository` dengan username `admin` dan passwordnya defaultnya `admin123` maka tampilan setelah login sebagai admin seperti berikut:

    ![welcome admin]({{site.baseurl}}/assets/img/posts/sonatype-nexus/nexus-admin.png)

- Klo mau menabahkan proxy dari central repository click **New Repository** Maka kita di suruh pilih jenis package mangement seperti apa dan Mau di set sebagai apa, ada 3 settingan yaitu

    ![type of package management and repository]({{site.baseurl}}/assets/img/posts/sonatype-nexus/type-repository.png)

    - Hosted, ini biasanya digunakan untuk local store package management atau package / lib yang kita publish
    - Proxy, ini digunakan untuk download dari Central Repository yang tersedia
    - Group, ini digunakan untuk melakukan pengelompokan repository berdasarkan jenis package menagernya atau supaya tidak menggunakan route yang ke duanya yaitu hosted dan proxy (gabungan).

- Misalnya saja kita pilih package manangement untuk Java yaitu Maven maka saya pilih **maven2 (Proxy)**

    ![create proxy repository]({{site.baseurl}}/assets/img/posts/sonatype-nexus/create-proxy-repository.png)

    - field name itu bebas aja isi terserah tapi harus unique
    - field Proxy remote storage di isi dengan url remote proxy contohnay `http://jaspersoft.artifactoryonline.com/jaspersoft/third-party-ce-artifacts/`

- Maka dengan begitu kita bisa download libs yang di proxy ke jasperreport tersebut.
    ![proxy repository]({{site.baseurl}}/assets/img/posts/sonatype-nexus/proxy-dependecy-management.png)

## Maven connect to Nexus Repository

Untuk maven secara default dia bakalan download ke maven central, untuk meng-override setting tersebut kita buat file `settings.xml` di folder `.m2` dengan configurasi public repository yang sedang saya gunakan seperti berikut:

{% gist page.gist settings.xml  %}

Nah sekarang tinggal coba perintah maven di command promt / terminal. Jika masih ke redirect ke maven repository central perlu cek kemali lokasi file `settings.xml` sudah benar?