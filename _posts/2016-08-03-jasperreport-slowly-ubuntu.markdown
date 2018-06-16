---
layout: post
title: "Jaspersoft Studio v6.3.1 tekesan lambat di Ubuntu 16.04"
date: 2016-08-03T06:05:00+07:00
category: ubuntu-16.04
tags: 
- Jaspersoft Studio
- Eclipse
- Linux
- Ubuntu 16.04
author: Dimas Maryanto
---

![jasperreport studio]({{site.baseurl}}/assets/img/posts/jaspersoft-slow/logo.png)

Halo, setelah sekain lama udah tidak posting lagi karena kerjaan dikantor yang seabrek hehe, ok baiklah sekarang mau nulis tentang kanapa ya 
[Jaspersoft Studio v6.3.1](http://community.jaspersoft.com/project/jaspersoft-studio) di Ubuntu 16.10 kok sering hang padahal dari 
spesifikasi leptop saya lumayan mempuni seperti berikut

- RAM : 12GB
- CPU : Intel® Core™ i5-4210U CPU @ 1.70GHz × 4 
- Storage : SSD Samsung Evo 256GB
- VGA : GeForce 920M

Setelah searcing2 di google saya mendapatkan...

<!--more-->

Oh ternyata kita harus mengganti settingan theme Eclipsenya seperti ini:

buka configurasi file ```/path/TIBCO/JaspersoftStudio/Jaspersoft Studio.ini```

{% highlight ini %}
--launcher.GTK_version
2

// dan juga menambahkan memory jvm ya...

-Dosgi.requiredJavaVersion=1.8

// ya secukupnya aja sesuai dengan spec komputer
// contohnya

-XX:MaxPermSize=512M
{% endhighlight %}

Berikut file konfigurasi yang saya gunakan:

{% gist 84d0a6b3e64cba87426140c5ee9219ed jaspersoft-studio.ini %}

Hasilnya wala... siap buat koding lagi, sekarang udah normal dan berasa ngebut lagi.