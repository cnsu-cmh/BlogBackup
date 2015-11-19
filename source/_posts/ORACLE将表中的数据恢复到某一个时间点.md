---
title: ORACLE将表中的数据恢复到某一个时间点
date: 2018-06-08 14:38:07
tags: [oracle]
---
[文章来源:ORACLE将表中的数据恢复到某一个时间点](http://blog.csdn.net/u011229848/article/details/80622875)


版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/u011229848/article/details/80622875

今早不小心删了表中的部分数据，由于存在新建的数据未备份就被误删除，oracle没有开启闪回，也没记录日志，最后根据某一时刻的数据还原，具体操作如下：

**根据oracle自己的快照备份查询某一时刻的某张表数据**
```sql
select /*from 表名 **as of timestamp to_timestamp**('2018-06-08 11:06:00', 'yyyy-MM-dd HH:mi:ss');
```
可直接删除表数据，再插入历史快照数据：
```sql
delete from 表名;

commit;

insert into 表名 select /*from 表名 as of timestamp to_timestamp('2018-06-08 11:06:00', 'yyyy-MM-dd HH:mi:ss');
```