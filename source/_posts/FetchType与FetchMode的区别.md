---
title: FetchType与FetchMode的区别
date: 2017-10-16 14:27:10
tags: [java]
---
[文章来源:FetchType与FetchMode的区别](http://blog.csdn.net/u011229848/article/details/78249338)


原文出处：[http://fantasy-lixk.iteye.com/blog/1602797](http://fantasy-lixk.iteye.com/blog/1602797)

使用例：

@OneToMany(mappedBy="item",cascade=CascadeType.ALL,fetch=FetchType.EAGER)

@Fetch(value=FetchMode.SUBSELECT)

两者比较：
<!--more-->
两者都是设定关联对象的加载策略。前者是JPA标准的通用加载策略注解属性，

后者是Hibernate自有加载策略注解属性。

FetchType可选值意义与区别如下：

FetchType.LAZY: 懒加载，在访问关联对象的时候加载(即从数据库读入内存)

FetchType.EAGER:立刻加载，在查询主对象的时候同时加载关联对象。

FetchMode可选值意义与区别如下：

@Fetch(FetchMode.JOIN)： 始终立刻加载，使用外连(outer join)查询的同时加载关联对象，忽略FetchType.LAZY设定。

@Fetch(FetchMode.SELECT) ：默认懒加载(除非设定关联属性lazy=false)，当访问每一个关联对象时加载该对象，会累计产生N+1条sql语句

@Fetch(FetchMode.SUBSELECT) 默认懒加载(除非设定关联属性lazy=false),在访问第一个关联对象时加载所有的关联对象。会累计产生两条sql语句。且FetchType设定有效。