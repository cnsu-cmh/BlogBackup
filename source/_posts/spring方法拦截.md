---
title: spring 方法拦截
date: 2016-11-04 10:26:21
tags: [java,spring]
---
[文章来源:spring 方法拦截](http://blog.csdn.net/u011229848/article/details/53032369)


说道AOP不得不提到几个概念：

切面：也就是我们自己的一些业务方法。

通知：用于拦截时出发的操作。

切点：具体拦截的某个业务点。

这样说可能还是有点抽象，举个例子，下面是一个纸糊的多面体。

![](spring方法拦截/041909182489271.jpg)

每个面都是一个业务方法，我们通过刺穿每一个面，都可以进入到内部，这个面就是一个**切面**。

刺穿的时候会发出声响，这就是一种**通知**。

而具体从哪个面刺入，这就是一个**切入点**的选择了。

这样说，应该能稍微了解一点。

## 那么下面看一个简单的例子：

为了便于理清关系，先放上一张相关的类图：

![](spring方法拦截/041925307019848.png)

首先定义个**接口**
```java
public interface IService {
  public void withAop();
  public void withoutAop();
}
```

有了接口，当然需要一个**实现类**：
```java
public class TestAOP implements IService {
    private String name;
    public void withAop() { 
        System.out.println("with AOP name:"+name);  
    }
    public void withoutAop() {  
     System.out.println("without AOP name:"+name);
    }
    public String getName() {  
        return name; 
    }
    public void setName(String name) { 
        this.name = name;
    }
}
```

这个实现类实现了接口定义的两个方法，下面我们定义几种**拦截方式**，这些拦截方式通过拦截的位置或者时机不同而不同。

通常有方法前拦截，方法后拦截，以及异常拦截。通过在这些拦截中编写自己的业务处理，可以达到特定的需求。

方法前拦截，需要实现MethodBeforeAdvice接口，并填写before方法。这样，当拦截到某个方法时，就会在方法执行前执行这个before()方法。
```java
public class BeforeAOPInterceptor implements MethodBeforeAdvice{
    public void before(Method method, Object[] args, Object instance)
      throws Throwable {
     System.out.println("before()"+method.getName());
    }
}
```

同理，方法后拦截，也是如此。需要实现AfterReturningAdvice接口。
```java
public class AfterAOPInterceptor implements AfterReturningAdvice{
    public void afterReturning(Object value, Method method, Object[] args,
      Object instance) throws Throwable {
     System.out.println("after()"+method.getName());
    }
}
```
以及异常拦截。
```java
public class ThrowsAOPInterceptor implements ThrowsAdvice{ 
    public void afterThrowing(Method method,Object[] args,Object instance,AccountException ex) throws Throwable{
        System.out.println("after()"+method.getName()+"throws exception:"+ex);
    }
    public void afterThrowing(NullPointerException ex) throws Throwable{
        System.out.println("throws exception:"+ex);
    }
}
```

接下来就需要配置一下**spring的配置文件**，把拦截器与切面方法关联起来。

![](spring方法拦截/041917494369533.png)

参考上面的图，可以看到配置文件中的层次关系。
```xml
<?xml version="1.0" encoding="UTF-8"?> 
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    xmlns="http://www.springframework.org/schema/beans" 
    xsi:schemaLocation="http://www.springframework.org/schema/beans 
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd"> 
<!-- 通过名字匹配 --> 
<!-- 
　　<bean id="before" class="org.springframework.aop.support.NameMatchMethodPointcutAdvisor"> 
　　　　<property name="advice"> 
　　　　　　<bean class="com.test.pointcut.beforeAOP"></bean> 
　　　　</property> 
　　　　<property name="mappedName" value="withoutAop"></property> 
　　</bean> 
--> 
<!-- 通过正则表达式 匹配 --> 
　　<bean id="before" class="org.springframework.aop.support.RegexpMethodPointcutAdvisor"> 
　　　　<property name="advice"> 
　　　　　　<bean class="com.test.pointcut.BeforeAOPInterceptor"></bean> 
　　　　</property> 
　　<property name="patterns"> 
　　　　<list> 
　　　　　　<value>.*out.*</value> 
　　　　</list> 
　　</property> 
　　</bean> 
　　<bean id="after" class="org.springframework.aop.support.RegexpMethodPointcutAdvisor"> 
　　　　<property name="advice"> 
　　　　　　<bean class="com.test.pointcut.AfterAOPInterceptor"></bean> 
　　　　</property> 
　　　　<property name="patterns"> 
　　　　　　<list> 
　　　　　　　　<value>.*out.*</value> 
　　　　　　</list> 
　　　　</property> 
　　</bean> 
　　<bean id="exception" class="org.springframework.aop.support.RegexpMethodPointcutAdvisor"> 
　　　　<property name="advice"> 
　　　　　　<bean class="com.test.pointcut.ThrowsAOPInterceptor"></bean> 
　　　　</property> 
　　　　<property name="patterns"> 
　　　　　　<list> 
　　　　　　　　<value>.*out.*</value> 
　　　　　　</list> 
　　　　</property> 
　　</bean> 
<!-- --> 
　　<bean id="aopService" class="org.springframework.aop.framework.ProxyFactoryBean"> 
　　　　<property name="interceptorNames"> 
　　　　　　<list> 
　　　　　　　　<value>before</value> 
　　　　　　　　<value>after</value> 
　　　　　　　　<value>exception</value> 
　　　　　　</list> 
　　　　</property> 
　　　　<property name="target"> 
　　　　　　<bean class="com.test.pointcut.TestAOP"> 
　　　　　　　　<property name="name" value="Hello"></property> 
　　　　　　</bean> 
　　　　</property> 
　　</bean> 
</beans>
```

ProxyFactoryBean下有两个属性，一个想要拦截的目标类，一个是拦截器。而拦截器又包括两种，主要是因为定位方法的不同而分类。分别是：

RegexpMethodPointcutAdvisor 通过正则表达式来定位业务方法。

NameMatchMethodPointcutAdvisor 通过名字来定位业务方法。

定位到了业务方法，还需要添加响应的拦截器，拦截器就是上面的三种。

最后看一下测试的方法：
```java
public class TestMain {
 public static void main(String[] args) {
  XmlBeanFactory factory = new XmlBeanFactory(new ClassPathResource("applicationContextAOP.xml"));
  IService hello = (IService)factory.getBean("aopService");
  hello.withAop();
  hello.withoutAop();
 }
}
```

我们上面通过正则表达式定位到所有包含out的方法，其实就是withoutAOP方法。这样当执行withoutAop方法时，会触发拦截器的操作。

执行结果：
```sybase
2014-12-4 16:46:58 org.springframework.beans.factory.xml.XmlBeanDefinitionReader loadBeanDefinitions
信息: Loading XML bean definitions from class path resource [applicationContextAOP.xml]
with AOP name:Hello
before()withoutAop
without AOP name:Hello
after()withoutAop
```

## 总结：

这是通过定义切入点的方式来实现AOP，通过这种编程方式，可以针对业务方法进行包装或者监控。

举个例子，比如有个业务方法想要进行数据的查询，那么可以再这个查询前面获取JDBC连接池的连接，这样就对用户屏蔽掉了复杂的申请过程。而销毁就可以放在方法后拦截函数里。

再比如，想要监控某个业务方法呗执行了多少次，那么就可以通过这样一种拦截方式，进行信息的统计，计数或者计时！

妙处多多，还待完善！

参考：《java web王者归来》《spring实战》《spring权威指南》

相关类似文章 [Spring AOP原理及简单应用](http://blog.csdn.net/u011229848/article/details/53032331) [http://blog.csdn.net/u011229848/article/details/53032331](http://blog.csdn.net/u011229848/article/details/53032331)