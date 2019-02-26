---
layout: post
title: "CentOS Server for My Configuration"
category: server
tags: 
- CentOS
- Servers
- Install
author: Dimas Maryanto
references:
- https://about.gitlab.com/install/#centos-7?version=ce
comments: true
---

![logo]({{site.baseurl}}/assets/img/posts/centos-server-18-04/centos.png)

Setelah kita meng-setup CentOS 18.04, berikut adalah package yang perlu kita install sebagai fondasi awal dengan perintah sebagai berikut:

<!--more-->

## Update package

Untuk pertama kalinya kita perlu update systemnya dengan perintah berikut:

```bash
yum update -y
```

## Base/Commons package

Berikut adalah package yang perlu kita install, diantaranya:

```bash
yum install wget curl gcc-c++ patch readline readline-devel zlib zlib-devel \
libyaml-devel libffi-devel openssl-devel make \
bzip2 autoconf automake libtool bison iconv-devel sqlite-devel \
ruby openssh-server firewalld vim tmux net-tools zip \
unzip file-roller sharutils -y \
&& systemctl enable firewalld.service \
&& systemctl start firewalld.service
```

## Allows firewall access for http & ssh

Berikut adalah perintah untuk memperolehkan protocol http dan ssh di akses remote

```bash
firewall --zone=public --add-port=http --permanent
firewall --zone=public --add-port=22/tcp --permanent
```

## Network configuration

Berikut adalah konfigurasi network yang saya gunakan, edit configurasion di `/etc/sysconfig/network-scripts/ifcfg-enp3s0` seperti berikut:

```conf
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="none"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="enp3s0"
UUID="8f3d7ad4-5c23-44aa-9329-402a1cd07607"
DEVICE="enp3s0"
ONBOOT="yes"
IPADDR="192.168.88.50"
PREFIX="32"
GATEWAY="192.168.88.1"
IPV6_PRIVACY="no"
DNS1="8.8.8.8"
DNS2="8.8.4.4"
ZONE=public
```

