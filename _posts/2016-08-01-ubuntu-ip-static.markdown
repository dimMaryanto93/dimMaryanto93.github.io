---
layout: post
title: "Setting Static/DHCP on Ubuntu 16.04 LTS"
date: 2016-08-01T09:01:56+07:00
category: ubuntu-16.04
tags: 
- Networking
- Ubuntu
- TCP/IP
author:
---

Halo, saat pertama kali masuk kantor saya ditugasin untuk ngurusin Server, 
dan servernya ternyata masih belum terinstall OS, alhasil saya harus setup sendiri server tersebut
untuk kebutuhan Development dikantor. Saya putuskan untuk menggunaan UBuntu Server 16.04 LTS karena setelah baca2 di inet
UBuntu 16.04 LTS udah lumayan stabil. Setelah itu saya install lah ke hdd server tersebut, kemudian
saya menemukan kendala yaitu gak bisa konek Inet terus bagaimana solusinya?....

<!--more-->

Setelah saya teliti, Untuk beberapa server product IBM ternyata nama interfacenya bukan eth0 atau eth1 tapi melainkan eno1 nih 
spesifikasinya:

{% highlight bash %}
ukabima@ubuntuukabima:~$ ifconfig eno1
eno1      Link encap:Ethernet  HWaddr 6c:ae:8b:59:46:f2  
          inet addr:XXX.XXX.XXX.XXX  Bcast:XXX.XXX.XXX.XXX  Mask:255.255.255.0
          inet6 addr: fe80::6eae:8bff:fe59:46f2/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:988793 errors:0 dropped:69 overruns:0 frame:0
          TX packets:588496 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:325855817 (325.8 MB)  TX bytes:306911657 (306.9 MB)
          Interrupt:17 
{% endhighlight %}

atau 

{% highlight bash %}
ukabima@ubuntuukabima:~$ sudo lshw -class network
*-network:0             
       description: Ethernet interface
       product: NetXtreme BCM5717 Gigabit Ethernet PCIe
       vendor: Broadcom Corporation
       physical id: 0
       bus info: pci@0000:06:00.0
       logical name: eno1
       version: 10
       serial: 6c:ae:8b:59:46:f2
       size: 1Gbit/s
       capacity: 1Gbit/s
       width: 64 bits
       clock: 33MHz
       capabilities: pm vpd msi msix pciexpress bus_master cap_list ethernet physical tp 10bt 10bt-fd 100bt 100bt-fd 1000bt 1000bt-fd autonegotiation
       configuration: autonegotiation=on broadcast=yes driver=tg3 driverversion=3.137 duplex=full firmware=5717-v1.63 NCSI v1.2.33.0 ip=10.1.1.50 latency=0 link=yes multicast=yes port=twisted pair speed=1Gbit/s
       resources: irq:17 memory:82a50000-82a5ffff memory:82a40000-82a4ffff memory:82a30000-82a3ffff
  *-network:1
       description: Ethernet interface
       product: NetXtreme BCM5717 Gigabit Ethernet PCIe
       vendor: Broadcom Corporation
       physical id: 0.1
       bus info: pci@0000:06:00.1
       logical name: eno2
       version: 10
       serial: 6c:ae:8b:59:46:f3
       capacity: 1Gbit/s
       width: 64 bits
       clock: 33MHz
       capabilities: pm vpd msi msix pciexpress bus_master cap_list ethernet physical tp 10bt 10bt-fd 100bt 100bt-fd 1000bt 1000bt-fd autonegotiation
       configuration: autonegotiation=on broadcast=yes driver=tg3 driverversion=3.137 firmware=5717-v1.63 NCSI v1.2.33.0 latency=0 link=no multicast=yes port=twisted pair
       resources: irq:18 memory:82a20000-82a2ffff memory:82a10000-82a1ffff memory:82a00000-82a0ffff
  *-network
       description: Ethernet interface
       physical id: 1
       logical name: enp0s26u1u3u5
       serial: 6e:ae:8b:59:46:f7
       capabilities: ethernet physical
       configuration: broadcast=yes driver=cdc_ether driverversion=22-Aug-2005 firmware=CDC Ethernet Device ip=169.254.95.120 link=yes multicast=yes
{% endhighlight %}

Nah sekarang kita harus daftarkan dulu name dari interface network tersebut ke listener networkingnya, dengan cara
ubah file `/etc/network/interfaces` dengan menggunakan text-editor contohnya `vim` atau `namo` atau `gedit` seperti berikut:

{% highlight bash %}
ukabima@ubuntuukabima:~$ sudo -i
[sudo] password for ukabima: # enter password sebagai root

root@ubuntuukabima:~# vim /etc/network/interfaces
{% endhighlight %}

Setelah itu edit menjadi seperti berikut:

## IP Static

{% highlight ini %}
source /etc/network/interfaces.d/*

# set enable network interface eno1
auto eno1
# set network interface static
iface eno1 inet static
# set IP Address
address 10.1.1.50
# set Subnet Mask sesuai dengan class IP Address yaitu kelas A
netmask 255.0.0.0
# set gateway di arahkan ke mana contohnya ke router
gateway 10.1.1.1
# set dns contohnya ke google yaitu 8.8.8.8 atau 8.8.4.4
dns-nameservers 8.8.8.8
{% endhighlight %}

## IP DHCP 

{% highlight ini %}
source /etc/network/interfaces.d/*

# set enable network interface eno1
auto eno1
# set network interface dhcp
iface eno1 inet dhcp
{% endhighlight %}

ok sekarang tinggal Save dan Exit, Lalu di restart networknya dengan cara seperti berikut:

{% highlight bash %}
sudo ifup eno1 && ifdown eno1
{% endhighlight %}

Atau di Restart 

{% highlight bash %}
sudo reboot
{% endhighlight %}

Sekarang tinggal kita check dengan menggunakan ping ke google 

{% highlight bash %}
ukabima@ubuntuukabima:~$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=58 time=26.3 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=58 time=26.0 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=58 time=26.4 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=58 time=26.4 ms
64 bytes from 8.8.8.8: icmp_seq=5 ttl=58 time=26.4 ms
64 bytes from 8.8.8.8: icmp_seq=6 ttl=58 time=26.3 ms
64 bytes from 8.8.8.8: icmp_seq=7 ttl=58 time=26.5 ms
64 bytes from 8.8.8.8: icmp_seq=8 ttl=58 time=25.9 ms
^C
--- 8.8.8.8 ping statistics ---
8 packets transmitted, 8 received, 0% packet loss, time 7008ms
rtt min/avg/max/mdev = 25.961/26.328/26.565/0.250 ms
{% endhighlight %}

Klo tidak ada Request Time Out berarti udah bisa koneksi dengan Internet, jadi bisa update system dengan tenang :) ...
