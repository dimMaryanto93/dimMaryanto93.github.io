---
layout: post
title: "You should be, after fresh install Ubuntu 18.04 LST"
category: ubuntu-18.04
tags: 
- Linux
- Ubuntu
- Server
author: Dimas Maryanto
references:
- 
comments: true
---

![logo ubuntu server]({{site.baseurl}}/assets/img/posts/ubuntu-server-18.04/images.jpeg)

Halo, kali ini saya mau membahas hal apa aja yang harus dilakukan setelah install UBuntu Server 18.04 LTS adapun seperti berikut:

<!--more-->

## Update software

Pertama temen-temen yang pasti harus update dulu software system os, perintahnya seperti berikut:

{% highlight bash %}
sudo apt-get update && apt-get upgrade -y
{% endhighlight %}

## Tools Tambahan

Kemudian berikut adalah tools tambahan yang perlu di install, seperti berikut:

{% highlight bash %}
apt-get install unace unrar zip unzip \
p7zip-full p7zip-rar sharutils rar \
uudeview mpack arj cabextract file-roller \
ubuntu-restricted-extras ubuntu-restricted-addons \
openssh-server vim tmux curl wget openjdk-8-jdk openjdk-8-jre
{% endhighlight %}


