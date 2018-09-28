---
layout: post
title: "Perbedaan List, Set dan Map?"
date: 2016-08-05T11:14:22+07:00
author: Dimas Maryanto
comments: yes
gist: dimMaryanto93/e1412b48ea8eb1512c9d2beb94324c1a
category: java
tags:
- Java
- Netbeans
- JDK-1.8
references:
- http://www.tutorialspoint.com/java/java_arrays.htm
- http://www.tutorialspoint.com/java/java_collections.htm
- https://docs.oracle.com/javase/tutorial/collections/intro/
- https://docs.oracle.com/javase/8/docs/technotes/guides/collections/overview.html
---

Halo kali ini saya mau membahas tentang element collection di Java tapi sebelum itu kita harus tau dulu apa sih element collection atau Framework collection.

> Framework Collection is unified architecture for representing and manipulating collections,
enabling collections to be manipulated independently of implementation details.

Jadi intinya framework collection adalah sama seperti array namun kita dapat dengan mudah melakukan manipulasi element atau value (dependencies) dari sebuah collection tersebut contohnya menambahkan, hapus, merubah dan lain-lain. contohnya kija jaman dahulu, **kita balik lagi jaman purba menggunakan Array** kita melakukan deklarasi variable dengan tipe array yang dimensi nya talah kita sebutkan seperti berikut:

{% highlight java %}
String[10] arg = {"Dimas", "Hanif"};
{% endhighlight %}

Jika kita nambahkan objek ke ```12```, apa yang terjadi? the answer is ```IndexOutOfBoundException``` artinya indexnya tidak cukup. kemudian klo kita mau merubah objek pada element tertentu ini kita harus menuliskan indexnya juga seperti berikut:

{% highlight java %}
// merubah value Hanif menjadi Riansyah
arg[1] = "Riansyah"
{% endhighlight %}

dan masih banyak lagi...

<!--more-->

