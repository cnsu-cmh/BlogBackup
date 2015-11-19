---
title: sql查询表默认约束 删除和添加约束
date: 2018-05-15 19:33:25
tags: [sql]
---
[文章来源:sql查询表默认约束 删除和添加约束](http://blog.csdn.net/u011229848/article/details/80328066)

```sql
1. declare@namevarchar(100)
1. --DF为约束名称前缀
1. select@name=b.namefromsyscolumns a,sysobjects bwherea.id=object_id('表名')andb.id=a.cdefaultanda.name='字段名'
1. select@name
1. --删除约束
1. altertableOaNewdropconstraintDF__OaNews__NewsTitl__72C60C4A--删约束
1. altertableOaNewaltercolumnNewsTitle nvarchar(250)--改类型,如果原先是300 varchar 就要改成 300nvarchar 不改会转换失败
1. USE [数据库名称] --添加约束,可以找到约束 右键脚本create到新建查询窗口
1. GO
1. ALTERTABLE[dbo].表名ADDCONSTRAINTDF__OaNews__NewsTitl__72C60C4ADEFAULT('')FOR[字段名]
1. GO
1. USE [数据库名称]
1. GO
```