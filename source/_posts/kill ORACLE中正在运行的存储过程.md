---
title: kill ORACLE中正在运行的存储过程
date: 2017-05-11 18:52:25
tags: [oracle]
---
[文章来源:kill ORACLE中正在运行的存储过程](http://blog.csdn.net//u011229848//article/details/71668877)


kill ORACLE中正在运行的存储过程
```sql
select * from v$access o where o.OBJECT like 'P_BDC2ZJ_FC%' --查找正在运行的对象,获取sid
select a.serial/# from v$session a WHERE A.SID=sid --通过sid 获取serial/#

alter system kill session 'sid,serial/#' --eg: alter system kill session '123,3211'
```
查找死锁
```sql
select sql_text,s.sid,s.SERIAL/# from v$sql sql inner join v$session s on sql.hash_value=s.sql_hash_value
where sid in (select session_id from v$locked_object)
```
