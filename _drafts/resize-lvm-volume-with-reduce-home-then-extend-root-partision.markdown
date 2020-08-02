---
layout: post
title: "Merge LVM Volume `/home` then extend to `/` partision"
category: linux
tags: 
- LVM
- Partision
- Linux
- Centos 7
author: Dimas Maryanto
references:
- https://www.tecmint.com/extend-and-reduce-lvms-in-linux/
comments: true
---

Storage server full like super-super done? 

![df -h]({{site.baseurl}}/assets/img/posts/resize-lvm/1-df-h.png)

thanks to LVM can handle it, First make sure your system using LVM / LVM encripted. 

My Server currently running configuration:

- Centos OS 7
- HDD 20 GB
- IP Server: `192.168.88.238`
- 1x Physical Volume `pv`
- 1x Volume Group `vg`
- 3x Logical Volumn `lv`

```bash
[root@localhost ~]# pvscan
  PV /dev/sda2   VG cl              lvm2 [<19.00 GiB / 4.00 MiB free]
  Total: 1 [<19.00 GiB] / in use: 1 [<19.00 GiB] / in no VG: 0 [0   ]
[root@localhost ~]# pvs
  PV         VG Fmt  Attr PSize   PFree
  /dev/sda2  cl lvm2 a--  <19.00g 4.00m
[root@localhost ~]# vgs
  VG #PV #LV #SN Attr   VSize   VFree
  cl   1   3   0 wz--n- <19.00g 4.00m
[root@localhost ~]# lvs
  LV   VG Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  home cl -wi-ao----  6.99g
  root cl -wi-ao---- 10.00g
  swap cl -wi-ao----  2.00g
```

In this case, saya mau resize `/home` menjadi `3GB` partision kemudian di tambahkan ke `/` berikut caranya:

- backup partision `/home` to any other device
- unmount `/home`
- remove volumn `cl-home` lvm
- create volumn `/home` with size `3GB`
- Restore & put back in
- Extend volumn `cl-root` with `100%FREE`

<!--more-->

Sekarang kita resize volumn `cl-home` pertama kita unmount volumenya dengan perintah sebagai berikut:

```bash
umount -fl /dev/mapper/cl-home
```