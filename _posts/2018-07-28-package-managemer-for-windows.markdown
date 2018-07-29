---
layout: post
title: "Package Manager for Windows like an Linux Users"
date: 2018-07-28T18:48:28-07:00
category: windows10
tags: 
- package-manager
- windows10
- chocolatey
author: Dimas Maryanto
references:
- https://chocolatey.org/
- https://chocolatey.org/packages
comments: true
---

![Windows 10]({{site.baseurl}}/assets/img/posts/package-managemer-choco/windows10.jpg){: width="400px"}

Windows 10, hampir semua orang menggunakan Sistem Operasi ini. Kredibelitas sistem operasi besutan Microsoft ini emang gak bisa di ragunakan lagi dari segi software-software yang beredar di internet. Karena terlalu banyak tersebutlah pengguna seperti saya merasa ada yang kurang atau terlalu bertele-tele karena proses installasi software kurang praktis dan terlalu banyak embel-embel **Next->Next->Next->Finish**

Jika anda seperti saya, yang terbiasa dengan sistem package manager di Sistem Operasi berbasi Unix seperti Mac OS dan Linux contohnya jika di Mac Os ada package manager namanya `brew`, `port` dan lain-lain sedangkan jika di linux ada package manager yang namanya `apt` berbasisi debian, `dnf` atau `yum` berbasis redhad dan sebagainya. Nah klo di Sistem operasi Windows ada yang namanya [Chocolatey](https://chocolatey.org/)

![choco package manager]({{site.baseurl}}/assets/img/posts/package-managemer-choco/choco.svg){: width="200px"}

<!--more-->

Kalau temen-teman mau menggunakan kita harus install dulu softwarenya di command promnt atau Powershell. Cara installnya pertama temen-temen harus **Run Command Promnt as Adminstrator** kemudian copy-paste script berikut:

```bash
@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```

Setelah binary `choco` terinstall di command promnt temen-temen bisa menggunakanya sekarang. Contohnya klo kita mau install beberapa software basic kebutuhan sehari berikut ada contohnya

```bash
choco install vlc 
choco install adobereader 
choco install flashplayerplugin 
choco install googlechrome 
choco install firefox 
choco install 7zip 
choco install winrar 
choco install ccleaner 
choco install inkscape 
choco install filezilla 
choco install curl 
choco install wget 
choco install teamviewer 
choco install gimp 
choco install winscp 
choco install thunderbird 
choco install virtualbox 
choco install itunes 
choco install dropbox 
choco install vim 
choco install keepass
```

Dan tara, aplikasi telah terinstall jadi kita tidak perlu mengunjugin satu-per-satu website app untuk mendownload exe, kemudian menginstall satu-satu. Dengan chocolatey ini akan lebih mudah. Selain itu juga ada ribuan aplikasi yang bisa di install [berikut ini listnya](https://chocolatey.org/packages)