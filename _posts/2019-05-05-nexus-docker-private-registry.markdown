---
layout: post
title: "Docker private registry dengan Nexus OSS"
date: 2019-05-05T21:23:04+07:00
category: docker
tags: 
- docker
- nexus
- repository
- private-registry
author: 
references:
- https://blog.sonatype.com/using-nexus-3-as-your-repository-part-3-docker-images
comments: true
---

![logo nexus-x-docker]({{site.baseurl}}/assets/img/posts/nexus-docker-private-registry/logo.png){: width="300px" }

Private Registry menggunakan Nexus OSS, sama halnya kita menggunakan gitlab / maven di local network kita yang tujuannya sebagai repository / proxy.
<!--more-->

Langsung aja kita setup, tapi pertama kita harus install dulu software seperti [Sonatype Nexus OSS](https://www.sonatype.com/nexus-repository-oss) dan [docker-ce](https://docs.docker.com/install/) / docker-ee tergantung dari versi yang akan digunakan. tpi klo saya menggunakan docker-ce aja yang gratissss.

setelah keduanya di install, login dulu [localhost:8081](http://localhost:8081) sebagai admin di nexus-oss seperti berikut:

![nexus login as admin]({{site.baseurl}}/assets/img/posts/nexus-docker-private-registry/as-admin.png)

Kemudian kita pilih -> **Settings** -> **Repository** seperti berikut:

![nexus settings -> repository]({{site.baseurl}}/assets/img/posts/nexus-docker-private-registry/admin-settings.png)

## Setup proxy from hub.docker.io

Kemudian kita buat dulu configurasi untuk proxy dari docker registry, jika kita ingin mendownload image dari hub.docker.io ke private-registry kita tpi klo kita tidak membutuhkanya kita bisa skip langkah ini.

**Create new repository** -> **docker (proxy)** 

Kemudian isi form seperti berikut configurasinya:

- name [bebas tapi harus di isi]: docker-registry-io
- online: di checklist
- remote storage [boleh pake registry lainnya]: **https://registry-1.docker.io**
- Docker Index [pilih]: **Use Docker Hub**
- Blob store [pilih bebas]: default

Setelah itu click **Save**

## Setup private hosted repository

Setelah itu, kita setup untuk fitur yang kita perlukan yaitu untuk menyimpan image yang kita miliki ke private-registry dengan menggunakan **docker (hosted)**

**Create new repository** -> **docker (hosted)**

Kemudian isi form seperti berikut configurasinya:

- name [bebas tpi harus di isi]: docker-repository
- online: di checklist
- HTTP [di checklist dan di isi port contohnya]: `8087`
- Allow anonnymuous docker pull [di checklist klo mau yang bisa ngepull tanpa login]: **uncheked**
- Enable Docker V1 API [docker engine version]: **unchecked**
- Deployment policy [Tergantung kebutuhan, klo saya pilih Allow redeploy supaya tanpa ganti tags]: **Allow redeploy**

Setelah itu di **Save** kemudian kita allow incoming * outbound dari firewall portnya

```bash
## centos / fedora / redhat
firewall-cmd --zone=public --add-port=8087/tcp --permanent &&  firewall-cmd --reload

## debian / ubuntu
sudo iptables -I INPUT 1 -i eth0 -p tcp --dport 8087 -j ACCEPT
```

## Setup group repository docker

Group repository, untuk mengkases ke-2 repository yang telah kita buat dalam satu port connection saja. atau kita bisa pisahkan misalnya yang push repository menggunakan port `8087` sedangkan yang pull bisa menggunkan group repository ini.

**Create new repository** -> **docker (group)**

Kemudian isi form seperti berikut configurasinya:

- Name [bebas, tapi harus diisi]: docker-repository-public
- Online: checked
- HTTP: `8086`
- Allow anonnymuous docker pull [di checklist klo mau yang bisa ngepull tanpa login]: **uncheked**
- Blob store: default
- Member repositories: pilih semua repository yang kita telah buat seperti berikut:

![group docker selected]({{site.baseurl}}/assets/img/posts/nexus-docker-private-registry/member-repositories-selected.png)

Setelah itu kita **Save**, dan allow connection inbound dan outbound di firewall seperti berikut

```bash
## centos / fedora / redhat
firewall-cmd --zone=public --add-port=8086/tcp --permanent &&  firewall-cmd --reload

## debian / ubuntu
sudo iptables -I INPUT 1 -i eth0 -p tcp --dport 8086 -j ACCEPT
```

## Configure insecure-connection

docker-engine hanya mengijinkan kita untuk melakukan pull/push dari registry docker itu sendiri, jadi kita harus daftarkan di docker-engine jika di mac kita bisa tambahkan dari Docker-Desktop -> **Preferences** -> **Daemon** -> **insecure registries** inputan [ip-nexus-server:http-connection] contohnya: `localhost:8086`, `localhost:8087`, `domain.com:8087` dan lain-lain seperti berikut:

![docker insecure registry mac]({{site.baseurl}}/assets/img/posts/nexus-docker-private-registry/docker-insecure-registries.png)

Setelah itu **Apply & restart**

Atau jika menggunakan linux: 

Buat atau tambahkan ke file `/etc/docker/daemon.json` seperti berikut

```json
{
    "insecure-registries" : ["localhost:8086", "localhost:8087"]
}
```

Kemudian restart docker dengan perintah seperti berikut:

```bash
systemctl restart docker.service
```

## Setup authentication

Sekarang kita setup untuk authenticationnya, kita bisa menu **Security** -> **Users** Kemudian kita buat usernya

Setelah membuat user kita setup untuk Realm di menu **Security** -> **Realms** untuk mengaktifkan pull tanpa login ke docker dulu, dengan cara Aktifkan **Docker Bearer Token Realm** seperti berikut:

![docker realms]({{site.baseurl}}/assets/img/posts/nexus-docker-private-registry/docker-realms-token.png)

## Testing login

```bash
docker login 192.168.88.50:8087
```

- Username: username yang kita setup di nexus repository
- Password: input passwordnya

Jika ada message: `Login Succeeded` berarti anda sudah bisa melakukan push ke repository

## Testing push repository

contohnya berikut saya punya docker image seperti berikut:

```bash
docker images
```

![docker images]({{site.baseurl}}/assets/img/posts/nexus-docker-private-registry/docker-images.png)

Misalnya kita mau push docker image postgresql dengan versi 9.6, kita buat tags dul seperti berikut perintahnya:

```bash
# example sintax `docker tag postgres:9.6 [nexus-host]:[docker-repository-http-port]/[username]/[new-image-name]:[new-version]`
docker tag postgres:9.6 192.168.88.50:8087/dimasm93/postgresql:9.7

# example sintax `docker push [nexus-host]:[docker-repository-http-port]/[username]/[new-image-name]:[new-version]`
docker push 192.168.88.50:8087/dimasm93/postgresql:9.7
```

Berikut hasilnya:

![docker image pushed]({{site.baseurl}}/assets/img/posts/nexus-docker-private-registry/docker-pushed.png)

## Testing pull from docker-registry

Jika kita mau melakukan pulling dari [hub.docker.io](https://hub.docker.io) atau docker registry
kita bisa menggunakan perintah seperti berikut:

```bash
## example sintax `docker pull [nexus-host]:[docker-group-http-port]/[image-name]:[version]
docker pull 192.168.88.50:8086/mysql:5.7
```

Berikut hasilnya:

![docker pull from hub.docker.io]({{site.baseurl}}/assets/img/posts/nexus-docker-private-registry/docker-pulled.png)

Ok sekian dulu setup docker-image di private registry menggunakan Sonartype Nexus OSS.





