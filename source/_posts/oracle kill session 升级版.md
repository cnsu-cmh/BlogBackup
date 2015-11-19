---
title: oracle kill session 升级版
date: 2018-04-27 16:57:17
tags: [oracle]
---
[文章来源:oracle kill session 升级版](http://blog.csdn.net//u011229848/article/details/80110433)


```sql
SELECT a.object_id, a.session_id, b.object_name, c.* FROM v$locked_object a, dba_objects b, v$session c
WHERE a.object_id = b.object_id AND a.SESSION_ID = c.sid(+);

alter system kill session '11,314';
```
