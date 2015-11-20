---
title: 加密Web项目中配置文件中的密码
date: 2017-10-16 14:48:39
tags: [java]
---
[文章来源:加密Web项目中配置文件中的密码](http://blog.csdn.net/u011229848/article/details/78249673)


原文出处： [http://blog.csdn.net/ziwen00/article/details/10729683](http://blog.csdn.net/ziwen00/article/details/10729683)

我们使用的项目经常是这个样子的：

<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource"  
    destroy-method="close"   
    p:driverClassName="oracle.jdbc.driver.OracleDriver"  
    p:url="jdbc:oracle:thin:@127.0.0.1:1523:orcl"   
    p:username="czw"  
    p:password="czw" /> 
这里会有一个致命的问题，如果有一个具备中间件服务器机器访问权限的人，看到了这个例如applicationContext.xml的文件，并且打开该文件，智商再低下的人也会知道数据库的用户名和密码是什么。这对于对安全有一定要求的行业是必须杜绝的，这个也是在一般技术面试中会问到的一个问题。那就让我们继续往下，解答这个问题吧！

首先，我们需要将配置文件抽取到property中来：
<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer"  
    p:location="classpath:jdbc.properties"  
    p:fileEncoding="utf-8"  
    />  
      
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource"  
    destroy-method="close"   
    p:driverClassName="${driverClassName}"  
    p:url="${url}"   
    p:username="${userName}"  
    p:password="${password}" /> 

将上面的第一个代码修改为第二个代码，第一个类是负责抓取jdbc.properties中的属性并且填充到dataSource当中来，这样，我们就可以将所有的注意力都集中在jdbc.properties上了。
下面的问题是，如何将jdbc.properties变成一个看不明白的字符呢？我们只需要扩展PropertyPlaceholderConfigurer父类PropertyResourceConfigurer的解密方法convertProperty就可以了:

package com.cardDemo.commonUtil;  
  
import org.springframework.beans.factory.config.PropertyPlaceholderConfigurer;  
  
public class ConvertPwdPropertyConfigurer extends PropertyPlaceholderConfigurer{  
    @Override  
    protected String convertProperty(String propertyName, String propertyValue) {  
        System.out.println("=================="+propertyName+":"+propertyValue);  
        if("userName".equals(propertyName)){  
            return "czw";  
        }  
        if("password".equals(propertyName)){  
            return "czw";  
        }  
        return propertyValue;  
    }  
}  
然后将上面完成的类替换配置文件中的PropertyPlaceholderConfigurer：

<bean class="com.cardDemo.commonUtil.ConvertPwdPropertyConfigurer"  
    p:location="classpath:jdbc.properties"  
    p:fileEncoding="utf-8"  
    />  
事实上,在我刚刚的Demo项目当中,里面的jdbc.properties里面的文件是如下内容的:

driverClassName=oracle.jdbc.driver.OracleDriver  
url=jdbc:oracle:thin:@127.0.0.1:1523:orcl  
userName=someOneElseUnknowUserName  
password=somePwdElseUnknowPassowrd  
而实际上,真实的密码却是czw/czw，web程序运行的时候，显示如下的内容：

<br/>2013-8-31 13:26:12 org.apache.catalina.core.StandardEngine start
<br/>
<br/>信息: Starting Servlet Engine: Apache Tomcat/6.0.18
<br/>2013-8-31 13:26:14 org.apache.catalina.core.ApplicationContext log
<br/>
<br/>信息: Initializing Spring root WebApplicationContext
<br/>==================url:jdbc:oracle:thin:@127.0.0.1:1523:orcl
<br/>
<br/>==================password:somePwdElseUnknowPassowrd
<br/>==================driverClassName:oracle.jdbc.driver.OracleDriver
<br/>
<br/>==================userName:someOneElseUnknowUserName
<br/>2013-8-31 13:26:17 org.apache.catalina.core.ApplicationContext log
<br/>
<br/>信息: Initializing Spring FrameworkServlet 'cardDemo'
<br/>2013-8-31 13:26:18 org.apache.coyote.http11.Http11Protocol start
<br/>
<br/>信息: Starting Coyote HTTP/1.1 on http-8080
<br/>2013-8-31 13:26:18 org.apache.jk.common.ChannelSocket init
<br/>
<br/>信息: JK: ajp13 listening on /0.0.0.0:8009
<br/>但是，在DATASOURCE里面获取到的内容，却是替换之后的正确的用户名和密码。这只是一个非常简单的例子，只是告诉我们一个解决数据库用户名和密码加密的一个渠道，比如，我们可以在PropertyPlaceholderConfigurer子类中写一些比较复杂的逻辑，比如根据jdbc.properties中配置的文件中进行各种手段的加密。在其中通过其他手段替换jdbc.propertes中的用户名和密码等等。