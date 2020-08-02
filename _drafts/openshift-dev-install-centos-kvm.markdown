---
layout: post
title: "Install Openshift on-premise for development on CentOS 7 with KVM"
category: openshift
tags: 
- Centos7
- KVM
- Docker
- Kubernetes
- Redhad
- Openshift 
author: Dimas Maryanto
references:
- https://www.okd.io/download.html
- https://docs.docker.com/install/linux/docker-ce/centos/
- https://kubernetes.io
comments: true
---

![openshift-logo]({{site.baseurl}}/assets/img/posts/openshift-on-premise-mac/openshift-logo.png)

Untuk menginstall Openshift on-premise, di Server Development seperti Centos7 dengan spesifikasi

- CPU: 2 Cores
- RAM: 2Gb
- Storage: SSD 20Gb

Platform yang kita gunakan untuk mencoba Openshift ini adalah `OKD is a distribution of Kubernetes optimized for continuous application development and multi-tenant deployment` atau silahkan kunjungi [website](https://www.okd.io/)

<!--more-->

Ada 2 cara untuk menginstall Openshift client tools yaitu dengan `Run OKD in a Container` dan `Run the All-In-One VM with Minishift` tapi disini kita installnya dengan method yang paling simple aja ya...

Nah sekarang kita install Openshift di Mac OS, ada berapa yang kita butuhkan untuk menginstall yaitu:
- Update System / OS Centos7
- Install docker
    - Setup `network-insecure-registry`
- Install KVM
- Install kubernetes CLI / `kubectl`
- Install Openshift cluster / `oc`
- Install Minishift 
- Deploy simple web application with PHP

## Update system

Pertama kita harus update system dulu, dengan perintah seperti berikut: 

```bash
yum update
```

## Install docker

Kemudian kita install docker-engine

```bash
# install required packages
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2

# add docker centos repository
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

# install docker
sudo yum install docker-ce docker-ce-cli containerd.io
```

Kemudian kita setup docker

- Menambahkan docker insecure registry
- Start & Run as daemon


Edit / Membuat file `/etc/docker/daemon.json` seperti berikut:

```bash
{
  "experimental": false,
  "insecure-registries": [
    "172.30.0.0/16"
  ],
  "debug": true
}
```

Kemudian kita start & register docker service sebagai daemon seperti berikut:

```bash
# start docker
systemctl restart docker.service
# auto start docker at startup
systemctl enable docker.service
```

Validation docker installed dan run

```bash
[root@xxxxx ~]# docker info
Client:
 Debug Mode: false

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 0
 Server Version: 19.03.5
 Storage Driver: overlay2
  Backing Filesystem: xfs
  Supports d_type: true
  Native Overlay Diff: true
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: b34a5c8af56e510852c35414db4c1f4fa6172339
 runc version: 3e425f80a8c931f88e6d94a8c831b9d5aa481657
 init version: fec3683
 Security Options:
  seccomp
   Profile: default
 Kernel Version: 3.10.0-862.3.2.el7.x86_64
 Operating System: CentOS Linux 7 (Core)
 OSType: linux
 Architecture: x86_64
 CPUs: 2
 Total Memory: 1.791GiB
 Name: dimas-maryanto.com
 ID: 5GB4:KJBC:7474:D7XI:A4B4:KI5Y:JLDA:7BPH:I62I:UXFJ:3RMN:TNCO
 Docker Root Dir: /var/lib/docker
 Debug Mode: true
  File Descriptors: 21
  Goroutines: 35
  System Time: 2020-02-06T11:33:10.147592148+07:00
  EventsListeners: 0
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  172.30.0.0/16
  127.0.0.0/8
 Live Restore Enabled: false
```

## Install KVM on centos 7

Setelah itu kita install kvm, karena kita mau jalanin minishift di vm. nah vm yang kita pake kvm berikut perintahnya:

```bash
# Install libvirt and qemu-kvm on your system as root user
yum install qemu-kvm libvirt libvirt-python libguestfs-tools virt-install wget curl

# Start the libvirtd service
systemctl enable libvirtd
systemctl start libvirtd

# Add yourself to the libvirt group
usermod -a -G libvirt $(whoami)

# Update your current session to apply the group change
newgrp libvirt

# As root, install the KVM driver binary
curl -L https://github.com/dhiltgen/docker-machine-kvm/releases/download/v0.10.0/docker-machine-driver-kvm-centos7 -o /usr/local/bin/docker-machine-driver-kvm 

# make it executable 
chmod +x /usr/local/bin/docker-machine-driver-kvm
```