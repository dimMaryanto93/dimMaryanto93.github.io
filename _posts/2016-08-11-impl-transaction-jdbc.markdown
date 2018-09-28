---
layout: post
title: "Java implementasi transaction di JDBC"
date: 2016-08-11T06:18:45+07:00
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

Halo, ok kita lanjutkan ya postingan [sebelumnya]({% post_url 2016-08-10-transactions-jdbc %}) yang telah membicarakan tentang konsep seberapa pentingnya menggunakan konsep transaction pada JDBC. Mari kita buktikan berdasarkan postingan tersebut.

<!--more-->

Ok silahkan buka lagi projectnya ```jdbc-mysql```. Diasumsikan disini kita punya data seperti berikut:

{% highlight mysql %}
MariaDB [jdbc_mysql]> desc jurusan;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | varchar(2)  | NO   | PRI | NULL    |       |
| nama  | varchar(25) | NO   |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.04 sec)

MariaDB [jdbc_mysql]> select * from jurusan;
+----+-------------+
| id | nama        |
+----+-------------+
| IF | Informatika |
+----+-------------+
1 row in set (0.01 sec)

MariaDB [jdbc_mysql]>
{% endhighlight %}

Kemudian saya mau menambahkan data dengan menggunakan koding seperti berikut:

{% highlight java %}
  Jurusan j = new Jurusan("SI", "SISTEM INFORMASI");
  Connection koneksi = null;

  // insert data SI
  try {
      koneksi = DriverManager
        .getConnection(
          "jdbc:mysql://localhost:3306/jdbc_mysql",
          "root",
          "admin"
        );

      String sql = "INSERT INTO jurusan (id, nama) VALUES (?,?)";
      PreparedStatement ps = koneksi.prepareStatement(sql);
      ps.setString(1, j.getId());
      ps.setString(2, j.getNama());

      ps.executeUpdate();
      ps.close();
  } catch (SQLException ex) {
      System.err.println("query ke 1 gagal disimpan");
  }

  // udpate jurusan IS ke IF
  j.setId("IF");
  try {
    koneksi = DriverManager
      .getConnection(
        "jdbc:mysql://localhost:3306/jdbc_mysql",
        "root",
        "admin"
      );

      String sql = "UPDATE jurusan SET id = ? WHERE nama = ?";
      PreparedStatement ps = koneksi.prepareStatement(sql);
      ps.setString(1, j.getId());
      ps.setString(2, j.getNama());

      ps.executeUpdate();
      ps.close();
  } catch (SQLException ex) {
      System.err.println("query ke 2 gagal disimpan");
  }

  // insert data baru
  j = new Jurusan("MI", "Management Informatika");
  try {
    koneksi = DriverManager
      .getConnection(
        "jdbc:mysql://localhost:3306/jdbc_mysql",
        "root",
        "admin"
      );

      String sql = "INSERT INTO jurusan (id, nama) VALUES (?,?)";
      PreparedStatement ps = koneksi.prepareStatement(sql);
      ps.setString(1, j.getId());
      ps.setString(2, j.getNama());

      ps.executeUpdate();
      ps.close();
  } catch (SQLException ex) {
      System.err.println("query ke 3 gagal disimpan");
  }
{% endhighlight %}

Maka klo di running maka hasilnya seperti berikut:

{% highlight bash %}
run-single:
query ke 2 gagal disimpan
BUILD SUCCESSFUL (total time: 2 seconds)
{% endhighlight %}

dan sekarang kita coba periksa ke tabel jurusan di dalam database maka hasilnya seperti berikut:

{% highlight mysql %}
MariaDB [jdbc_mysql]> select * from jurusan;
+----+------------------------+
| id | nama                   |
+----+------------------------+
| IF | Informatika            |
| MI | Management Informatika |
| SI | SISTEM INFORMASI       |
+----+------------------------+
3 rows in set (0.00 sec)

MariaDB [jdbc_mysql]>
{% endhighlight %}

Bisa anda lihat khan hasilnya kalo **query ke 1 dan ke 3 berhasil dinput ke table** buktinya ada khan data dengan id ```SI``` dan ```MI```, sedangkan untuk **query ke 2 itu gagal** karena untuk meng-update data dengan id ```SI``` ke ```IF``` tidak dapat dilakukan karena id ```IF``` sendiri udah ada sebelumnya. Saya rasa apa yang ada dikonsep sejajar dengan apa yang kita praktekan barusan ya, selanjutnya kita akan bahas tentang menggunakan konsep Transaction.

Sekarang kita bahas yang transaction tapi sebelum itu kita hapus dulu data sebelumnya dengan id ```SI``` dan ```MI``` dengan query seperti berikut:

{% highlight sql %}
DELETE FROM jurusan WHERE id in ('SI', 'MI');
{% endhighlight %}

Berikut adalah outputnya:

{% highlight mysql %}
MariaDB [jdbc_mysql]> DELETE FROM jurusan WHERE id in ('SI', 'MI');
Query OK, 2 rows affected (0.06 sec)

MariaDB [jdbc_mysql]> select * from jurusan;
+----+-------------+
| id | nama        |
+----+-------------+
| IF | Informatika |
+----+-------------+
1 row in set (0.00 sec)
{% endhighlight %}

Jadi datanya telah kita kembalikan ke awal ya. sekarang perhatikan koding berikut ini:

{% gist page.gist TransactionJDBC.java %}

