---
layout: post
title: "nvm -> Node Version Management"
date: 2019-07-14T22:49:31+07:00
category: nodejs
tags: 
- javascript
- nodejs
- nvm
author: 
references:
- https://nodejs.org/en/download/package-manager/#nvm
- https://github.com/nvm-sh/nvm
comments: true
---

![node version management]({{site.baseurl}}/assets/img/posts/nvm-node-version-management/logo.jpg)

Rada ngebelin emang klo teknologi frontend menggunakan nodejs, why???? ya karena perkembangannya cepet banget baru sebulan s/d dua bulan eh udah harus ganti / upgrade node version dan klo versinya udah ke yang major version yang ngebelin kadang suka nge-breakdown / eliminate code yang udah existing.

<!--more-->

Nodejs atau package yang berbasis npm biasanya di manage sama `package.json`, itu terkadang version dari package yang kita gunakan kita di `npm install` versionnya suka naik sendiri dan sering mengebabkan build error bahkan beberapa fitur yang udah stable jadi gak sesuai harapan ada ja yang not work properly. jadi biasanya kita downgrade lagi versionnya atau kita refactor sekalian ke versi yang up to date.

Jadi untuk mengatasi solusinya kita harus install multiple node version di local kita, tpi khan klo install node misalnya node version 12 trus klo butuh node versi 10 kita harus uninstall yang versi 12 baru install lagi version 10 dan begitu pula sebaliknya khan ribet ya. nah solusinya kita bisa menggunakan nvm (Node Version Manager).

Untuk tahap installasinya bisa di lihat detailnya [di sini](https://github.com/nvm-sh/nvm)

## Install nvm dengan homebrew

```bash
brew install nvm
```

Kemudian tambahan script berikut di `.bash_profile` atau shell yang anda gunakan seperti `zsh` seperti berikut:

```env
export NVM_DIR="$HOME/.nvm"
  [ -s "/usr/local/opt/nvm/nvm.sh" ] && . "/usr/local/opt/nvm/nvm.sh"  # This loads nvm
  [ -s "/usr/local/opt/nvm/etc/bash_completion" ] && . "/usr/local/opt/nvm/etc/bash_completion"
```

dan sekarang kita tinggal test:

```bash
nvm -version
```

## Install node 12 as default nodejs

Setelah kita install nvm kita install nodejs v12 dari nvm dengan cara berikut:

```bash
nvm install 12
```

Dan kita set nodejs v12 menjadi compiler dengan menggunakan command `use` seperti berikut:

```bash
nvm use 12
```

maka hasilnya seperti berikut:

```bash
➜  ~ nvm list
       v10.16.0
       v12.6.0
default -> 12 (-> v12.6.0)
node -> stable (-> v12.6.0) (default)
stable -> 12.6 (-> v12.6.0) (default)
iojs -> N/A (default)
unstable -> N/A (default)
lts/* -> lts/dubnium (-> v10.16.0)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.16.0 (-> N/A)
lts/dubnium -> v10.16.0
➜  ~ node -v
v12.6.0
```

## N/A: version "N/A -> N/A" is not yet installed.

Jika ada output seperti berikut solusinya kita belum membuat default compilernya dengan perintah berikut:

```bash
nvm alias default node
```

## Summary

dengan menginstall nodejs version di nvm (Node Version Manager) kita dengan mudah switch version nodejs tanpa harus install-uninstall.