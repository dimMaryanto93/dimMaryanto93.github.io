---
layout: post
title: "Tipe data, Variabel dan Scope di Bahasa"
date: 2016-07-31T00:10:30+07:00
category: java
tags: 
- Java Fundamental
author: Dimas Maryanto
references:
- https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html
- https://docs.oracle.com/javase/tutorial/java/nutsandbolts/variablesummary.html
comments: true
---

Halo salam programmer, kali ini saya akan menjelakan tentang tipe data, variable dan scope di bahasa pemograman Java. Seperti yang kita ketahui variabel digunakan untuk menampung nilai atau bahasa kerennya _value_. Di Java variabelnya lumanyan banyak dan memiliki sifatnya masing-masing, contohnya klo bilangan bulat harus Integer sedangkan bilangan pecahan yang bisa dibilang kecil menggunakan Float dan untuk bilangan pecahan besar menggunakan Double binggung? gak lah ya gampang kok sebenarnya ini cuman masalah kebiasaan aja kok. Nah jadi pada dasarnya variable di Java ada 4 jenis yaitu

<!--more-->

- Number
    * Short
    * Integer
    * Long
    * Float
    * Double
- String
    - Character
    - Object String
- Date
    - Timestamp
    - Date
    - Time
    - Month
    - Year
    - Day
- Object

Gimana lumayan banyak khan? tapi kali ini saya tidak akan membahas semua tapi hanya sebagian saja yang penting adalah anda tau cara penggunaanya dan fungsinya seperti apa.


## Tipe data Integer

Tipe data integer ini pada dasarnya adalah data yang hanya dapat menampung nilai atau bilangan positif dan negatif dengan jangka ```-2,147,483,648``` sd ```2,147,483,648```. ok sekarang kita buat kodingnya:

```java
public class VariableInteger {
  public static void main(String[] args) {
    Integer bilPositif = 10;
    Integer bilNegatif = -5;
    Integer pertambahan = bilPositif + bilNegatif;
    System.out.println("Bilangan Positif : "+ bilPositif);
    System.out.println("Bilangan Negatif : "+ bilNegatif);
    System.out.println("Perjumlahan antara bilPositif dan bilNegatif adalah "+ pertambahan);
  }
}
```

Berikut outputnya:

{% highlight bash %}
Bilangan Positif : 10
Bilangan Negatif : -5
Perjumlahan antara bilPositif dan bilNegatif adalah 5
{% endhighlight %}

Jadi penjelasnya seperti berikut:

* kita membuat tiga buah variable dengan tipe data ```Integer``` yaitu ```bilPositif```, ```bilNegatif``` dan ```pertambahan```
* ```bilPositif``` diisi dengan nilai ```10```, ```bilNegatif``` diisi dengan nilai ```-5``` dan ```pertambahan``` diisi dengan hasil perjumlahan ```10 + (-5)``` atau sama dengan ```10 - 5``` jadi hasilnya adalah ```5```.
* kemudian ditampilkan menggunakan fungsi ```println()```.

## Tipe data Double

Tipe data Double ini pada dasarnya adalah sama seperti ```Integer``` namun memiliki kelebihan yaitu berbentuk pecahan dengan jangka ```-1.7976931348623157E308``` sd ```1.7976931348623157E308```. ok sekarang kita buat kodingnya:

```java
public class VariableDouble {
  public static void main(String[] args) {
    Double bilPositif = new Double(10);
    Double bilNegatif = -5.4;
    Double pertambahan = bilPositif + bilNegatif;
    System.out.println("Bilangan Positif : "+ bilPositif);
    System.out.println("Bilangan Negatif : "+ bilNegatif);
    System.out.println("Perjumlahan antara bilPositif dan bilNegatif adalah "+ pertambahan);
  }
}
```

Berikut outputnya:

{% highlight bash %}
Bilangan Positif : 10.0
Bilangan Negatif : -5.4
Perjumlahan antara bilPositif dan bilNegatif adalah 4.6
{% endhighlight %}

Berikut penjelasnya:

