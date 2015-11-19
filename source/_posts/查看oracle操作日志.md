---
title: 查看oracle操作日志
date: 2018-09-04 15:35:58
tags: [oracle]
---
[文章来源:查看oracle操作日志](http://blog.csdn.net/u011229848/article/details/82384530)

查看oracle操作日志

```sql
select t.SQL_TEXT, t.FIRST_LOAD_TIME
from v$sqlarea t
where t.FIRST_LOAD_TIME like '2018-09-04%'
order by t.FIRST_LOAD_TIME desc
```
