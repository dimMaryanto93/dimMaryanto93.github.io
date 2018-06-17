---
layout: post
title: "You should be, after fresh install Ubuntu 16.04 LST"
date: 2016-08-01T07:57:15+07:00
category: ubuntu-16.04
tags:
- Linux
- Ubuntu 
author: Dimas Maryanto
references:
comments: true
---

![logo ubuntu 17.04 lts]({{site.baseurl}}/assets/img/posts/after-install-ubuntu-17.04/logo.png)

Halo, kali ini saya mau membahas hal apa aja yang harus dilakukan setelah install UBuntu 16.04 LTS adapun seperti berikut:

<!--more-->

{% highlight bash %}
sudo apt-get update && apt-get upgrade -y
{% endhighlight %}

## Driver tambahan

install driver yang dibutuhkan seperti VGA, Broadcom dan lain-lain.

![Driver]({{ site.baseurl }}/assets/img/posts/after-install-ubuntu-17.04/driver-ubuntu.png)

## synaptic

Install ```synaptic``` package manager, untuk mengelola aplikasi yang broken atau mencari software fungsinya sih sama kayak Ubuntu Software.

{% highlight bash %}
sudo apt-get install synaptic -y
{% endhighlight %}

![synaptic package manager]({{ site.baseurl }}/assets/img/posts/after-install-ubuntu-17.04/synaptic.png)

## vlc

Install ```vlc``` untuk memutar video

{% highlight bash %}
sudo apt-get install vlc -y
{% endhighlight %}

![vlc]({{ site.baseurl }}/assets/img/posts/after-install-ubuntu-17.04/vlc.png)

## gimp

Install ```gimp``` sebagai pengganti Photoshop

{% highlight bash %}
sudo apt-get install gimp gimp-data gimp-plugin-registry gimp-data-extra -y
{% endhighlight %}

![gimp]({{ site.baseurl }}/assets/img/posts/after-install-ubuntu-17.04/gimp.png)

## Inkscape

Install ```inkscape``` sebagai Corel Draw

{% highlight bash %}
sudo apt-get install inkscape -y
{% endhighlight %}

![inkscape]({{ site.baseurl }}/assets/img/posts/after-install-ubuntu-17.04/inkscape.png)

## BleachBit

Install ```bleachbit```, sebagai pengganti CCleaner di Windows

{% highlight bash %}
sudo apt-get install bleachbit -y
{% endhighlight %}

![bleachbit]({{ site.baseurl }}/assets/img/posts/after-install-ubuntu-17.04/bleachbit.png)

## Flash Plugins

Install flashplugin-installer, plugin untuk memutar video Youtube, dailymotion dll.

{% highlight bash %}
sudo apt-get install flashplugin-installer -y
{% endhighlight %}

## DropBox

Install ```dropbox``` tempat penyimpanan online

{% highlight bash %}
sudo apt-get install nautilus-dropbox -y
{% endhighlight %}

setelah download dan install selesai jalankan perintah berikut:

{% highlight bash %}
nautilus --quit
{% endhighlight %}

kemudian jalankan ```dropbox``` dari launcher maka akan tampil seperti berikut:

![download dropbox]({{ site.baseurl }}/assets/img/posts/after-install-ubuntu-17.04/dropbox-install-1.png)

Lalu setelah selesai maka tampil form:

![Login dropbox]({{ site.baseurl }}/assets/img/posts/after-install-ubuntu-17.04/dropbox.png)

## Blender

Install ```blender``` sebagai pengganti 3D Modeling

{% highlight bash %}
apt-get install blender -y
{% endhighlight %}

![blender]({{ site.baseurl }}/assets/img/posts/after-install-ubuntu-17.04/blender.png)

## Media Codec

Install ```codec``` untuk mutar mp3, video mp4 dll.

{% highlight bash %}
apt-get install ubuntu-restricted-extras ubuntu-restricted-addons
{% endhighlight %}

## Simple Screen Recorder

Install ```simplescreenrecorder``` untuk merekam video di desktop.

{% highlight bash %}
sudo add-apt-repository ppa:maarten-baert/simplescreenrecorder
sudo apt-get update
sudo apt-get install simplescreenrecorder
{% endhighlight %}

![ssr]({{ site.baseurl }}/assets/img/posts/after-install-ubuntu-17.04/ssr.png)

## Unity Tweak Tool

Install ```unity-tweak-tool``` untuk mengganti theme, icon dll.

{% highlight bash %}
apt-get install unity-tweak-tool -y
{% endhighlight %}

![unity tweak tool]({{ site.baseurl }}/assets/img/posts/after-install-ubuntu-17.04/unity-twak-tool.png)

## File Compressed

Install file compressed seperti zip, rar dll.

{% highlight bash %}
apt-get install unace unrar zip unzip
  p7zip-full p7zip-rar sharutils rar
  uudeview mpack arj cabextract file-roller -y
{% endhighlight %}

## Brasero

Install ```brasero``` untuk membuat iso, burning dll.

{% highlight bash %}
apt-get install brasero -y
{% endhighlight %}

![brasero]({{ site.baseurl }}/assets/img/posts/after-install-ubuntu-17.04/brasero.png)

## Audacity

Install ```audacity``` untuk merekam suara atau voice recorder

{% highlight bash %}
apt-get install audacity -y
{% endhighlight %}

![audacity]({{ site.baseurl }}/assets/img/posts/after-install-ubuntu-17.04/audacity.png)

## WPS Office

Install ```wps``` sebagai pengganti Microsoft Office

silahkan download di alamat berikut: [http://wps-community.org/downloads](http://wps-community.org/downloads) karena pake Ubuntu pilih yang extensinya ```.deb``` kemudian install menggunakan perintah seperti berikut:

{% highlight bash %}
dpkg -i /home/user/Downloads/namafile.deb
{% endhighlight %}

* Word

![Writer]({{ site.baseurl }}/assets/img/posts/after-install-ubuntu-17.04/word.png)

* Powerpoint

![Persentation]({{ site.baseurl }}/assets/img/posts/after-install-ubuntu-17.04/powerpoint.png)

* Excel

![Spreadsheets]({{ site.baseurl }}/assets/img/posts/after-install-ubuntu-17.04/excel.png)

## Chromimum Browser

Install ```chromium-browser``` Sebagai pengganti Browser Google Chrome

{% highlight bash %}
apt-get -i chromium-browser
{% endhighlight %}

## Full software

Berikut adalah kumpulan software yang berguna bagi anda sebagai pengguna ubuntu:

{% gist fd44873b40d54790bd9b0c1abae16f15 %}


