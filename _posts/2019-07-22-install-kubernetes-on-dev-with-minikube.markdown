---
layout: post
title: "Kubernetes single cluster for development with minikube"
date: 2019-07-22T06:22:42+07:00
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

![minikube]({{site.baseurl}}/assets/img/posts/minikube-dev/logo.jpeg){: width="500px" }

Untuk mengunakan system k8s base on minikube, ada beberapa system required diantarnya yang harus di install adalah:

- [docker]({% post_url 2018-09-25-devops-with-docker %})
- private repository seperti [nexus oss]({% post_url 2018-09-25-nexus-repository %})
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

<!--more-->

Pertama kita siapkan dulu environtmentnya seperti yang telah saya sebutin diatas, misalnya saya menggunakan speck Macbook Pro Ram 8gb dan intel core i5 (2 core). 

Untuk installasi docker, udah pernah saya bahas di [artikel sebelumnya]({% post_url 2018-09-25-devops-with-docker %}). setelah itu kita siapkan **Repository docker** yang telah saya bahas juga di [artikel ini]({% post_url 2019-05-05-nexus-docker-private-registry %}) dan **Docker image** yang kita publish di private repository contohnya menggunakan springboot seperti [artikel ini]({% post_url 2019-07-18-springboot-docker-image %})

## Setup minikube

Kubernate on local, mengunakan minikube. pertama kita harus buat dulu vm untuk minikubenya dengan spec seperti berikut:

```bash
minikube --memory 4096 --cpus 2 --insecure-registry repository.dimas-maryanto.com:8086 start
```

Penjelasan

- `memory`, ramnya disarankan lebih besar dari 4gb
- `cpus`, cpu core disarankan lebih besar dari 2
- `insecure-registry`, docker private registry yang kita gunakan adalah `repository.dimas-maryanto.com:8086`

Berikut hasilnya:

```bash
😄  minikube v1.0.0 on darwin (amd64)
🤹  Downloading Kubernetes v1.14.0 images in the background ...
🔥  Creating virtualbox VM (CPUs=2, Memory=4096MB, Disk=20000MB) ...
📶  "minikube" IP address is 192.168.99.127
🐳  Configuring Docker as the container runtime ...
🐳  Version of container runtime is 18.06.2-ce
⌛  Waiting for image downloads to complete ...
✨  Preparing Kubernetes environment ...
🚜  Pulling images required by Kubernetes v1.14.0 ...
🚀  Launching Kubernetes v1.14.0 using kubeadm ... 
⌛  Waiting for pods: apiserver proxy etcd scheduler controller dns
🔑  Configuring cluster permissions ...
🤔  Verifying component health .....
💗  kubectl is now configured to use "minikube"
🏄  Done! Thank you for using minikube!
```

## Commandline API minikube

Berikut adalah API commandline yang bisa kita gunakan:

- login in vm minikube

    ```bash
    minikube ssh

    # output
    #                             _             _            
    #             _         _ ( )           ( )           
    # ___ ___  (_)  ___  (_)| |/')  _   _ | |_      __  
    # /' _ ` _ `\| |/' _ `\| || , <  ( ) ( )| '_`\  /'__`\
    # | ( ) ( ) || || ( ) || || |\`\ | (_) || |_) )(  ___/
    # (_) (_) (_)(_)(_) (_)(_)(_) (_)`\___/'(_,__/'`\____)
    #
    # $ docker info
    #  Containers: 18
    #   Running: 18
    #   Paused: 0
    #   Stopped: 0
    #  Images: 13
    #  Server Version: 18.09.6
    #  Storage Driver: overlay2
    #   Backing Filesystem: extfs
    #   Supports d_type: true
    #   Native Overlay Diff: true
    #  Logging Driver: json-file
    #  Cgroup Driver: cgroupfs
    #  Plugins:
    #   Volume: local
    #   Network: bridge host macvlan null overlay
    #   Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
    #  Swarm: inactive
    #  Runtimes: runc
    #  Default Runtime: runc
    #  Init Binary: docker-init
    #  containerd version: bb71b10fd8f58240ca47fbb579b9d1028eea7c84
    #  runc version: N/A
    #  init version: N/A (expected: )
    #  Security Options:
    #   seccomp
    #    Profile: default
    #  Kernel Version: 4.15.0
    #  Operating System: Buildroot 2018.05
    #  OSType: linux
    #  Architecture: x86_64
    #  CPUs: 2
    #  Total Memory: 3.85GiB
    #  Name: minikube
    #  ID: V3SZ:IVN4:3T6O:HIVX:YXSZ:EOGD:5X4F:Z2Q4:CLXF:L536:QRCS:GS4I
    #  Docker Root Dir: /var/lib/docker
    #  Debug Mode (client): false
    #  Debug Mode (server): false
    #  Registry: https://index.docker.io/v1/
    #  Labels:
    #   provider=virtualbox
    #  Experimental: false
    #  Insecure Registries:
    #   repository.dimas-maryanto.com:8086
    #   10.96.0.0/12
    #   127.0.0.0/8
    #  Live Restore Enabled: false
    #  Product License: Community Engine
    ```

