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
- How to install [Gitlab-CE on linux Ubuntu Server]({% post_url 2018-10-25-self-hosting-central-repository %})
- Setelah semua requiremnt di install, tahap selanjutnya yang kita butuhkan yaitu
    - install gitlab-runner, sebagai task executor / listener ketika event tertentu seperti new commit atau new tags.
    - create repository, dan setup ci/cd (`.gitlab-ci.yml`)

## Install gitlab-runner

Setelah kita menambahkan apt-repository gitlab, `curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash` kita bisa langsung install gitlab-runner dengan perintah berikut:

```bash
apt-get install -y gitlab-runner
```

Pastikan setelah install service `gitlab-runner` telah berjalan dengan perintah:

```bash
service gitlab-runner status
```

Seperti berikut:

![gitlab-runner status]({{site.baseurl}}/assets/img/posts/gitlab-ci-cd/gitlab-runner-status.png)

## Create project, example Java Spring

Sekarang kita buat aja, sample project template yang telah disedikan oleh gitlab yaitu springboot template seperti berikut:

![springboot template new]({{site.baseurl}}/assets/img/posts/gitlab-ci-cd/springboot-template-choose.png)

Setelah itu click **Use template** dengan pilihan **Spring** maka tampilannya seperti berikut:

![springboot template]({{site.baseurl}}/assets/img/posts/gitlab-ci-cd/springboot-project-details.png)

Kemudian kita isi aja detailsnya dengan property seperti berikut:

- Project name : `spring-gitlab-ci-example`
- Project slug: `spring-gitlab-ci-example`
- Project Description (optional): `Belajar gitlab ci/cd dengan springboot v2`
- Visibilty project: `Public` 
- Kemudian click **Create project**

Setelah projectnya udah di clone oleh gitlab, dan udah jadi. Sekarang kita ke menu **Settings** -> **CI/CD** -> **Runner** seperti berikut:

![gitlab ruunner menu]({{site.baseurl}}/assets/img/posts/gitlab-ci-cd/gitlab-setting-ci-cd.png)

Di tab **Runner** kemudian **Expanse** kemudian cari `ci token` seperti berikut:

![gitlab ci token]({{site.baseurl}}/assets/img/posts/gitlab-ci-cd/gitlab-setting-runner.png)

## Register gitlab-runner

Untuk menjalankan build automated kita butuh me-register gitlab-runner ke projectnya dengan menggunakan perintah seeprti berikut:

```bash
sudo gitlab-runner register
```

Setelah itu akan muncul form seperti berikut:

![gitlab runner register new]({{site.baseurl}}/assets/img/posts/gitlab-ci-cd/gitlab-runner-register-new.png)

kemudian kita bisa check di gitlabnya dengan me-refresh halaman setting tersebut, kemudian ckeck kemali di tab **Runner** pastikan lampu indikator berwana hijau seperti berikut:

![gitlab runner registered]({{site.baseurl}}/assets/img/posts/gitlab-ci-cd/gitlab-runner-registered.png)

## Setup script deployment

Di gitlab, secara default akan meng-aktifkan auto-devops, tpi saya mau custome contohnya untuk case kali ini sya cuman mau build kemudian di package menjadi jar saja. Jadi sekarang kita buat file dengan nama `.gitlab-ci.yml` di root project dengan isi seperti berikut:

```yml
variables:
  MAVEN_CLI_OPTS: "--show-version"

# Cache downloaded dependencies and plugins between builds.
cache:
  paths:
    - .m2/repository

building:
  stage: build
  script:
    - 'mvn $MAVEN_CLI_OPTS clean package'
  only:
    - master
  artifacts:
    paths:
      - target/*.jar
```

Seperti berikut:

![file .gitlab-ci.yml location]({{site.baseurl}}/assets/img/posts/gitlab-ci-cd/gitlab-ci-yml.png)

Setelah itu **Commit** and **Push** dan setelah itu maka proses build akan berjalan, kita check di menu **CI/CD** -> **Pipeline** seperti berikut:

![pipeline running]({{site.baseurl}}/assets/img/posts/gitlab-ci-cd/gitlab-pipeline-running.png)

Selain itu juga kita bisa breakdown lebih dalem lagi proses apa yang sedang berjlaan dengan meng-click yang sedang running seperti berikut:

![job status]({{site.baseurl}}/assets/img/posts/gitlab-ci-cd/gitlab-job-status.png)

Setelah itu kita juga bisa liat consolenya yang di jalankan di gitlab-runner dengan click **Jobs** maka seperti berikut hasilnya jika success:

![job success]({{site.baseurl}}/assets/img/posts/gitlab-ci-cd/gitlab-job-success.png)

**Summery** 

- Dengan menggunakan gitlab CI/CD kita tidak perlu melakukan build jika ada perubahan coding cukup push perubahan ke branch tertentu maka gitlab ci akan otomatis build
- Effort setiap project harus di daftarkan gitlab-runner tapi hanya sekali setting aja, klo kata pribahasa bahasa Java `write ones execute everytime`