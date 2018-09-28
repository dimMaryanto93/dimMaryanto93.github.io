---
layout: post
title: "Konsep transaction dalam JDBC"
date: 2016-08-10T21:30:57+07:00
author: Dimas Maryanto
comments: yes
gist: dimMaryanto93/9670215f595e5415326e78df2d22977a
repository: https://github.com/dimMaryanto93/belajar-java-jdbc.git
category: java
tags:
- Java
- JDBC
- MariaDB
- Netbeans
references:
---

Halo kembali lagi di blog saya, kalo dilihat-lihat udah lumayan panjang topik tentang JDBC ini mulai dari [dasar CRUD]({% post_url 2016-07-31-java-jdbc %}) terus kita udah memisahkan antara model dengan struktur JDBC menggunakan [JavaBeans]({% post_url 2016-08-02-javabeans-jdbc %}) setelah itu kita melakukan modularisasi yang memisahkan antara struktural JDBC dengan menggunakan [Data Akses Objek]({% post_url 2016-08-06-modular-jdbc %}) atau orang banyak bilang (DAO) dan yang terakhir kita melakukan [externalisasi configurasi]({% post_url 2016-08-08-external-setup-jdbc %}) dengan properties. Nah jadi kali ini saya mau membahas tentang transaction, yang jadi pertanyaan apa itu transaction dan kenapa menggunakan transaction? padahal khan di materi sebelumnya khan it's work fine!!

<!--more-->

Jawabannya yang pertama, transaction itu kita bisa mengatur atau bisa mengontrol datanya semacam cache lah tapi di database contoh realnya misal kita punya multi query yang akan dikirimkan ke database tetapi salah satu datanya error maka kita bisa melakukan rollback. nah magsudnya rollback ini konsepnya sama seperti di game-game balapan salah satunya crash bandicoot (CTR) anak 90'an pasti tau game ini hehehe, tau khan checkpoint? jadi gini kalo kita melintas checkpoint itu maka setelah melewatinnya trus anda nabrak maka anda akan kembali lagi ke last checkpoint. Saya akan ilustrasikan perbedaan dengan menggunakan transaction dan yang tidak seperti pada gambar berikut ini:

![Perbedaan transaction jdbc dengan non transaction]({{ site.baseurl }}/assets/img/posts/transaction-jdbc/deferent-non-and-transaction.jpg)

Gambar sebelah kiri yaitu **tanpa menggunakan transaction** jadi kita punya 3 query yang dikirim ke database tapi pada saat query ke 2 database mendeteksi terjadi kesalahan sql maka query ke 2 tidak akan di simpan database sedangkan untuk query ke 1 dan ke 3 yaitu berhasil disimpan. Gambar sebelah kanan yaitu dengan **menggunakan transaksi**, jadi ini supaya keliatan jadi kita buat 2 blok commit. jadi ketika query 1 berhasil maka di commit kemudian query 2 dan query 3 berhasil maka dicommit. Namun disini kasusnya pada saat query ke 2 dia error maka hasilnya adalah query ke 3 dieksekusi tapi ada interupt yaitu di rollback maka query 2 dan query 3 tidak akan disimpan jadi kesimpulannya query pertama yang disimpan.

Selanjutnya kita basah pertanyaan yang ke dua yaitu kenapa menggunakan transaction, jawabannya adalah ada 3 point yaitu seperti berikut:

* Meningkatkan performa
* Seperti yang telah kita bahas tadi yaitu bisa mengontrol datanya apakah disimpan ke sebelumnya atau mau di rollback.
* Untuk memudahkan data integrity, magsudnya kalo punya mapping one-to-many jadi khan klo mau tambah data khan pasti harus input dulu data masternya baru detailnya jadi klo ada data detailnya yang error maka gak disimpan atau pun sebaliknya jadi ketika data masternya error maka detailnya gak akan disimpan.

Ok mungkin segitu dulu pembahasan tentang konsep JDBC Transactions selanjutnya kita akan implementasikan apa yang kita telah bahas di atas. see you next post!.
