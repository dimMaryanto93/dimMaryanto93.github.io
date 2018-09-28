---
layout: post
title: "Redundant array of independent disks (RAID-0) on a Solid State Drives"
category: system
tags: 
- RAID-0
- SSD
- Samsung 750 SSD EVO
author: Dimas Maryanto
references:
- https://en.wikipedia.org/wiki/RAID
- https://en.wikipedia.org/wiki/Standard_RAID_levels
comments: true
---

Teknologi RAID, atau Redudant array of Independent Disk bukan hal yang baru. Teknologi ini biasanya digunakan untuk yang memiliki Storage lebih dari 1 yang kemudian ingin digabungkan menjadi satu volume atau lebih. Dalam kasus ini saya memiliki 2 buah main Storage Samsung 750 EVO + Samsung 850 EVO yang keduannya memiliki size 250gb. 

![samsung ssd box]({{site.baseurl}}/assets/img/posts/ssd-raid-0/samsung-box.jpeg)

Dulu saya menggunakan Operation System yang beda, yaitu Linux Ubuntu Server dan Windows 10 yang di install di masing-masing SSD. Sekarang berniat hanya **satu sistem operasi** aja yaitu **Linux Ubuntu server** dengan tujuan Jadi **Web Server, Repository Host, Coding, Data Center**. Nah dari pada sdd yang satu lagi ngagur dan klo di pikir-pikir ukuran ssd hanya 250gb gak akan cukup untuk kebutuhan coding dan belum lagi harus store data di database. Yaudah saya buatlah configure RAID-0 untuk 2 SSD tersebut.

<!--more-->

## Hasil branchmark

![gparted volumes]({{site.baseurl}}/assets/img/posts/ssd-raid-0/gparted-partision.png){: width="500px" }

Partisinya berada di `/dev/mapper/isw_dbaaeehadj_Volume1` gabungan antara `/dev/sda` dan `/dev/sdb`. Berikut hasil branchmarknya menggunakan `hdparam`

![branchmark disk via terminal]({{site.baseurl}}/assets/img/posts/ssd-raid-0/branchmark-disk.png){: width="500px" }

Dengan configurasi RAID-0 kita dikasih performa yang luarbiasa kenceng. Tapi ada kelemahanya klo terjadi crash pada salah satu drive maka semua drive pada volume tersebut tidak akan bisa 

