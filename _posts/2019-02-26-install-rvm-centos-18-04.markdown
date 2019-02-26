---
layout: post
title: "Install RVM di Centos 18.04"
date: 2019-02-26T12:57:17+07:00
category: rvm
tags: 
- Ruby
- RVM
- CentOS
author: Dimas Maryanto
references:
- https://rvm.io
- https://askubuntu.com/a/330115
comments: true
---

![logo]({{site.baseurl}}/assets/img/posts/install-rvm-centos-18-04/logo.png)

Berikut ada perintah untuk menginstall rvn & ruby di CentOS 18.04

```bash
# add repository & import gpg2 run as root
curl -sSL https://rvm.io/mpapis.asc | gpg2 --import -
curl -sSL https://rvm.io/pkuczynski.asc | gpg2 --import -
```

<!--more-->

Now, install rvn with this script:

```bash
## instal rvn
curl -L get.rvm.io | bash -s stable

## then 
source /etc/profile.d/rvm.sh

## finising
rvm reload
```

Setelah itu, kita check depencencynya dengan perintah berikut:

```bash
rvm requirements run
```

Kemudian kita install versi ruby yang diiginkan contohnya versin `2.6.0` dengan perintah berikut:

```bash
# checking ruby version available
rvm list known

# installing ruby version 2.6.0
rvn install 2.6

# set default ruby
rvn use 2.6 --default
```