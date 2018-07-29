---
layout: post
title: "Devops with docker"
category: docker
tags: 
- Devops
- Docker
- Virtual machine
author: Dimas Maryanto
references:
- https://docs.docker.com/docker-for-mac/install/#what-to-know-before-you-install
- https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install
comments: true
---

Sebagai seorang Research and Development dan sekaligus Java Software Programer, tidak jarang loh berurusan dengan product brended sekelas IBM, Oracle dan lainnya. Misalnya karena kerjaan kita adalah mendevelop aplikasi setelah itu aplikasi tersebut harus run di product IBM misalnya JBoss EAP versi 7.x.x kemudian Untuk database menggunakan product Microsoft MS-SQL Server 2017. 

Permasalahnya tidak berhenti sampai situ? Permasalahan sebenarnya product branded tersebut punya spesifikasi masing-masin contohnya MS SQL Server hanya run di Microsoft Azure, dan Linux Server sedangkan tidak semua Developer memiliki platform tersebut dan spesifikasi laptop dan komputer yang memadai untuk me-runing product tersebut.

![docker]({{site.baseurl}}/assets/img/posts/devops-docker/logo.png){: width="400px"}

Solusinya yaitu dengan virtualisasi, virtualisasi seperti apa? VMware atau Virtual box??? opppsss itu udah kuno. Perkenalkan ada teknologi yang baru-baru ini ramai di perbincangkan yaitu Docker.
<!--more-->

## What is docker

Docker adalah ...

![what is docker]({{site.baseurl}}/assets/img/posts/devops-docker/what-is-docker.png)

## Mininum System required

- Linux
- Mac Os Sierra or lastest
    - Mac hardware must be a 2010 or newer model, with Intel’s hardware support for memory management unit (MMU) virtualization, including Extended Page Tables (EPT) and Unrestricted Mode
    - macOS El Capitan 10.11 and newer macOS releases are supported
    - At least 4GB of RAM
- your machine must have a 64-bit operating system running Windows 7 
    - Docker for Windows requires Microsoft Hyper-V to run
    - Virtualization must be enabled in BIOS and CPU SLAT-capable

## Docker Image

Docker images adalah ibaratnya installer aplikasi

## Docker Container

Docker container adalah aplikasi yang telah terinstall di virtual

## Docker Volumes

Docker volumes adalah storage / data dari aplikasi yang terinstall (docker container)

## Docker Compose

Docker compose adalah multiple container run in paralel