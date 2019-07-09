---
layout: post
title: "Stress test dengan JMeter"
date: 2019-07-09T11:45:27+07:00
category: jmeter
tags: 
- TestingManagement
- StresTest
- MeasurePerformance
author: Dimas Maryanto
references:
- https://jmeter.apache.org
comments: true
---

![jmater performance test]({{site.baseurl}}/assets/img/posts/jmeter/logo.png)

JMeter adalah aplikasi untuk performance testing yang berbasis Java desktop / cli (Command line interface) yang bisa di implementasikan ke berbagai jenis protocol seperti http, database via jdbc, ldap, jms dan masih banyak lagi, temen-temen bisa lihat di [dokumentasinya](https://jmeter.apache.org) 
<!--more-->

## Installation JMeter

Pertama kita harus download dulu Aplikasi JMeter [download disini](https://jmeter.apache.org/download_jmeter.cgi) pilih yang **binary** dengan format `tgz` untuk linux / MacOS distribution dan `zip` untuk Windows distribution.

Setelah di download kita cukup extract dengan menggunakan unzip, atau tar.

Using `zip`: 

```bash
unzip apache-jmeter-5.1.1.zip
```

Using `tar.gz`:

```bash
tar -zxvf apache-jmeter-5.1.1.tgz
```

## Running JMeter

Untuk menjalankan aplikasi JMeter membutuhkan JDK 8 minimal, kalo belum punya jdk install dulu, download oracle jdk8 [disini](https://www.oracle.com/technetwork/es/java/javase/downloads/jdk8-downloads-2133151.html).

```bash
path-to-installation/apache-jmeter/bin/jmeter
```

![jmeter in progress]({{site.baseurl}}/assets/img/posts/jmeter/jmeter-run.png)

## HTTP request test

http test request adalah method yang paling relevant karena semua di jaman sekarang aplikasi backend menggunakan sistem API baik Restful Web Service maupun SOAP. Untuk menggunakan test request kita buat **group** dulu di `TEST PLAN` dengan cara

click kanan di `TEST PLAN` -> `ADD` -> `Threads (Users)` seperti berikut:

![jmeter thread user]({{site.baseurl}}/assets/img/posts/jmeter/thread-group-user.png)

Konfigurasi di atas, untuk menentukan jumlah user hit.

Setelah itu di `Threads Group` yang kita buat kita tambahkan sample http request dengan cara click kanan di `Threads Group` -> `ADD` -> `Sampler` -> `HTTP Request` seperti berikut:

![http request]({{site.baseurl}}/assets/img/posts/jmeter/http-request-sample.png)

Untuk configurasi di atas sama seperti kita membuat request di Postman, kemudian untuk membuat report kita tambahkan listener di dalam `HTTP Request` yang telah kita buat dengan cara click kanan di `HTTP Request` -> `ADD` -> `Listener` -> `Summary report` seperti berikut:

![summary report]({{site.baseurl}}/assets/img/posts/jmeter/http-request-summary-report.png)

dan jika kita coba di run dengan click button

![start button]({{site.baseurl}}/assets/img/posts/jmeter/start-button.png){: width="100px" }

Hasilnya seperti berikut:

![summary report]({{site.baseurl}}/assets/img/posts/jmeter/jmeter-result-summary-report.png)

