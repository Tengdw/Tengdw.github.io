---
title: MySql数据库忘记密码怎么办？
date: 2017-11-02 23:16:46
tags: Linux 数据库
category: 服务器
---
环境：CentOS 7，MariaDB 5.5
第一步：
`vim /etc/my.cnf`
```
[mysqld]
skip-grant-tables #添加这句跳过密码验证
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
# Settings user and group are ignored when systemd is used.
# If you need to run mysqld under a different user or group,
# customize your systemd unit file for mariadb according to the
# instructions in http://fedoraproject.org/wiki/Systemd

[mysqld_safe]
log-error=/var/log/mariadb/mariadb.log
pid-file=/var/run/mariadb/mariadb.pid

#
# include all files from the config directory
#
!includedir /etc/my.cnf.d

```
第二步：
重启`systemctl restart mariadb`
无密码登录`mysql -u root -p`提示要输入密码直接回车就行
```
[root@VM_129_4_centos ~]# mysql -u root -p #无密码登录
Enter password: #直接回车
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 3
Server version: 5.5.50-MariaDB MariaDB Server

Copyright (c) 2000, 2016, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> use mysql; #注意每句SQL都要带 ; 号
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [mysql]> update user set authentication_string=password('123456') where user='root'; #更改root密码为123456
Query OK, 4 rows affected (0.00 sec)
Rows matched: 4  Changed: 4  Warnings: 0

MariaDB [mysql]> flush privileges; #执行
Query OK, 0 rows affected (0.00 sec)

MariaDB [mysql]> exit; #退出
Bye

```

