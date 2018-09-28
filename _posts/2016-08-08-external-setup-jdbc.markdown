---
layout: post
title: "Externalisasi konfigurasi JDBC menggunakan file properties"
date: 2016-08-08T19:14:34+07:00
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
- https://docs.oracle.com/cd/E23095_01/Platform.93/ATGProgGuide/html/s0204propertiesfileformat01.html
---

Halo kembali lagi di blog saya, Jadi kali ini saya mau membahas tentang externalisasi konfigurasi pada JDBC yaitu tujuannya adalah memudahkan untuk melakukan perubahan konfigurasi ketika aplikasi di deploy ke server sesungguhnya. Jadi klo kita perhatikan pada koding [postingan sebelumnya]({% post_url  2016-08-06-modular-jdbc %}) seperti berikut:

{% highlight java %}
  @Override
  public List<Jurusan> selectAll() throws SQLException {
     Connection koneksi = DriverManager
      .getConnection("jdbc:mysql://localhost:3306/jdbc_mysql", "root", "admin");
     // logika select * from jurusan
     return null;
   }

  @Override
  public Jurusan save(Jurusan jurusan) throws SQLException {
     Connection koneksi = DriverManager
      .getConnection("jdbc:mysql://localhost:3306/jdbc_mysql", "root", "admin");
     // logika insert data jurusan
     return null;
   }
{% endhighlight %}

Nah kalo anda perhatikan, setiap kita mau lakukan sesuatu ke database entah itu select, insert, update ataupun delete kita pasti ada koding seperti berikut:

{% highlight java %}
   Connection koneksi = DriverManager
    .getConnection("jdbc:mysql://localhost:3306/jdbc_mysql", "root", "admin");
{% endhighlight %}

Nah jadi ketika aplikasi di deploy khan otomatis kita harus menyesuaikan dengan alamat lokasi server magsudnya lokasi servernya juga gak mungkin ```localhost``` pasti diganti dengan ip address. jadi masalhnya khan kita harus ganti tuh semua yang ada dalam method ```getConnection(bla,bla,bla)```, maka dari itu supaya memudahkan kali ini kita akan melakukan externalisasi dengan menggunakan file properties.

<!--more-->

Nah jadi saya jelaskan dulu cara menggunakan file properties, file properties ini pada dasarnya cuman ada 2 yaitu property name dan value. jadi kurang lebih kayak gini format penulisanya:

{% highlight properties %}
propName = propValue

propNama:propNilai
{% endhighlight %}

Jadi ```propName``` dan ```propNama``` adalah sebuah **key**, sedangkan ```propValue``` dan ```propNilai``` adalah **nilai** atau isinya. Nah klo udah paham sekarang kita buat file dengan nama ```jdbc.properties``` dalam folder ```src``` jadi hasilnya seperti berikut:

Pindahkan ke tab **File** seperti berikut:

![Folder src]({{ site.baseurl }}/assets/img/posts/external-setup-jdbc/folder-src.png)

Klik kanan di folder ```src``` kemudian pilih **File** lalu **Other** maka akan tampil form seperti berikut:

![new properties file]({{ site.baseurl }}/assets/img/posts/external-setup-jdbc/properties-file.png)

Setelah itu filter aja menggunakan key work ```prop``` seperti gambar diatas, lalu akan tampil **file properties** di klik terus **Next** selanjutnya tampil form seperti berikut:

![Properties file name]({{ site.baseurl }}/assets/img/posts/external-setup-jdbc/properties-file-name.png)

Nah sekarang kita tinggal kasih nama filenya contohnya kali ini saya mau namain filenya ```jdbc.properties``` setelah itu klik **Finish** maka hasilnya seperti berikut:

![Hasil membuat file properties]({{ site.baseurl }}/assets/img/posts/external-setup-jdbc/result-properties.png)

Nah pastikan ya anda membuatnya dalam folder ```src``` karena nanti kita menaggilnya akan dari classpath jadi tidak menggunakan absolut path seperti contohnya ```C:\Documents\bla\bla\jdbc.properties``` tapi kita load ke java dari file yang ada di dalam ```jar```.

