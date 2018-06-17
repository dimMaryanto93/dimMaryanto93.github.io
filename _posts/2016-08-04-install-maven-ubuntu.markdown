---
layout: post
title: "Install Apache Maven on Ubuntu 16.04 LTS"
date: 2016-08-04T07:00:00+07:00
category: ubuntu-16.04
tags: 
- Ubuntu
- Apache
- Maven
- Java
author: Dimas Maryanto
---


Pastikan anda telah menginstall Oracle JDK dan setup JAVA_HOME klo belum silahkan install dulu di [artike berikut]({% post_url 2016-08-02-install-oracle-jdk8-ubuntu %}), setelah setting Path JAVA_HOME kemudian kita buka terminal kemudian masukan perintah berikut

{% highlight bash %}
apt-get install maven
{% endhighlight %}

Dependency yang dibutuhkan oleh maven seperti berikut:

<!--more-->

{% highlight bash %}
The following additional packages will be installed:
  ant ant-optional junit junit4 libaopalliance-java libapache-pom-java
  libasm4-java libatinject-jsr330-api-java libbsh-java libcdi-api-java
  libcglib3-java libclassworlds-java libcommons-cli-java libcommons-codec-java
  libcommons-httpclient-java libcommons-io-java libcommons-lang-java
  libcommons-lang3-java libcommons-logging-java libcommons-net-java
  libcommons-net2-java libcommons-parent-java libdom4j-java libdoxia-core-java
  libeasymock-java libeclipse-aether-java
  libgeronimo-interceptor-3.0-spec-java libguava-java libguice-java
  libhamcrest-java libhttpclient-java libhttpcore-java libjaxen-java
  libjaxp1.3-java libjdom1-java libjetty-java libjsch-java libjsoup-java
  libjsr305-java liblog4j1.2-java libmaven-parent-java libmaven2-core-java
  libmaven3-core-java libobjenesis-java libplexus-ant-factory-java
  libplexus-archiver-java libplexus-bsh-factory-java libplexus-cipher-java
  libplexus-classworlds-java libplexus-classworlds2-java libplexus-cli-java
  libplexus-component-annotations-java libplexus-component-metadata-java
  libplexus-container-default-java libplexus-container-default1.5-java
  libplexus-containers-java libplexus-containers1.5-java
  libplexus-interactivity-api-java libplexus-interpolation-java
  libplexus-io-java libplexus-sec-dispatcher-java libplexus-utils-java
  libplexus-utils2-java libqdox2-java libservlet2.5-java libservlet3.1-java
  libsisu-inject-java libsisu-plexus-java libslf4j-java libwagon-java
  libwagon2-java libxalan2-java libxbean-java libxerces2-java
  libxml-commons-external-java libxml-commons-resolver1.1-java libxom-java
  libxpp2-java libxpp3-java
0 upgraded, 80 newly installed, 0 to remove and 0 not upgraded.
Need to get 28,0 MB of archives.
After this operation, 39,7 MB of additional disk space will be used.
Do you want to continue? [Y/n]
{% endhighlight %}

Setelah beres installnya kita bisa cek dengan menggunakan perintah seperti berikut:

{% highlight bash %}
mvn -version
{% endhighlight %}

Maka outputnya seperti berikut:

{% highlight java %}
root@Acer-E14:~# mvn -version
Apache Maven 3.3.9
Maven home: /usr/share/maven
Java version: 1.8.0_101, vendor: Oracle Corporation
Java home: /usr/lib/jvm/java-8-oracle/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "4.4.0-34-generic", arch: "amd64", family: "unix"
root@Acer-E14:~#
{% endhighlight %}