* Untuk penjelasannya kurang lebih sama kayak variable dengan tipe integer hanya berbeda hasilnya saja.
* klo menggunakan ```Double``` meskipun ```bilPositif``` bernilai ```10``` tetap saja memiliki pecahan yaitu ```.0``` jadi ```10.0``` jadi klo ```bilPositif - bilNegatif``` maka hasilnya akan ```4.6``` karena ```10.0 - 5.4 = 4.6```

## Tipe data Float

Tipe data Float ini pada dasarnya adalah sama seperti ```Double``` namun memiliki jangka lebih rendah yaitu ```1.4E-45``` sd ```3.4028235E38```. ok sekarang kita buat kodingnya:

```java
public class VariableFloat {
  public static void main(String[] args) {
    float bilPositif = new Float(10);
    float bilNegatif = -5.4F;
    float pertambahan = bilPositif + bilNegatif;
    System.out.println("Bilangan Positif : "+ bilPositif);
    System.out.println("Bilangan Negatif : "+ bilNegatif);
    System.out.println("Perjumlahan antara bilPositif dan bilNegatif adalah "+ pertambahan);
  }
}
```

Berikut outputnya:

{% highlight bash %}
Bilangan Positif : 10.0
Bilangan Negatif : -5.4
Perjumlahan antara bilPositif dan bilNegatif adalah 4.6
{% endhighlight %}

berikut adalah penjelasanya:

* Untuk hasilnya memang tidak ada bedanya dengan ```Double``` hanya yang perlu diingat adalah ```Float``` ini memiliki nilai yang lebih kecil dari ```Double```
* ```-5.4F``` adalah kita melakukan parsing nilai bertipe ```Double``` ke ```Float``` dengan karakter ```F``` di akhir bilangan.

## Tipe data String

tipe data **String** ini dapat mencakup semua karakter yang ada di keyboard contohnya ```A```, ```3.2```, ```//``` dan lain-lain. ok kita buat kodingnya:
tipe data **String** ini dapat mencakup semua karakter yang ada di keyboard contohnya ```A```, ```3.2```, ```//``` dan lain-lain. ok kita buat kodingnya:

```java
public class VariableString {
  public static void main(String[] args){
    String fistName = "Dimas";
    String lastName = "Maryanto";
    System.out.println(nama + " "+ lastName);
  }
}
```

berikut outputnya:

{% highlight bash %}
Dimas Maryanto
{% endhighlight %}

Berikut penjelasan berdasarkan output tersebut:

* kita membuat variable ```String``` dengan nama yaitu ```firstName``` dan ```lastName```
* kemudian variable ```firstName``` diisi dengan nilai ```Dimas``` dan ```lastName``` dengan nilai ```Maryanto``` yang ditandai atau diapit oleh kutip dua ```""```
* Kemudian kita tampilkan ke layar menggunakan fungsi ```System.out.println();```
* argument yang ada pada fungsi ```println()``` adalah nilainya ```Dimas``` kemudian ditambahkan dengan karakter spasi kemudian ditambahkan lagi dengan nilai ```Maryanto``` maka hasilnya ```Dimas Maryanto```.

## Tipe data Date

di Java tipe data Date sebenarnya ada lumayan banyak contohnya ```java.sql.Date```, ```java.util.Date```, ```java.time.LocalDate``` (mulai java-1.8) dan library external seperti ```org.jodatime``` tapi biasanya klo saya bekerja di desktop (JavaFX) menggunakan ```java.time.LocalDate``` klo bekerja di web menggunakan ```java.lang.Date```. ok kita buat kodingnya:

```java
import java.time.LocalDate;
import java.util.Date;

public class VariableDate {
  public static void main(String[] args) {
    LocalDate localDate = LocalDate.now();
    System.out.println("java.time.LocalDate : "+localDate);
    Date date = new Date();
    System.out.println("java.util.Date : "+date);
  }
}
```

Berikut outputnya berdasarkan output diatas:

{% highlight bash %}
java.time.LocalDate : 2016-05-05
java.util.Date : Thu May 05 15:46:15 WIB 2016
{% endhighlight %}

Nah barusan kita telah mengenal beberapa tipe data Variabel diantaranya Integer, Double, Float, Date, dan String. Sekarang kita akan membahas scope dari variabel tersebut. Pada dasarnya scope variable di Java ada 3 yaitu

* Local
* Global
* Parameter

## Local Variabel

