---
title: jstl fmt标签
date: 2016-03-18 00:40:24
tags: [jstl]
---
[文章来源:jstl fmt标签](http://blog.csdn.net//u011229848//article/details/50922293)


版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/u011229848/article/details/50922293

1）导入jstl 包，加载ftm标签

首先将jstl的jar包放入类库中，使用1.2版本

其次在jsp文件中引入所需要的 标记库，对于 ftm 标签，如下：

```htm
<%@ taglib prefix='fmt' uri="http://java.sun.com/jsp/jstl/fmt" %>
```
2）输出 .properties 文件中的信息
```html
 <fmt:bundle basename="fmt"> 
    test value:<fmt:message key="test" />
</fmt:bundle>
```

其中 <fmt:bundle basename="fmt"> 指定了资源文件的位置，例如： fmt 表示类根路径下的 fmt.properties 文件，my.fmt 表示 包my下的ftm.properties文件；

<fmt:message key="test" />表示读取 key为test的值，并输出；

3）给出1个例子，包含许多标签的使用

fmt.jsp:
```html
<%@ page language="java" contentType="text/html; charset=utf-8" pageEncoding="utf-8"%>  
<%@ taglib prefix='c' uri="http://java.sun.com/jsp/jstl/core" %>  
<%@ taglib prefix='fmt' uri="http://java.sun.com/jsp/jstl/fmt" %>  
<%  
String path = request.getContextPath();  
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";  
%>  
  
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">  
<html>  
  <head>  
    <base href="<%=basePath%>">  
    <!-- 
    <link rel="stylesheet" type="text/css" href="styles.css"> 
    -->  
    <style type="text/css">  
        body {background-color: black;color: white;}  
        span {text-align: center;color: green;background-color: yellow;}  
        .notice {color: rgb(250,37,62);}  
        hr { background-color: fuchsia; height: 5px;}  
    </style>  
  </head>  
    
  <body>  
    <fmt:bundle basename="jstl.jstl">  
        <span>从 .properties 文件中读取最简单的信息输出：</span>  
        <fmt:message key="basemsg" />  
        <hr/>  
        <span>从 .properties 文件中读取带有可填参数的信息，填入参数并输出：</span>  
        <fmt:message key="msgwithparam">  
            <span class="notice"><fmt:param value="param-1-value" />  
            <span class="notice"><fmt:param value="param-2-value" />  
        </fmt:message>  
        <hr/>  
        <span>数字 格式化并输出：</span><br/>  
        数字:<fmt:formatNumber value="1234567890" type="number"/><br/>  
        <!-- 定制数字格式时，# 表示按照默认格式来， -->  
        数字，定制了格式:<fmt:formatNumber value="1234567890" type="number" pattern="#,#00.0#" /><br/>  
        货币：<fmt:formatNumber value="35000" type="currency" /><br/>  
        百分比：<fmt:formatNumber value="0.317" type="percent" /><br/>  
        <hr/>  
        <span>格式化日期：</span><br/>  
        <jsp:useBean id="now" class="java.util.Date"></jsp:useBean>  
        <fmt:formatDate  value="${now}" type="date" /><br/>  
        <fmt:formatDate  value="${now}" type="both" dateStyle="long" timeStyle="long" /><br/>  
        <fmt:formatDate  value="${now}" type="both" pattern="yyyy.MM.dd HH:mm:ss" /><br/>  
        <hr/>  
        <span>将字符串转化到正确的数字：<br/>  
        忽略第一个不符合数字条件的字符和其之后的所有字符，如果字符串不是以数字开头则报错</span><br/>  
        <fmt:parseNumber type="number" >123.02a</fmt:parseNumber><br/>  
        <fmt:parseNumber type="number" pattern="#,#00.0#">123</fmt:parseNumber><br/>  
        <fmt:parseNumber type="number" pattern="#,#00.0#">123.00a1</fmt:parseNumber><br/>  
        <fmt:parseNumber type="number" pattern="#,#00.0#">3saaa</fmt:parseNumber><br/>  
          
    </fmt:bundle>  
          
  </body>  
</html>  
```