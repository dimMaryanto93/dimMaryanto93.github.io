---
layout: post
title: "Enabled gzip | deflate compression on httpd"
date: 2018-11-07T23:44:00+07:00
category: httpd
tags: 
- Apache Httpd
- gzip
- deflate
author: Dimas Maryanto
references:
- https://httpd.apache.org/docs/2.4/mod/mod_deflate.html
comments: true
---

Ada kalanya, ketika load element resource seperti js, css, image, html karena size of file is too large maka yang terjadi adalah performa akan menurun dari segi response time. Nah di Apache httpd kita bisa compress dengan dengan menggunakan `mod_gzip` atau `mod_deflate` tpi kita harus check dulu modulenya udah terpasang belum di Apache httpd-nya. Tetapi ini disarankan **tidak untuk mesin development**.

<!--more-->

Untuk meng-check module udah terpasang / install di system kita bisa menggunakan perintah berikut:

```bash
# sample command 'httpd -M | grep module_name'

httpd -M | grep deflate
```

Jika menampilkan hasil seperti `deflate_module (Shared)` artinya udah terinstall.

Sekarang kita tambahkan file `mod_deflate.conf` dalam folder `/etc/httpd/conf.d/` seperti berikut:

{% gist dimMaryanto93/b23009663fcc3c143eee157f4165f5df mod_deflate.conf %}

Sekarang kita tinggal check, di **Browser console** -> **Networks** -> **Headers** di bagian `Accept-Encoding: gzip, deflate` seperti berikut:

![compression gzip]({{site.baseurl}}/assets/img/posts/httpd-gzip-deflate/compression-gzip.png)
