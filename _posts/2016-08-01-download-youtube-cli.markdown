---
layout: post
title: "Download video youtube via Command line / Terminal"
date: 2016-08-01T08:32:20+07:00
category: other
tags:
- Download
- Youtube 
author: Dimas Maryanto
references:
comments: true
---

![dowload youtube]({{site.baseurl}}/assets/img/posts/youtube-dl/logo.png)

Download video dari youtube emang udah jadi kebisaan saya, semenjak lulus kuliah karena di youtube banyak sekali materi-materi ataupun hiburan yang bisa saya dapatkan misalnya tutorial bahasa pemograman, VLog, Music dan lain-lain. Jaman dulu sya download masih pake **Internet Download Manager (Fake serial)** tpi sekarang udah tobat **gak mau lagi pake software bajakan** :P itu juga klo download harus satu persatu, sekarang saya menemukan cara download yang lebih mudah yaitu dengan menggunakan [youtube-dl](https://rg3.github.io/youtube-dl/index.html)

<!--more-->

### youtube-dl

> youtube-dl basicly command atau perintah yang dibuat diatas bahasa pemograman python untuk dapat digunakan di semua platform seperti Windows (Command Prompt), Linux (Terminal) dan MacOS (Terminal).

### Cara install youtube-dl

Cara installnya sendiri ada banyak yaitu klo di Windows bisa menggunakan file `.exe` download [di sini](https://rg3.github.io/youtube-dl/download.html) dan pastikan sebelum mengginstal file tersebut haruslah memiliki **Python Interpreter** seperti keterangan yang ada di websitenya

Klo saya sih installnya dari `pip` karena cukup mudah caranya kita tingal download [python development environmentnya](https://www.python.org/downloads/) kemudian tingall install `pip` seperti berikut:

{% highlight bash %}
python get-pip.py
{% endhighlight %}

udah gitu tingall install **youtube-dl**

{% highlight bash %}
pip install --upgrade youtube-dl
{% endhighlight %}

### Cara menggunakan youtube-dl

Cara menggunakannya cukup mudah tingall masukan perintah seperti berikut:

{% highlight bash %}
youtube-dl url-video-youtube
{% endhighlight %}

Kita tingall masukin link atau source dari youtube seperti berikut

![cpy]({{site.baseurl}}/assets/img/posts/youtube-dl/url.png)

kita copy-paste ke **Command prompt** atau **Terminal**

![paste]({{site.baseurl}}/assets/img/posts/youtube-dl/downloaded.png)

Sekarang tinggal cek deh folder Downloads. hasilnya seperti berikut secara otomatis dia akan mendownload max video res rata2 sih 720p. seperti berikut:

![img]({{site.baseurl}}/assets/img/posts/youtube-dl/file.png)

### Download semua video dari playlist.

Terkadang kita butuh download video semuanya dan langsung diantrikan berdasarkan playlist yang dibuat oleh uploader atau bahasa youtubenya yaitu youtubers. caranya kurang lebih sama yaitu kita copy-paste link playlistnya seperti berikut:

![url-playlist]({{site.baseurl}}/assets/img/posts/youtube-dl/url-playlist.png)

Kita paste ke terminal atau command prompt seperti berikut:

{% highlight bash %}
youtube-dl https://www.youtube.com/playlist?list=PLV1-tdmPblvxrO0uAWlite9Z5xnx9CVFR
{% endhighlight %}

hasilnya seperti berikut:

![my protopolio]({{site.baseurl}}/assets/img/posts/youtube-dl/download-playlist.png)

Sekarang kita tinggal cek aja dalam folder `Download/myPortopolio/`

![multi files]({{site.baseurl}}/assets/img/posts/youtube-dl/multi-files.png)

Ok mungkin sekian dulu apa yang bisa saya share tentang bagaimana cara mudah download video-video dari youtube. See you next post!.
