---
layout: post
title: "Install Apache Maven on Fedora 23"
date: 2016-08-04T08:00:00+07:00
category: fedora-23
tags: 
- Apache
- Maven
- Fedora
- Java
author: Dimas Maryanto
references:
comments: true
---

Pastikan anda telah menginstall Oracle JDK dan setup JAVA_HOME klo belum silahkan install dulu di [artike berikut]({% post_url 2016-08-02-install-oracle-jdk8-fedora %}), setelah setting Path JAVA_HOME kemudian kita buka terminal kemudian masukan perintah berikut

{% highlight bash %}
$ sudo dnf install maven -y
{% endhighlight %}

maka akan ```dnf``` kurang lebih akan menginstall dependencynya seperti output berikut:

<!--more-->

{% highlight bash %}
Dependencies resolved.
================================================================================
 Package                           Arch   Version              Repository Size
================================================================================
Installing:
 aether-api                        noarch 1:1.0.2-4.fc24          fedora  138 k
 aether-connector-basic            noarch 1:1.0.2-4.fc24          fedora   47 k
 aether-impl                       noarch 1:1.0.2-4.fc24          fedora  173 k
 aether-spi                        noarch 1:1.0.2-4.fc24          fedora   37 k
 aether-transport-wagon            noarch 1:1.0.2-4.fc24          fedora   35 k
 aether-util                       noarch 1:1.0.2-4.fc24          fedora  145 k
 aopalliance                       noarch 1.0-12.fc24             fedora   16 k
 apache-commons-cli                noarch 1.3.1-3.fc24            fedora   72 k
 apache-commons-codec              noarch 1.10-3.fc24             fedora  247 k
 apache-commons-io                 noarch 1:2.4-15.fc24           fedora  192 k
 apache-commons-lang               noarch 2.6-18.fc24             fedora  281 k
 apache-commons-lang3              noarch 3.4-4.fc24              fedora  416 k
 apache-commons-logging            noarch 1.2-5.fc24              fedora   86 k
 atinject                          noarch 1-22.20100611svn86.fc24 fedora   19 k
 cdi-api                           noarch 1.2-3.fc24              updates 142 k
 glassfish-el-api                  noarch 3.0.0-8.fc24            fedora   81 k
 google-guice                      noarch 4.0-4.fc24              fedora  459 k
 guava                             noarch 18.0-4.fc23             fedora  1.9 M
 httpcomponents-client             noarch 4.5.2-2.fc24            fedora  700 k
 httpcomponents-core               noarch 4.4.4-2.fc24            fedora  633 k
 jboss-interceptors-1.2-api        noarch 1.0.0-4.fc24            fedora   32 k
 jsoup                             noarch 1.8.3-2.fc24            fedora  312 k
 jsr-305                           noarch 0-0.19.20130910svn.fc24 fedora   33 k
 maven                             noarch 3.3.9-4.fc24            fedora  1.4 M
 maven-wagon-file                  noarch 2.10-2.fc24             fedora   25 k
 maven-wagon-http                  noarch 2.10-2.fc24             fedora   48 k
 maven-wagon-http-shared           noarch 2.10-2.fc24             fedora   25 k
 maven-wagon-provider-api          noarch 2.10-2.fc24             fedora   62 k
 objectweb-asm                     noarch 5.0.4-2.fc24            fedora  582 k
 plexus-cipher                     noarch 1.7-11.fc24             fedora   28 k
 plexus-classworlds                noarch 2.5.2-4.fc24            fedora   64 k
 plexus-containers-component-annotations
                                   noarch 1.6-5.fc24              fedora   17 k
 plexus-interpolation              noarch 1.22-5.fc24             fedora   78 k
 plexus-sec-dispatcher             noarch 1.4-21.fc24             fedora   31 k
 plexus-utils                      noarch 3.0.22-3.fc24           fedora  254 k
 publicsuffix-list                 noarch 20160713-1.fc24         updates  67 k
 sisu-inject                       noarch 1:0.3.2-4.fc24          fedora  269 k
 sisu-plexus                       noarch 1:0.3.2-4.fc24          fedora  189 k
 slf4j                             noarch 1.7.18-1.fc24           fedora   70 k
Transaction Summary
================================================================================
Install  39 Packages
{% endhighlight %}

setelah semua diinstall maka kita bisa cek, apakah maven udah dikenali oleh system fedora, dengan menggunakan perintah berikut:

{% highlight bash %}
mvn -version
{% endhighlight %}

maka akan menampilkan output seperti berikut:

{% highlight bash %}
[dimmaryanto93@localhost ~]$ mvn -version
Apache Maven 3.3.9 (NON-CANONICAL_2016-04-07T23:15:34Z_mockbuild; 2016-04-08T06:15:34+07:00)
Maven home: /usr/share/maven
Java version: 1.8.0_101, vendor: Oracle Corporation
Java home: /usr/java/jdk1.8.0_101/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "4.6.4-301.fc24.x86_64", arch: "amd64", family: "unix"
[dimmaryanto93@localhost ~]$
{% endhighlight %}