- show ip address

    ```bash
    minikube ip

    # output
    # 192.168.99.104
    ```

- Kubernetes dashboard

    ```bash
    minikube dashboard
    ```

    Berikut adalah link dashboard minikube:  [http://host:50609/api/v1/namespaces/kube-system/services/http:kubernetes-dashboard:/proxy/](http://127.0.0.1:50609/api/v1/namespaces/kube-system/services/http:kubernetes-dashboard:/proxy/)

    ![dashboard minikube]({{site.baseurl}}/assets/img/posts/minikube-dev/minikube-dashboard.png)

## Create deployment

Pertama kita buat dulu untuk pod dengan menggunakan script deployment, seperti berikut:

- buat file `kubernetes.namespaces.yaml`

    ```yaml
    apiVersion: v1
    kind: Namespace
    metadata:
        name: springboot
    ```
- buat file `kubernetes.deployment.yaml`

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
        name: springboot-app
        namespace: springboot
    spec:
        replicas: 1
        selector:
            matchLabels:
                app: springboot-app
        template:
            metadata:
                labels:
                    app: springboot-app
            spec:
                containers:
                    - name: springboot-example
                      image: repository.dimas-maryanto.com:8086/tabeldata/springboot2-k8s-minikube-example:0.0.1-release
                      imagePullPolicy: "IfNotPresent"
                      ports:
                        - containerPort: 8080
                      command:
                        - "java"
                        - "-jar"
                        - "-Djava.security.egd=file:/dev/./urandom"
                        - "/var/applications/application.jar"
    ```

- buat file `kubernetes.service.yaml`

    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
        name: springboot-service
        namespace: springboot
    spec:
        selector:
            app: springboot-app
        ports:
            - port: 8080
              targetPort: 8080
    ```

- sekarang execute semua filenya dengan menggunakan command berikut:

    ```bash
    for i in path-to-files/kubernetes*yaml; do kubectl apply -f $i; done
    ```

    Berikut adalah status pod running pada `namespace` => `springboot`

    ![springboot status]({{site.baseurl}}/assets/img/posts/minikube-dev/status-kubernetes-dashboard.png)

## Test springboot is run

Pertama kita test aplikasi springboot kita yang telah kita deploy di minikube dengan menggunakan kube-expose-port, kita lihat dulu servicenya:

```bash
kubectl get services -n springboot

# Output
#
# NAME                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
# springboot-service   ClusterIP   10.107.250.236   <none>        8080/TCP   12m
```

Sekarang kita test menggunakan dalam kubernetes minikube vm, dengan menggunakan cluster-ip yaitu `10.107.250.236` yang kita dapatkan dari service name seperti berikut

```bash
minikube ssh

#                       _             _            
#          _         _ ( )           ( )           
#___ ___  (_)  ___  (_)| |/')  _   _ | |_      __  
#/' _ ` _ `\| |/' _ `\| || , <  ( ) ( )| '_`\  /'__`\
#| ( ) ( ) || || ( ) || || |\`\ | (_) || |_) )(  ___/
#(_) (_) (_)(_)(_) (_)(_)(_) (_)`\___/'(_,__/'`\____)

$ curl 10.107.250.236:8080/actuator

{
   "_links":{
      "self":{
         "href":"http://10.107.250.236:8080/actuator",
         "templated":false
      },
      "health-component":{
         "href":"http://10.107.250.236:8080/actuator/health/{component}",
         "templated":true
      },
      "health-component-instance":{
         "href":"http://10.107.250.236:8080/actuator/health/{component}/{instance}",
         "templated":true
      },
      "health":{
         "href":"http://10.107.250.236:8080/actuator/health",
         "templated":false
      },
      "info":{
         "href":"http://10.107.250.236:8080/actuator/info",
         "templated":false
      }
   }
}
```