---
title: Oracle和plsql developer编码设置
date: 2017-09-20 00:42:08
tags: [oracle]
---
[文章来源:Oracle和plsql developer编码设置](http://blog.csdn.net/u011229848/article/details/78039477)

在使用pl/sql developer时，查询出来中文字段显示乱码，因为数据库的编号格式和pl /sql developer的编码格式不统一造成的。

一、查看和修改oracle数据库字符集

```sql
select userenv('language') from dual; --查询结果： 
```
-- SIMPLIFIED CHINESE_CHINA.AL32UTF8 
--修改oracle数据库字符集：（在SQL Plus中） 

sql> conn / as sysdba; 

sql> shutdown immediate; 
 <br/>database closed. 
 <br/>database dismounted. 
 <br/>oracle instance shut down. 
 
sql> startup mount; 
<br/>oracle instance started. 
<br/>total system global area 135337420 bytes fixed size 452044 bytes variable size 109051904 bytes database buffers 25165824 bytes redo buffers 667648 bytes database mounted. 
 
sql> alter system enable restricted session;
<br/>system altered. 
 
sql> alter system set job_queue_processes=0; 
<br/>system altered. 
  
 sql> alter system set aq_tm_processes=0; 
<br/>system altered. 

sql> alter database open; 
 <br/>database altered. 
 
 sql> alter database character set internal_use JA16SJIS; 
 
 sql> shutdown immediate;
  
 sql> startup;

# 二、修改pl/sql developer 的编码

在windows中创 建一个名为“NLS_LANG”的系统环境变量，设置其值为“SIMPLIFIED CHINESE_CHINA.ZHS16GBK”，然后重新启动 pl/sql developer，这样检索出来的中文内容就不会是乱码了。如果想转换为UTF8字符集，可以赋予“NLS_LANG”为 “AMERICAN_AMERICA.UTF8”，然后重新启动 pl/sql developer。其它字符集设置同上

文章转载于博客园 http://www.cnblogs.com/geekdc/p/5817306.html