Nah maka dari itu para profesor Java membuat framework yang mereka sebut Collection. Framework collection ini jaman sekarang sudah sangat pintar karena tidak hanya untuk tipe data default Java saja (String, Integer, Boolean dan lain-lain) tetapi untuk Class, Interface dan lain-lain juga bisa yaitu dengan menggunakan fitur **Java Generics** tpi by the way disini saya tidak akan membahas lebih lanjut tentang Generics, silahkan baca [artikel ini](http://www.tutorialspoint.com/java/java_generics.htm)

## Macam-macam framework collections

Sebenarnya framework collection ini ada lumayan banyak tapi basicnya ada 3 yaitu

* List
* Set
* Map

kelas-kelas tersebut adalah sebuah interface yang biasanya diimplemtasi menggunakan klo ```List``` biasanya ```java.util.ArrayList``` klo ```Set``` biasanya ```java.util.HashSet``` dan sedangkan ```Map``` adalah ```java.util.HashMap```.

### Collection List

```List``` pada dasarnya ini sama seperti array biasa karena memiliki key yang bersifat index ```{0, 1, 2, 3 dst}``` ingat index di Java berawal dari angka ```0```. Penggunaan sederhanya kurang lebih seperti berikut:

{% gist page.gist BelajarList.java %}

Jika dijalnkan maka outputnya seperti berikut:

{% highlight bash %}
Email pertama: software.dimmaryanto@hotmail.com
Email setelah diset: dimmaryanto@gmail.com
Jumlah email: 0
{% endhighlight %}

Dan berikut adalah penjelasnnya berdasarkan koding atas:

* Baris ke 4: Membuat variable dengan tipe data ```java.util.List``` kemudian saya menggunakan generics dengan tipe data ```java.lang.String``` yang tidak lain adalah ```String```, jadi kesimpulanya variable ```daftarEmails``` hanya bisa diisi dengan value ```String```. Kalo di array sama seperti kita membuat seperti ini ```String[] daftarEmails = {};```
* Baris ke 6: Menambahkan value ke variable ```daftarEmails``` dengan memanggil fungsi ```add(value)```
* Baris ke 7: Kita menampilkan element ke ```0``` yang tidak lain hasilnya adalah ```software.dimmaryanto@hotmail.com```
* Baris ke 9: Saya mau merubah value pada element ke ```0```, artinya nilai ```software.dimmaryanto@hotmail.com``` akan di replace dengan ```dimmaryanto@gmail.com```
* Baris ke 10: Kita menampilkan element ke ```0``` yaitu ```dimmaryanto@gmail.com``` bukan lagi ```software.dimmaryanto@hotmail.com``` karena pada baris ke 9 kita telah merubah nilainya dengan memanggil fungsi ```set(index, nilai_baru)```.
* Baris ke 12: Saya menghapus element ke 0, jadi setelah dieksekusi kita hanya memiliki sebuah array kosong.
* Baris ke 13: Menampilkan jumlah email : 0, karena kita tadi pada baris ke 10 telah menghapus element satu-satunya pada variabel ```daftarEmails```.

Setelah ```List``` kita masuk ke pembahasan selanjutnya yaitu ```Set```.

## Collection Set

Sama halnya seperti ```List``` tapi ```Set``` ini valuenya tidak boleh ada yang sama, klo ada yang sama maka value yang terakhir akan di upadate menjadi yang baru tapi tetap key menggunakan index. berikut kurang lebih kasusnya:

{% gist page.gist BelajarSet.java %}

Setelah itu coba anda jalankan, maka berikut outputnya:

{% highlight bash %}
Jumlah email setelah add ke 1: 1
Jumlah email setelah add ke 2: 1
Jumlah email setelah add ke 3: 1
{% endhighlight %}

Dan berikut adalah pembahasannya:

* Baris ke 4: Membuat variable dengan tipe data ```java.util.Set``` menggunakan generics dengan tipe data ```String```.
* Baris ke 6, 9, 12: Menambahkan element ke variable ```daftarEmails``` dengan value yang sama yaitu ```software.dimmaryanto@hotmail.com```.
* Baris ke 7, 10, 13: Menampilkan jumlah element setelah memanggil fungsi ```add(value)```, ini akan menghasilkan angka ```1``` kenapa ```1```? padahal saya menambahkannya sebangak ```3x```. seperti pembahasan diatas klo menggunakan objek ```Set``` maka nilai yang sama tidak akan ditambahkan kembali atau bahasa kerennya **tidak ada value yang duplikat atau ganda**.

Ok, sampai sini anda telah mempelajari 2 element collections yaitu ```List``` dan ```Set```. Sekarang kita lanjutkan ke element selajutnya yaitu ```Map```.

## Collection Map

The last one, kalo ```Map``` ini berbeda dengan 2 hal tadi. tetapi intinya tetap sama yaitu dapat menyimpan data lebih dari satu. jadi beda bagaimana?

```Map``` ini dia keynya bisa ditentukan oleh kita, magsudnya bebas gitu? yap tapi dalam tipe data yang sama contohnya ```String``` jadi keynya boleh angka, huruf dan lain-lain, klo Integer berati harus integer juga dst. jadi kalo saya gambarkan ke tabel kurang lebih kayak gini:

| key             | value           |
| :-------------  | :-------------  |
| "001"           | "Product 001"   |
| "P102"          | "Product 002"   |
| "-123"          | "Product 002"   |

nah itukan bayanganya berikut ini implementasinya:

{% gist page.gist BelajarMap.java %}

Sekarang coba anda jalankan, maka hasilnya akan seperti berikut:

{% highlight bash %}
Nilai 001 adalah Product 001
{% endhighlight %}

Dan berikut adalah penjelasannya:

* Baris ke 4: Membuat variable dengan tipe data ```java.util.Map``` dan menggunakan generics dengan 2 parameters yaitu keduanya bernilai ```String``` magsudnya adalah param pertama menamdakan key dan param kedua adalah valuenya, jadi kesimpulanya adalah **key dan valuenya bernilai ```String```**
* Baris ke 6: Nambahkan element dengan key ```001``` dan valuenya ```Product 001```
* Baris ke 8: kurang lebih sama seperti beris ke 6
* Baris ke 10: Menampilkan element dengan key ```001``` maka hasilnya adalah ```Product 001```.

Ok, mungkin sekian dulu postingan tentang element collection. see you next post!.
