---
layout: post
title: "Install nodejs & npm di Ubuntu 18.04"
date: 2018-11-01T22:51:04+07:00
category: ubuntu-18.04
tags: 
- Ubuntu
- NodeJs
- NPM
author: Dimas Maryanto 
references:
comments: true
---

![nodejs & npm]({{site.baseurl}}/assets/img/posts/install-nodejs-npm-ubuntu/logo.png)

Aplikasi frontend di PT. Tabeldata, menggunakan Angular CLI Based dan menggunakan package management seperti NPM. Nah jadi untuk menggunakan package management dengan NPM jadi kita harus install dulu NodeJS. Dengan mengginstal NodeJs kita akan diinstallkan juga package manager npm secara default.

<!--more-->

Untuk menginstallnya cukup mudah yaitu kita bisa menggunakan package-manager yang disediakan dari official website [NodeJs](https://nodejs.org/en/download/package-manager/) seperti berikut:

```bash
# Using Ubuntu
curl -sL https://deb.nodesource.com/setup_11.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Sekarang kita tinggal check ja dengan perintah berikut apakah `nodejs` dan `npm` telah terinstall:

```bash
# check nodejs
nodejs -v

# check npm
npm -v
```

## Install Typescript & Angular CLI

Untuk menggunakan angular-cli kita harus install dulu typescript dari npm seperti berikut:

```bash
sudo npm install -g typescript lite-server
```

Nah sekarang kita check lagi apakah `typescript` udah terinstall dengan perintah seperti berikut:

```bash
# check typescript compiler
tsc -v
```

Setelah itu kita bisa install angular-cli dengan perintah seperti berikut:

```bash
sudo npm install -g @angular/cli
```

Yang terakhir kita check lagi apakah `@angular/cli` udah terinstall dengan perintah seperti berikut:

```bash
ng --version
```