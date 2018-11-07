---
layout: post
title: "Konfigurasi ssl/https for httpd on rhel7"
date: 2018-11-07T21:54:48+07:00
category: rhel7
tags: 
- Apache Httpd
- Redhat 7.4
- SSL
- https
author: Dimas Maryanto 
references:
- https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/security_guide/sec-using_openssl
comments: true
---

![openssl]({{ site.baseurl }}/assets/img/posts/httpd-ssl-rehel7/openssl.jpg){: width="300px" }

Untuk mengatifkan ssl di web server Apache Httpd, ada beberapa yang harus kita siapkan yaitu diataranya:

- [Repo from dvd]({% post_url 2018-09-11-enabled-yum-rhel7 %}) atau subscribe redhat 7.4 repository
- [How to install httpd]({% post_url 2018-11-07-install-httpd-rhel7 %}) 
- openssl (Untuk generate private key dan public key)
- mod_ssl
- firewalld (Untuk membuat port 443)

<!--more-->

The first think, you should install dependencies like httpd, mod_ssl, openssl

```bash
yum install mod_ssl openssl
```

Setelah instal kita open port `443/tcp` berikut perintahnya:

```bash
# to open https protocol
firewall-cmd --zone=public --add-port=443/tcp --permanent

## reload config firewall
firewall-cmd --reload
```

Kemudian kita buat private dan public keynya dengan menggunakan `openssl` berikut perintahnya:

```bash
## contoh basic sintaks
#  openssl req -x509 -nodes -days 365 -newkey rsa:2048 
#  -keyout www_example_com.key 
#  -out www_example_com.crt

## domain (Common Name) kita adalah repository.dimas-maryanto.com kemudian kita simpan filenya di /etc/pki/tls/
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /etc/pki/tls/private/repository_dimas_maryanto_com.key \
    -out /etc/pki/tls/certs/repository_dimas_maryanto_com.crt
```

Mantinya akan muncul form seperti berikut:

![form information crt]({{site.baseurl}}/assets/img/posts/httpd-ssl-rehel7/generate-crt-key.png)

Berikut deskripsinya:

- `Country Name (2 letter code) [XX]:` masukan saja `ID` karena kita lokasinya di Indonesia
- `State or Province: ` masukan lokasi anda seperti `Jawa Barat`, `Jakarta` dan lain-lain
- `Locality Name: ` masukan kota anda seperti `Bandung`, `Jakarta Selatan` dan lain-lain
- `Organitation Name: ` masukan nama PT anda, klo saya dikosongkan
- `Common Name: ` masukan **domain name** anda seperti `repository.dimas-maryanto.com`
- `Email Address:` masukan email address anda, klo saya di kosongkan.

Setelah itu nanti kita akan dapat file `/etc/pki/tls/private/tabeldata_ip_dynamic_com.key` untuk private key, disarankan ini untuk di backup dan satu file lagi yaitu public keynya yang nantinya kita upload ke SSL/TLS Certifacate seperti contohnya:

- [DigiCert](https://www.digicert.com)
- [Comodo SSL Certifacates](https://www.ssls.com)

Setelah itu kita modif file `/etc/httpd/conf.d/ssl.conf` dengan mengganti property seperti berikut:

- `#ServerName www.example.com:443` menjadi `ServerName repository.dimas-maryanto.com:443`
- `SSLCertificateFile /etc/pki/tls/certs/localhost.crt` menjadi `SSLCertificateFile /etc/pki/tls/certs/repository_dimas_maryanto_com.crt`
- `SSLCertificateKeyFile /etc/pki/tls/private/repository_dimas_maryanto_com.key` menjadi `SSLCertificateKeyFile /etc/pki/tls/private/localhost.key`

Berikut full versinya:

{% gist dimMaryanto93/b23009663fcc3c143eee157f4165f5df ssl.conf %}

Kemudian restart Apache httpd server

```bash
apachectl restart
```

Kemudian kita coba, di browser dengan menggunakan https berikut alamanya [https://{IP-SERVER}](https://localhost) maka akan muncul halaman berikut:

![ssl exception allow]({{site.baseurl}}/assets/img/posts/httpd-ssl-rehel7/ssl-exception-allow.png)

Nah karen kita belum beli, certificatenya atau kita bisa aja sih pake yang gratis seperti menggunakan [let's encritp](https://letsencrypt.org/) nanti kita tinggal ubah aja path nya yang di configurasi httpd. Tpi sekarang kita `Add Exception` aja ya, setelah itu di url browser kita bisa lihat certificatenya seperti berikut:

![https]({{site.baseurl}}/assets/img/posts/httpd-ssl-rehel7/httpd-with-https.png)

**View security ssl**

![view security]({{site.baseurl}}/assets/img/posts/httpd-ssl-rehel7/view-security.png)

**View certificate ssl**

![View certificate]({{site.baseurl}}/assets/img/posts/httpd-ssl-rehel7/view-certificate.png)
