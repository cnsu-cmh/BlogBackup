---
title: MethodReplacer替换方法实例
date: 2017-10-16 15:45:31
tags: [java,spring]
---
[文章来源:MethodReplacer替换方法实例](http://blog.csdn.net//u011229848/article/details/78250480)


原文：[http://www.cnblogs.com/shizhongtao/p/3468713.html](http://www.cnblogs.com/shizhongtao/p/3468713.html)

MethodReplacer比较简单，改文章简单明了，直接贴的别人博客，望原著谅解。

org.springframework.beans.factory.support.MethodReplacer这个接口作用是替换方法时候用的。就是执行时候用新建的逻辑替换已有的方法逻辑。具体使用实例如下：

```java
public class MvcService{
     public String getTime(){
         SimpleDateFormat formate=new SimpleDateFormat("yy-MM-dd");
         return formate.format(new Date());
     }
 }
```
替代的类及方法：
```java
import java.lang.reflect.Method;
import java.text.SimpleDateFormat;
import java.util.Calendar;

import org.springframework.beans.factory.support.MethodReplacer; 

public class MvcServiceReplaceImpl implements MethodReplacer{
     
     @Override
     public Object reimplement(Object arg0, Method arg1, Object[] arg2)
             throws Throwable {
         SimpleDateFormat formate=new SimpleDateFormat("yy-MM-dd HH:mm:ss.SS");
         Calendar c=Calendar.getInstance();
         c.add(Calendar.YEAR, 2);
         return formate.format(c.getTime());
     }
 
 }
 
```
配置文件：

```xml
    <bean id="mvcService" class="com.bing.service.MvcService">
         <replaced-method name="getTime" replacer="replacementComputeValue">
             <!-- <arg-type>String</arg-type> -->
         </replaced-method>
 
     </bean>
     <bean id="replacementComputeValue" class="com.bing.service.MvcServiceReplaceImpl" />
```
这里的意思是用MvcServiceReplaceImpl中的方法替代类com.bing.service.MvcService中的getTime方法。当你运行时候返回的结果就不是上面的“yy-MM-dd”的时间格式而是"yy-MM-dd HH:mm:ss.SS"的时间格式