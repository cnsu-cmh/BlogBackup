---
title: oracle创建用户
date: 2017-09-04 15:34:52
tags: [oracle]
---
[文章来源:oracle创建用户](http://blog.csdn.net/u011229848/article/details/82384461)


版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/u011229848/article/details/82384461

创建表空间
```sql
create tablespace ZJ2BDC datafile 'd:\app\Administrator\product\11.2.0\dbhome_1\oracore\ORCL\ZJ2BDC.DBF' size 4096m autoextend on next 5m extent management local;
```
创建用户
```sql
create user REDDATA identified by xqx1234 default tablespace ZJ2BDC;
```
授予权限
```sql
grant dba to REDDATA;
```

