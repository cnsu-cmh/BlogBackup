---
title: IOC和spring基本配置
date: 2016-10-28 00:10:53
tags: [java,spring]
---
[文章来源:IOC和spring基本配置](http://blog.csdn.net//u011229848//article/details/52951439)


对于IoC的一些知识点，相信大家都知道他在[spring](http://lib.csdn.net/base/javaee "Java EE知识库")框架中所占有的地位，应该可以算的上是核心之一吧，所以IOC是否理解清楚，决定了大家对Spring整个框架的理解
<br/><font color="red">Ioc的理解</font>
<br/>spring的两个核心概念：<font color="red">一个是控制反转IoC，也可以叫做依赖注入DI。还有一个是面向切面编程AOP。</font>
<br/><font color="red">控制反转：</font>当某个[Java](http://lib.csdn.net/base/javaee "Java EE知识库") 对象需要（依赖）另一个java 对象时，不是自身直接创建依赖对象，而是由实现IoC的容器（如spring框架的IoC容器）来创建，并将它注入需要这个依赖对象的java对象中。
<br/><font color="red">spring的容器</font>
</br/>spring管理的基本单元是Bean，在spring的应用中，所有的组件都是一个个的Bean，它可以是任何的java对象。spring负责创建这些Bean的实例。并管理生命周期。而spring框架是通过其内置的容器来完成Bean的管理的，Bean在spring的容器中生存着，使用时只需要通过它提供的一些方法从其中获取即可。spring的容器有两个接口：BeanFactory和ApplicationContext这两个接口的实例被陈为spring的上下文。
```java
ApplicationContext ac = new ClassFathXmlApplicationContext("app/*.xml");
AccountService accountService =(AccountService)ac.getBean("accountServiceImpl");
```

<font color="red">注：由于ApplicationContext是基于BeanFactory之上的，所以，一般ApplicationContext功能比较强大，建议使用
</font>

ApplicationContext经常用到的三个实现：

1.ClassPathXmlApplicationContext：从类路径中的XML文件载入上下文定义信息。把上下文定义文件当成类路径资源。

2.FileSystemXmlApplicationContext：从文件系统中的XML文件载入上下文定义信息。

3.XmlWebApplicationContext：从Web系统中的XML文件载入上下文定义信息。

**<font color="red">一：spring的依赖注入</font>**

**<font color="#0099ff">1）、构造器注入</font>**

****
<bean id="accoutDaoImpl" class="cn.csdn.dao.AccoutDaoImpl" scope=”singleton”/> <bean id="accoutServicImpl" class="cn.csdn.service.AccoutServicImpl" scope=”"> <span style="white-space:pre"> </span><!-- 构造方法注入方式--> <span style="white-space:pre"> </span><constructor-arg ref="accoutDaoImpl"/> </bean>

****注**：这种注入方式很少用，如果是注入对象一般为上例注入，但有时要注入基本数据类型，一般用下面方法注入**

<span style="background-color: rgb(255, 255, 255);"><strong><constructor-arg> <span style="white-space:pre"> </span><value>hello world!</value> </constructor-arg></strong></span>
如果构造方法不只一个参数时，应指明所注入参数的索引或者数据类型，例如：

<strong><constructor-arg index="0" type="java.lang.String"> <span style="white-space:pre"> </span><value>sunDriver</value> </constructor-arg> <constructor-arg index="1" type="java.lang.String"> <span style="white-space:pre"> </span><value>jdbc:odbc:School</value> </constructor-arg></strong>

**<font color="#0099ff">2）、设值（set方法）注入</font>**
<strong><span style="font-family: Arial; font-size: 18px; line-height: 26px;"></span></strong><pre name="code" class="html"><bean id="accountDaoImpl" class="cn.csdn.dao.AccoutDaoImpl"/> <bean id="accoutServicImpl" class="cn.csdn.service.AccoutServicImpl"> <span style="white-space:pre"> </span><!-- 设值（set 方法）注入--> <span style="white-space:pre"> </span><property name="accountDaoImpl" ref="accoutDaoImpl"/> </bean>

注：<property name="accountDaoImpl" ref="accoutDaoImpl"/>

相当于调用setAccountDaoImpl方法，把值设为accoutDaoImpl

3）<font color="#0099ff">接口注入（很少用）</font>

<font color="red">二：xml装配Bean属性含义</font>

1.id:指定该Bean 的唯一标识。

2.class:指定该Bean 的全限定名。

3.name:为该Bean 指定一到多个别名。多个别名可以用“，”和“；”分割。

4.<font color="#0099ff">autowire:指定该Bean的属性的装配方式。</font>

所谓自动装配是指在<BEAN>标签中不用指定其依赖的BEAN，而是通过配置的自动装配来自动注入依赖的BEAN,这种做法让我们的配置更加简单

1）<font color="#0099ff">no</font>:不使用自动装配。必须通过ref 元素指定依赖，这是默认设置。由于显式指定协作者可以使配置更灵活、更清晰，因此对于较大的部署配置，推荐采用该设置。而且在某种程度上，它也是系统[架构](http://lib.csdn.net/base/architecture "大型网站架构知识库")的一种文档形式。
<strong><bean id="bean1" class="cn.csdn.service.Bean1" scope="singleton"> <property name="studentDaoImpl" ref="studentDaoImpl"> </property> </bean></strong>

备注：有property 属性指定ref

2）<font color="#0099ff">byName</font>:根据属性名自动装配。此选项将检查容器并根据

名字查找与属性完全一致的bean，并将其与属性自动装配。例如，在

bean 定义中将autowire 设置为by name，而该bean 包含master 属性（同时提供setMaster(..)方法），Spring 就会查找名为master 的bean 定义，并用它来装配给master 属性。

<bean id="bean1" class="cn.csdn.service.Bean1"

scope="singleton" autowire="byName"/>

备注：没有property 属性

3）<font color="#0099ff">byType</font>:如果容器中存在一个与指定属性类型相同的

bean，那么将与该属性自动装配。如果存在多个该类型的bean，那么

将会抛出异常，并指出不能使用byType 方式进行自动装配。若没有找到相匹配的bean，则什么事都不发生，属性也不会被设置。如果你不希望这样，那么可以通过设置dependency-check="objects"让Spring 抛出异常。

备注：spring3.0 以上不抛异常。

<bean id="bean1" class="cn.csdn.service.Bean1"

scope="singleton" autowire="byType"/>

备注：没有property 属性

4）<font color="#0099ff">Constructor</font>:与byType 的方式类似，不同之处在于它应用

于构造器参数。如果在容器中没有找到与构造器参数类型一致的bean，那么将会抛出异常。

<bean id="bean1" class="cn.csdn.service.Bean1"

scope="singleton"autowire="constructor"/>

备注：没有property 属性

5）<font color="#0099ff">autodetect</font>:通过bean 类的自省机制（introspection）来决定是使用constructor 还是byType 方式进行自动装配。如果发现默认的

构造器，那么将使用byType 方式。

<bean id="bean1" class="cn.csdn.service.Bean1"

scope="singleton" autowire="autodetect"/>

5.<font color="#0099ff">scope</font>:指定该Bean的生存范围

scope用来声明IOC容器中的对象应该处的限定场景或者说该对象的存活空间，即在IOC容器在对象进入相应的scope之前，生成并装配这些对象，在该对象不再处于这些scope的限定之后，容器通常会销毁这些对象。

1）singleton类型的bean定义，在一个容器中只存在一个实例，所有对该类型bean的依赖都引用这一单一实例

2）scope为prototype的bean，容器在接受到该类型的对象的请求的时候，会每次都重新生成一 个新的对象给请求方，虽然这种类型的对象的实例化以及属性设置等工作都是由容器负责的，但是只要准备完毕，并且对象实例返回给请求方之后，容器就不在拥有 当前对象的引用，请求方需要自己负责当前对象后继生命周期的管理工作，包括该对象的销毁

3）<font color="#0099ff">request ，session和global session</font>

这三个类型是spring2.0之后新增的，他们只适用于web程序，通常是和XmlWebApplicationContext共同使用

<font color="#0099ff">request：</font>

<bean id ="requestPrecessor" class="...RequestPrecessor" scope="request" />

Spring容器，即XmlWebApplicationContext会为每个HTTP请求创建一个全新的RequestPrecessor对象，当请求结束后，，该对象的生命周期即告结束

<font color="#0099ff">session</font>

<bean id ="userPreferences" class="...UserPreferences" scope="session" />

Spring容器会为每个独立的session创建属于自己的全新的UserPreferences实例，他比request scope的bean会存活更长的时间，其他的方面真是没什么区别。

<font color="#0099ff">global session：</font>

<bean id ="userPreferences" class="...UserPreferences" scope="globalsession" />

global session只有应用在基于porlet的web应用程序中才有意义，他映射到porlet的global范围的session，如果普通的servlet的web 应用中使用了这个scope，容器会把它作为普通的session的scope对待。
<font color="#0099ff">
6.init-method:指定该Bean的初始化方法。destroy-method:指定该Bean的销毁方法。这个就像servlet中init和destroy方法一样，只不过这里在配置文件配置的
</font>
7.abstract:指定该Bean 是否为抽象的。如果是抽象的，则

spring 不为它创建实例。

8.parent

如果两个Bean 的属性装配信息很相似，那么可以利用继

承来减少重复的配置工作。

<!-- 装配Bean 的继承

父类作为模板，不需要实例化，设置abstract=”true”-->

` <bean id=”parent” class=”cn.csdn.service.Parent”

abstract=”true”>

<property name=”name” value=”z_xiaofei168”/>

<property name=”pass” value=”z_xiaofei168”/>

</bean>

<!-- 装配Bean 的继承

子类中用parent 属性指定父类标识或别名

子类可以覆盖父类的属性装配，也可以新增自己的属性装配

-->

` <bean id=”child” class=”cn.csdn.service.Chlid”

parent=”parent”>

<property name=”pass” value=”123123”/>

<property name=”age” value=”22”/>

</bean>

<font color="red">三：装配Bean的各种类型属性值</font>

1..简单类型属性值的装配
```xml
<bean id="bean1" class="cn.csdn.domain.Bean1">
    <property name="name" value="z_xiaofei168"/>
    <property name="age">
        <value>22</value>
    </property>
</bean>

```

2 . 引用其他 Bean 的装配
```xml
    <bean id="bean1" class="cn.csdn.domain.Bean1"> ... </bean>
    <bean id="bean2" class="cn.csdn.domain.Bean2"> <!-- 引用自其他Bean 的装配-->
        <property name="bean1" ref="bean1"/> 
    </bean>
```
另外一种不常使用的配置方式是在property元素中嵌入

一个bean 元素来指定所引用的Bean.
```xml
    <bean id="bean2" class="cn.csdn.domain.Bean2"><!-- 引用自其他Bean 的装配--> 
        <property name="bean1">
            <bean id="bean1" class="cn.csdn.domain.Bean1"/> 
        </property> 
    </bean>
```
3.集合的装配

其实集合的装配并不是复杂，反而感觉到很简单，用一个例子来说明问题吧：
```java
package com.bebig.dao.impl;  
import java.util.List;  
import java.util.Map;  
import java.util.Properties;  
import java.util.Set;  
import com.bebig.dao.UserDAO;  
import com.bebig.model.User;  
public class UserDAOImpl implements UserDAO {  
    private Set<String> sets;  
    private List<String> lists;  
    private Map<String, String> maps;  
    private Properties props;  
    public Set<String> getSets() {  
        return sets;  
    }  
    public void setSets(Set<String> sets) {  
        this.sets = sets; }  
    public List<String> getLists() {  
        return lists; }  
    public void setLists(List<String> lists) {  
        this.lists = lists;  }  
    public Map<String, String> getMaps() {  
        return maps;  
    }  
    public void setMaps(Map<String, String> maps) {  
        this.maps = maps;  
    }  
    public Properties getProps() {  
        return props;  
    }  
    public void setProps(Properties props) {  
        this.props = props;  
    }  
    public void save(User u) {  
        System.out.println("a user saved!");  
    }  
    @Override  
    public String toString() {  
        return "sets.size:" + sets.size() + " lists.size:" + lists.size()  
                + " maps.size:" + maps.size() + " props.size:" + props.size();  
    }  
}
```
**配置如下：
**

****
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"  
     >  
    <!-- a service object; we will be profiling its methods -->  
    <bean name="u" class="com.bebig.dao.impl.UserDAOImpl">  
        <!-- set -->  
        <property name="sets">  
            <set>  
                <value>1</value>  
                <value>2</value>  
            </set>  
        </property>  
        <!-- list -->  
        <property name="lists">  
            <list>  
                <value>a</value>  
                <value>b</value>  
            </list>  
        </property>  
        <!-- map -->  
        <property name="maps">  
            <map>  
                <entry key="1" value="aa"></entry>  
                <entry key="2" value="bb"></entry>  
            </map>  
        </property>  
        <!-- properties -->  
        <property name="props">  
            <props>  
                <prop key="a">haha</prop>  
                <prop key="b">hi</prop>  
            </props>  
        </property>  
    </bean>  
    <bean id="userService" class="com.bebig.service.UserService"  
        scope="prototype">  
        <constructor-arg>  
            <ref bean="u" />  
        </constructor-arg>  
    </bean>  
    <!-- this switches on the load-time weaving -->  
    <!-- <context:load-time-weaver /> -->  
</beans>
```

<font color="red">四：Spring bean生命周期</font>

在传统的Java应用中，Bean的生命周期非常简单。Java的关键词new用来实例化Bean（或许他是非序列化的）。这样就够用了。 相反，Bean的生命周期在Spring容器中更加细致。 理解Spring Bean的生命周期非常重要，因为你或许要利用Spring提供的机会来订制Bean的创建过程。

<font color="red">bean生命周期</font>

1.容器寻找Bean的定义信息并且将其实例化。

2.受用依赖注入，Spring按照Bean定义信息配置Bean的所有属性。

3.如果Bean实现了BeanNameAware接口，工厂调用Bean的setBeanName()方法传递Bean的ID。

4.如果Bean实现了BeanFactoryAware接口，工厂调用setBeanFactory()方法传入工厂自身。

5.如果BeanPostProcessor和Bean关联，那么它们的postProcessBeforeInitialzation()方法将被调用。

6.如果Bean指定了init-method方法，它将被调用。

7.最后，如果有BeanPotProcessor和Bean关联，那么它们的postProcessAfterInitialization()方法将被调用。

到这个时候，Bean已经可以被应用系统使用了，并且将被保留在Bean Factory中知道它不再需要。

有两种方法可以把它从Bean Factory中删除掉。

1.如果Bean实现了DisposableBean接口，destory()方法被调用。

2.如果指定了订制的销毁方法，就调用这个方法。

Bean在Spring应用上下文的生命周期与在Bean工厂中的生命周期只有一点不同， 唯一不同的是，如果Bean实现了ApplicationContextAwre接口，setApplicationContext()方法被调用。

只有singleton行为的bean接受容器管理生命周期。

non-singleton行为的bean，Spring容器仅仅是new的替代，容器只负责创建。

对于singleton bean，Spring容器知道bean何时实例化结束，何时销毁，Spring可以管理实例化结束之后，和销毁之前的行为，管理bean的生命周期行为主要未如下两个时机：

Bean全部依赖注入之后

Bean即将销毁之前

1）依赖关系注入后的行为实现：

有两种方法：A.编写init方法B.实现InitializingBean接口

afterPropertiesSet和init同时出现，前者先于后者执行，使用init方法，需要对配置文件加入init-method属性

2）bean销毁之前的行为

有两种方法：A.编写close方法B.实现DisposableBean接口

destroy和close同时出现，前者先于后者执行，使用close方法，需要对配置文件加入destroy-method属性

总体上分只为四个阶段

1. BeanFactoyPostProcessor实例化

2. Bean实例化，然后通过某些BeanFactoyPostProcessor来进行依赖注入

3. BeanPostProcessor的调用.Spring内置的BeanPostProcessor负责调用Bean实现的接口: BeanNameAware, BeanFactoryAware, ApplicationContextAware等等，等这些内置的BeanPostProcessor调用完后才会调用自己配置的BeanPostProcessor

4.Bean销毁阶段

<font color = "red" >注：xml依赖注入中的bean.xml例子：</font>
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans  
           http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">  
  <bean id="u" class="com.bjsxt.dao.impl.UserDAOImpl"/>
  <bean id="userService" class="com.bjsxt.service.UserService">  
    <!--   <property name="userDAO" ref="u" />  -->  
     <constructor-arg>  
        <ref bean="u"/>  
     </constructor-arg>  
  </bean>  
</beans>
```