untuk local variabel ini sebenarnya kita telah peraktekan sebelumnya, tpi gpp kita bahas aja. ok local variabel itu biasanya dideklarasikan didalam method atau dalam scope terdalam contohnya kita khan membuat ```class``` kemudian didalam ```class``` terdiri dari method-method, nah didalam salah satu method itu lah kita mendefinisikan variabel hal itu disebut local variabel. berikut adalah sample codenya:

```java
public class LocalVariabel {

  public static void main(String[] args) {
    String nama = "Dimas Maryanto";
    System.out.println(nama);
  }

  public void fungsiLain(){
    // System.out.println(nama); # variabel nama tidak dikenali  
  }
}
```

Berikut klo dijalankan maka outputnya:

{% highlight bash %}
Dimas Maryanto
{% endhighlight %}

Dari output diatas ada beberapa hal yang harus di pahami yaitu

Variabel dengan nama ```nama``` adalah local variabel jadi variabel tersebut memiliki sifat turunan dari method main yaitu ```static```, kemudian variabel ```nama``` tersebut dipanggil untuk ditampilkan nilainya!. klo variabel ```nama``` tersebut dipanggil di luar method ```main()``` maka terjadi error karena variabel tidak dikenali contohnya seperti yg ditandai oleh komentar.

## Global Variabel

Global variabel seperti yang kita tahu klo variabel bisa diakses oleh member dalam kelas tersebut seperti method, inner class dan lain-lain. untuk lebih jelasnya berikut ini adalah sample codenya:

```java
public class GlobalVariabel {

  private static String nama = "Dimas Maryanto";

  public static void main(String[] args) {
    System.out.println("ini dari main : "+ nama);
    fungsiLain();
  }

  public static void fungsiLain(){
    System.out.println("Ini dari fungsiLain : "+ nama);
  }
}
```

Jika dijalankan maka akan menampilkan output seperti berikut:

{% highlight bash %}
ini dari main : Dimas Maryanto
ini dari fungsiLain : Dimas Maryanto
{% endhighlight %}

Berdasarkan output tersebut berikut adalah penjelasanya:

* Variabel dengan nama ```nama``` adalah global variabel, kenapa disebut global variabel karena bisa diakses oleh method-method dalam 1 ```class``` contohnya diakses oleh method ```main()``` dan ```fungsiLain()```.
* Variabel ```nama``` di isi dengan nilai ```Dimas Maryanto``` kemudian dipanggil di ```main()``` kemudian didalamnya memanggil fungsi lagi dengan nama ```fungsiLain()```, di dalam ```fungsiLain()``` memanggil variabel ```nama```.
* outputnya manghasilkan ```ini dari main : Dimas Maryanto``` kemudian menanggil method ```fungsiLain()``` yang menampilkan ```ini dari fungsiLain : Dimas Maryanto```.

## Arguments atau Parameter variabel

Argument sebenarnya sama seperti local variabel karena hanya dikenal oleh internal method tersebut tetapi nilainya di inject dari external. berikut ini adalah implementasinya:

```java
public class ParamsVariabel {

  private static String nama = "Dimas";

  public static void main(String[] args) {
    fungsiLain("Maryanto");
  }

  public static void fungsiLain(String nama){
    System.out.println(ParamsVariabel.nama + " " + nama);
  }
}
```

Jika dijalankan maka akan menampilkan output seperti berikut:

{% highlight bash %}
Dimas Maryanto
{% endhighlight %}

Berdasarkan output diatas maka berikut adalah penjelasnya:

* Disini kita punya dua variabel dengan nama ```nama``` yaitu Global variabel ```static String nama``` dan Argument variabel ```fungsiLain(String nama)```.
* ok kita lihat method ```fungsiLain(String nama)```, ```ParamsVariabel.nama``` adalah mengacu ke Global variabel yaitu nilainya ```Dimas``` dan klo ```nama``` adalah mengacu ke nilai yang diinput dari parameter yaitu ```Maryanto``` lihat di ```main(String[] args)```.
* Maka hasilnya ```Dimas Maryanto```.

Ok, mungkin penbahasan tentang Variable, Tipe Data dan Scope sudah cukup jelas dan semoga bermanfaat. see you next post!.
