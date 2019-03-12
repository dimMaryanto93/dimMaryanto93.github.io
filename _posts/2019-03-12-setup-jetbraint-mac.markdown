---
layout: post
title: "Setup / Plugins / Configuration for IntelliJ IDEA on mac"
date: 2019-03-12T11:35:09+07:00
category: jetbraint
tags: 
- Jetbraint
- IntelliJ IDEA
- Java
author: Dimas Maryanto 
references:
- https://www.jetbrains.com/idea/features/
- https://blog.jetbrains.com/idea/category/tips-tricks/
comments: true
imageDir: /assets/img/posts/idea-conf
---

![jetbraint idea]({{site.baseurl}}{{page.imageDir}}/display.png){: width="500px" }

Berikut adalah beberapa configurasi/plugins/setup Jetbraint IntelliJ IDEA

<!--more-->

## Plugins

Berikut ada beberapa plugin yang recommended untuk di install yaitu

- [Ignore](https://plugins.jetbrains.com/plugin/7495--ignore), Biasanya digunakan untuk git, docker dan lain-lain untuk membuat template ignore file seperti `.gitignore`
- [BashSupport](https://plugins.jetbrains.com/plugin/4230-bashsupport), Biasanya digunakan untuk membuat bash / Unix Script atau kebutuhan devops.
- [CSV Plugin](https://plugins.jetbrains.com/plugin/10037-csv-plugin), Sebagai editor CSV, Excel dan sejenisnya
- [Flyway Migration Creation](https://plugins.jetbrains.com/plugin/8597-flyway-migration-creation), Sebagai generator file sql migration untuk [flyway](https://flywaydb.org)
- [Genymotion](https://plugins.jetbrains.com/plugin/7269-genymotion), Sebagai alternatif emulator android
- [IDEA Restart](https://plugins.jetbrains.com/plugin/5892-idea-restart), Tambahan menu restart di IntelliJ IDEA
- [Lombok Plugin](https://plugins.jetbrains.com/plugin/6317-lombok-plugin), Plugin untuk mengkatifkan pre-compile lombok di IntelliJ IDEA
- [PhoneGap/Cordova Plugin](https://plugins.jetbrains.com/plugin/7436-phonegap-cordova-plugin), Support build PhoneGap/Cordova
- [Presentation Assistant](https://plugins.jetbrains.com/plugin/7345-presentation-assistant), Untuk menambahkan fitur show shortcut
- [WakaTime](https://plugins.jetbrains.com/plugin/7425-wakatime), Tracking management
- [Docker integration](https://plugins.jetbrains.com/plugin/7724-docker-integration), Docker plugin di IntelliJ IDEA dengan menggunakan GUI atau setara dengan [Kitematic](https://kitematic.com)

## Configuration JVM

Untuk spec Macbook Pro 13" (2017, i5 dual core dan 8gb ram) berikut configurasi JVM yang saya sarankan, Edit `Configure` -> `Edit Custome VM Options...`

```conf
-Xms256m
-Xmx1024m
-XX:ReservedCodeCacheSize=240m
-XX:+UseCompressedOops
-Dfile.encoding=UTF-8
-XX:+UseConcMarkSweepGC
-XX:SoftRefLRUPolicyMSPerMB=50
-ea
-Dsun.io.useCanonCaches=false
-Djava.net.preferIPv4Stack=true
-Djdk.http.auth.tunneling.disabledSchemes=""
-XX:+HeapDumpOnOutOfMemoryError
-XX:-OmitStackTraceInFastThrow
-Xverify:none

-XX:ErrorFile=$USER_HOME/java_error_in_idea_%p.log
-XX:HeapDumpPath=$USER_HOME/java_error_in_idea.hprof
```

## Setup Preperences

Menu `Configure` -> `Preperences` -> `Preperences & Behavior` :

- `Apperance`

    ![apperance setup]({{site.baseurl}}{{page.imageDir}}/apperance-windows-options.png)

- `System Settings`

    ![System Settings]({{site.baseurl}}{{page.imageDir}}/system-settings.png)

- `Persentation Assist`

    ![Persentation assist]({{site.baseurl}}{{page.imageDir}}/persentation-assist.png)

Menu `Configure` -> `Preperences` -> `Editors`

- `General` -> `text wrap`

    ![menu general]({{site.baseurl}}{{page.imageDir}}/general-text-wrap.png)

- `General` -> `Auto Imports`

    ![auto import]({{siste.baseurl}}{{page.imageDir}}/java-auto-import.png)

- `General` -> `Editor Tabs`

    ![tabs]({{site.baseurl}}{{page.imageDir}}/editor-tabs.png)

- `Font`

    ![font source code]({{site.baseurl}}{{page.imageDir}}/font-editor.png)

- `Color Schema` -> `Language Default`

    ![highlight schema]({{site.baseurl}}{{page.imageDir}}/highlight-schema-color.png)

Menu `Configure` -> `Preperences` -> `Other Settings`

- `Flyway Migration`

    ![flyway file generator]({{site.baseurl}}{{page.imageDir}}/flyway-migration.png)

