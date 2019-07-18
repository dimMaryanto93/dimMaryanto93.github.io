---
layout: post
title: "Kubernetes single cluster for development with minikube"
category: k8s
tags: 
- Kubernetes
- k8s
- istio
- springboot
- cloud
author: Dimas Maryanto
references:
- https://istio.io/docs/setup/kubernetes/install/kubernetes/
- https://kubernetes.io/docs/tasks/tools/install-minikube/
comments: true
---

![minikube]({{site.baseurl}}/assets/img/posts/kubernetes-minikube/logo.jpeg){: width="500px" }

Untuk mengunakan system k8s base on minikube, ada beberapa system required diantarnya yang harus di install adalah:

- [docker]({% post_url 2018-09-25-devops-with-docker %})
- private repository seperti [nexus oss]({% post_url 2018-09-25-nexus-repository %})
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

<!--more-->

Pertama kita siapkan dulu environtmentnya seperti yang telah saya sebutin diatas, misalnya saya menggunakan speck Macbook Pro Ram 8gb dan intel core i5 (2 core). 

Untuk installasi docker, udah pernah saya bahas di [artikel sebelumnya]({% post_url 2018-09-25-devops-with-docker %}). setelah itu kita siapkan **Repository docker** yang telah saya bahas juga di [artikel ini]({% post_url 2019-05-05-nexus-docker-private-registry %}) yang nantinya kita akan nge push image ke repository tersebut.

## Setup minikube