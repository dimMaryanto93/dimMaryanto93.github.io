---
layout: post
title: "Modularisasi untuk JDBC"
date: 2016-08-06T18:39:28+07:00
author: Dimas Maryanto
comments: yes
gist: dimMaryanto93/9670215f595e5415326e78df2d22977a
category: java
tags:
- Java
- JDBC
- MariaDB
- Netbeans
references:
---

Halo kembali lagi di blog saya, jadi kali ini saya akan membahas tentang kelanjutan dari [Optimalisasi JDBC dengan JavaBeans]({% post_url 2016-08-02-javabeans-jdbc %}) yaitu tadinya kita memisahkan antara yang sifatnya value dengan sturuktur JDBCnya. Sekarang kita akan memecah kembali karena khan pada dasarnya klo kita ngoding untuk bikin apalikasi tidak hanya untuk 1 tabel tapi bisa menjadi ratusan tabel bahkan ribuan table dalam 1 database, jadi disini kita bakalan membahas tentang modularisasi dengan konsep Repository.

<!--more-->

Nah sebelum kita melanjutkan ke topik utama sebelumnya sama mau menambahkan dulu tabel untuk database yang kemarin yaitu ```jdbc_mysql``` yang berelasi ke table ```mahasiswa``` yaitu ```jurusan```. berikut adalah perintah sqlnya:

{% gist page.gist jdbc-mysql-v.1.sql %}

silahkan eksekusi kemudian check hasilnya maka akan menampilkan output seperti berikut:

{% highlight bash %}
MariaDB [jdbc_mysql]> drop database if exists jdbc_mysql;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> create database if not exists jdbc_mysql;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> use jdbc_mysql;
Database changed
MariaDB [jdbc_mysql]> CREATE TABLE IF NOT EXISTS mahasiswa(
    ->     nim varchar(8) primary key,
    ->     nama varchar(25) not null,
    ->     id_jurusan varchar(2) not null
    -> );
Query OK, 0 rows affected (0.26 sec)

MariaDB [jdbc_mysql]>
MariaDB [jdbc_mysql]> CREATE TABLE IF NOT EXISTS JURUSAN(
    ->     id varchar(2) primary key,
    ->     nama varchar(25) not null
    -> );
Query OK, 0 rows affected (0.19 sec)

MariaDB [jdbc_mysql]>
{% endhighlight %}

kemudian setelah itu kita buat kelas baru dengan nama ```Jurusan.java``` seperti berikut:

{% gist page.gist Jurusan.java %}

Kemudian setelah itu kita update kelas ```Mahasiswa.java``` atau supaya tidak menggangu kelas yang sebelumnya kita buat ja kelas baru misalnya ```MahasiswaUpdated.java``` seperti berikut:

{% gist page.gist MahasiswaUpdated.java %}

Jadi kita hanya mengganti tipe data yang tadinya

{% highlight java %}
private String jurusan;

public void setJurusan(String jurusan){
  this.jurusan = jurusan;
}

public String getJurusan(){
  return this.jurusan;
}
{% endhighlight %}

Menjadi seperti berikut:

{% highlight java %}
private Jurusan jurusan;

public void setJurusan(Jurusan jurusan){
  this.jurusan = jurusan;
}

public Jurusan getJurusan(){
  return this.jurusan;
}
{% endhighlight %}

Ok, sekarang kit kembali ke topik tentang bagaimana cara memodularisasi JDBC dengan konsep repository. Jadi konsep ini sebenarnya adalah MVC + S yaitu Model View Controller + Service, jadi service inilah yang disebut repository. jadi kita menyediakan sebuah layanan yang dinamis dan biasanya diterpkannya menggunakan ```interface```. yang jadi kenapa penggunakan ```interface```? gak pake kelas biasanya yaitu tujuannya sederhana supaya suatu saat kita mau data tersebut di open ke RMI (Romote Method Invocation) bisa gampang tinggal implementasikan interface tersebut setelah itu kita buat implementasi klasnya.

Selanjutnya kita terapkan ke koding yang kita buat untuk kelas ```Mahasiswa``` dan ```Jurusan``` tapi pertama saya mau membuat yang jurusan dulu.

## Getting started

Pertama anda buat dulu package dalam ```belajar.jdbc``` dengan nama ```repo``` jadi seperti berikut:

![Package Repo]({{ site.baseurl }}/assets/img/posts/modular-jdbc/package-repo.png)

Kemudian buat ```interface``` dengan nama ```RepositoryJurusan.java``` seperti berikut:

Klik kanan di folder atau source **repo** kemudian **New File** maka akan tampil form seperti berikut:

![New interface]({{ site.baseurl }}/assets/img/posts/modular-jdbc/new-interface.png)

Di menu categories pilih **Java** kemudian di File Type pilih **Java Interface** kemudian klik **Next** maka akan tampil form sebagai berikut:

![New File Interface]({{ site.baseurl }}/assets/img/posts/modular-jdbc/new-interface-2.png)

Kemudian di Class name input ```RepositoryJurusan``` maka hasilnya seperti berikut:

{% highlight java %}
package belajar.jdbc.repo;

/**
 *
 * @author dimMaryanto
 */
public interface RepositoryJurusan {

}
{% endhighlight %}

Setelah itu, kita bisa tambahkan fungsi-fungsi ```abstract``` karena kita khan cuman menggunakan database jadi ya kurang lebih fungsi yang kita gunakan yaitu

* insert
* update
* delete
* findById
* selectAll.

Maka berikut implementasinya:

{% gist page.gist RepositoryJurusan.java %}

Nah jadi dalam interface ```RepositoryJurusan``` itu, kita hanya mendefinisikan method-method kosong, kemudian kita buat package baru dengan nama ```belajar.jdbc.service``` seperti berikut:

![Package service]({{ site.baseurl }}/assets/img/posts/modular-jdbc/package-service.png)

Kemudian didalam package tersebut anda buat kelas baru dengan nama ```ServiceJurusan``` maka hasilnya seperti berikut:

{% highlight java %}
package belajar.jdbc.service;

/**
 *
 * @author dimMaryanto
 */
public class ServiceJurusan {

}
{% endhighlight %}

Kemudian implements interface ```RepositoryJurusan``` seperti berikut:

{% highlight java %}
package belajar.jdbc.service;

import belajar.jdbc.repo.RepositoryJurusan;

public class ServiceJurusan implements RepositoryJurusan{

}
{% endhighlight %}

Setelah itu, ada error nih di Netbeans nya seperti berikut:

![Error Implement all abastract method]({{ site.baseurl }}/assets/img/posts/modular-jdbc/error-implement-all-method.png)

Ini karena ada method yang harus di implement oleh kelas tersebut, caranya untuk mengenerate method-method yang harus implementasikan yaitu klik **lampu yang merah itu** terus pilih **Implement all abstract methods** maka hasil generatenya seperti berikut:

{% highlight java %}
package belajar.jdbc.service;

import belajar.jdbc.model.Jurusan;
import belajar.jdbc.repo.RepositoryJurusan;
import java.sql.SQLException;
import java.util.List;

/**
 *
 * @author dimMaryanto
 */
public class ServiceJurusan implements RepositoryJurusan{

    @Override
    public List<Jurusan> selectAll() throws SQLException {
        throw new UnsupportedOperationException("Not supported yet.");
    }

    @Override
    public Jurusan save(Jurusan jurusan) throws SQLException {
        throw new UnsupportedOperationException("Not supported yet.");
    }

    @Override
    public Jurusan update(Jurusan jurusan) throws SQLException {
        throw new UnsupportedOperationException("Not supported yet.");
    }

    @Override
    public void delete(String id) throws SQLException {
        throw new UnsupportedOperationException("Not supported yet.");
    }

    @Override
    public Jurusan findById(String id) throws SQLException {
        throw new UnsupportedOperationException("Not supported yet.");
    }

}
{% endhighlight %}

Nah sekarang kita tinggal ganti dengan fungsi-fungsi yang seharunya seperti insert, update, delete, select data dari database. karena kita udah punya kita bisa copy-paste kemudian modifikasi menjadi seperti berikut:

{% gist page.gist ServiceJurusan.java %}

Setelah itu kita bisa mecoba merunningnya dengan membuat kelas baru contohnya saya mau buat kelas ```ModularJDBC``` dalam package ```belajar.jdbc``` seperti berikut:

{% highlight java %}
package belajar.jdbc;

/**
 *
 * @author dimMaryanto
 */
public class ModularJDBC {
    public static void main(String[] args) {

    }
}
{% endhighlight %}

