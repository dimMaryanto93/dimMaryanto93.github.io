---
layout: post
title: "Gitlab CI/CD to Automated Deployment 'like a Boss'"
category: gitlab
tags: 
- Git
- Gitlab
- CI/CD
author: Dimas Maryanto
references:
- https://en.wikipedia.org/wiki/Continuous_integration
- https://en.wikipedia.org/wiki/Continuous_delivery
comments: true
---

![gitlab flow deployment]({{site.baseurl}}/assets/img/posts/gitlab-ci-cd/gitlab-ci-instrument.png)

Sometime, proses deploy itu merepotkan bagi sebagaian orang seperti saya yang bekerja di pursahaan kecil dengan jumlah `!>= 20 orang programer` apalagi sekarang jaman2nya microservice yang aplikasinya harus build 3 - 10 project. Klo dulu enak aplikasi yang di deploy cuman 1 war buncit (Monolith) klo sekarang aplikasi udah di pecah2 jadi ya makin banyak project makin banyak juga proses deploynya (jadi makin banyak kerjaannya~ heheh).

![gitlab logo]({{site.baseurl}}/assets/img/posts/gitlab-ci-cd/gitlab-ci-logo.png)

Dari pada saya deploy aplikasinya setiap project mending saya buatkan script deployment dengan [CI/CD](https://www.digitalocean.com/community/tutorials/an-introduction-to-ci-cd-best-practices), supaya proses deployment dijalankan otomatis dan terintegrasi dengan Version Controls System (git) dan waktunya bisa untuk main atau explore technology baru. Untuk menggunakan CI/CD, kita harus menyediakan toolsnya diantarnya:

- Gitlab CE
- Gitlab Runner
- Environtment
    - OS seperti Linux, Mac or Windows, rekomendasi Linux
    - SDK seperti Java, PHP, AndroidSDK dan lain-lain
    - Build Tools seperti maven, gradle, composer, npm, gulp dan lain-lain.

<!--more-->

Ok, langsung ja kita setup-setup dulu ya. pertama kita harus siapkan dulu environmentnya seperti OS, SDK dan build tools. Karena kita biasanya untuk server menggunakan Linux yang free, maka saya pilih Ubuntu Server dan Untuk development kita biasanya kebanyakan project menggunakan Java jadi didalam OS **Ubuntu server** kita akan install juga **Oracle JDK 8** atau **OpenJDK 8** serta buildtoolsnya yang paling mudah yaitu **Apache Maven**

- How to install [Oracle JDK 8 on linux Ubuntu Server]({% post_url 2016-08-02-install-oracle-jdk8-ubuntu %})
- How to install [Apache Maven on linux Ubuntu server]({% post_url 2016-08-04-install-maven-ubuntu %})