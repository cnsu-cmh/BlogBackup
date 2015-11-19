---
title: hibernate 的 CascadeType 属性
date: 2016-11-18 00:12:05
tags: [java]
---
[文章来源:hibernate 的 CascadeType 属性](http://blog.csdn.net/u011229848/article/details/53208818)

今天同时遇到一个问题，级联保存的时候遇到Error : javax.persistence.EntityExistsException异常，查阅资料后发现是因为配置文件中一对多配置的CascadeType.PERSIST修改为CascadeType.MERGE 或者在方法上面添加事物的注解（暂时不知道这里添加事物注解能解决的原因）

CascadeType.PERSIST只有A类新增时，会级联B对象新增。若B对象在数据库存（跟新）在则抛异常（让B变为持久态）CascadeType.MERGE指A类新增或者变化，会级联B对象（新增或者变化）CascadeType.REMOVE只有A类删除时，会级联删除B类；CascadeType.ALL包含所有；

CascadeType.REFRESH：级联刷新，当多个用户同时作操作一个实体，为了用户取到的数据是实时的，在用实体中的数据之前就可以调用一下refresh()方法！
ascadeType.REFRESH：级联刷新，当多个用户同时作操作一个实体，为了用户取到的数据是实时的，在用实体中的数据之前就可以调用一下refresh()方法！

CascadeType.REFRESH：级联刷新，当多个用户同时作操作一个实体，为了用户取到的数据是实时的，在用实体中的数据之前就可以调用一下refresh()方法！