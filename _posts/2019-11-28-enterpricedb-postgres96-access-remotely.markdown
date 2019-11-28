---
layout: post
title: "How to configure EnterpriseDB PostgreSQL can access remotely on Linux"
date: 2019-11-28T20:39:41+07:00
category: postgresql
tags: 
- enterpisedb
- PostgreSQL
- Centos
- UBuntu
author: Dimas Maryanto
references:
- https://www.enterprisedb.com/enterprise-postgres/edb-postgres-platform
- https://support.plesk.com/hc/en-us/articles/115003321434-How-to-enable-remote-access-to-PostgreSQL-server-on-a-Plesk-server-
comments: true
---

![enterprisedb-postgres]({{site.baseurl}}/assets/img/posts/enterprisedb-access-remote/logo.png)

by default database PostgreSQL tidak dapat di akses secara remote / outside network, Untuk grant access remote database EnterpriseDB PostgreSQL berikut caranya:

<!--more-->

Edit `postgresql.conf` biasanya lokasinya di folder `/opt/PostgreSQL/[version]/data` dan cari properties `listener_address` kemudian edit menjadi seperti ini:

```ini
listen_addresses = '*'
```

dan edit `pg_hba.conf` biasanya lokasinya di folder `/opt/PostgreSQL/[version]/data` kemudian edit seperti berikut:

```ini
host all all 0.0.0.0/0 md5
```

Setelah itu restart PostgreSQL server service, dengan cara login user `postgres`

```bash
# login as postgres
su postgres

# restart service PostgreSQL
/opt/PostgreSQL/[version]/bin/pg_ctl restart -D /opt/PostgreSQL/[version]/data
```

