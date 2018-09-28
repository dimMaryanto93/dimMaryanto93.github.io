---
layout: post
title: "IF dan Switch-case pada pemograman Java"
date: 2016-08-05T23:34:07+07:00
author: Dimas Maryanto
comments: yes
category: java
tags:
- Java
- Netbeans
references:
---

Halo kembali lagi di blog saya, kali ini saya mau melanjutkan pembahasan struktur kendali percabangan atau kata lainnya yaitu seleksi. Di Java pada dasarnnya ada 2 yaitu ```IF``` dan ```SWITCH```, jadi di postingan kali ini kita tidak hanya menggunakan bahasa dari bahasa pemograman java tapi juga kita akan sedikit menggambarkan bagaimana alur supaya lebih paham dan bagaimana cara memvisualisasikan koding yang kita buat. Ok langsung aja ke topik yang pertama yaitu kita akan membahas tentang seleksi dengan ```IF```

<!--more-->

## Seleksi dengan ```IF```

berikut ini adalah sample code percabangan dengan ```if``` dengan kasus yang umum digunakan:

{% highlight java %}
int bil1 = 10;
int bil2 = -10;

if (bil1 == bil2) {
  System.out.println("10 == -10");
}else if(bil1 < bil2){
  System.out.println("10 < -10");
} else {
  System.out.println("lain-lain")
}
{% endhighlight %}

Pada dasarnya seleksi dengan IF ini kalo digambarkan ke diagram UML dengan activity diagram berdasarkan koding diatas jadi seperti berikut:

![Basic IF]({{ site.baseurl }}/assets/img/posts/java-selection/if-basic.jpg)

* Nah jadi disini kita mendefinisikan 2 variable integer yaitu ```bil1 = 10``` dan ```bil2 = -10```.
* Seleksi pertama yaitu ```bil1 == bil2``` jadi kita ubah ke bilangan jadi ```10 == -10``` hasilnya ```false``` karena 10 tidak sama dengan -10 maka perintah dalam ```(bil1 == bil2)``` dilewat atau tidak dijalankan.
* Seleksi ke dua yaitu ```bil1 < bil2``` jadi kita ubah ke bilangan jadi ```10 < -10``` hasilnya ```false``` karena 10 lebih besar dari -10 maka peritah dalam ```(bil1 < bil2)``` dilewat atau tidak dijakankan.
* ```else``` adalah pilihan terakhir artinya jika kedua seleksi gagal atau bernilai salah maka otomatis perintah dalam else akan dijalankan dengan begitu kesimpulannya adalah ```lain-lain```

Nah sekarang kita akan coba studi kasus yang lain, berikut codingannya:

{% highlight java %}
int bil = 10;
if (bil == 10) {
  System.out.println("yang ini diksekusi (==)");
} else if(bil % 2 = 0){
  System.out.println("yang ini diksekusi (%)");
} else{
  System.out.println("Lain-lain");
}
{% endhighlight %}

Sekarang coba tebak outputnya yang mana?

* ```yang ini diksekusi (==)```?
* ```yang ini diksekusi (%)```?
* ```Lain-lain```?

Ok, nah jadi jawabanya seperti berikut supaya lebih gampang kita buat dulu activity diagramnya dulu seperti berikut:

![IF yang aneh]({{ site.baseurl }}/assets/img/posts/java-selection/if-aneh.jpg)

Berikut penjelasnnya:

* ```bil = 10``` kemudian diseksi ```10 == 10``` maka hasilnya benar. ok kita udah tau ya jawabanya yang ini. tpi angap ja gak tau.
* ```bil = 10``` kemudian diseleksi ```10 % 2 = 0``` jadi 10 dibagi 2 sama dengan 5 sisanya khan 0 maka hasilnya benar tpi perintah didalamnya tidak akan diksekusi karena buat apa khan statement pertama udah benar dan sebenarnya statement ke 2 ini tidak dieksekusi lagi karena statement ke 1 udah bener jadi hasilnya adalah ```yang ini dieksekusi (==)```

Sekarang anda sudah tau ya konsep percabagan dengan ```IF``` jadi kesimpulannya dari kasus diatas adalah dimana hasil seleksinya pertama kali udah bener selanjutanya akan diabaikan, Selanjutnya kita bahas tentang seleksi dengan ```Switch-case```

## Peracabagan dengan ```Switch-Case```

Percabagan ```switch-case``` sebenarnya tidak jauh berbeda tpi disini kita akan lihat bagaimana cara kerja dari percabagan ini, berikut adalah contoh kasus sederhaan dari kodingnya sering kita jumpai yaitu seperti berikut:

{% highlight java %}
Integer ip = 4;
switch(ip){
  case 4: System.out.println("A"); break;
  case 3: System.out.println("B"); break;
  case 2: System.out.println("C"); break;
  case 1: System.out.println("D"); break;
  default: System.out.println("E");
}
{% endhighlight %}

Nah kasus diatas kita menentukan ip anda memiliki grade apa?

* A
* B
* C
* D
* E

Ok, supaya gampang seperti biasa kita visualisaksikan menggunakan diagram activity seperti berikut:

![Normal switch-case]({{ site.baseurl }}/assets/img/posts/java-selection/switch-basic.jpg)


Nah jadi hasilnya adalah ```A```, kenapa A jadi gini penjelasnnya:

* Membuat variable ```int ip = 4```, kemudian pada dasarnya sama seperti if yaitu kita memasukan expresinya kemudian si switch ini akan mencari mana value yang cocok
* Setelah menemukan case pertama yaitu ```case: 4``` maka dia akan menjalankan perintah didalamnya yaitu ```System.out.println("A")``` kemudian ada keyword ```break;``` ini artinya kita keluar dari seleksi. maka dari itu hasilnya ```4```

Selanjutnya sekarang kita modifikasi sedikin dari koding yang sama seperti berikut:

{% highlight java %}
Integer ip = 2;
switch(ip){
  case 4: System.out.print("A");
  case 3: System.out.print("B"); break;
  case 2: System.out.print("C");
  case 1: System.out.print("D"); break;
  default: System.out.print("E");
}
{% endhighlight %}

Nah sekarang apa outputnya? seperti biasa kita bisa gunakan diagram activity untuk mempermudah menggambarkan dari koding diatas seperti berikut:

![Switch Modif]({{ site.baseurl }}/assets/img/posts/java-selection/switch-modif.jpg)

Gimana jelaskan apa yang ada saya visualisaksikan dengan, mungkin anda bisa menafsirkan dengan bahasa diri sendiri bagaimana cara kerjanya, tapi disini saya juga bakalan menjelaskan tapi maaf kalo kata-katanya belibet hheehehe, maklum bukan anak bahasa yang pintar merangkai kata-kata.

* Ok jadi gini, pertama sama seperti tadi kita membuat dulu variable ```ip = 2```.
* Setelah itu si switch meriksa nih case mana yang sesuia.
* Ketemulah ```case : 2``` setelah itu dia akan menampilkan kelayar yaitu ```C``` setelah itu menampilkan lagi ke layar yaitu ```D``` setelah itu baru dia keluar karena ada keyword ```break;```.
* Kesimpulannya jadi hasilnya dalah ```CD```.

Ok mungkin cukup sekian dulu penjelasan mengenai cara mudah untuk membaca koding dengan mentranslate ke UML Diagrams - Activity diagrams. see you next post!.
