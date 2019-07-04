---
layout: post
title: "Membangun aplikasi dengan arsitektur microservice"
category: microservice
tags: 
- Java
- SpringBoot
- Microservice
author: Dimas Maryanto
references:
- https://microservices.io
comments: true
---

Arsitektur microservice ini, sedang booming. kita di PT. Tabeldata informatika telah mengimplementasikan arstektur ini di beberapa project terakhir. Jika kita lihat dari kelebihan seperti 

- arsitektur ini memang kita bisa lebih fokus dalam sisi development
- lebih cepat juga karena kita bisa memecah dari beberapa orang menjadi beberapa team karena 1 project akan di pecah menjadi beberapa module kecil. 

tetapi kita jangan liat dari segi keuntungannya aja tpi kita lihat dari segi kekurangannya juga seperti 

- bagaimana ribetnya integrasi antar modul (jika berbeda teknologi atau beda perusahaan), 
- sistem tunggu karena module yang di deploy depend ke modul lain.

<!--more-->

more content here...