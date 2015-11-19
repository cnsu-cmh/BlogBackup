---
title: java，Linux通用换行代码
date: 2016-11-15 23:39:20
tags: [java]
---
[文章来源:java，Linux通用换行代码](http://blog.csdn.net//u011229848//article/details/53180153)

lunix 下换行符只有: \n

Mac 下换行符有：\r

windows 下换行方式： \r\n

代码可移植，换行统一写成 System.getProperty("line.separator")
```java
public class newLineTest {  

    public static void main(String[] args) {  
        String newLine = System.getProperty("line.separator");  
        System.out.println("我是第一行" + newLine + "我是第二行");  
    }  
}  
```
