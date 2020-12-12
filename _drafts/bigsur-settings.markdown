---
layout: post
title: "macOS BigSur Settings"
date: 2020-12-12T14:23:13+07:00
category: mac-os
tags: 
- MacOS BigSur
author: Dimas Maryanto
references:
- https://www.apple.com/id/macos/big-sur/
comments: true
image_path: /assets/img/posts/mac-os-bigsur-settings
---

![bigsur]({{site.baseurl}}{{page.image_path}}/bigsur.png){:width="400px"}


Akhirnya, Apple telah me-release operation system untuk Mac Pro / iMac / Mac Mini / Macbook yaitu dengan code name `macOs BigSur`. Jadi pembahasan kali ini yaitu setelah kita fresh install macOs BigSur apa yang harus dilakukan?

<!--more-->

Beberapa hal yang harus di setting untuk kebutuhan sehari diantarnya:

- Setting Trackpad (multi-gesture, three finger, dan lain-lain)
- Setting Security (enabled firewall, dan lain-lain)
- Setting top panel (show battery persentage, clock dan lain-lain)
- Setting iTunes / Music
- Setting User & Group
- Install software kebutuhan sehari-hari.

## Trackpad

Trackpad itu perannya penting buat saya, karena saya gak pake mouse external. ada beberapa feature yang meningkatkan productive seperti multi-gesture, three finger drag, tap to click. berikut adalah konfurasinya

- Enabled Three finger drag

    Untuk mengaktifkan three finger drag kita bisa masuk ke menu `Settings` -> `Accessiblity` -> `Pointer Control` -> `Trackpad Options...` kemudian aktifkan `Enable dragging` pilih `three finger drag` seperti berikut:

    ![three finger drag]({{site.baseurl}}{{page.image_path}}/touchpad/trackpad-option.png)

- Enable tap to click

    Supaya trackpad gak terlalu berisik `cetak-cetuk-cetok` kita bisa aktifin feature `tap to click` di menu `Settings` -> `Trackpad` -> tab `Point & Click` -> checklist `Tap to click`

    ![tap to click]({{site.baseurl}}{{page.image_path}}/touchpad/tap-to-click.png)

- Multi Gesture Trackpad

    Untuk settings multi-gesture di macbook saya saya buat seperti berikut:

    ![multi-gesture]({{site.baseurl}}{{page.image_path}}/touchpad/multi-gesture.png)

## Security

Untuk konfigurasi security, bisanya saya selalu mengaktifkan firewall dan juga FileVault seperti ini confignya. dan yang karena saya pake Apple Watch series 5 kita juga bisa unlock Macbook kita menggunakan Double Tap apple watch supaya gak masukin password lagi

- Enabled Unlock by Apple Watch

    Untuk meng-enable unlock by Apple Watch pastikan apple watch dalam keadaan unlock atau digunakan, setelah itu kita masuk ke `Settings` -> `Security` -> tab `General` -> checklist `Use your Apple watch to unlock apps and your Mac`

    ![unlock by apple watch]({{site.baseurl}}{{page.image_path}}/security/general.png)

- Enabled Firewall

    Untuk meng-enable firewall masuk ke menu `Settings` -> `Security` -> tab `Firewall` -> `Turn On Firewall` sampai status firewall menjadi warna hijau seperti berikut:

    ![firewall]({{site.baseurl}}{{page.image_path}}/security/firewall.png)

## Sidecar

Kemudian karena saya juga pengguna iPad Pro 11" (2018), bisanya juga aku menggunakan sebagai secondary display di Mac, Untuk settingnya biasanya saya mau full layar jadi untuk Touchbar dan Sidebar aku matiin. untuk mendisable kita ke menu -> `Settings` -> `Sidecar` -> kemudian kita uncheck untuk `Show Sidebar` dan `Show Touch Bar` seperti berikut:

![sidecar]({{site.baseurl}}{{page.image_path}}/sidecar/sidecar.png)

## Top Panel / Dock

Jika temen-temen perhatikan ada yang hilang khak dengan top panel? jika ya brati temen-temen sama seperti saya yang merasa dimana untuk menampilkan status battery dalam persen. jawabanya 

- Enabled Battery persentage

    Untuk menampilkan status persen battery kita bisa ke menu `Settings` -> `Dock & Menu Bar` -> pada list item pilih -> `Battery` -> Nah disinilah kita bisa menampilakan persentage battery seperti berikut:

    ![persentage-battery]({{site.baseurl}}{{page.image_path}}/dock/battery.png)

- Set clock 24 hour

    Untuk menampilkan Tanggal dan jam 24 jam kita bisa ke menu `Settings` -> `Dock & Menu Bar` -> pilih list item `Clock` seperti berikut:

    ![clock]({{site.baseurl}}{{page.image_path}}/dock/clock.png)

## User & Group

Selain Konfigurasi tersebut biasanya saya juga mengaktifkan auto startup aplikasi seperti email, whatapp, telegram dan lain-lain yang berkaitan dengan chat jadi gak perlu buka satu2. kita bisa aktifkan di menu `Settings` -> `User & Group` -> pilih `Login item` -> kemudian kita bisa tambahkan aplikasi yang kita mau ketika Mac dihidupkan dan di load.

![login items auto startup]({{site.baseurl}}{{page.image_path}}/user-group/login-items.png)