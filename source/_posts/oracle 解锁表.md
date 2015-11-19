---
title: oracle 解锁表
date: 2017-10-17 11:46:42
tags: [oracle]
---
[文章来源:oracle 解锁表](http://blog.csdn.net//u011229848/article/details/78258828)


## 数据库执行增删改出现 oralce record is locked by another user

oralce 有个锁机制，一旦一张表的数据被更新，删除，修改 而没有commit (提交)，那么PL/SQL就会执行锁命令，把这张表给锁定，使得智能查询，一切增删改都无法操作，那么这个时候我们就要解锁了。

查看当前锁：
```sql
select t2.username,t2.sid,t2.serial/#,t2.logon_time from v$locked_object t1,v$session t2 where t1.session_id=t2.sid ;

```

解锁（杀进程）:
```sql
alter system kill session 'sid,serial/#';
```
