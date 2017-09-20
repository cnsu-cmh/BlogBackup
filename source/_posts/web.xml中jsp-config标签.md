---
title: web.xml中jsp-config标签
date: 2016-10-19 13:48:37
tags: [java]
---
[文章来源:web.xml中jsp-config标签](http://blog.csdn.net/u011229848/article/details/52858749)


**WEB-INF/web,xml中<jsp-config>**
<!--more-->
<br/><jsp-config> 包括<taglib> 和<jsp-property-group> 两个子元素。
<br/>其中<taglib>元素在JSP 1.2时就已经存在；而<jsp-property-group>是JSP 2.0 新增的元素。
<br/><jsp-property-group>元素主要有八个子元素，它们分别为：
<br/>1.<description>：设定的说明；
<br/>2.<display-name>：设定名称；
<br/>3.<url-pattern>：设定值所影响的范围，如：/CH2 或 //*.jsp；
<br/>4.<el-ignored>：若为true，表示不支持EL 语法；
<br/>5.<scripting-invalid>：若为true，表示不支持<% scripting %>语法；
<br/>6.<page-encoding>：设定JSP 网页的编码；
<br/>7.<include-prelude>：设置JSP 网页的抬头，扩展名为.jspf；
<br/>8.<include-coda>：设置JSP 网页的结尾，扩展名为.jspf。
<br/>一个简单的<jsp-config>元素完整配置：
<br/><jsp-config>
<br/><taglib>
<br/><taglib-uri>Taglib</taglib-uri>
<br/><taglib-location>/WEB-INF/tlds/MyTaglib.tld</taglib-location>
<br/></taglib>
<br/><jsp-property-group>
<br/><description>Special property group for JSP Configuration JSP example.</description>
<br/><display-name>JSPConfiguration</display-name>
<br/><url-pattern>/jsp//* </url-pattern>
<br/><el-ignored>true</el-ignored>
<br/><page-encoding>UTF-8</page-encoding>
<br/><scripting-invalid>true</scripting-invalid>
<br/><include-prelude>/include/prelude.jspf</include-prelude>
<br/><include-coda>/include/coda.jspf</include-coda>
<br/></jsp-property-group>
<br/></jsp-config>
<br/>--------------------------------------------------------------------------------------------------
<br/>下面是我本地机器的jsp-config文件的配置，用来解决html页面乱码问题：
<br/><jsp-config>
<br/><jsp-property-group>
<br/><description>HTML Encoding</description>
<br/><display-name>HTML Encoding Config</display-name>
<br/><url-pattern>/*.html</url-pattern>
<br/><el-ignored>false</el-ignored>
<br/><page-encoding>UTF-8</page-encoding>
<br/><scripting-invalid>true</scripting-invalid>
<br/></jsp-property-group>
<br/></jsp-config>