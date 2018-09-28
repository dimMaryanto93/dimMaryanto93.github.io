---
layout: post
title: "Optimalisasi JDBC dengan Java Beans"
date: 2016-08-02T14:19:50+07:00
author: Dimas Maryanto
comments: yes
gist: dimMaryanto93/9670215f595e5415326e78df2d22977a
repository: https://github.com/dimMaryanto93/belajar-java-jdbc.git
category: java
tags:
- Java
- JDBC
- MariaDB
- MySQL
- Netbeans
references:
- http://www.tutorialspoint.com/jsp/jsp_java_beans.htm
- https://en.wikipedia.org/wiki/JavaBeans
---

Halo kembali lagi di blog saya, kali ini saya ingin melanjutkan pembahasan JDBC. Di [postingan sebelumnya]({% post_url 2016-07-31-java-jdbc %}) tentang JDBC yang sangat sederhana, jadi di pembahasan yang sebelumnya itu masih memiliki kekurangan yaitu source codenya akan sulit pelihara ketika ada perubahan struktur data yang database.

<!--more-->

Sekarang saya mau jelaskan dulu apasih JavaBean? apa fungsinya? trus kenapa menggunakan JavaBean? gak pake yang biasa aja seperti postingan sebelumnya?

Ok kita bahas satu-persatu, yang pertama apa itu JavaBean menurut [Wikipedia](http://www.tutorialspoint.com/jsp/jsp_java_beans.htm) adalah

> JavaBeans are classes that encapsulate many objects into a single object (the bean). They are serializable, have a zero-argument constructor, and allow access to properties using getter and setter methods. The name "Bean" was given to encompass this standard, which aims to create reusable software components for Java.

Jadi kesimpulanya adalah sebuah kelas yang membungkus semua properties dan hanya dapat diakses melalui method yaitu setter dan getter, kemudian JavaBeans ini harus mengimplementasikan _interface_ ```serializable``` dan juga harus memiliki default construktor yang tidak memiliki argument contohnya seperti berikut:

{% highlight java %}
public class Mahasiswa implements Serializable{
    // default construktor
    public Mahasiswa(){}

    // property
    private String nama;

    // method setter
    public void setNama(String nama){
      this.nama = nama;
    }

    // method getter
    public String getNama(){
      return this.nama;
    }
}
{% endhighlight %}

Jadi cara mengaksesnya dengan cara contohnya saya membuat kelas baru dengan nama ```MainAplikasi.java``` seperti berikut:

{% highlight java %}
public class MainAplikasi{
  public static void main(String[] args){
    Mahasiswa m = new Mahasiswa();
    m.setNama("Halaman admin");
    System.out.println("JavaBeans Mahasiswa.nama adalah "
    + m.getNama);
  }
}
{% endhighlight %}

Jika dieksekusi maka akan menampilkan hasil seperti berikut:

{% highlight bash %}
JavaBeans Mahasiswa.nama adalah Halaman admin
{% endhighlight %}

Saya rasa konsep JavaBeans udah clear ya? Ok sekarang saya lanjutkan ke pertanyaan berikutnya yaitu kenapa menggunakan JavaBeans? jawabanya adalah

* Fleksible, magsudnya klo anda pake text editor canggih seperti Netbeans, Eclipse atau IDEA. kita akan menadapatkan fungsi autocomplete dengan menggunakan <kbr>CTRL + Space</kbr> maka akan muncul method-method yang tersedia seperti berikutnya

![Netbeans autocomplete]({{ site.baseurl }}/assets/img/posts/java-beans-jdbc/netbeans-autocomplate.png)

Jika anda perhatikan disana ada method ```getNama, getJurusan, getNim``` dan lain-lain jadi kita gampang untuk dimaintance, Sedangkan klo menggunakan variable temporary seperti postingan sebelumnya fungsi ini tidak bisa dilakukan.

* Maintanceable, jadi kalo ada perubahan sturkutur tabel di database maka kita cukup menambahkan atau mengurangi properties atau mengubah tipe data yang ada di kelas tersebut. tidak perlu merubah disetiap fungsi yang menggunakan atau mengakses tabel tersebut.
* Development lebih cepat, jadi magsudnya karena source codenya bisa dimaintance dengan mudah jadi kita akan mengurangi proses atau waktu development ketika terjadi revisi atau ada bugs.
* Performa tidak terlalu penting, jadi magsudnya karena kita mendefinisikan sebuah object di memory yang memungkinkan di akses di Global jadi ini akan mengurangi performa aplikasi kita, tapi klo menurut saya performa itu tidaklah terlalu menjadi tujuan utama dalam membuat aplikasi yang terpenting adalah correctness atau kebenaran dari source code karena teknologi informasi sekarang udah sangat cepat dan didukung dengan komputer-komputer yang sangallah cepat dalam prosessing data.

Ok saya harap penjelasan saya diatas tadi jelas dan terbayang oleh anda. Nah sekarang kita masuk ke study kasusnya, masih pada tabel yang sama seperti postingan sebelumnya yaitu tabel mahasiswa seperti berikut:

![Describe table of mahasiswa]({{ site.baseurl }}/assets/img/posts/java-beans-jdbc/desc-tabel-mahasiswa.png)

Nah sekarang kita buat lebih modular yaitu dengan menggunakan JavaBeans. sekarang anda buat kelas baru dengan nama ```Mahasiswa.java``` dalam package ```belajar.jdbc.model``` seperti berikut:

{% highlight java %}
package belajar.jdbc.model;

public class Mahasiswa{

}
{% endhighlight %}

Kemudian tambahkan property seperti berikut:

{% gist page.gist Mahasiswa.java %}

Nah jadi berdasarkan koding diatas kita punya 3 variable yang merepresetansikan columns di dalam tabel mahasiswa yaitu ```nim varchar(8)```, ```nama varchar(25)``` dan ```jurusan varchar(2)``` karena semua tipe datanya ```varchar``` jadi saya buat menjadi ```java.lang.String``` semua. Kemudian kita buat method setter dan getter, oh ia yang harus diperhatikan juga untuk variable kita kasih akses ```private``` sedangkan untuk method ```public``` jadi kesimpulannya kita tidak bisa meng-akses variable dari luar, semua kegiatan dari luar hanya bisa mengakses method-method bersifat ```public```.

Tahap selanjutnya adalah membuat fungsi untuk memanggil kelas tersebut untuk digunakan kedalam atau diintegrasikan dengan komponen JDBC, yang pertama coba perhatikan koding berikut tentang Tambah data:

{% gist page.gist JavaBeanTambahMahasiswa.java %}

Kalo anda perhatikan memang tidak ada perubahan yang sangat signifikan karena kita hanya mengganti value yang siftatnya hardcode contohnya di postingan sebelumnya:

{% highlight java %}
PreparedStatement ps = koneksi.prepareStatement(sql);
ps.setString(1, "10511150");
ps.executeUpdate();
{% endhighlight %}

Menjadi objek seperti berikut:

{% highlight java %}
// membuat objek mahasiswa
Mahasiswa mhs = new Mahasiswa();
// setting value untuk variable nim
mhs.setNim("10511150");

PreparedStatement ps = koneksi.prepareStatement(sql);
// ambil nilai nimnya yaitu 10511150 kemudian masukan ke parameter ke 2
ps.setString(1, mhs.getNim());
ps.executeUpdate();
{% endhighlight %}

Jadi kesimpulannya koding yang kita buat ini sudah modularisasi artinya kita sudah memisahkan antara value dengan komponen JDBCnya.

Studi kasus yang ke dua, sekarang kita akan menampilkan data dari database pada tabel mahasiswa tersebut. Ok sekarang coba perhatikan koding berikut:

{% gist page.gist JavaBeanSelectAllMahasiswa.java %}

Sekilas memang gak ada perubahan dari postingan sebelumnya tapi jika anda perhatikan kembali, kita telah memisahkan data yang dihasilkan dan dikeluarkan berupa Array atau  List pada bagian berikut:

{% highlight java %}
List<Mahasiswa> daftarMahasiswa = new ArrayList<>();
while (rs.next()) {
  // membuat objek mahasiswa
  Mahasiswa m = new Mahasiswa();

  /**
   * mengambil nilai dari database dengan rs.getTipeData('nama_kolom');
   * kemudian value tersebut dimasukan ke method setter
   **/
  m.setNim(rs.getString(1));
  m.setNama(rs.getString(2));
  m.setJurusan(rs.getString(3));

  // menambahkan object mahasiswa ke array
  daftarMahasiswa.add(m);
}
{% endhighlight %}

Jadi disini saya membuat sebuah Array atau List kemudian datanya diisi dari objek yang kita buat dari objek mahasiswa yang diambil dari database pada tabel mahasiswa. Ok saya harap ini paham ya karena pada dasarnya sama seperti studi kasus pertama.

Studi kasus selanjutnya mengedit data mahasiswa, berikut kodingnya:

{% gist page.gist JavaBeanUbahMahasiswa.java %}

Penjelasannya kurang lebih sama seperti studi kasus pertama yaitu tambah data mahasiswa jadi kita membutuhkan objek mahasiswa tapi sebenarnya kita bisa select dulu dari database berdasarkan nim terus kita set value yang mau di ubah setelah itu kita jalankan perintah tesebut, tapi supaya gak terlalu panjang kita buat datanya sendiri aja seperti berikut:

{% highlight java %}
Mahasiswa mhs = new Mahasiswa();
mhs.setNim("10511173");
mhs.setJurusan("IF");
{% endhighlight %}

Sebenarnya seperti itu juga sudah cukup karenan yang kita gunakan untuk mengupdate hanya ```jurusan``` aja lebih jelasnya pada baris berikut:

{% highlight java %}
String sql = "UPDATE mahasiswa SET jurusan = ? WHERE nim = ?";
PreparedStatement ps = koneksi.prepareStatement(sql);

ps.setString(1, mahasiswa.getJurusan());
ps.setString(2, mahasiswa.getNim());
ps.executeUpdate();
{% endhighlight %}

Kemudian studi kasus terakhir yaitu hapus data mahasiswa, berikut adalah kodingnya:

{% gist page.gist JavaBeanDeleteMahasiswa.java %}

Penjelasanya kurang-lebih sama kaya studi kasus ke tiga, jadi saya gak perlu jelasin lagi ya panjang euy udah mulai males ngetiknya hehehe. Ok mungkin saya rasa cukup dulu pembahasan tentang Optimalisasi JDBC dengan JavaBeans. see you next post!.
