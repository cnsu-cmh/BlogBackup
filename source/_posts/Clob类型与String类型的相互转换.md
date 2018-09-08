---
title: Clob类型与String类型的相互转换
date: 2017-05-02 15:43:50
tags: [java]
---
[文章来源:Clob类型与String类型的相互转换](http://blog.csdn.net/u011229848/article/details/71082056)

```java
// Clob类型转换成String类型  
public String ClobToString(final Clob clob) {  
      
    if (clob == null) {  
        return null;  
    }  
      
    Reader is = null;  
    try {  
        is = clob.getCharacterStream();  
    } catch (Exception e) {  
        e.printStackTrace();  
    }  
    BufferedReader br = new BufferedReader(is);  
  
    String str = null;  
    try {  
        str = br.readLine();    // 读取第一行  
    } catch (Exception e) {  
        e.printStackTrace();  
    }  
  
    StringBuffer sb = new StringBuffer();  
    while (str != null) {    // 如果没有到达流的末尾，则继续读取下一行  
        sb.append(str);  
        try {  
            str = br.readLine();  
        } catch (Exception e) {  
            e.printStackTrace();  
        }  
    }  
      
    return sb.toString();  
      
}  
  
// String类型转换成Clob类型  
public Clob StringToClob(final String string) {  
  
    if(string == null || string.trim().length() == 0){  
        return null;  
    }  
    return new org.hibernate.lob.ClobImpl(string);  
}
```
