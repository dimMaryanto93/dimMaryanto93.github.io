---
layout: post
title: "Gitlab CI/CD for Springboot deploy as service Systemd"
category: gitlab
tags: 
- Git
- Gitlab
- CI/CD
- Springboot
- Ubuntu
- Systemd
author: Dimas Maryanto
references:
- https://docs.spring.io/spring-boot/docs/current/reference/html/deployment-install.html
- https://www.baeldung.com/spring-boot-app-as-a-service
comments: true
---

![Systemd logo]({{site.baseurl}}/assets/img/posts/gitlab-springboot-systemd/systemd.png)

Sebelumnya kita udah membuat deploy untuk [system traditional deployment]({% post_url 2018-10-26-gitlab-ci-springboot-as-war %}) yaitu menggunakan `war` bundle, sekarang saya akan buat yang sistem deployment as service di Linux. 

Berapa yang harus kita siapkan diantaranya:

- Project spring-boot yang embeded Unix service
- Systemd script
- Gitlab repository
- Gitlab CI/CD
- User to start/stop/restart service

<!--more-->

## Create project Springboot
Ok sekarang kita buat project springbootnya dari [springboot starter](http://start.spring.io) dengan properties seperti berikut:

- GroupId: `com.maryanto.dimas.example`
- ArtifactId: `springboot-systemd`
- Dependencies: 
    - Web
    - Lombok
    - DevTools
    - Actuator

Kemudian click **Generate Project**, setelah itu kita rubah configurasi `pom.xml` menjadi seperti berikut:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <packaging>jar</packaging>
    
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.6.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <dependencies>
        <!-- dependencies -->
    </dependencies>

    <build>
        <!-- jar file name is springboot-systemctl.jar  -->
        <finalName>springboot-systemctl</finalName>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <executable>true</executable>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

Setelah itu kita harus kasih spesifikasi port yang akan digunakan dengan mengedit file `application.properties` di dalam folder `src/main/resources` seperti berikut:

```properties
server.port=1234
spring.application.name=springboot-systemctl
```

Sekarang kita siapkan dulu file `.jar` dengan menggunakan perintah seperti berikut:

```bash
mvn clean package
```

Maka file `springboot-systemctl.jar` akan terbuat di dalam folder `target/` seperti berikut hasil compilenya:

```bash
target
├── springboot-systemctl.jar
├── springboot-systemctl.jar.original
```

Ada 2 `.jar` file kita pilih aja yang `springboot-systemctl.jar` kemudian kita coba jalankan dengan perintah seperti berikut:

```bash
java -jar target/springboot-systemctl.jar

# logs
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.0.6.RELEASE)

2018-10-26 11:19:50.676  INFO 3013 --- [           main] c.m.dimas.example.GitlabCIExample        : Starting GitlabCIExample v0.0.1-SNAPSHOT on Dimass-MacBook-Pro.local with PID 3013 (/Users/dimasm93/Workspaces/examples/gitlab-ci-springboot-systemd/target/gitlab-ci.jar started by dimasm93 in /Users/dimasm93/Workspaces/examples/gitlab-ci-springboot-systemd)
2018-10-26 11:19:50.683  INFO 3013 --- [           main] c.m.dimas.example.GitlabCIExample        : No active profile set, falling back to default profiles: default
2018-10-26 11:19:50.760  INFO 3013 --- [           main] ConfigServletWebServerApplicationContext : Refreshing org.springframework.boot.web.servlet.context.AnnotationConfigServletWebServerApplicationContext@2db0f6b2: startup date [Fri Oct 26 11:19:50 WIB 2018]; root of context hierarchy
2018-10-26 11:19:52.767  INFO 3013 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 1234 (http)
2018-10-26 11:19:52.831  INFO 3013 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2018-10-26 11:19:52.831  INFO 3013 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet Engine: Apache Tomcat/8.5.34
2018-10-26 11:19:54.052  INFO 3013 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error],produces=[text/html]}" onto public org.springframework.web.servlet.ModelAndView org.springframework.boot.autoconfigure.web.servlet.error.BasicErrorController.errorHtml(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse)
2018-10-26 11:19:54.082  INFO 3013 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/webjars/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018-10-26 11:19:54.082  INFO 3013 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018-10-26 11:19:54.361  INFO 3013 --- [           main] o.s.b.a.e.web.EndpointLinksResolver      : Exposing 2 endpoint(s) beneath base path '/actuator'
2018-10-26 11:19:54.371  INFO 3013 --- [           main] s.b.a.e.w.s.WebMvcEndpointHandlerMapping : Mapped "{[/actuator/health],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}" onto public java.lang.Object org.springframework.boot.actuate.endpoint.web.servlet.AbstractWebMvcEndpointHandlerMapping$OperationHandler.handle(javax.servlet.http.HttpServletRequest,java.util.Map<java.lang.String, java.lang.String>)
2018-10-26 11:19:54.372  INFO 3013 --- [           main] s.b.a.e.w.s.WebMvcEndpointHandlerMapping : Mapped "{[/actuator/info],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}" onto public java.lang.Object org.springframework.boot.actuate.endpoint.web.servlet.AbstractWebMvcEndpointHandlerMapping$OperationHandler.handle(javax.servlet.http.HttpServletRequest,java.util.Map<java.lang.String, java.lang.String>)
2018-10-26 11:19:54.373  INFO 3013 --- [           main] s.b.a.e.w.s.WebMvcEndpointHandlerMapping : Mapped "{[/actuator],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}" onto protected java.util.Map<java.lang.String, java.util.Map<java.lang.String, org.springframework.boot.actuate.endpoint.web.Link>> org.springframework.boot.actuate.endpoint.web.servlet.WebMvcEndpointHandlerMapping.links(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse)
2018-10-26 11:19:54.431  INFO 3013 --- [           main] o.s.j.e.a.AnnotationMBeanExporter        : Registering beans for JMX exposure on startup
2018-10-26 11:19:54.504  INFO 3013 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 1234 (http) with context path ''
2018-10-26 11:19:54.508  INFO 3013 --- [           main] c.m.dimas.example.GitlabCIExample        : Started GitlabCIExample in 4.511 seconds (JVM running for 5.362)
```

## Unix users, to run systemd

Setelum kita membuat script unix systemd, kita buat user unix dulu supaya build dijalankan tanpa menggunakan password super user. Misalnya saya membuat username dengan nama `app` seperti berikut:

```bash
# interactive mode
sudo -i

# create user with username app
adduser app

# log output
Adding user 'app' ...
Adding new group 'app' (1001) ...
Adding new user 'app' (1001) with group `app' ...
Creating home directory '/home/app' ...
Copying files from '/etc/skel' ...
## enter password
New password:
## enter password again
Retype new password:
passwd: password updated successfully
```

Kemudian kita copy manual dulu, filer `springboot-systemctl.jar` ke servernya dengan perintah seperti berikut:

```bash
# create folder applications
ssh app@192.168.1.23 "mkdir -p applications"
# copy jar file to folder application
scp target/springboot-systemctl.jar app@192.168.1.23:~/applications/springboot-systemctl.jar

## log output
# scp target/gitlab-ci.jar.original app@192.168.1.23:~/applications/springboot-systemctl.jar
# app@192.168.1.23's password: 
# gitlab-ci.jar.original                        100% 3492     1.6MB/s   00:00 
```

Setelah file `springboot-systemctl.jar` siap, kita siapkan dulu script unix systemd yang disimpan di folder `/etc/systemd/system/` dengan nama `springboot-systemctl.service` seperti berikut:

```service
[Unit]
Description=springboot-systemctl
After=syslog.target

[Service]
User=app
ExecStart=/home/app/applications/springboot-systemctl.jar
SuccessExitStatus=143

[Install]
WantedBy=multi-user.target
```

Sekarang kita test dulu, dengan merestart service daemon berikut perintahnya:

```bash
## reload daemon for systemctl
systemctl daemon-reload

## start service 
systemctl start springboot-systemctl.service

## status service 
systemctl status springboot-systemctl.service
```

Setelah servicenya bisa di running, dan statusnya **Active** kita bisa check dengan menggunakan perintah berikut:

```bash
curl http:192.168.1.23:1234/actuator
```

## Create gitlab repository

Setelah itu kita ketahap selanjutnya yaitu membuat repository gitlab, proses ini intinya sama seperti sebelumnya jadi saya kasih stepnya ja ya:

- Create repository, dengan name `gitlab-ci-springboot-systemd`
- Setelah itu **commit** dan **push** ke repository tersebut

## Run systemctl from unix user `app`

Tahap selajutnya yaitu, kita harus setting supaya user di Unix bisa menjalankan perintah **systemd** tanpa menggunakan promnt password. sebenarnya bisa menggunakan user lain yaitu 'root' tpi terkadang root tidak bisa di buka lewat ssh remote jadi kita pakai user `app` saja ya yang kita grant seperti root dengan mengedit file dengan perintah sebagai berikut:

```bash
# view user sudo
cat /etc/sudoers

# to grant user app group with root
visudo
```

Edit menjadi seperti berikut:

```conf
# other configuration

# User privilege specification
root	ALL=(ALL:ALL) ALL
app	    ALL=(ALL:ALL) ALL # add this line

# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo	ALL=(ALL:ALL) ALL
%app    ALL=NOPASSWD: ALL # add this line
# See sudoers(5) for more information on "#include" directives:
```

Setelai itu coba login sebagai user `app` kemudian test perintah berikut:

```bash
sudo systemctl restart springboot-systemctl.service
```

Kalau sudah tidak ada promnt password yang muncul, dan langsung by pass berati configurasi sudah bener

## Script gitlab-ci

Tambahakan file `.gitlab-ci.yml` di root project seperti berikut:

```yml
variables:
  MAVEN_CLI_OPTS: "--show-version"

# Cache downloaded dependencies and plugins between builds.
# To keep cache across branches add 'key: "$CI_JOB_NAME"'
cache:
  paths:
    - .m2/repository

building:
  stage: build
  script:
    - 'mvn $MAVEN_CLI_OPTS clean package install'
  only:
    - master
  artifacts:
    paths:
      - target/*.jar
      
copy-artifact:
    stage: deploy
    script:
        - 'cp -u target/*.jar applications/'
        - 'sudo systemctl restart gitlab-ci.service'
    only:
        - master
```

Setelah itu daftarkan di gitlab runner `gitlab-runner` setelah tahapnya sama seperti sebelumnya tinggal kita checkout aja task, jobs dan pipelinenya. Jika sukses hasilnya seperti berikut:

![spring systemctl success]({{site.baseurl}}/assets/img/posts/gitlab-springboot-systemd/pipeline-status-success.png)

