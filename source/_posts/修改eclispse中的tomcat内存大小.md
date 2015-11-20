---
title: 修改eclispse中的tomcat内存大小
date: 2017-02-09 18:08:29
tags: [java,tomcat]
---
[文章来源:修改eclispse中的tomcat内存大小](http://blog.csdn.net/u011229848/article/details/54950743)


修改1：

在Eclipse中下面Servers双击Tomcat Server... 然后点击General InformAtion 下的Open launch configuration；

会弹出Edit Configuration，然后在选中Atguments选项卡；在VM atguments文本框中最后面添加

-Xms256m -Xmx1024m -XX:MaxPermSize=256m 。

修改2：

在Eclipse菜单栏中Window ——》Preferences ——》Server ———》 Runtime Environment；

选择您用的Tomcat 然后点击Edit...弹出Edit Server Runtime Ecvironment 下面JRE选项后面的Installed JREs...

点击弹出Installed JREs；在选中您用的Tomcat在点击Edit..在Defaul VM Atguments：中填入-Xms256m -Xmx1024m

-XX:MaxPermSize=256m。

修改3：

在Eclipse项目列表里找到Servers，展开后有所有配置的tomcat服务，点击你需要的服务右键Run As Run Configuration...; 然后在选中Atguments选项卡；在VM atguments文本框中最后面添加

-Xms256m -Xmx1024m -XX:MaxPermSize=256m 。