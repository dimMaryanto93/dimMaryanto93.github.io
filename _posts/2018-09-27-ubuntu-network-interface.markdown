---
layout: post
title: "Ubuntu 18.04, Setup network Static/DHCP ip-address"
date: 2018-09-27T23:26:28+07:00
category: ubuntu-18.04
tags: 
- Ubuntu
- Network
- TCP
- Ip Address
author: Dimas Maryanto
references:
- https://help.ubuntu.com/lts/serverguide/network-configuration.html.en
- https://netplan.io/
comments: true
---

Ada beberapa perubahan, yang lumayan signifikan di Ubuntu 18.04 dan Ubuntu Server 18.04 untuk setting networking seperti [postingan sebelumnya]({% post_url 2016-08-01-ubuntu-ip-static %}). 

<!--more-->

Pertama make sure dulu networking ter-plugin dengan baik, dengan perintah berikut:

```bash
sudo lshw -class network

# *-network                 
#    description: Ethernet interface
#    product: Killer E220x Gigabit Ethernet Controller
#    vendor: Qualcomm Atheros
#    physical id: 0
#    bus info: pci@0000:03:00.0
#    logical name: enp3s0
#    version: 13
#    serial: d4:3d:7e:da:4a:d8
#    size: 100Mbit/s
#    capacity: 1Gbit/s
#    width: 64 bits
#    clock: 33MHz
#    capabilities: pm pciexpress msi msix bus_master cap_list ethernet physical tp 10bt 10bt-fd 100bt 100bt-fd 1000bt-fd autonegotiation
#    configuration: autonegotiation=on broadcast=yes driver=alx duplex=full ip=192.168.88.50 latency=0 link=yes multicast=yes port=twisted pair speed=100Mbit/s
#    resources: irq:19 memory:f7100000-f713ffff ioport:d000(size=128)
```

Nah sekarang kita tau informasi tentang network kita, seperti 
- `logical name: enp3s0`, 
- `description: Ethernet interface` dan 
- `serial: d4:3d:7e:da:4a:d8`.

Nah sekarang saatnya configurasi network static/DHCP

## Static IP-ADDRESS

Edit file `/etc/netplan/01-network-manager-all.yaml` menjadi seperti berikut:

```yml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp3s0:
      addresses: [192.168.88.50/24] # ip address static
      dhcp4: no # matikan dhcp4
      dhcp6: no
      gateway4: 192.168.88.1 # route gateway to modem                     
      nameservers:
        addresses: [8.8.8.8,8.8.4.4] # dns server
      optional: false
```

## DHCP IP-ADDRESS

Edit file `/etc/netplan/01-network-manager-all.yaml` menjadi seperti berikut:

```yml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp3s0:
      dhcp4: yes
      dhcp6: no
      optional: false
```

Setelah semua dikonfigurasi, kita **reload / apply network konfigurasinya** dengan perintah:

```bash
sudo netplan apply
```