Kemudian tambahkan seperti berikut:

{% gist page.gist ModularJDBC.java %}

Klo di running maka hasilnya seperti berikut:

{% highlight bash %}
compile-single:
run-single:
INSERT ---------------------

FIND BY ID------------------
Kode IF adalah Informatika

UPDATE -----------------------

FIND BY ID after Update------------------
Kode IF adalah INFORMATIKA

SELECT ALL ------------------
Kode: IF, Namanya: INFORMATIKA

DELETE ---------------------
BUILD SUCCESSFUL (total time: 1 second)
{% endhighlight %}

Bagaimana jadi lebih sederhana khan? OK sekarang kita buat yang satu lagi yaitu ```Mahasiswa``` yang berelasi dengan ```Jurusan```. seperti berikut kodingnya:

{% gist page.gist RepositoryMahasiswa.java %}

Setelah membuat repository tahap selanjutnya membuat kelas baru dengan nama ```ServiceMahasiswa``` seperti berikut dan jangan lupa **implements** interface ```RepositoryMahasiswa```:

{% highlight java %}

package belajar.jdbc.service;

import belajar.jdbc.model.MahasiswaUpdated;
import belajar.jdbc.repo.RepositoryMahasiswa;
import java.sql.SQLException;
import java.util.List;

public class ServiceMahasiswa implements RepositoryMahasiswa {

    @Override
    public List<MahasiswaUpdated> selectAll() throws SQLException {
        throw new UnsupportedOperationException("Not supported yet.");
    }

    @Override
    public MahasiswaUpdated save(MahasiswaUpdated mhs) throws SQLException {
        throw new UnsupportedOperationException("Not supported yet.");
    }

    @Override
    public MahasiswaUpdated update(MahasiswaUpdated mhs) throws SQLException {
        throw new UnsupportedOperationException("Not supported yet.");
    }

    @Override
    public void delete(String id) throws SQLException {
        throw new UnsupportedOperationException("Not supported yet.");
    }

    @Override
    public MahasiswaUpdated findById(String id) throws SQLException {
        throw new UnsupportedOperationException("Not supported yet.");
    }

}
{% endhighlight %}

Kemudian kita edit file tersebut masukin fungsi-fungsi yang telah kita buat sebelumnya kita masukin ke kelas tersebut, maka hasilnya seperti berikut:

{% gist page.gist ServiceMahasiswa.java %}

nah sekarang kita buat kelas baru lagi untuk mengobain atau menjalankannya dengan nama ```ModularJDBCMahasiswa``` seperti berikut:

{% highlight java %}
package belajar.jdbc;

public class ModularJDBCMahasiswa {

    public static void main(String[] args) {

    }
}
{% endhighlight %}

Selanjutnya karena kita ingin menambahkan data Mahasiswa jadi kita membutuhkan juga data Jurusan, jadi kita copy-paste aja perintah berikut ke dalam file ```ModularJDBCMahasiswa``` seperti berikut:

{% highlight java %}
package belajar.jdbc;

import belajar.jdbc.model.Jurusan;
import belajar.jdbc.repo.RepositoryJurusan;
import belajar.jdbc.service.ServiceJurusan;
import java.sql.SQLException;

public class ModularJDBCMahasiswa {

    public static void main(String[] args) throws SQLException {
        RepositoryJurusan repo = new ServiceJurusan();

        // cara menggunakan untuk insert data
        System.out.println("INSERT ---------------------");
        repo.save(new Jurusan("IF", "Informatika"));
    }
}
{% endhighlight %}

kemudian tambahkan lagi contohnya untuk tambah data mahasiswa dan menampilkan datanya seperti berikut:

{% gist page.gist ModularJDBCMahasiswa.java %}

Kalo anda jalankan maka hasilnya seperti berikut:

{% highlight bash %}
run-single:
INSERT JURUSAN---------------------

INSERT MAHASISWA ------------------

Jumlah data mahasiswa: 1
NIM: 10511148, Nama: 10511148, Nama Jurusan: Informatika
BUILD SUCCESSFUL (total time: 1 second)
{% endhighlight %}

Sisanya silahkan anda improve sendiri seperti update mahasiswa, dan hapus data. Ok mungkin cukup dulu pembahasan tentang modularisasi JDBC. see you next post!.
