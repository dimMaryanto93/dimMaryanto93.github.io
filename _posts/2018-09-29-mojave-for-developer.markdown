---
layout: post
title: "macOS Mojave for Developer"
date: 2018-09-29T21:00:18+07:00
category: mac-os
tags: 
- macOS Mojave
- Developer
- Programmer
gist: dimMaryanto93/4749c87ad654a44fde1a0916f5126343
author: Dimas Maryanto
references:
comments: true
---


Setelah sebelumnya, kita install software kebutuhan sebagai daily driver dan setting-setting seperti touchpad, security, dan mencoba fitur-fitur baru di macOS Mojave. Sekarang saatnya install untuk kebutuhan programming seperti 

- Oracle JDK 8
- MySQL Database
- PostgreSQL Database
- Docker CE
- Text editor, Jetbraint Toolbox, Visual studio code
- Git
- xcode
- homebrew
- Jekyll / octopress
- Terminal plugin oh-my-zsh + font awesome icon powerline

<!--more-->

## Git

Untuk install git, cukup mudah buka terminal kemudian ketik ja perintah berikut:

```bash
git status
```

Nanti bakalan muncul xcode-select form supaya di install, seperti berikut

![xcode select install]({{site.baseurl}}/assets/img/posts/mac-os-mojave-dev/xcode-select-install.png)

Click **Install**, setelah di install secara otomatis git nya sudah terinstall di system. Sekarang tambahkan configurasi seperti berikut:

```bash
git config --global user.name dimMaryanto93
git config --global user.email software.dimas_m@icloud.com
```

**If you want to add ssh key**

```bash
## Generate ssh key pair
ssh-keygen -t rsa -b 4096 -C "software.dimas_m@icloud.com"

## register ssh agent
eval "$(ssh-agent -s)"

## add ssh key to ssh agent
ssh-add -K ~/.ssh/id_rsa
```
<br/>

## Oracle Java JDK 8

Download file `.dmg` from [oracle website](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

- Double click `jdk-8u181-macosx-x64.dmg`, Akan muncul halaman seperti berikut:

    ![oracle jdk 8 package]({{site.baseurl}}/assets/img/posts/mac-os-mojave-dev/java-jdk8-package.png)

- Double click icon package itu,

    ![oracle jdk 8 installer]({{site.baseurl}}/assets/img/posts/mac-os-mojave-dev/java-installer.png)

- Click **Continues** untuk melanjutkan installasi

    ![oracle jdk 8 install type]({{site.baseurl}}/assets/img/posts/mac-os-mojave-dev/java-jdk8-install-type.png)

- Click **Install** untuk melakukan installasi di hardisk/ssd macbook anda:

    ![oracle install finish]({{site.baseurl}}/assets/img/posts/mac-os-mojave-dev/java-jdk8-install-finish.png)

- Click **Finish**, ok sekarang install udah selesai sekarang tinggal di check aja di terminal dengan perintah berikut:

```bash 
java -version

## Output
# java version "1.8.0_181"
# Java(TM) SE Runtime Environment (build 1.8.0_181-b13)
# Java HotSpot(TM) 64-Bit Server VM (build 25.181-b13, mixed mode)
```
<br/>

## Homebrew

```bash 
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Berikut adalah software-software yang bisa kita install dari brew package manager

```bash
brew install wget && \ 
brew install vim && \
brew install tmux && \
brew install youtube-dl && \
brew install maven && \
brew install gradle && \
brew install npm && \
brew install node && \
brew install openvpn && \
brew cask install postman && \
brew cask install virtualbox && \
brew cask install pencil
```
<br/>

## angular/cli

![angular cli]({{site.baseurl}}/assets/img/posts/mac-os-mojave-dev/angular-cli.png)

Jika anda ada developer, angular dengan angular-cli. 

```bash
npm install -g @angular/cli typescript
```
<br/>


## Jekyll

Software ini biasanya saya gunakan untuk membuat tulisan di blog. Seperti website ini dibagun di atas Octopress dan Jekyll

```bash
gem install bundler jekyll octopress
```
<br/>

## Jetbrants Toolbox

![Jetbraint toolbox]({{site.baseurl}}/assets/img/posts/mac-os-mojave-dev/jetbraint-toolbox.png){: height="400px" width="300px"}

Software jetbraint, ini adalah software yang paling sering saya gunakan bahkan setiap hari. Jetbraint toolboox ini adalah daftar produk-produknya jetbraint. Jadi kita bisa download langsung dari situ dan klo ada update / patch langusng dari situ gak perlu download manual ke websitenya. [Download disini](https://www.jetbrains.com/toolbox/)

<br/>

## Browser

Untuk browser, sebagai web developer kita harus punya semua perkakas perang yaitu web browser jadi gak updol klo gak punya semua web browser

- Safari
- Chrome, [download disini](https://www.google.com/chrome/)
- Firefox, [download disini](https://www.mozilla.org/id/firefox/new/)

<br/>

## Emulator

Untuk bermain VM saya meggunakan Oracle Virtualbox, dan untuk running android device menggunakan Genymotion.

- Genymotion personal use, [Download disini](https://www.genymotion.com/fun-zone/)

<br/>

## Docker

![Docker]({{site.baseurl}}/assets/img/posts/mac-os-mojave-dev/docker.png){: width="400px" }

Untuk urusan, development. Biasanya saya menggunakan docker sebagai main driver atau cuman coba2 teknologi baru supaya gak ribet install dan klo udah gak kepake bisa langsung di hapus containernya. [Download disini](https://store.docker.com/editions/community/docker-ce-desktop-mac)

<br/>

## Oh-my-zsh (terminal plugin)

![oh-my-zsh]({{site.baseurl}}/assets/img/posts/mac-os-mojave-dev/oh-my-zshrc.png)

Command line yang user-frindly salah satunya ini. karena banyak kemudahan ketinbang menggunakan `bash` biasa, tapi tergantung selera sih. Klo saya lebih suka menggunakan ini.

```bash
## install oh-my-zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

## change default to zsh
chsh -s /bin/zsh
```

Untuk meng-aktifikan plugin yang telah disediakan oleh zsh, kita modifikasi file `~/.zshrc` kemudian cari script `plugins=(...)` kemudian modifikasi seperti berikut contohnya: 


```conf
plugins=(
  git
  github
  pip
  python
  mvn
  brew
  osx
  docker
  docker-compose
  docker-machine
  minikube
  gradle
  kubectl
  node
  postgres
  spring
)

fpath+=($ZSH/plugins/docker)
autoload -U compinit && compinit
```


Berikut hasilnya setelah di pasang `zsh`

![terminal zsh]({{site.baseurl}}/assets/img/posts/mac-os-mojave-dev/terminal-zsh.png)

<br/>

## Bash profile

Adakalanya kita mau menabahkan execute application oleh kita sendiri, contohnya kita punya mysql tpi belum di register ke `$PATH` jadi ya harus kita tambahin sendiri, caranya kita modifikasi file `~/.bash_profile` seperti berikut:

{% gist page.gist .bash_profile %}

Nah jangan lupa karena kita pake `zsh` default configurasinya ada di `~/.zshrc` jadi kita harus import `~/.bash_profile`nya berikut sourcenya

{% gist page.gist .zshrc %}

<br/>