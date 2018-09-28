---
layout: post
title: "Java implementasi Batch processing untuk JDBC"
date: 2016-08-11T12:07:53+07:00
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

Halo kita lanjutkan postingan [sebelumnya]({% post_url 2016-08-11-batch-jdbc %}) tentang konsep batch processing di JDBC. Nah jadi kali ini saya mau langsung aja bahas yang Batch Processing ya supaya gak terlalu panjang klo belum ngerti konsepnya silangkan [baca lagi]({% post_url 2016-08-11-batch-jdbc %}) dengan seksama setelah mengerti baru lanjut ke artikel ini. Kodingnya seperti berikut tapi asalkan data dalam **tabel ```jurusan``` harus kosongkan dulu**, klo ada databasenya silahkan hapus dengan query seperti berikut:

<!--more-->

{% highlight sql %}
DELETE FROM jurusan;
{% endhighlight %}

Berikut outputnya:

{% highlight mysql %}
MariaDB [jdbc_mysql]> delete from jurusan;
Query OK, 6 rows affected (0.02 sec)

MariaDB [jdbc_mysql]> select * from jurusan;
Empty set (0.00 sec)

MariaDB [jdbc_mysql]>
{% endhighlight %}

Kodingnya seperti berikut:

{% gist page.gist BatchProcessing.java %}

Kalo di jalankan maka hasilnya seperti berikut

{% highlight bash %}
ditambahkan ke batch: com.mysql.jdbc.JDBC4PreparedStatement@26a1ab54: INSERT INTO jurusan(id, nama) VALUES ('AK','Komputer Akutansi')
ditambahkan ke batch: com.mysql.jdbc.JDBC4PreparedStatement@26a1ab54: INSERT INTO jurusan(id, nama) VALUES ('MI','Management Informatika')
ditambahkan ke batch: com.mysql.jdbc.JDBC4PreparedStatement@26a1ab54: INSERT INTO jurusan(id, nama) VALUES ('SE','Ekonomi')
ditambahkan ke batch: com.mysql.jdbc.JDBC4PreparedStatement@26a1ab54: INSERT INTO jurusan(id, nama) VALUES ('IF','Informatika')
ditambahkan ke batch: com.mysql.jdbc.JDBC4PreparedStatement@26a1ab54: INSERT INTO jurusan(id, nama) VALUES ('SI','Sistem Informasi')
ditambahkan ke batch: com.mysql.jdbc.JDBC4PreparedStatement@26a1ab54: INSERT INTO jurusan(id, nama) VALUES ('HI','Hubungan Internasional')
queries dikirim ke database!
queries disimpan secara permanen
BUILD SUCCESSFUL (total time: 0 seconds)
{% endhighlight %}

Dan ini adalah penjelasannya seperti berikut:

* Jadi alurnya si sama seperti biasa kita buka dulu koneksinya
* Buat daftar datanya klo saya kasusnya ingin menambahkan data dari ```java.util.List<Jurusan>``` seperti berikut:

{% highlight java %}
List<Jurusan> daftarJurusan = new ArrayList<>();
daftarJurusan.add(new Jurusan("AK", "Komputer Akutansi"));
daftarJurusan.add(new Jurusan("MI", "Management Informatika"));
daftarJurusan.add(new Jurusan("SE", "Ekonomi"));
daftarJurusan.add(new Jurusan("IF", "Informatika"));
daftarJurusan.add(new Jurusan("SI", "Sistem Informasi"));
daftarJurusan.add(new Jurusan("HI", "Hubungan Internasional"));
{% endhighlight %}

* Kita siapkan sqlnya seperti berikut:

{% highlight java %}
PreparedStatement ps = koneksi.prepareStatement(
  "INSERT INTO jurusan(id, nama) VALUES (?,?)"
  );
{% endhighlight %}

* Setelah itu kita loop listnya kemudian tambahkan ke batch seperti berikut:

{% highlight java %}
for (Jurusan j : daftarJurusan) {
    ps.setString(1, j.getId());
    ps.setString(2, j.getNama());

    // ditambahkan ke batch / memory
    ps.addBatch();
    System.out.println("ditambahkan ke batch: " + ps.toString());
}
{% endhighlight %}

* Setelah loopnya selesai baru querynya dikirim ke database menggunakan:

{% highlight java %}
ps.executeBatch();
System.out.println("queries dikirim ke database!");
// bersihkan batch
ps.clearBatch();
ps.close();
koneksi.commit();
koneksi.close();
{% endhighlight %}

Ok cukup jelasya, terus ada pertanyaan lagi itu khan cuman satu tabel terus bisakah ke beda tabel? tentu aja bisa. untuk tau jawabanya silahkan lihat source di [repository github]({{ page.repository }}). Mungkin sekian dulu untuk postingan tentang Batch Processing. see you next post!.
