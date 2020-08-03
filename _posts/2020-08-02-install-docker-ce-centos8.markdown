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
- https://success.docker.com/article/how-do-i-enable-the-remote-api-for-dockerd
comments: true
---

![centos 8 logo]({{site.baseurl}}/assets/img/posts/install-docker-centos8/khggm10hhsm52aitdr79.png){:width="400px"}

Docker di centos 8, belum sepenuhnya support oleh docker.io masih ada beberapa package yang belum compatible seperti versi dari `containerd.io > 1.2.0-3.el7` dan ada beberapa problem lagi yaitu `firewalld` akan memblock comunication antar container, bagaimana cara menghandlenya. ok sekarang langsung ja kita install.

<!--more-->

## Set selinux = `permissive`

Edit file `/etc/selinux/config` ganti `SELINUX=permissive` menjadi seperti berikut:

```config
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=permissive
# SELINUXTYPE= can take one of these three values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected. 
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
```

Kemudian restart / reboot servernya, setelah itu baru install dependency

## Install Dependency

Sebelum kita install docker-ce package kita install dulu dependencynya seperti berikut:

```bash
dnf install dnf-utils device-mapper-persistent-data lvm2 fuse-overlayfs wget
```

## Add docker-ce repository for centos

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

## Install Docker CE

Setelah itu kita [Downloads](https://download.docker.com/linux/centos/7/x86_64/stable/Packages/) dan Install dulu package `containerd.io` dengan perintah berikut:

```bash
dnf install containerd.io-1.2.13-3.2.el7.x86_64
```

Setelah terinstall baru kita, install package `docker-ce`

```bash
dnf install docker-ce docker-ce-cli
```

## (Optional) expose dockerd via http

Kita edit file `/lib/systemd/system/docker.service` tambahkan host `tcp://0.0.0.0:2375` seperti berikut:

```conf
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
BindsTo=containerd.service
After=network-online.target firewalld.service containerd.service
Wants=network-online.target
Requires=docker.socket

[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2375  --containerd=/run/containerd/containerd.sock
ExecReload=/bin/kill -s HUP $MAINPID
TimeoutSec=0
RestartSec=2
Restart=always

# Note that StartLimit* options were moved from "Service" to "Unit" in systemd 229.
# Both the old, and new location are accepted by systemd 229 and up, so using the old location
# to make them work for either version of systemd.
StartLimitBurst=3

# Note that StartLimitInterval was renamed to StartLimitIntervalSec in systemd 230.
# Both the old, and new name are accepted by systemd 230 and up, so using the old name to make
# this option work for either version of systemd.
StartLimitInterval=60s

# Having non-zero Limit*s causes performance problems due to accounting overhead
# in the kernel. We recommend using cgroups to do container-local accounting.
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity

# Comment TasksMax if your systemd version does not support it.
# Only systemd 226 and above support this option.
TasksMax=infinity

# set delegate yes so that systemd does not reset the cgroups of docker containers
Delegate=yes

# kill only the docker process, not all processes in the cgroup
KillMode=process

[Install]
WantedBy=multi-user.target
```

## (Optional) add insecure registry

Untuk menambahkan insecure registry kita buat file / edit pada `/etc/docker/daemon.json` seperti berikut:

```json
{
	"insecure-registries": [
		"your.private.registry:8086",
		"your.private.registry.com:8087"
	],
	"debug": true,
	"experimental": false
}
```

## Start service docker

Kemudian kita jalankan service dockernya dengan perintah seperti berikut:

```bash
systemctl enable --now docker
```

Ok di tahap ini install docker udah selesai, sekarang kita setting supaya `DNS (Domain Names Server)` bisa dikenali routenya dengan cara disable firewald atau  dengan cara berikut:

```bash
# Allows container to container communication, the solution to the problem
firewall-cmd --zone=public --add-masquerade --permanent

# Allow port 2375 expose to outside network
firewall-cmd --zone=public --add-port=2375/tcp --permanent

# reload the firewall
firewall-cmd --reload
```