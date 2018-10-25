---
layout: post
title: "Gitlab CI/CD for Springboot deploy as war to Tomcat"
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

