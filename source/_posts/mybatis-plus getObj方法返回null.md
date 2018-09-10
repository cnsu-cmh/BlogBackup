---
title: mybatis-plus getObj方法返回null
date: 2018-08-18 20:13:23
tags: [java,mybatis,mybatis-plus]
---
[文章来源:mybatis-plus getObj方法返回null](http://blog.csdn.net//u011229848/article/details/81812235)


在mybatis-plus低版本中自定义查询，selectObj(Condition.create().setSqlSelect("columns..."))，低版本中
```java
    selectObj(Condition.create().setSqlSelect("max(sort)").isNull("parent_id"))；
 ```

返回Object为查询结果max(sort)。可自己做类型转换。

升级到高版本后放弃Condition拼接SQL,代码如下：
```java 
    QueryWrapper<Menu> wrapper = new QueryWrapper<>(); 
    Object o = getObj(wrapper.select("max(sort)").isNull("parent_id"));
```

此处返回的Object为空，认真一点会发现，返回对象应该是泛型对应的Menu对象，个人暂认为（猜测）是属性名对应不到实体，所以返回时处理了异常返回null（在下在这里偷懒了，没看源码全个人猜想，猜想错了望批评指正），我加上别名的时候发现跟我预想的一样。
<!--more-->
```java
    QueryWrapper<Menu> wrapper = new QueryWrapper<>(); 
    Object o = getObj(wrapper.select("max(sort) as sort").isNull("parent_id"));
```
此时返回一个Menu对象，sort为我需要的结果。希望看到文章的你也能细心一点，注意返回对象。
```java
QueryWrapper<Menu> wrapper = new QueryWrapper<>(); 
Object o = getObj(wrapper.select("max(sort) as sort").isNull("parent_id"));
```