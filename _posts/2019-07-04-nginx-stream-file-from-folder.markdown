---
layout: post
title: "Streaming video/images from local volume with NGINX"
date: 2019-07-04T12:52:36+07:00
category: nginx
tags: 
- Streaming
- Video
- Image
- Nginx
author: Dimas Maryanto 
references:
- https://docs.nginx.com/nginx/admin-guide/web-server/serving-static-content/
comments: true
---

Aplikasi streaming memang banyak, mulai dari fitur yang lengkap seperti 
- [Plex Streaming Server]({% post_url 2019-02-28-plex-media-streaming %}), 
- [Wowza Streaming Engine](https://www.wowza.com/products/streaming-engine)
- dan masih banyak lagi yang lainnya

Kita juga bisa membuat media streaming server sendiri dengan menggunakan NGINX, yang file media (image / video) di simpan di local storage (local volume). Berikut ada cara setupnya:

<!--more-->

Sebelumnya kita install dulu nginx di server kita, misalnya kita menggunakan ubuntu berikut ada installnya:

```bash
apt-get install nginx
```

Dan proses install selesai, tahap selajutnya adalah meng-configurasi nginx supaya kita bisa streaming video/image dari web server nginx.

## Set user nginx as root

Kita harus set nginx user as root, karena supaya bisa di akses dan di baca oleh nginx, dengan mengubah configurasi pada file `/etc/nginx/nginx.conf` seperti berikut:

```conf
user root;

http {
    ## enable to send a file to nginx
	sendfile on;
	tcp_nopush on;
	tcp_nodelay on;
	keepalive_timeout 65;
}
```


## Setup local volume to bind

Kita setup lokasi mounting point pada storage server kita ke nginx, dengan mengedit file pada `/etc/nginx/conf.d/default.conf` seperti berikut:


```conf
server {
    listen 80;
    server_name localhost;

    sendfile        	on;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    /streaming/ {
        sendfile on;
        sendfile_max_chunk 20m;
        autoindex on;
        autoindex_exact_size off;
        # lokasi mounting point pada storage kita
        alias /home/vsftpd/admin/;
    }

}
```

Nah sekarang klo kita coba nanti hasilnya akan seperti berikut:

## File on local volume

Berikut adalah daftar file yang saya sediakan di local volumn

![local store volume]({{site.baseurl}}/assets/img/posts/nginx-streaming-from-local-volumn/list-files.png)

## Streaming nginx

Berikut adalah hasil streaming dengan mengakses url [http://localhost/streaming/file-name.mp4](http://localhost/streaming/c4a9c212-0165-4002-8ce9-2c87abc56a40.mp4)

![streaming video]({{site.baseurl}}/assets/img/posts/nginx-streaming-from-local-volumn/streaming-nginx.png)


