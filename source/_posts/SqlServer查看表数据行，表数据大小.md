---
title: SqlServer查看表数据行，表数据大小
date: 2018-10-18 12:56:32
tags: [sql]
---

### SqlServer查看表数据行，表数据大小

```sql
--创建零时表tableUsedInfo
if exists(select 1 from tempdb..sysobjects where id=object_id('tempdb..tableUsedInfo') and xtype='u')
  drop table tableUsedInfo
go
create table tableUsedInfo(
  tabname varchar(100),
  rowsNum varchar(100),
  reserved varchar(100),
  data varchar(100),
  index_size varchar(100),
  unused_size varchar(100)
);
```
<!--more-->
```sql
--查询表数据使用情况，插入到零时表
declare @name varchar(100)
declare cur cursor for
  select name from sysobjects where xtype='u' order by name
open cur
fetch next from cur into @name
while @@fetch_status=0
  begin
    insert into tableUsedInfo
    exec sp_spaceused @name
    --print @name

    fetch next from cur into @name
  end
close cur
deallocate cur;

--查询表使用情况
select tabname as '表名',rowsNum as '表数据行数',reserved as '保留大小',data as '数据大小',index_size as '索引大小',unused_size as '未使用大小'
from tableUsedInfo
--where tabName not like 't%'
order by cast(rowsNum as int) desc

```