Karena kita kali ini kasusnya hanya menggunakan JDBC aja jadi kita buat aja seperti berikut:

{% gist page.gist jdbc.properties %}

Nah sekarang kita tinggal buat bagaimana cara load file ```jdbc.properties``` tersebut:

Pertama buat kelas baru misalnya ```ExternalisasiSetupJDBC``` seperti berikut:

{% highlight java %}
package belajar.jdbc;

public class ExternalisasiSetupJDBC {
  public static void main(String[] args) throws IOException {

  }
}
{% endhighlight %}

setelah itu tambahkan fungsi tersebut ke dalam method main seperti berikut:

{% gist page.gist ExternalisasiSetupJDBC.java %}

Kalo dijalankan seperti berikut:

{% highlight bash %}
Host localhost
{% endhighlight %}

Nah sekarang anda tingal implementasikan ke struktur JDBCnya anda bisa gunakan variabel global atau membuat kelas baru contohnya ```JdbcConfig``` dalam package ```config```:

{% highlight java %}
package belajar.jdbc.config;

public class JdbcConfig {

}
{% endhighlight %}

Kemudian buat method yang mengembalikan nilai ```properties``` seperti berikut:

{% highlight java %}
package belajar.jdbc.config;

import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;
import java.util.logging.Level;
import java.util.logging.Logger;

public class JdbcConfig {

    public static Properties getJdbcProperties() {
        try {
            Properties prop = new Properties();
            InputStream input = JdbcConfig.class.getResourceAsStream("/jdbc.properties");
            prop.load(input);
            input.close();
            return prop;
        } catch (IOException ex) {
            Logger.getLogger(JdbcConfig.class.getName()).log(Level.SEVERE, null, ex);
            return null;
        }
    }
}
{% endhighlight %}

Kemudian di-sisi Repository atau bisanaya DAO (Data Access Object) kita rubah sedikit jadi seperti berikut atau tidak saya buat kelas baru yang mengimplementasikan interface ```RepositoryJurusan``` seperti berikut:

{% highlight java %}
package belajar.jdbc.service;

import belajar.jdbc.model.Jurusan;
import belajar.jdbc.repo.RepositoryJurusan;
import java.sql.SQLException;
import java.util.List;

public class ServiceJurusanExternConfig implements RepositoryJurusan {

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

Selanjutnya kita bisa buat variabel seperti berikut:

{% highlight java %}
package belajar.jdbc.service;

import belajar.jdbc.config.JdbcConfig;
import belajar.jdbc.model.Jurusan;
import belajar.jdbc.repo.RepositoryJurusan;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;
import java.util.Properties;

public class ServiceJurusanExternConfig implements RepositoryJurusan {

    private final Properties prop = JdbcConfig.getJdbcProperties();

    // jdbc:mysql://localhost:3306/jdbc_mysql
    private final String url
            = "jdbc:"
            + prop.getProperty("db") + "://"
            + prop.getProperty("host") + ":"
            + prop.getProperty("port") + "/"
            + prop.getProperty("dbname");

    @Override
    public List<Jurusan> selectAll() throws SQLException {
        Connection koneksi = DriverManager.getConnection(url, prop.getProperty("user"), prop.getProperty("passwd"));

        String sql = "select * from jurusan";
        List<Jurusan> daftarJurusan = new ArrayList<>();
        Statement st = koneksi.createStatement();
        ResultSet rs = st.executeQuery(sql);

        while (rs.next()) {
            Jurusan m = new Jurusan(rs.getString(1), rs.getString(2));
            daftarJurusan.add(m);
        }

        st.close();
        rs.close();
        return daftarJurusan;
    }

    // method lainnya
}
{% endhighlight %}

Silahkan yang lainnya juga diubah juga, sebagai latihan ya!. Ok mungkin cukup sekian dulu postingan kali ini tentang memisahkan konfigurasi ke file properties yang diterapkan ke JDBC. see you next post!.
