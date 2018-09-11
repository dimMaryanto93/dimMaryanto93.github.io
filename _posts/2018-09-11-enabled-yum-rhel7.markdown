---
layout: post
title: "Enabled yum from dvd/iso rhel7 (redhat 6/7)"
date: 2018-09-11T19:12:10+07:00
category: rhel7
tags: 
- linux
- rhel
author: Dimas Maryanto 
references:
- https://linuxconfig.org/how-to-mount-cd-dvd-rom-on-rhel-7-linux
- https://access.redhat.com/solutions/1355683
- https://developers.redhat.com/products/rhel/download/
comments: true
---

![rhel7-logo]({{site.baseurl}}/assets/img/posts/rhel7-setup/redhat.jpg){:height="200px" width="200px"}

Setelah install rhel7 dengan kondisi `minimal install`, saatnya kita setup beberapa software diantaranya 

- GUI (Desktop Environment) *optional
- Kebutuhan software development seperti, Oracle JDK8 / IceTea JDK, Mysql/MariaDB, Text Editor, Software dependency management, Web Server dan masih banyak,
- Kebutuhan networking
- Dan lain-lain.

Tpi sebelum menginstall semua itu, di RedHat semua itu klo kita mau install dari repository-nya redhad maka kita harus subcribe dulu yang mungkin harganya tidak murah untuk kalangan mahasiswa yang notabene masih mau coba sistem. Jadi sekarang kita akan membahas cara menggunakan repository dari dvd/iso yang tersedia. Iso rhel7 yang terbaru bisa didownload [website resminya RedHat](https://developers.redhat.com/products/rhel/download/)

<!--more-->

Kondisi awal, redhat setelah di install dengan mode `minimal dependency` kurang lebih seperti ini.

![redhat fresh install]({{site.baseurl}}/assets/img/posts/rhel7-setup/rhel7.png)

Nah, sekarang saya mau install beberapa software tpi sebelum itu kita aktifkan dulu repository-nya dari local dvd/iso. Berikut cara mount dvd/iso

```bash
# check dulu lokasi dvd/iso yang telah di-mount
blkid
```

Berikut outputnya

```bash
/dev/xvda1: UUID="b4f59ae4-f5c8-49c2-94cf-103e10eef407" TYPE="xfs" 
/dev/xvda2: UUID="zcwKzx-w2EM-NH4E-wQW3-7QTu-62JZ-s6Ok0j" TYPE="LVM2_member" 
/dev/sr0: UUID="2016-10-19-18-32-06-00" LABEL="RHEL-7.3 Server.x86_64" TYPE="iso9660" PTTYPE="dos" 
/dev/mapper/rhel-root: UUID="ea637699-1d70-49ef-9a0a-54bc87d1e571" TYPE="xfs" 
/dev/mapper/rhel-swap: UUID="617ccf82-602c-472d-bff6-484d95530293" TYPE="swap"
```

Biasanya hasil mountnya ada di `/dev/sr0`

Sekarang kita mount ke `/media/rhel7-iso` tpi buat dulu directorynya

```bash
# create folder
mkdir -p /tmp/rhel7-iso
mkdir -p /media/rhel7-iso

# mount sr0 to /tmp/rhel7-iso
mount /dev/sr0  /tmp/rhel7-iso

# copy all files and directory to /media/rhel7-iso
cp -pRf /tmp/rhel7-iso /media/rhel7-iso

# unmount kembali /tmp/rhel7-iso
umount /tmp/rhel7-iso
```

Sekarang kita tambahkan `media.repo` yang ada di `/media/rhel7-iso` ke system `yum` repository.

```bash
cp /media/rhel7-iso/media.repo /etc/yum.repos.d/rhel7dvd.repo
chmod 644 /etc/yum.repos.d/rhel7dvd.repo
```

Kemudian kita edit file `/etc/yum.repos.d/rhel7dvd.repo` menjadi seperti berikut, gunakan editor seperti `nano` atau `vi`

```bash
[InstallMedia]
name=DVD for Red Hat Enterprise Linux 7.1 Server
mediaid=1359576196.686790
metadata_expire=-1
gpgcheck=1
cost=500
# tambahkan ini
enabled=1
baseurl=file:///media/rhel7-iso
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
```

Setelah itu clean all cache yum dengan menggunakan perintah berikut:

```bash
## clean all cache yum repository
yum clean all && rm -rf /var/cache/yum/ && subscription-manager clean

## check dvd / iso local configuration
yum --noplugins list && yum --noplugins update
```

And welcome repository dvd udah bisa digunakan, sekarang kita bisa mengginstall software dari repository seperti Desktop environtment, Network tools, dan lain-lain.