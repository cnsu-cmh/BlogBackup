---
title: java中instanceof，isInstance ，isAssignableForm的使用
date: 2016-11-16 23:22:48
tags: [java]
---
[文章来源:java中instanceof，isInstance ，isAssignableForm的使用](http://blog.csdn.net//u011229848//article/details/53192954)


## java中instanceof，isInstance，isAssignableForm的使用

1. **instanceof**

****

**介绍：** java的关键词之一，反射过程中常常调用。
**语法：** A instanceof B ，翻译为 A是B的实例

**用法： a** （a：实例化后的对象） **instanceof B** （B：ClassName，类名）
**举例：**

*String s ="";*
*s instanceof Object;*

这里Object一定要是一个具体的类，如果传进来的是一个泛型
Class<T> clazz

User u = new User();
u instanceof clazz；

这样将会编译错误，因为clazz是不确定不具体的类，这将用下面方法解决
2. **isInstance()**

****
**介绍：** java.lang.Class中的方法

**语法：** A.isinstance( B ) , 翻译为B是A的实例
**用法：** (A：Class，java标准类对象) **A.isInstance( b )** (b：java实例)
**举例：**

*Object ob=new Object();*

*String s="";*
*ob.getClass().isInstance(s);*
这里对于1中instanceof 泛型时做处理

Class<T> clazz

User u = new User();
clazz isInstance(u);

3. **isAssignableForm()**

****
**介绍：** java.lang.Class中的方法

**语法：** A.isAssignableForm( B ) , 翻译为B是A的子类
**用法：** (A：Class，java标准类对象) **A.isAssignableForm( B)** (B： Class，java标准类对象 )

**举例：**
*Object ob=new Object();*

*String s="";*
*ob.getClass().isAssignableFrom(s.getClass());*

今天与同事在使用泛型时判断该泛型类是否与实例化类是同一个类时发现问题，在这里做个记录，希望对于instanceof ,isInstance有所记忆，区分使用。希望对读者会有所帮助，共勉之......

instanceof 是静态比较。instanceof 后面的类名在编译时就已知且固定了，即 obj instanceof ClassA，ClassA 必须是已经存在的类名，不然编译都过不了。

isInstance() 是动态比较。isInstance() 的左边可以在运行时决定，即可以这样 objA.getClass().isInstance(objB)，objA 可以作为某个方法的参数被传进来，这样可以动态的看两个对象是否类型兼容。

这是主要区别，如果还有其它区别就是 instanceof 是 Java 内置的比较运算符，isInstance() 是个方法。