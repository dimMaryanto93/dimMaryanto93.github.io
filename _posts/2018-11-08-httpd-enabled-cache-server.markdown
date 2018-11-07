---
layout: post
title: "Enabled Control Browser Caching on httpd"
date: 2018-11-08T00:13:24+07:00
category: httpd
tags: 
- Apache Httpd
- Caching
author: 
references:
- https://www.liquidweb.com/kb/how-to-configure-apache-2-to-control-browser-caching/
comments: true
---

Setelah di compress dengan menggunakan gzip atau deflate tidaklah cukup, terkadang kita harus mikir jika di suatu daerah terpencil yang jaringannya bisa dibilang 'busuk' atau loading mulu ini bisa kena imbas. Jadi kita harus membuat Control Browser Caching untuk menghemat brandwith, dan untuk mengurangi cunsumsi resource yang berlebihan.

<!--more-->

Untuk meng-aktifkan Control Browser Caching di httpd, kita harus cek juga module `mod_header` dan `mod_expires` dengan perintah berikut:

```bash
apachectl -M | grep header
# return headers_module (shared)

apachectl -M | grep expires
# return expires_module (shared)
```

Jika udah menghasilkan output seperti atas, berarti udah terinsall secara default. Sekarang kita buat konfigurasinya dengan membuat file baru dengan nama `mod_expires.conf` di dalam folder `/etc/httpd/conf.d/` seperti berikut:

{% gist dimMaryanto93/b23009663fcc3c143eee157f4165f5df mod_session.conf %}

Dengan configurasi tersebut untuk file type image akan diberikan session selama waktu tertentu karena klo gambar khan gak pernah berubah jadi buat apa load ulang resourcenya. klo mau ditambahin seperti type file lain juga bisa tpi tidak disarankan seperti contohnya js.