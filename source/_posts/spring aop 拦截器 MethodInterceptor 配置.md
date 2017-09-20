---
title: spring aop 拦截器 MethodInterceptor 配置
date: 2016-11-04 10:56:17
tags: [java,spring]
---
[文章来源:spring aop 拦截器 MethodInterceptor 配置](http://blog.csdn.net/u011229848/article/details/53032420)


版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/u011229848/article/details/53032420

在此之前呢，这篇文章是基于**[Spring方法拦截](http://blog.csdn.net/u011229848/article/details/53032369) [http://blog.csdn.net/u011229848/article/details/53032369](http://blog.csdn.net/u011229848/article/details/53032369) 、**[**Spring AOP原理及简单应用**](http://blog.csdn.net/u011229848/article/details/53032331) 后写的另一种配置方式方法拦截，好了，进入正题：

首先看一下配置文件中aop的配置，其中<aop:pointcut id="serviceMethodPointcut" expression="execution(/* com.xqx.fcch.service../*(..))"/>是其切入点，<aop:advisor advice-ref="serviceMethodInterceptor" pointcut-ref="serviceMethodPointcut" />是在该切入点使用自定义拦截器。

```xml

<bean id="serviceMethodInterceptor" class="com.xqx.fcch.exception.ServiceMethodInterceptor"></bean>

   	<!-- 方法拦截器（拦截Service包中的所有的方法） MethodInterceptor -->
  	<aop:config>
       	<aop:pointcut id="serviceMethodPointcut" expression="execution(* com.xqx.fcch.service..*(..))"/>
		<aop:advisor advice-ref="serviceMethodInterceptor" pointcut-ref="serviceMethodPointcut" />
	</aop:config>

```
<!--more-->
这里需要注意的是，上面的配置文件是对service进行拦截，很多人配置文件拦截请求返回异常，只是对controller方法拦截，这里面我单独用一个aop:config

```xml

   	<!-- 方法拦截器（拦截controller包中的所有带有@RequestMapping注解的方法） MethodInterceptor -->
	<aop:config>
		<aop:pointcut id="controllerMethodPointcut" expression="execution(* com.xqx.fcch.controller..*(..)) and
        	@annotation(org.springframework.web.bind.annotation.RequestMapping)"/>
		<aop:advisor advice-ref="controllerMethodInterceptor" pointcut-ref="controllerMethodPointcut" />
	</aop:config>

```

注意：一般项目SpringMVC的配置文件扫描Controller，spring的配置扫描Serice，Dao一类的，配置文件单独分开了，所以上面两个不同包的拦截应该 放在不同配置文件中， Controller应该陪在servler的配置文件，也就是MVC下，Service不能放MVC视图，控制的配置文件，应该放在spring的applicationContext中。

下面是拦截器实现java代码：

```java
package com.xqx.fcch.exception;

import org.aopalliance.intercept.MethodInterceptor;
import org.aopalliance.intercept.MethodInvocation;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class ServiceMethodInterceptor implements MethodInterceptor{
	final Logger log = LoggerFactory.getLogger(ServiceMethodInterceptor.class);
	
	@Override
	public Object invoke(MethodInvocation invocation) throws Throwable {
		Object result = null;  
        StringBuffer info = new StringBuffer();  
        info.append("intercept the method: ");  
        info.append(invocation.getMethod().getDeclaringClass(). getName());  
        info.append(".");  
        info.append(invocation.getMethod().getName());  
        try {  
           result = invocation.proceed();  
        } catch (Exception e) {
        	log.error(info.toString()+"\n"+e.getMessage(),e);
        	throw e;
        }
        return result;  
    }


}
```

应为我的controller已经有spring全局异常捕获的实现，所以我这里只是对service的拦截记录日志。

以上观点只是个人见解，如有不对的地方还希望大神们多多指教，也希望对读者有所帮助，谢谢。