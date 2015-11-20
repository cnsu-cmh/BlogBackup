---
title: Spring AOP原理及简单应用
date: 2016-11-04 10:22:30
tags: [java,spring]
---
[文章来源:Spring AOP原理及简单应用](http://blog.csdn.net/u011229848/article/details/53032331)


相信只要使用过[spring](http://lib.csdn.net/base/javaee "Java EE知识库")框架的，大家对于AOP都不陌生，尤其提起它就能立刻随口说出，一般用在日志处理、异常处理、权限验证等方面。但刚开始接触难免会有各种各样的疑惑，今天抽时间，按照之前的理解整理了一份关于Spring AOP的简单教程，希望能够帮助大家尽快的了解它的实现过程及原理。首先来明确几个概念：

* **JointPoint**

系统在运行之前，AOP的功能模块需要织入到OOP的功能模块中。要进行这种织入过程，需要知道在系统的哪些功能点上进行织入操作，这些将要在其上进行织入操作的系统功能点就称为JointPoint。如某方法调用的时候或者处理异常的时候，在Spring AOP中，一个连接点总是表示一个方法的执行。常见的几种类型的JoinPoint:

Ø 方法调用：当某个方法被调用的时候所处的程序执行点；

Ø 方法执行：该类型表示的是某个方法内部执行开始时的点，应该与方法调用相区分；

Ø 构造方法调用：程序执行过程中对某个对象调用其构造方法进行初始化时的点；

Ø 构造方法执行：它与构造方法调用关系如同方法调用与方法执行间的关系；

Ø 字段设置：对象的某个属性通过setter方法被设置或直接被设置的执行点；

Ø 字段获取：某个对象相应属性被访问的执行点；

Ø 异常处理执行：某些类型异常抛出后，对应的异常处理逻辑执行点；

Ø 类初始化：类中某些静态类型或静态块的初始化时的执行点。

* **Pointcut**

Pointcut代表的是JoinPoint的表述方式。在将横切逻辑织入当前系统的过程中，虽然知道需要在哪些功能点上织入AOP的功能模块，但需要一种表达方法。Pointcut和一个切入点表达式关联，并在满足这个切入点的Joinpoint上运行。目前通常使用的Pointcut方式有以下几种：

Ø 直接指定Joinpoint所在的方法名称；

Ø 正则表达式，Spring的AOP支持该种方式；

Ø 使用特定的Pointcut表述语言，Spring 2.0后支持该方式。

* **Advice**

Advice是单一横切关注点逻辑的载体，它代表将会织入到JoinPoint的横切逻辑。在切面的某个特定的连接点上执行的逻辑。根据它在Joinpoint位置执行时机的差异或完成功能的不同，可分为以下几种形式：

Ø Before Advice：在Joinpoint指定位置之前执行的Advice类型，可以采用它来做一些系统的初始化工作，如设置系统初始值，获取必要系统资源等。

Ø After Advice：在相应连接点之后执行的Advice类型，它还可以细分为以下三种：

² After Returning Advice：只有当前Joinpoint处执行流程正常完成后，它才会执行；

² After throwing Advice：在当前Joinpoint执行过程中抛出异常的情况下会执行；

² After Advice：该类型的Advice不管JoinPoint处执行流程是正常还是抛出异常都会执行。

Ø Around Advice：对附加其上的Joinpoint进行包裹，可以在joinpoint之前和之后都指定相应的逻辑，甚至中断或忽略joinpoint处原来程序流程的执行。

* **Aspect**

它是对系统中横切关注点逻辑进行模块化封装的AOP概念实体，它可以包含多个Pointcut以及相关的Advice定义。

* **织入器**

经过织入过程后，以Aspect模块化的横切关注点才会集成到oop的现存系统中，而完成织入过程实体称为织入器。Spring中使用一组类来完成最终的织入操作，ProxyFactory类是Spring AOP最通用的织入器。

* **目标对象**

符合Pointcut所指定的条件，将在织入过程中被织入横切逻辑的对象，称之为目标对象。

单看上述的概念，可能会觉得有点眼花缭乱，其实通过一个简单的AOP的实例即可以帮助我们很快的了解其内部的机制。其实对于方法拦截有不同的实现方式，常用的即有直接采用Spring提供的各种Advice进行拦截，另一种则是采用MethodInterceptor方式进行拦截。

**Spring提供的Advice拦截方式**

定义一个逻辑接口IBusinessLogic：

```java
package com.wow.asc.aop;  
  
public interface IBusinessLogic {  
  
    public void foo();  
  
    public void bar() throws BusinessLogicException;  
      
    public long time();  
```
 其中有一个BusinessLogicException异常，它用于后面对于ThrowsAdvice进行检验的实例，在此定义为：
```java
package com.wow.asc.aop;  
  
public class BusinessLogicException extends Exception {  
  
```
 对于该业务逻辑的实现BusinessLogic，如下所示：
```java
package com.wow.asc.aop;  
  
public class BusinessLogic implements IBusinessLogic {  
    @Override  
    public void foo() {  
        System.out.println("Inside BusinessLogic.foo()");  
    }  
    @Override  
    public void bar() throws BusinessLogicException {  
        System.out.println("Inside BusinessLogic.bar()");  
        throw new BusinessLogicException();  
    }  
     /*  
     * 返回该方法执行的时间 
     */  
    @Override  
    public long time() {  
        System.out.println("Inside BusinessLogic.time()");  
        long startTime = System.currentTimeMillis();  
        for(int i = 0; i < 100000000; i++);  
        long endTime = System.currentTimeMillis();  
          
        return (endTime - startTime);  
    }  
}
```
 在完成上述业务逻辑编码后，接下来将进行更多的横切插入点的设计，如在方法执行前或返回时、抛出异常时进行各种处理。对于Advice的写法如下所示：
```java
package com.wow.asc.aop;  
  
import java.lang.reflect.Method;  
import org.springframework.aop.MethodBeforeAdvice;  
/* 
* 表示一个在方法执行前进行拦截的一个Advice 
 */  
public class TracingBeforeAdvice implements MethodBeforeAdvice {  
    @Override  
    public void before(Method method, Object[] args, Object target) throws Throwable {  
        System.out.println("execute before (by " + method.getDeclaringClass().getName() + "." + method.getName() + ")");  
    }  
}  
  
package com.wow.asc.aop;  
  
import java.lang.reflect.Method;  
import org.springframework.aop.AfterReturningAdvice;  
/* 
 * 表示一个在方法返回时进行拦截的Advice 
*/  
public class TracingAfterAdvice implements AfterReturningAdvice {  
    @Override  
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {  
        System.out.println(method.getDeclaringClass().getName() + "." + method.getName() + "spend time: " + returnValue);  
        System.out.println("execute after (by " + method.getDeclaringClass().getName() + "." + method.getName() + ")");  
    }  
}  
  
package com.wow.asc.aop;  
  
import java.lang.reflect.Method;  
import org.springframework.aop.ThrowsAdvice;  
/* 
 * 表示一个异常抛出时进行拦截的Advice 
*/  
public class TracingThrowsAdvice implements ThrowsAdvice {  
      
    public void afterThrowing(Method method, Object[] args, Object target, Throwable subclass) {  
         System.out.println( "Logging that a " + subclass + "Exception was thrown.");  
      }  
}  
```
 在设计完上述的代码及逻辑后，即可以通过applicationContext.xml将上述类进行组合，在装配过程中即可明确哪个类的哪些方法需要被拦截，及拦截前、后会做哪些事情。具体的配置示例：
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"  
    xmlns:tx="http://www.springframework.org/schema/tx" xmlns:context="http://www.springframework.org/schema/context"  
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.5.xsd  
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.5.xsd  
        http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-2.5.xsd  
        http://www.springframework.org/schema/context http://localhost:8080/schema/www.springframework.org/schema/context/spring-context-2.5.xsd">  
    <bean id="businessLogic" class="com.wow.asc.aop.BusinessLogic" />  
    <bean id="businessLogicBean" class="org.springframework.aop.framework.ProxyFactoryBean">  
        <property name="proxyInterfaces">  
            <value>com.wow.asc.aop.IBusinessLogic</value>  
        </property>  
        <property name="target">  
            <ref local="businessLogic"/>   
        </property>  
        <property name="interceptorNames">  
            <list>  
                <value>theTracingBeforeAdvisor</value>  
                    <value>theTracingAfterAdvisor</value>  
                    <value>theTracingThrowsAdvisor</value>  
            </list>  
        </property>  
    </bean>  
      
    <bean id="theTracingBeforeAdvisor" class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">  
        <property name="advice">  
            <ref local="theTracingBeforeAdvice"/>  
        </property>  
        <property name="pattern">  
            <value>.*</value>  
        </property>  
    </bean>  
    <bean id="theTracingAfterAdvisor" class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">  
        <property name="advice">  
            <ref local="theTracingAfterAdvice"/>  
        </property>  
        <property name="pattern">  
            <value>.*time.*</value>  
        </property>  
    </bean>  
    <bean id="theTracingThrowsAdvisor" class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">  
        <property name="advice">  
            <ref local="theTracingThrowsAdvice"/>  
        </property>  
        <property name="pattern">  
            <value>.*bar.*</value>  
        </property>  
    </bean>  
    <bean id="theTracingBeforeAdvice" class="com.wow.asc.aop.TracingBeforeAdvice"/>  
    <bean id="theTracingAfterAdvice" class="com.wow.asc.aop.TracingAfterAdvice"/>  
    <bean id="theTracingThrowsAdvice" class="com.wow.asc.aop.TracingThrowsAdvice"/>  
</beans>  
```
通过上述的配置，我们可以看出我们将IBusinessLogic做为代理接口，同时它的真正的目标类是BusinesssLogic。同时会对所有进入方法之前采用TracingBeforeAdvice进行拦截，进行方法前的预处理；对time方法采用TracingAfterAdvice进行拦截，进行方法返回后的处理；对于bar则采用TracingThrowsAdvice进行拦截，当方法返回BusinessLogicException时进行相应的处理。
在配置完上述类的依赖关系及需要拦截的方法后，即可以编写客户端程序来调用，查看它的运行机制。客户端调用代码：
```java
package com.wow.asc.test;  
  
import org.springframework.context.ApplicationContext;  
import org.springframework.context.support.ClassPathXmlApplicationContext;  
import com.wow.asc.aop.BusinessLogicException;  
import com.wow.asc.aop.IBusinessLogic;  
  
public class AOPTest {  
    public static void main(String[] args) {  
        ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");  
        IBusinessLogic ibl = (IBusinessLogic)ac.getBean("businessLogicBean");  
        ibl.foo();  
        try {  
            ibl.bar();  
        } catch (BusinessLogicException e) {  
            System.out.println("Caught BusinessLogicException");  
        }  
        ibl.time();  
    }  
}  
```
 通过运行结果来详细的了解下，看是否真正的如上所述，会在方法前、后及异常抛出时能够拦截并进行相应处理。结果如下：
```sybase
1、execute before (by com.wow.asc.aop.IBusinessLogic.foo)  
2、Inside BusinessLogic.foo()  
3、execute before (by com.wow.asc.aop.IBusinessLogic.bar)  
4、Inside BusinessLogic.bar()  
5、Logging that a com.wow.asc.aop.BusinessLogicExceptionException was thrown.  
6、Caught BusinessLogicException  
7、execute before (by com.wow.asc.aop.IBusinessLogic.time)  
8、Inside BusinessLogic.time()  
9、com.wow.asc.aop.IBusinessLogic.timespend time: 46  
10、execute after (by com.wow.asc.aop.IBusinessLogic.time)  
```

其实通过1、3、7行可以非常清晰的了解到，每个方法在执行前都被TracingBeforeAdvice拦截到，并执行了预处理。5、6行表示当调用的是bar方法时，会被TracingThrowsAdvice拦截，当有异常抛出时，会执行相应的处理；8、9、10行则表示当调用的是time方法，返回时会被TracingAfterAdvice拦截，对其返回值进行处理。

**MethodInterceptor拦截方式**

采用该种方式进行拦截，需要实现一个继承自MethodInterceptor的类，并将该类注册至spring Context中，具体如下：

```java
package com.wow.asc.aop;  
  
import org.aopalliance.intercept.MethodInterceptor;  
import org.aopalliance.intercept.MethodInvocation;  
  
public class MyInterceptor implements MethodInterceptor {  
    @Override  
    public Object invoke(MethodInvocation invocation) {  
        Object result = null;  
        StringBuffer info = new StringBuffer();  
        info.append("intercept the method: ");  
        info.append(invocation.getMethod().getDeclaringClass().  
getName());  
        info.append(".");  
        info.append(invocation.getMethod().getName());  
        System.out.println("start " + info.toString());  
        try {  
           result = invocation.proceed();  
        } catch (Throwable e) {  
            e.printStackTrace();  
        } finally {  
            System.out.println("end " + info.toString());  
        }  
        return result;  
    }  
}  
``` 
对于类的装配，其实和上面的非常类似，示例：
```xml
<bean id="testBean" class="org.springframework.aop.framework.ProxyFactoryBean">  
        <property name="proxyInterfaces">  
            <value>com.wow.asc.aop.IBusinessLogic</value>  
        </property>  
        <property name="target">  
            <ref local="businessLogic"/>   
        </property>  
        <property name="interceptorNames">  
            <list>  
                <value>myInterceptor</value>  
            </list>  
        </property>  
</bean>  
<bean id="myInterceptor" class="com.wow.asc.aop.MyInterceptor"/>  
```
 再通过客户端进行调用，可得到运行结果，从结果来分析可以看出它在方法执行的前、后均添加了相应的日志。

```sybase
start intercept the method: com.wow.asc.aop.IBusinessLogic.foo  
Inside BusinessLogic.foo()  
end intercept the method: com.wow.asc.aop.IBusinessLogic.foo  
start intercept the method: com.wow.asc.aop.IBusinessLogic.time  
Inside BusinessLogic.time()  
end intercept the method: com.wow.asc.aop.IBusinessLogic.time  
```

至此，采用两种不同方式实现的AOP就结束了，希望大家能够体会到其中的奥妙。