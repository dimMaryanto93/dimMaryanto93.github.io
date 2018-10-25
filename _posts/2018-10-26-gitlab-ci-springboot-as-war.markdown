---
layout: post
title: "Gitlab CI/CD for Springboot deploy as war to Tomcat"
date: 2018-10-26T03:57:42+07:00
category: gitlab
tags: 
- Git
- Gitlab
- CI/CD
- Springboot
- war
- Tomcat
author: Dimas Maryanto
references:
- https://start.spring.io/
- https://spring.io/guides/gs/convert-jar-to-war/
- https://www.baeldung.com/spring-boot-war-tomcat-deploy
comments: true
---

![gitlab as war]({{site.baseurl}}/assets/img/posts/gitlab-springboot-as-war/tomcat-springboot.png)

Setelah sebelumnya kita telah, membuat deploy automated dengan gitlab CI/CD untuk process build, Sekarang kita akan membuat aplikasi dengan template spring-boot v2 yang di deploy secara tradisional yaitu dengan `war`. Karena proses deployment dengan teknik ini masih relevant dengan project yang kami jalani sekarang.

So, langung ja kita pertama harus siapkan yaitu seperti berikut:

- Web Server, such as Tomcat, Jboss EAP, Jetty dan lain-lain
- Project springboot
- Gitlab repository
- Gitlab CI

<!--more-->

## Setup

- How to [Install tomcat8 on ubuntu server]({% post_url 2018-10-26-install-tomcat8-ubuntu %})
- Create project springboot from scratch, to generate project spring [click here](https://start.spring.io/)
- Create new repository, with name `gitlab-ci-springboot-as-war` and set level `public`
- Adjust project spring-boot to tradisional deployment
- Push to repository
- Create `.gitlab-ci.yml` file to deploy `war` application into Tomcat8

## Create project springboot

Create project from [starter template spring](https://start.spring.io/) seperti berikut:

![springboot starter web]({{site.baseurl}}/assets/img/posts/gitlab-springboot-as-war/spring-boot-new-project.png)

Berikut propertiesnya:

- GroupId: `com.maryanto.dimas.example`
- ArtifactId: `springboot-as-war`
- Dependencies: 
    - Web
    - Lombok
    - Actuator
    - DevTools

## Create repository

![gitlab new repository]({{site.baseurl}}/assets/img/posts/gitlab-springboot-as-war/gitlab-repository-new.png)

Create new repository on gitlab, dengan properties seperti berikut:

- Project name: `gitlab-ci-springboot-as-war`
- Project slug: `gitlab-ci-springboot-as-war`
- Visible Level : `Public`

## Init project & first commit

Untuk pertama kita init project dulu di project `springboot-as-war` dengan perintah git seperti berikut:

```bash
## initial git project
git init

## inital commit
git add . && git commit -m "init project springboot"

## register remote repository
git remote add origin http://192.168.88.253/dimasm93/gitlab-ci-springboot-war.git

## push to remote master branch
git push -u origin master
```

## Springboot as war

Secara defautl springboot akan menggunakan package sebagai `.jar`, sekarang saya mau menjadikannya jadi `.war` dan dapat di running di Tomcat8 dengan cara berikut:

- Edit file `pom.xml` seperti berikut:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>

        <!-- replace this to war <packaging>jar</packaging> --> 
        <packaging>war</packaging>

        <dependencies>
            <!-- add dependency -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
                <scope>provided</scope>
            </dependency>
            <!-- other dependecies -->
        </dependencies>
    </project>
    ```

- Edit file main class `SpringbootAsWarApplication.java` seperti berikut:

```java
package com.maryanto.dimas.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;

@SpringBootApplication
public class SpringbootAsWarApplication extends SpringBootServletInitializer {

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(SpringbootAsWarApplication.class);
    }

    public static void main(String[] args) {
        SpringApplication.run(SpringbootAsWarApplication.class, args);
    }
}
```

Setelah itu commit dan push kembali

## Build as war

Untuk membuat build project manjadi `war` file kita coba dulu yaitu dengan menggunakan perintah berikut:

```bash
mvn clean package
```

Maka file `.war` akan terbuat di dalam folder `target/`

## Create `.gitlab-ci.yml`

Setelah file `.war` terbuat maka tahap salanjutnya adalah kita bisa membuat deployment scriptnya dengan manbahkan file `.gitlab-ci.yml` di root project sama seperti pada posting sebelumnya, berikut isi filenya:

```yml
stages:
    - build
    - deploy

variables:
  MAVEN_CLI_OPTS: "-DskipTests"

cache:
  paths:
    - .m2/repository

packaging-localhost:
  stage: build
  script:
    - 'mvn $MAVEN_CLI_OPTS package'
  artifacts:
    paths:
    - target/*.war
  only:
    - master

deploy-localhost:
  # Use stage test here, so the pages job may later pickup the created site.
  stage: deploy
  script:
    - 'rm -rf /var/lib/tomcat8/webapps/spring-war*'
    - 'cp target/spring-war.war /var/lib/tomcat8/webapps/spring-war.war'
  only:
    - master
```

Kemudian kita daftarkan lagi `gitlab-runner` untuk project `gitlab-ci-springboot-as-war` seperti berikut:

![gitlab runner register]({{site.baseurl}}/assets/img/posts/gitlab-springboot-as-war/gitlab-runner-register.png)

Sekarang kita check jobs listnya, klo udah centang ijo brati semuanya aman dan bisa di check di jbossnya online:

![gitlab jobs]({{site.baseurl}}/assets/img/posts/gitlab-springboot-as-war/gitlab-runner-jobs-list.png)

Berikut ini management tomcatnya:

![tomcat management application list]({{site.baseurl}}/assets/img/posts/gitlab-springboot-as-war/tomcat-management-list.png)

Dan berikut adalah webapp

![springboot actuator]({{site.baseurl}}/assets/img/posts/gitlab-springboot-as-war/spring-boot-actuator.png)
