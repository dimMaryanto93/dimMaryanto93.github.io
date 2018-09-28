---
layout: post
title: "Install PostgreSQL on Fedora 23"
date: 2016-07-29T06:47:56+07:00
category: Fedora-23
gist: dimMaryanto93/62ffa0d81f3835a4e9401baf14590cd2
tags: 
- PostgreSQL
- Database
- SQL
author: Dimas Maryanto
references:
comments: true
---

![postresql logo]({{ site.baseurl }}/assets/img/posts/install-postgresql-f23/postgresql.png){:height="200px" width="260px"}

Halo, kali ini saya akan membahas bagaimana cara install **Database OpenSource** yang paling populer menurut [webiste db_enginers](http://db-engines.com/en/ranking "dilihat pada tanggal 29 juli 2016") PostgreSQL adalah database top 5 untuk kategori database dan top 4 untuk database relational yang bersanding bersama Oracle, Mysql dan Microsoft SQL Server.
<!--more-->

seperti biasa klo berbicara _operation system_ yang berkaitan dengan Linux atau UNIX pasti tidak akan lepas dari yang namanya **command line (terminal)** so buka terminalnya kemudian masukan script berikut:

{% highlight bash %}
$ sudo dnf install postgresql-server postgresql-contrib pgadmin3 -y
{% endhighlight %}

setelah semuanya di install tahap selajutnya adalah melakukan inisialisasi config untuk data. masukan command berikut berikut ini:

{% highlight bash %}
$ sudo postgresql-setup initdb
{% endhighlight %}


![Perintah initdb]({{ site.baseurl }}/assets/img/posts/install-postgresql-f23/postgresql-setup-initdb.png)

jika berhasil maka hasilnya seperti berikut:

![Hasil initdb]({{ site.baseurl }}/assets/img/posts/install-postgresql-f23/postgresql-initdb-post.png)

tetapi jika menemukan error seperti berikut:

![Error initdb]({{ site.baseurl }}/assets/img/posts/install-postgresql-f23/postgresql-initdb-error.png)

solusinya adalah hapus folder ```pgsql/data```

{% highlight bash %}
$ sudo rm -rf /var/lib/pgsql/
{% endhighlight %}

setelah itu lakukan lakukan **initdb lagi**, klo berhasil masuk ke step selanjutnya untuk saat ini berarti anda sudah menginstall Database di System anda, apakah udah selesai? tentu belum, kita harus **setting untuk menjalankan servicenya** tpi ini optional sih apakah mau dijalankan secara otomatis atau dijalankan secara manual oleh kita? klo mau otomatis berikut adalah perintahnya:

{% highlight bash %}
$ systemctl enable postgresql.service
{% endhighlight %}

nah klo outputnya seperti ini maka settingan ini udah bener:

```bash
Created symlink from /etc/systemd/system/multi-user.target.wants/postgresql.service to /usr/lib/systemd/system/postgresql.service.
```

nah sekarang kita harus jalankan servicenya untuk melakukan configurasi password untuk user atau schema defaultnya yaitu ```postgres``` dengan menggunakan perintah berikut:

{% highlight bash %}
$ sudo systemctl start postgresql.service
{% endhighlight %}

setelah itu bisa cek statusnya apakah sudah jalan atau failed dengan perintah berikut:

{% highlight bash %}
$ sudo systemctl status postgresql.service
{% endhighlight %}

ini adalah hasilnya:

```bash
postgresql.service - PostgreSQL database server
   Loaded: loaded (/usr/lib/systemd/system/postgresql.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2016-04-18 08:16:38 WIB; 7h ago
  Process: 5930 ExecStart=/usr/libexec/postgresql-ctl start -D ${PGDATA} -s -w -t ${PGSTARTTIMEOUT} (code=exited, status=0/SUCCESS)
  Process: 5926 ExecStartPre=/usr/libexec/postgresql-check-db-dir %N (code=exited, status=0/SUCCESS)
 Main PID: 5934 (postgres)
   CGroup: /system.slice/postgresql.service
           ├─5934 /usr/bin/postgres -D /var/lib/pgsql/data
           ├─5935 postgres: logger process   
           ├─5937 postgres: checkpointer process   
           ├─5938 postgres: writer process   
           ├─5939 postgres: wal writer process   
           ├─5940 postgres: autovacuum launcher process   
           └─5941 postgres: stats collector process   

Apr 18 08:16:37 localhost.localdomain systemd[1]: Starting PostgreSQL databas...
Apr 18 08:16:37 localhost.localdomain postgresql-ctl[5930]: LOG:  redirecting...
Apr 18 08:16:37 localhost.localdomain postgresql-ctl[5930]: HINT:  Future log...
Apr 18 08:16:38 localhost.localdomain systemd[1]: Started PostgreSQL database...
Hint: Some lines were ellipsized, use -l to show in full.
```

tahap selanjutnya jika servicenya udah jalan seperti output diatas kita harus update configurasi file berikut dengan cara update file ```/var/lib/pgsql/data/pg_hba.conf``` untuk lebih mudah bisa gunakan text editor default fedora yaitu ```gedit```

{% highlight bash %}
$ sudo gedit /var/lib/pgsql/data/pg_hba.conf
{% endhighlight %}

setelah file tersebut terbuka di ```gedit``` maka ubahlah **method** menjadi ```md5``` dan `address` menjadi `0.0.0.0/24` seperti berikut:

{% gist page.gist pg_hba.conf %}
 
kemudian restart servicenya

{% highlight bash %}
$ sudo systemctl restart postgresql.service
{% endhighlight %}

setelah itu coba login dengan user postgres dan db postgres (masih dalam root):

{% highlight postgresql-console %}
psql -h localhost -U postgres postgres
{% endhighlight %}

berikut outputnya:

{% highlight postgresql-console %}
Password for user postgres:
psql (9.4.7)
Type "help" for help.

postgres=#
{% endhighlight %}

setelah itu ubah password defaultnya menjadi (bebas terserah anda) klo saya menggunakan ```admin``` menggunakan perintah berikut

{% highlight postgresql-console %}
\password
{% endhighlight %}

berikut outpunya:

{% highlight postgresql-console %}
postgres=# \password
Enter new password: # admin
Enter it again: # admin
{% endhighlight %}

kemudian exit dari terminal dan buka lagi terminalnya setelah itu coba login sebagai postgres.

berikut hasilnya:

![Login as user normal]({{ site.baseurl }}/assets/img/posts/install-postgresql-f23/login-as-postgres.png)

penjelasanya:

* ```-h``` adalah hostname / ip address
* ```-U``` adalah username kemudian yang diikuti dengan nama databasenya, jadi klo kita perhatikan params diatas ada 2 postgres jadi maksudnya adalah **postgres yang pertama nama user / role** dan **postgres yang kedua adalah nama database**.
* ```-W``` digunakan untuk memastikan bahwa user tersebut memiliki password tetapi jika tidak memiliki password bisa menggukan ```-w```

ok mungkin cukup sekian dulu tentang bagaimana cara install postgresql di Fedora 23