Jadi disini kita punya 2 block, yaitu

* kita menyimpan data jurusan dengan id ```SI``` dan nama ```Sistem Informasi```
* kita menyimpan data dengan id ```MI``` dan namanya ```Management Informatika``` dan meng-update id jurusan dengan id ```SI``` ke ```IF```

Ketika dijalankan berikut hasilnya:

{% highlight bash %}
query ke 1 di eksekusi
query ke 2 di eksekusi
query ke 2 dan 3 gagal disimpan
BUILD SUCCESSFUL (total time: 1 second)
{% endhighlight %}

Berikut adalah data di tabel jurusan dalam database seperti berikut:

{% highlight mysql %}
MariaDB [jdbc_mysql]> select * from jurusan;
+----+------------------+
| id | nama             |
+----+------------------+
| IF | Informatika      |
| SI | SISTEM INFORMASI |
+----+------------------+
2 rows in set (0.00 sec)

MariaDB [jdbc_mysql]>
{% endhighlight %}

Nah itu tadi hasilnya, tapi anda ngerti gak alur program yang saya buat itu? klo gak silahkan baca dengan duduk manis, kosongkan pikiran, relax, ambil teh hangat bila perlu.

Pada **block pertama** yaitu seperti berikut:

{% highlight java %}
try {
    koneksi = DriverManager
      .getConnection(
        "jdbc:mysql://localhost:3306/jdbc_mysql",
        "root",
        "admin");
    koneksi.setAutoCommit(false);

    String sql = "INSERT INTO jurusan (id, nama) VALUES (?,?)";
    PreparedStatement ps = koneksi.prepareStatement(sql);
    ps.setString(1, j.getId());
    ps.setString(2, j.getNama());

    ps.executeUpdate();
    ps.close();
    koneksi.commit();
    System.out.println("query ke 1 di eksekusi");
} catch (SQLException ex) {
    System.err.println("query ke 1 gagal disimpan");
    try {
        koneksi.rollback();
    } catch (SQLException sql) {
        System.err.println("gak bisa di rollback");
    }
}
{% endhighlight %}

Disini kita, sama seperti biasa ya cuman menyimpan data dengan id ```SI``` dan namanya ```Sistem Informasi``` tapi ada yang beda gak? coba perhatikan

* ```koneksi.setAutoCommit(false);``` jadi ini magsudnya supaya tidak setiap ada query yang dikirim ke database itu disimpan ke database secara permanen tapi di cache atau disimpan secara temporary dulu dalam database.
* ```koneksi.commit()``` ini digunakan untuk menyimpan secara permanen selama tidak ada error.
* dan satu lagi ```koneksi.rollback()``` jadi yang satu ini akan mengembalikan ke commit terakhir, karena tidak ada yang di commit sebelumnya maka database tidak akan melakukan update apapun dalam sistemnya.

Sedangkan **block ke dua** yaitu seperti berikut:

{% highlight java %}
try {
    koneksi = DriverManager
      .getConnection(
        "jdbc:mysql://localhost:3306/jdbc_mysql",
        "root",
        "admin");
    koneksi.setAutoCommit(false);

    // insert data baru
    j = new Jurusan("MI", "Management Informatika");
    String sql = "INSERT INTO jurusan (id, nama) VALUES (?,?)";
    PreparedStatement ps = koneksi.prepareStatement(sql);
    ps.setString(1, j.getId());
    ps.setString(2, j.getNama());
    ps.executeUpdate();
    ps.close();
    System.out.println("query ke 2 di eksekusi");

    // udpate jurusan IS ke IF
    j.setId("IF");
    sql = "UPDATE jurusan SET id = ? WHERE nama = ?";
    ps = koneksi.prepareStatement(sql);
    ps.setString(1, j.getId());
    ps.setString(2, j.getNama());
    ps.executeUpdate();
    ps.close();
    System.out.println("query ke 3 di eksekusi");

    koneksi.commit();
    koneksi.close();
} catch (SQLException ex) {
    System.err.println("query ke 2 dan 3 gagal disimpan");
    try {
        koneksi.rollback();
    } catch (SQLException sql) {
        System.err.println("gak bisa di rollback");
    }
}
{% endhighlight %}

Berikut penjelasannya:

Disini pada dasarnya sama dengan block pertama tapi disini ada 2 query yang dikirim ke database query pertama adalah tambah data jurusan dengan id ```MI``` dan yang ke dua yaitu meng-update id ```SI``` ke ```IF```

* Pada query pertama, klo dieksekusi berhasil karena tidak ada yang seperti itu sebelumnya makanya terdapat output ```query ke 2 berhasil di eksekusi``` **tapi datanya belum di commit** maka klo di ```select * from jurusan``` pada saat ini belum ada datanya yang tampil dengan id ```MI``` tersebut klo gak percara coba aja tambahin lagi untuk menampilkan data diantara 2 query tersebut.
* Pada query kedua, klo dieksekusi maka akan **error** kenapa error? karena sebelumnya klo anda perhatikan udah ada data dengan id ```IF``` karena id ini khan **primary key** jadi gak boleh ada data yang duplicat atau sama. karena pada query pertama block kedua belum di commit maka akan di **rollback** (kembalikan datanya ke commit pertama atau block pertama).

Ok saya rasa penjelasan diatas cukup jelas. Mungkin cukup sekian dulu postingan tentang implemantasi transaction di JDBC. see you next post!.
