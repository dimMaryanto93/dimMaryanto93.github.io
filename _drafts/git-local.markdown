---
layout: post
title: "Version control system di local"
category: git
tags: 
- version control system
- git
author: Dimas Maryanto 
references:
- https://git-scm.com
comments: true
---

![git flow]({{site.baseurl}}/assets/img/posts/git-flow/flow.png)

Version control system, atau singkatan dari vcs adalah sebuah control terhadap beberapa file yang dapat diatur versi atau mungkin anda pasti tau fitur di Microsoft Office yaitu **Autosave** atau klo di dunia game balapan yaitu **Checkpoint**? 

<!--more-->

Yap sama halnya dengan istilah di balapan game-game kita bisa kembali ke **savepoint** sebelumnya jika apa yang telah kita kerjakaan terjadi corrupt atau reset ke **checkpoint** tertentu.

Ada beberapa aplikasi untuk mengimplementasikan version control system, diantarnya:

- git
- apache subversion (SVN)
- Mercurial


## Git Version Control System

Zaman now, perusaahan-perusahaan mewajibkan developer untuk memahami version control minimum menggunakan git. Git Version Control System, mengapa menggunakan git? jawabanya yaitu open-source, mudah digunakan dan banyak di implementasikan oleh perusaahan besar.

Ada beberapa Istilah yang yang harus di pahami oleh developer yaitu seperti berikut:

- `init`
- `add`
- `commit`
- `reset`
- `checkout`
- `branch`
- `tags`
- `merge`
- `head`
- `detached`
- `log`

## init

Perintah `init` adalah perintah untuk membuat project git, perintah ini biasanya hanya dilakukan satu kali saja yaitu pada saat folder yang anda inginkan menjadi project git (terdapat folder `.git` dalam root project).

printahnya: 

```bash
git init
```

## add

Perintah `add` biasanya digunakan untuk melakukan indexing terhadap file, atau istilah lainya melakukan system tracking terhadap perubahan suatu file. contoh pengunaannya adalah sebagai berikut:

```bash 
git add filename.ext
```

atau anda dapat melakukan traking filenya lebih dari satu file

```bash
git add filename1.ext filename2.ext dst..
```

atau selain itu juga anda bisa menambahkan semua filenya berubah dengan menggunakan perintah seperti berikut:

```bash
git add .
```

