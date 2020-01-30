---
layout: post
title: "Install Openshift on-premise for development on Mac OS"
date: 2020-01-30T12:05:40+07:00
category: openshift
tags: 
- Docker
- Kubernetes
- Redhad
- Openshift 
author: Dimas Maryanto
references:
- https://www.okd.io/download.html
- https://www.docker.com
- https://kubernetes.io
comments: true
---

![openshift-logo]({{site.baseurl}}/assets/img/posts/openshift-on-premise-mac/openshift-logo.png)

Untuk menginstall openshift on-premise, Oh ia berikut adalah spesifikasi laptop saya yaitu `Macbook Pro 13" Dual Core Intel Core i5, 8Gb of Ram, and 256 SSD Storage` yang saya gunakan untuk Install Openshift.

Platform yang kita gunakan untuk mencoba Openshift ini adalah `OKD is a distribution of Kubernetes optimized for continuous application development and multi-tenant deployment` atau silahkan kunjungi [website](https://www.okd.io/)

<!--more-->

Karena kita di PT. Tabeldata Informatika partner dari PT. Multipolar Technology yang merupakan Brand Ambasador dari IBM dan RedHat, so jadi application yang kita dukung / develop harus support dengan system PaaS (Platform as a service) seperti Openshift. 

Ada 2 cara untuk menginstall Openshift client tools yaitu dengan `Run OKD in a Container` dan `Run the All-In-One VM with Minishift` tapi disini kita installnya dengan method yang paling simple aja ya...

Nah sekarang kita install Openshift di Mac OS, ada berapa yang kita butuhkan untuk menginstall yaitu:
- Install docker
    - Config update VM docker settings
    - Setup `network-insecure-registry`
- add docker images `k8s.gcr.io/*`
- Install kubernetes CLI
- Install Openshift cluster

## Instal Docker

Untuk Mac OS, install docker cukup mudah kita hanya download Docker Desktop dan install, it's done!, silahkan [download disini](https://docs.docker.com/docker-for-mac/install/)

hasinya seperti berikut: 

```bash
Client: Docker Engine - Community
 Version:           19.03.5
 API version:       1.40
 Go version:        go1.12.12
 Git commit:        633a0ea
 Built:             Wed Nov 13 07:22:34 2019
 OS/Arch:           darwin/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.5
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.12
  Git commit:       633a0ea
  Built:            Wed Nov 13 07:29:19 2019
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v1.2.10
  GitCommit:        b34a5c8af56e510852c35414db4c1f4fa6172339
 runc:
  Version:          1.0.0-rc8+dev
  GitCommit:        3e425f80a8c931f88e6d94a8c831b9d5aa481657
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```

## Docker Config settings

Untuk menjalankan openshift dengan keterbatasan resource di laptop saya ini, configurasi yang saya gunakan adalah seperti berikut:

- `Docker menu -> Preferences -> Resources -> Advanced`
![docker-preferences-resource]({{site.baseurl}}/assets/img/posts/openshift-on-premise-mac/docker-pref-resources.png)

- `Docker menu -> Preferences -> Docker Engine`
```json
{
    "experimental": false,
    "insecure-registries": [
        "172.30.0.0/16" // add this ip-address network to your insecure-registry
    ],
    "debug": true
}
```

Setelah semua selesai kemudian `Apply & Restart`

## Add docker images `k8s.gcr.io/*`

Openshift membutuhkan docker image dari kubernetes seperti berikut:

```bash
➜ docker images 
REPOSITORY                           TAG                 IMAGE ID            CREATED             SIZE
k8s.gcr.io/kube-controller-manager   v1.15.5             1399a72fa1a9        3 months ago        159MB
k8s.gcr.io/kube-proxy                v1.15.5             cbd7f21fec99        3 months ago        82.4MB
k8s.gcr.io/kube-apiserver            v1.15.5             e534b1952a0d        3 months ago        207MB
k8s.gcr.io/kube-scheduler            v1.15.5             fab2dded59dd        3 months ago        81.1MB
docker/kube-compose-controller       v0.4.23             a8c3d87a58e7        7 months ago        35.3MB
docker/kube-compose-api-server       v0.4.23             f3591b2cb223        7 months ago        49.9MB
k8s.gcr.io/coredns                   1.3.1               eb516548c180        12 months ago       40.3MB
k8s.gcr.io/etcd                      3.3.10              2c4adeb21b4f        14 months ago       258MB
k8s.gcr.io/pause                     3.1                 da86e6ba6ca1        2 years ago         742kB
```

Untuk mendapatkan images nya kita aktifkan kubernetes dari Docker preferences seperti berikut

![docker-enabled-kubernetes]({{site.baseurl}}/assets/img/posts/openshift-on-premise-mac/docker-kubernetes-enabled.png)

Setelah statusnya `kubernetes running` kemudian **Unchecked Enabled Kubernetes** dan `Apply & Restart`.

## Install Kubernetes CLI

Untuk install kubernetes cli, kita bisa install menggunakan homebrew berikut perintahnya:

```bash
brew install kubernetes-cli
```

berikut hasilnya:

```bash
➜ kubectl version
Client Version: version.Info 
{   Major:"1", 
    Minor:"15", 
    GitVersion:"v1.15.5", 
    GitCommit:"20c265fef0741dd71a66480e35bd69f18351daea", 
    GitTreeState:"clean", 
    BuildDate:"2019-10-15T19:16:51Z", 
    GoVersion:"go1.12.10", 
    Compiler:"gc", 
    Platform:"darwin/amd64"
}
```

## Install Openshift

Untuk install openshift download dulu [openshift client tools](https://github.com/openshift/origin/releases/download/v3.11.0/openshift-origin-client-tools-v3.11.0-0cbc58b-mac.zip)

Setelah itu extract, kemudian pindahkan file `oc` ke executeable di mac yaitu 

```bash
mv path-file/oc /usr/local/bin/
```

Setelah itu selesai berikut hasilnya:

```bash
➜ oc version
oc v3.11.0+0cbc58b
kubernetes v1.11.0+d4cacc0
features: Basic-Auth
```

## Create Cluster Openshift

Untuk menjalankan platform Openshift berikut perintahnya:

```bash
➜  ~ oc cluster up
Getting a Docker client ...
Checking if image openshift/origin-control-plane:v3.11 is available ...
Pulling image openshift/origin-control-plane:v3.11
Pulled 5/5 layers, 100% complete
Extracting
Image pull complete
Pulling image openshift/origin-cli:v3.11
Image pull complete
Pulling image openshift/origin-node:v3.11
Pulled 5/6 layers, 84% complete
Pulled 6/6 layers, 100% complete
Extracting
Image pull complete
Creating shared mount directory on the remote host ...
Determining server IP ...
Checking if OpenShift is already running ...
Checking for supported Docker version (=>1.22) ...
Checking if insecured registry is configured properly in Docker ...
Checking prerequisites for port forwarding ...
Checking if required ports are available ...
Checking if OpenShift client is configured properly ...
Checking if image openshift/origin-control-plane:v3.11 is available ...
Starting OpenShift using openshift/origin-control-plane:v3.11 ...
I0130 11:41:23.052449    7215 flags.go:30] Running "create-kubelet-flags"
I0130 11:41:23.583241    7215 run_kubelet.go:49] Running "start-kubelet"
I0130 11:41:23.819622    7215 run_self_hosted.go:181] Waiting for the kube-apiserver to be ready ...
I0130 11:43:01.650173    7215 interface.go:26] Installing "kube-proxy" ...
I0130 11:43:01.650224    7215 interface.go:26] Installing "kube-dns" ...
I0130 11:43:01.650234    7215 interface.go:26] Installing "openshift-service-cert-signer-operator" ...
I0130 11:43:01.650242    7215 interface.go:26] Installing "openshift-apiserver" ...
I0130 11:43:01.650525    7215 apply_template.go:81] Installing "openshift-apiserver"
I0130 11:43:01.650526    7215 apply_template.go:81] Installing "kube-dns"
I0130 11:43:01.650672    7215 apply_template.go:81] Installing "openshift-service-cert-signer-operator"
I0130 11:43:01.650795    7215 apply_template.go:81] Installing "kube-proxy"
I0130 11:43:12.594361    7215 interface.go:41] Finished installing "kube-proxy" "kube-dns" "openshift-service-cert-signer-operator" "openshift-apiserver"
I0130 11:44:37.779350    7215 run_self_hosted.go:242] openshift-apiserver available
I0130 11:44:37.779391    7215 interface.go:26] Installing "openshift-controller-manager" ...
I0130 11:44:37.779416    7215 apply_template.go:81] Installing "openshift-controller-manager"
I0130 11:44:41.202499    7215 interface.go:41] Finished installing "openshift-controller-manager"
Adding default OAuthClient redirect URIs ...
Adding centos-imagestreams ...
Adding registry ...
Adding persistent-volumes ...
Adding router ...
Adding sample-templates ...
Adding web-console ...
I0130 11:44:41.271065    7215 interface.go:26] Installing "centos-imagestreams" ...
I0130 11:44:41.271100    7215 interface.go:26] Installing "openshift-image-registry" ...
I0130 11:44:41.271111    7215 interface.go:26] Installing "persistent-volumes" ...
I0130 11:44:41.271118    7215 interface.go:26] Installing "openshift-router" ...
I0130 11:44:41.271124    7215 interface.go:26] Installing "sample-templates" ...
I0130 11:44:41.271131    7215 interface.go:26] Installing "openshift-web-console-operator" ...
I0130 11:44:41.272056    7215 interface.go:26] Installing "sample-templates/mongodb" ...
I0130 11:44:41.272108    7215 interface.go:26] Installing "sample-templates/mysql" ...
I0130 11:44:41.272118    7215 interface.go:26] Installing "sample-templates/dancer quickstart" ...
I0130 11:44:41.272124    7215 interface.go:26] Installing "sample-templates/rails quickstart" ...
I0130 11:44:41.272138    7215 interface.go:26] Installing "sample-templates/mariadb" ...
I0130 11:44:41.272201    7215 interface.go:26] Installing "sample-templates/postgresql" ...
I0130 11:44:41.272213    7215 interface.go:26] Installing "sample-templates/cakephp quickstart" ...
I0130 11:44:41.272220    7215 interface.go:26] Installing "sample-templates/django quickstart" ...
I0130 11:44:41.272227    7215 interface.go:26] Installing "sample-templates/nodejs quickstart" ...
I0130 11:44:41.272234    7215 interface.go:26] Installing "sample-templates/jenkins pipeline ephemeral" ...
I0130 11:44:41.272241    7215 interface.go:26] Installing "sample-templates/sample pipeline" ...
I0130 11:44:41.272924    7215 apply_list.go:67] Installing "sample-templates/rails quickstart"
I0130 11:44:41.272066    7215 apply_list.go:67] Installing "centos-imagestreams"
I0130 11:44:41.273075    7215 apply_list.go:67] Installing "sample-templates/cakephp quickstart"
I0130 11:44:41.273216    7215 apply_list.go:67] Installing "sample-templates/sample pipeline"
I0130 11:44:41.273276    7215 apply_list.go:67] Installing "sample-templates/mariadb"
I0130 11:44:41.273283    7215 apply_list.go:67] Installing "sample-templates/mongodb"
I0130 11:44:41.273470    7215 apply_list.go:67] Installing "sample-templates/postgresql"
I0130 11:44:41.273480    7215 apply_list.go:67] Installing "sample-templates/django quickstart"
I0130 11:44:41.273699    7215 apply_list.go:67] Installing "sample-templates/nodejs quickstart"
I0130 11:44:41.273748    7215 apply_list.go:67] Installing "sample-templates/jenkins pipeline ephemeral"
I0130 11:44:41.273894    7215 apply_list.go:67] Installing "sample-templates/mysql"
I0130 11:44:41.274194    7215 apply_list.go:67] Installing "sample-templates/dancer quickstart"
I0130 11:44:41.272066    7215 apply_template.go:81] Installing "openshift-web-console-operator"
I0130 11:44:54.711865    7215 interface.go:41] Finished installing "sample-templates/mongodb" "sample-templates/mysql" "sample-templates/dancer quickstart" "sample-templates/rails quickstart" "sample-templates/mariadb" "sample-templates/postgresql" "sample-templates/cakephp quickstart" "sample-templates/django quickstart" "sample-templates/nodejs quickstart" "sample-templates/jenkins pipeline ephemeral" "sample-templates/sample pipeline"
I0130 11:44:55.855551    7215 interface.go:41] Finished installing "centos-imagestreams" "openshift-image-registry" "persistent-volumes" "openshift-router" "sample-templates" "openshift-web-console-operator"
Server Information ...
OpenShift server started.

The server is accessible via web console at:
    https://127.0.0.1:8443

WARNING: An HTTP proxy (gateway.docker.internal:3128) is configured for the Docker daemon, but you did not specify one for cluster up
WARNING: An HTTPS proxy (gateway.docker.internal:3129) is configured for the Docker daemon, but you did not specify one for cluster up
WARNING: A proxy is configured for Docker, however 172.30.1.1 is not included in its NO_PROXY list.
   172.30.1.1 needs to be included in the Docker daemon's NO_PROXY environment variable so pushes to the local OpenShift registry can succeed.
```

Sekarang coba buka [https://127.0.0.1:8443](https://127.0.0.1:8443) dan trush certificate maka hasilnya seperti berikut:

![okd-login]({{site.baseurl}}/assets/img/posts/openshift-on-premise-mac/openshift-login.png)

Untuk user kita bisa menggunakan `developer` dan passwordnya `developer` kemudian login, setelah login maka akan muncul halaman seperti berikut:

![openshift-catalog]({{site.baseurl}}/assets/img/posts/openshift-on-premise-mac/openshift-catalog.png)

## Management cluster openshift

Untuk management cluster kita bisa menggunakan command seperti berikut:

```bash
➜  ~ oc cluster --help
Manage a local OpenShift cluster 

The OpenShift cluster will run as an all-in-one container on a Docker host. The Docker host may be a
local VM (ie. using docker-machine on OS X and Windows clients), remote machine, or the local Unix
host. 

Use the 'up' command to start a new cluster on a docker host. 

To use an existing Docker connection, ensure that Docker commands are working and that you can
create new containers. 

Default routes are setup using nip.io and the host ip of your cluster. To use a different routing
suffix, use the --routing-suffix flag.

Usage:
  oc cluster ACTION [flags]

Available Commands:
  add         Add components to an 'oc cluster up' cluster
  down        Stop OpenShift on Docker
  status      Show OpenShift on Docker status
  up          Start OpenShift on Docker with reasonable defaults

Use "oc <command> --help" for more information about a given command.
Use "oc options" for a list of global command-line options (applies to all commands).
```