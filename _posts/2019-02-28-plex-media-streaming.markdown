---
layout: post
title: "Streaming Video, Music and more via Plex Media Server"
date: 2019-02-28T21:23:02+07:00
category: streaming
tags: 
- Entertaiment
- Streaming
- Media 
- Server
author: Dimas Maryanto
references:
- https://www.plex.tv/media-server-downloads/#plex-app
- https://www.plex.tv/media-server-downloads/#plex-media-server
comments: true
---

![Plex Media Server]({{site.baseurl}}/assets/img/posts/plex-media-server/plex-logo.jpg){: width="300px" }

Menggunakan Stream Media Server adalah solusi cerdas untuk me-manage content di berbagai perangkat yang menggunakan/memiliki storage yang minimum. Sementara kebutuhan entertaiment seperti menonton Movies, Videos, Tutorial dan lain-lain kurang terpenuhi karena faktor penyimpanan. 

<!--more-->

Dengan menggunakan Plex Media Server & Plex App kita bisa membuat centralization content management dengan bermodalkan 1 PC / NAS (Network as Storage) Server sebagai media server stream.

Pertama kita harus [download](https://www.plex.tv/media-server-downloads/#plex-media-server)/install Plex Media Server sesuai dengan Operation Sistem yang sedang digunakan.

Saya menggunakan OS Centos 7, jadi pilih yang RPM package. Kemdian install menggunakan yum dengan perintah seperti berikut:

```bash
sudo yum install plexmediaserver_xxxxx.rpm
```

Setelah di install kita jalankan dulu servicenya dengan menggunakan perintah seperti berikut:

```bash
# starting plex-media-server
systemctl start plexmediaserver.service
# enable plex-media-server start on startup
systemctl enable plexmediaserver.service

# check status
systemctl status plexmediaserver.service
```

Setelah itu coba akses halaman admin plex via browser di alamat [ip-server:32400](http://localhost:32400)

maka hasilnya seperti berikut:

![web-app]({{site.baseurl}}/assets/img/posts/plex-media-server/plex-web.png)

Setelah itu, kita Register account. Kemudian kita tambahkan Plex Media Server ke account kita seperti berikut:

![plex-media-server-registered]({{site.baseurl}}/assets/img/posts/plex-media-server/plex-media-server-registered.png)

Setelah kita register Plex Media Server ke account kita bisa tambahkan library atau media content kita seperti berikut:

![plex-media-server-libs]({{site.baseurl}}/assets/img/posts/plex-media-server/plex-add-library.png)

And lastly enjoy your movies...