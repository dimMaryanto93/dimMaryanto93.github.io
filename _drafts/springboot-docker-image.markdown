---
layout: post
title: "Publish SpringBoot as Docker Image using maven plugin"
category: springboot
tags: 
- maven-plugin
- docker
- dockerfile
- docker-image
author: Dimas Maryanto
references:
- https://docs.docker.com/engine/reference/commandline/images/
- https://github.com/spotify/docker-maven-plugin
comments: true
---

![docker maven plugin]({{site.baseurl}}/assets/img/posts/docker-maven-plugin/logo.jpeg)

Pertama kita buat dulu project springboot menggunakan [starter project](https://start.spring.io)

<!--more-->

## Create project

![starter template project]({{site.baseurl}}/assets/img/posts/docker-maven-plugin/starter-springboot.png)

properties;

- Project: `Maven Project`
- Language: `Java`
- Spring Boot: `2.1.6` pilih yang paling baru aja.
- Project Metadata:
    - Group: `com.maryanto.dimas.example`
    - Artifact: `springboot2-k8s-minikube-example`
- Dependencies:
    - `Spring Boot Web Starter`
    - `Lombok`
    - `Spring Boot Actuator`
    - `Spring Boot DevTools`

## Install plugin

Untuk menggunakan maven plugin `docker-maven-plugin` kita modif file `pom.xml` seperti berikut:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <!-- metadata maven project goes here!-->

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
        <!-- docker registry properties -->
        <docker-registry.group>tabeldata</docker-registry.group>
        <docker-registry.host.push>repository.dimas-maryanto.com:8087</docker-registry.host.push>
        <docker-registry.host.pull>repository.dimas-maryanto.com:8086</docker-registry.host.pull>
        <!-- jar file name  -->
        <finalName>${project.artifactId}-${project.version}</finalName>
    </properties>

    <!-- repository maven pull -->
    <repositories>
        <repository>
            <id>repository.dimas-maryanto.com</id>
            <url>http://repository.dimas-maryanto.com:8081/repository/maven-public/</url>
        </repository>
    </repositories>

    <!-- repository maven publish -->
    <distributionManagement>
        <repository>
            <id>repository.dimas-maryanto.com</id>
            <url>http://repository.dimas-maryanto.com:8081/repository/maven-releases/</url>
        </repository>
        <snapshotRepository>
            <id>repository.dimas-maryanto.com</id>
            <url>http://repository.dimas-maryanto.com:8081/repository/maven-snapshots/</url>
        </snapshotRepository>
    </distributionManagement>

    <!-- dependencies goes here -->
    <dependencies>
    <!-- dependency as springboot, jdbc, etc -->
    </dependencies>

    <build>
        <finalName>${finalName}</finalName>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <!-- run springboot as service on linux -->
                <configuration>
                    <executable>true</executable>
                </configuration>
            </plugin>
            <!-- maven versioning plugin -->
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>versions-maven-plugin</artifactId>
            </plugin>
            <!-- maven jdk 1.8 as defautl compiler -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <target>8</target>
                    <source>8</source>
                </configuration>
            </plugin>
            <!-- docker maven plugin for build image docker from maven command line -->
            <plugin>
                <groupId>com.spotify</groupId>
                <artifactId>docker-maven-plugin</artifactId>
                <version>1.1.1</version>
                <executions>
                    <!-- build image on mvn package command executed -->
                    <execution>
                        <id>build-image</id>
                        <phase>install</phase>
                        <goals>
                            <goal>build</goal>
                        </goals>
                    </execution>
                    <!-- create tag on mvn deploy command executed  -->
                    <execution>
                        <id>tag-image</id>
                        <phase>deploy</phase>
                        <goals>
                            <goal>tag</goal>
                        </goals>
                        <configuration>
                            <forceTags>true</forceTags>
                            <image>${project.artifactId}:${project.version}</image>
                            <newName>
                                ${docker-registry.host.push}/${docker-registry.group}/${project.artifactId}:${project.version}
                            </newName>
                        </configuration>
                    </execution>
                    <!-- publish to private repository on mvn deploy command executed -->
                    <execution>
                        <id>push-image</id>
                        <phase>deploy</phase>
                        <goals>
                            <goal>push</goal>
                        </goals>
                        <configuration>
                            <retryPushCount>2</retryPushCount>
                            <imageName>
                                ${docker-registry.host.push}/${docker-registry.group}/${project.artifactId}:${project.version}
                            </imageName>
                        </configuration>
                    </execution>
                </executions>
                <configuration>
                    <baseImage>${docker-registry.host.pull}/openjdk:8-alpine</baseImage>
                    <imageName>${project.artifactId}:${project.version}</imageName>
                    <resources>
                    <!-- copy jar file after build to docker image -->
                        <resource>
                            <targetPath>/var/applications</targetPath>
                            <directory>${project.build.directory}</directory>
                            <includes>
                                <include>${project.build.finalName}.jar</include>
                            </includes>
                        </resource>
                    </resources>
                    <exposes>
                        <expose>8080</expose>
                    </exposes>
                    <runs>
                    <!-- execute jar on container location -->
                        <run>
                            ln -s /var/applications/${project.build.finalName}.jar /var/applications/application.jar
                        </run>
                    </runs>
                    <maintainer>Dimas Maryanto (software.dimas_m@icloud.com)</maintainer>
                    <imageTags>
                        <imageTag>${project.version}</imageTag>
                    </imageTags>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

## Build image

Sebelum kita execute command docker maven plugin kita harus login dulu ke private registry kita dengan menggunakan perintah:

```bash
# url: nexus-host:nexus-port example
docker login repository.dimas-maryanto:8087 # for pushing repository
```

Setelah berhasil login, kita baru jalankan perintah maven berikut:

```bash
mvn clean install
```

Berikut outputnya:

```bash
[INFO] --- maven-jar-plugin:3.1.2:jar (default-jar) @ springboot2-k8s-minikube-example ---
[INFO] Building jar: /Users/dimasm93/Workspaces/examples/springboot2-k8s-minikube/target/springboot2-k8s-minikube-example-0.0.1-SNAPSHOT.jar
[INFO] 
[INFO] --- spring-boot-maven-plugin:2.1.6.RELEASE:repackage (repackage) @ springboot2-k8s-minikube-example ---
[INFO] Replacing main artifact with repackaged archive
[INFO] 
[INFO] --- maven-dependency-plugin:3.1.1:unpack (unpack) @ springboot2-k8s-minikube-example ---
[INFO] Configured Artifact: com.maryanto.dimas.example:springboot2-k8s-minikube-example:0.0.1-SNAPSHOT:jar
[INFO] Unpacking /Users/dimasm93/Workspaces/examples/springboot2-k8s-minikube/target/springboot2-k8s-minikube-example-0.0.1-SNAPSHOT.jar to /Users/dimasm93/Workspaces/examples/springboot2-k8s-minikube/target/dependency with includes "" and excludes ""
[INFO] 
[INFO] --- maven-install-plugin:2.5.2:install (default-install) @ springboot2-k8s-minikube-example ---
[INFO] Installing /Users/dimasm93/Workspaces/examples/springboot2-k8s-minikube/target/springboot2-k8s-minikube-example-0.0.1-SNAPSHOT.jar to /Users/dimasm93/.m2/repository/com/maryanto/dimas/example/springboot2-k8s-minikube-example/0.0.1-SNAPSHOT/springboot2-k8s-minikube-example-0.0.1-SNAPSHOT.jar
[INFO] Installing /Users/dimasm93/Workspaces/examples/springboot2-k8s-minikube/pom.xml to /Users/dimasm93/.m2/repository/com/maryanto/dimas/example/springboot2-k8s-minikube-example/0.0.1-SNAPSHOT/springboot2-k8s-minikube-example-0.0.1-SNAPSHOT.pom
[INFO] 
[INFO] --- docker-maven-plugin:1.1.1:build (build-image) @ springboot2-k8s-minikube-example ---
[INFO] Using authentication suppliers: [ConfigFileRegistryAuthSupplier]
[INFO] Copying /Users/dimasm93/Workspaces/examples/springboot2-k8s-minikube/target/springboot2-k8s-minikube-example-0.0.1-SNAPSHOT.jar -> /Users/dimasm93/Workspaces/examples/springboot2-k8s-minikube/target/docker/var/applications/springboot2-k8s-minikube-example-0.0.1-SNAPSHOT.jar
[INFO] Building image springboot2-k8s-minikube-example:0.0.1-SNAPSHOT
Step 1/5 : FROM repository.dimas-maryanto.com:8086/openjdk:8-alpine

Pulling from openjdk
Digest: sha256:44b3cea369c947527e266275cee85c71a81f20fc5076f6ebb5a13f19015dce71
Status: Downloaded newer image for repository.dimas-maryanto.com:8086/openjdk:8-alpine
 ---> a3562aa0b991
Step 2/5 : MAINTAINER Dimas Maryanto (software.dimas_m@icloud.com)

 ---> Running in 7571ffbcd479
Removing intermediate container 7571ffbcd479
 ---> 0dfe9332cde4
Step 3/5 : ADD /var/applications/springboot2-k8s-minikube-example-0.0.1-SNAPSHOT.jar /var/applications/

 ---> edbe8a6ac6af
Step 4/5 : RUN ln -s /var/applications/springboot2-k8s-minikube-example-0.0.1-SNAPSHOT.jar /var/applications/application.jar

 ---> Running in 0af3f39022e4
Removing intermediate container 0af3f39022e4
 ---> 8ae9302775c2
Step 5/5 : EXPOSE 8080

 ---> Running in e9cf8bc2716b
Removing intermediate container e9cf8bc2716b
 ---> a70cd052c197
ProgressMessage{id=null, status=null, stream=null, error=null, progress=null, progressDetail=null}
Successfully built a70cd052c197
Successfully tagged springboot2-k8s-minikube-example:0.0.1-SNAPSHOT
[INFO] Built springboot2-k8s-minikube-example:0.0.1-SNAPSHOT
[INFO] Tagging springboot2-k8s-minikube-example:0.0.1-SNAPSHOT with 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  22.277 s
[INFO] Finished at: 2019-07-18T14:07:20+07:00
[INFO] ------------------------------------------------------------------------
```

Nah setelah kita build kita check docker image di docker:

```bash
docker images
```

Berikut outputnya:

```bash
REPOSITORY                                      TAG                 IMAGE ID            CREATED              SIZE
springboot2-k8s-minikube-example                0.0.1-SNAPSHOT      a70cd052c197        About a minute ago   125MB
repository.dimas-maryanto.com:8086/openjdk      8-alpine            a3562aa0b991        2 months ago         105MB
```

Nah ini berati docker image udah terbuild dan di save ke local repository kita, 

## Publish docker image

nah sekarang kata push ke docker registry kita dengan menggunakan maven command deploy:

```bash
mvn clean deploy
```