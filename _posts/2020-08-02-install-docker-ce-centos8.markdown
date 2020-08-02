---
layout: post
title: "Install Docker CE for Centos 8"
date: 2020-08-02T21:25:17+07:00
category: docker
tags: 
- centos
- docker
author: Dimas Maryanto
references:
- https://stackoverflow.com/a/63067436/6685789
- https://linuxhint.com/install_docker_ce_centos8/
comments: true
---

![centos 8 logo]({{site.baseurl}}/assets/img/posts/install-docker-centos8/khggm10hhsm52aitdr79.png){:width="400px"}

Docker di centos 8, belum sepenuhnya support oleh docker.io masih ada beberapa package yang belum compatible seperti versi dari `containerd.io > 1.2.0-3.el7` dan ada beberapa problem lagi yaitu `firewalld` akan memblock comunication antar container, bagaimana cara menghandlenya. ok sekarang langsung ja kita install.

<!--more-->

Sebelum kita install docker-ce package kita install dulu dependencynya seperti berikut:

```bash
dnf install dnf-utils device-mapper-persistent-data lvm2 fuse-overlayfs wget
```

Kemudian kita tambahkan repository docker-ce untuk centos dengan perintah seperti berikut:

```bash
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

hasilnya seperti berikut:

```bash
[root@dev01 ~]# ls /etc/yum.repos.d/docker-ce.repo
/etc/yum.repos.d/docker-ce.repo

[root@dev01 ~]# cat /etc/yum.repos.d/docker-ce.repo
[docker-ce-stable]
name=Docker CE Stable - $basearch
baseurl=https://download.docker.com/linux/centos/7/$basearch/stable
enabled=1
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg
```

Setelah itu kita [Downloads](https://download.docker.com/linux/centos/7/x86_64/stable/Packages/) dan Install dulu package `containerd.io` dengan perintah berikut:

```bash
dnf install containerd.io-1.2.13-3.2.el7.x86_64
```

Setelah terinstall baru kita, install package `docker-ce`

```bash
dnf install docker-ce docker-ce-cli
```

Kemudian kita jalankan service dockernya dengan perintah seperti berikut:

```bash
systemctl enable --now docker
```

Ok di tahap ini install docker udah selesai, sekarang kita setting supaya `DNS (Domain Names Server)` bisa dikenali routenya dengan cara disable firewald atau  dengan cara berikut:

```bash
# Allows container to container communication, the solution to the problem
firewall-cmd --zone=public --add-masquerade --permanent

# reload the firewall
firewall-cmd --reload
```