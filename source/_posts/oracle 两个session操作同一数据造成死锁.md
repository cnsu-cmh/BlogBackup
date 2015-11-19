---
title: oracle 两个session操作同一数据造成死锁
date: 2018-08-01 11:41:06
tags: [oracle]
---
[文章来源:oracle 两个session操作同一数据造成死锁](http://blog.csdn.net//u011229848/article/details/81327728)


今天新写的一个系统添加数据源使用jdbcTemplate操作另一系统数据库（oracle），原系统(struts1+hibernate)操作完之后数据库中session依旧存在，新系统再操作这条记录出现死锁，最终找到原因----------- 两个session操作一条记录，原因是没有及时提交事务。

原系统Hibernate执行update没有使用事务，业务执行完发现oracle库中session仍然存在，这个时候其他session不能更新该条记录。

更改原系统，执行update添加事务，手动提交事务后oracle库中session立刻不存在了，问题解决。