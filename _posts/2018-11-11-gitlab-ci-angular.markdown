---
layout: post
title: "Gitlab CI, for deploy Angular CLI template project"
date: 2018-11-11T19:58:55+07:00
category: gitlab
tags: 
- Gitlab
- CI/CD
- Frontend
- Angular
- angular-cli
author: Dimas Maryanto
references:
- 
comments: true
---

Sebelumnya kita mulai Ada beberapa yang harus kita siapakan yaitu diantaranya:

- [Install apache2 on ubuntu]({% post_url 2018-11-01-install-apache2-ubuntu %})
- [Install nodejs & angular-cli]({% post_url 2018-11-01-install-nodejs-ubuntu %})
- [Generate project, setup apache2 documentRoot]({% post_url 2018-11-02-deploy-angular-to-apache2 %})
- Create repository for angular project
- Create file `.gitlab-ci.yml`
- Push to repository

<!--more-->

Sama seperti proses sebelumnya, kita buat repository terlebih dahulu seperti berikut:

![create repository]({{site.baseurl}}/assets/img/posts/gitlab-angular/create-repo.png)

Setelah itu kita buat project angular dengan `angular-cli` dengan perinta seperti berikut:

```bash
ng new gitlab-ci-angular
```

Sekarang kita buat file `.gitlab-ci.yml` seperti berikut:

```yml
variables:
    SITE_URL: ""
    APP_NAME: "gitlab-ci-angular"

stages:
    - build
    - deploy

cache:
    paths:
        - node_modules/

build_localhost:
    stage: build
    before_script:
        - 'npm install'
    script:
        - ng build --prod  --aot
    artifacts:
        paths:
            - dist/$APP_NAME/*
    only:
        - master

deploy_localhost:
    stage: deploy
    script:
        - 'rm -rf /var/www/html/*'
        - 'cp -r dist/$APP_NAME/* /var/www/html/'
    only:
        - master
```

Sekarang kita push ke repository, nah sekarang kita check scriptnya apakah udah valid. jika sudah maka akan tampil seperti berikut:

![valid gitlab-ci]({{site.baseurl}}/assets/img/posts/gitlab-angular/valid-gitlab-ci-yml.png)

Sekarang kita register `gitlab-runner` di server seperti berikut:

![register gitlab-runner]({{site.baseurl}}/assets/img/posts/gitlab-angular/register-runner.png)

Kemudian kita check pipelinenya seperti berikut:

![pipeline]({{site.baseurl}}/assets/img/posts/gitlab-angular/gitlab-ci-pipeline.png)

Dan berikut jobs-nya:

![jobs]({{site.baseurl}}/assets/img/posts/gitlab-angular/gitlab-ci-jobs.png)

Berikut console untuk job `building`:

![job building]({{site.baseurl}}/assets/img/posts/gitlab-angular/gitlab-job-building.png)

Berikut console untuk job `deploy`:

![job deploy]({{site.baseurl}}/assets/img/posts/gitlab-angular/gitlab-job-deploy.png)

Nah sekarang kita check webnya udah online:

![angular cli deployed]({{site.baseurl}}/assets/img/posts/deploy-angular-apache2/angular-deployed-apache2.png)


