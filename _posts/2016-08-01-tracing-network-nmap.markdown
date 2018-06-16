---
layout: post
title: "Tracing network dengan nmap"
date: 2016-08-01T08:14:37+07:00
category: Network
tags: 
- Network
- TCP/IP
author: Dimas Maryanto
---

Halo, kali ini saya mau membahas software untuk men-tracing networking seperti contohnya mengetahui port, ip-address, mac-address yang terhubung dalam satu network atau lebih yaitu dengan menggunakan program `nmap`

<!--more-->

Pertama kita harus install dulu program `nmap`,

## Install nmap di Ubuntu

Berikut adalah cara install `nmap` di ubuntu

{% highlight bash %}
sudo apt-get install nmap
{% endhighlight %}

## Install nmap di Mac OS

Berikut adalah cara install `nmap` di mac os dengan brew

{% highlight bash %}
brew install nmap
{% endhighlight %}

## Menggunakan nmap

Berikut adalah cara untuk melihat ip-address yang aktif dari range `192.168.1.0` s/d `192.168.1.254`

{% highlight bash %}
sudo nmap -sP 192.168.1.*
{% endhighlight %}

Berikut hasilnya

```bash
MAC Address: 30:B5:C2:F6:D3:74 (Tp-link Technologies)
Nmap scan report for 192.168.1.101
Host is up.
Nmap scan report for 192.168.1.106
Host is up.
Nmap done: 256 IP addresses (2 hosts up) scanned in 55.29 seconds
root@Aspire-E5:~# exit
```


