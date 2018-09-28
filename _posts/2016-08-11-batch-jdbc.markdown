---
layout: post
title: "Konsep Batch Processing di JDBC"
date: 2016-08-11T10:04:47+07:00
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
- http://www.oracle.com/technetwork/database/features/plsql/index.html
---

Halo kembali lagi saya mau nulis sesuatu tentang batch processing di JDBC, di [postingan sebelumnya]({% post_url 2016-08-11-impl-transaction-jdbc %}) tentang transaction, itu aja belum cukup. klo anda perhatikan koding berikut:

{% highlight java %}
// opeon connection
try {
  // ngirim query ke database
  String sql = "INSERT INTO jurusan (id, nama) VALUES (?,?)";
  ps.executeUpdate();

  // ngirim lagi query ke database
  sql = "UPDATE jurusan SET id = ? WHERE nama = ?";
  ps.executeUpdate();

  // ngirim signal untuk disimpan secara permanen
  koneksi.commit();
  koneksi.close();
} catch (SQLException ex) {
  System.err.println("query ke 2 dan 3 gagal disimpan");
  // error handling
}
{% endhighlight %}

Dari koding tersebut kita ngirim query ke database sebanyak 2x, that is OK but how about more than 100x??? berapa banyak memory yang harus ditanggung CPU dan HDD? hal ini juga bakalan mengurangi performa aplikasi kamu karena anda akan membuka session di database sebanyak jumlah yang diopen. Maka dari itu kita butuh batch processing, yang jadi pertanyaan apa itu batch processing dan bagai mana cara kerjanya?

<!--more-->

Batch processing ini pada dasarnya adalah kita mengirim sekaligus perintah sql ke database. Kalo anda tau [PL/SQL](http://www.oracle.com/technetwork/database/features/plsql/index.html) nah kurang lebih sama kayak gitu atau gak gini deh ya saya ilustrasikan.

JDBC biasa tanpa Batch processing, sama kayak kita **ngirim barang pake motor (satu-persatu)** ditempuh dalam 60km/jam jaraknya 10km. Sedangkan untuk JDBC yang menggunakan Batch processing, sama kayak kita ngirim barang **menggunakan mobil box (sekumpulan sql)** ditempuh dalam kecepatan yang sama dan jarak yang sama pula dan tujuan yang sama (Data Manipulation Language atau DML). Terus klo dibandingkan mana yang lebih cepat? Mobil ato Motor? **Mobil??** belum tentu juga tergantung dari jumlah querynya klo cuman 1 ya lebih optimal pake motor, optimal loh ya!! bukan cepat karena klo pake mobil cuman ngirim satu barang khan lebar bensin (CPU loaded). tpi klo querynya lumanyan banyak lebih optimal pke mobil karena kita bisa ngirim sekaligus perintah sqlnya tanpa harus bolak balik (klo pake motor).

Jadi ilustrasinya klo **menggunakan motor  (Tanpa Batch Processing)** di ubah menjadi diagram activity maka kyak gini:

![pake motor]({{ site.baseurl }}/assets/img/posts/batch-jdbc/nonBatchProcessing.jpg)

Sedangkan berikut ini **menggunakan mobil (Menggunakan Batch Processing)** jadi klo diubah ke diagram activity maka kayak gini:

![pake mobil]({{ site.baseurl }}/assets/img/posts/batch-jdbc/BatchProcessing.jpg)

OK saya rasa konsepnya cukup jelas ya tentang Batch Processing ini mulai dari apa fungsinnya dan bagaimana cara kerjanya, see you next post!.
