---
title: spring基本配置详解(属性bean的基本配置)
date: 2016-10-29 18:23:50
tags: [java,spring]
---
[文章来源:spring基本配置详解(属性bean的基本配置)](http://blog.csdn.net/u011229848/article/details/52965645)


版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/u011229848/article/details/52965645

其实一些配置在 《ioc与spring基本配置》中已经有了 ，但是有一些不够细致，单独用一篇文章来介绍一下。

(1).**属性注入**即通过 setter 方法注入Bean 的属性值或依赖的对象

属性注入使用 <property> 元素, 使用 name 属性指定 Bean 的属性名称，value 属性或 <value> 子节点指定属性值
**属性注入是实际应用中最常用的注入方式**
```xml
<!-- 配置一个 bean -->
    <!--通过全类名配置bean -->
	<bean id="user" class="com.xiaoshu.spring.test.User">
		<!-- 为属性赋值 -->
		<property name="username" value="Jerry"></property>
	</bean>
	
	<!-- 配置一个 bean -->
	<bean id="user2" class="com.xiaoshu.spring.test.User">
		<!-- 为属性赋值 -->
		<!-- 通过属性注入: 通过 setter 方法注入属性值 -->
		<property name="username" value="Tom"></property>
	</bean>
```

(2) 构造方法注入
```xml
    <!-- 通过构造器注入属性值 -->
	<bean id="user3" class="com.xiaoshu.spring.test.User">
		<!-- 要求: 在 Bean 中必须有对应的构造器.  -->
		<constructor-arg value="Mike"></constructor-arg>
	</bean>
	
	<!-- 若一个 bean 有多个构造器, 如何通过构造器来为 bean 的属性赋值 -->
	<!-- 可以根据 index 和 value 进行更加精确的定位. 
	(索引匹配入参   类型匹配入参) -->
	<bean id="car" class="com.xiaoshu.spring.test.Car">
		<constructor-arg value="KUGA" index="1"></constructor-arg>
		<constructor-arg value="ChangAnFord" index="0"></constructor-arg>
		<constructor-arg value="250000" type="float"></constructor-arg>
	</bean>
	
	<bean id="car2" class="com.xiaoshu.spring.test.Car">
		<constructor-arg value="ChangAnMazda"></constructor-arg>
		<!-- 若字面值中包含特殊字符, 则可以使用 DCDATA 来进行赋值. ( “<，>”在xml里面是特殊符号 ) -->
		<constructor-arg>
			<value><![CDATA[<ATARZA>]]></value>
		</constructor-arg>
		<constructor-arg value="180" type="int"></constructor-arg>
	</bean>
```

(3) 引用其它 Bean

组成应用程序的 Bean 经常需要相互协作以完成应用程序的功能. 要使 Bean 能够相互访问, 就必须在 Bean 配置文件中指定对 Bean 的引用
在 Bean 的配置文件中, 可以通过 <ref> 元素或 ref 属性为 Bean 的属性或构造器参数指定对 Bean 的引用.
也可以在属性或构造器里包含 Bean 的声明, 这样的 Bean 称为内部 Bean
```xml
	<!-- 配置 bean -->
	<bean id="dao5" class="com.xiaoshu.spring.test.Dao"></bean>

	<bean id="service" class="com.xiaoshu.spring.test.Service">
		<!-- 通过 ref 属性值指定当前属性指向哪一个 bean! -->
		<property name="dao" ref="dao5"></property>
	</bean>
	
	<!-- 声明使用内部 bean -->
	<bean id="service2" class="com.xiaoshu.spring.test.Service">
		<property name="dao">
			<!-- 内部 bean, 类似于匿名内部类对象. 不能被外部的 bean 来引用, 也没有必要设置 id 属性 -->
			<bean class="com.xiaoshu.spring.test.Dao">
				<property name="dataSource" value="c3p0"></property>
			</bean>
		</property>
	</bean>
```
（4）级联属性（了解）

```xml
    <bean id="action" class="com.xiaoshu.spring.test.Action">
		<property name="service" ref="service2"></property>
		<!-- 设置级联属性(必须先初始化才能赋值，否则出异常，这里跟struts2不同) 
			如果没有初始化service  即没有 <property name="service" ref="service2"></property>
			直接<property name="service.dao.dataSource" value="c3p0"></property>将出异常
		-->
		<property name="service.dao.dataSource" value="DBCP2"></property>
	</bean>
	
	<bean id="dao2" class="com.xiaoshu.spring.test.Dao">
		<!-- 为 Dao 的 dataSource 属性赋值为 null, 若某一个 bean 的属性值不是 null, 使用时需要为其设置为 null(了解) -->
		<property name="dataSource"><null/></property>
	</bean>
```
（5）装配集合属性
```xml
<!-- 装配集合属性 -->
	<bean id="user" class="com.xiaoshu.spring.test.User">
		<property name="username" value="Jack"></property>
		<property name="cars">
			<!-- 使用 list 元素来装配集合属性 -->
			<list>
				<ref bean="car"/>
				<ref bean="car2"/>
			</list>
		</property>
	</bean>
	
	<!-- 声明集合类型的 bean -->
	<!-- 需要引入spring的util http://www.springframework.org/schema/util/spring-util-4.0.xsd" -->
	<util:list id="cars">
		<ref bean="car"/>
		<ref bean="car2"/>
	</util:list>
	
	<bean id="user2" class="com.xiaoshu.spring.test.User">
		<property name="username" value="Rose"></property>
		<!-- 引用外部声明的 list -->
		<property name="cars" ref="cars"></property>
	</bean>
```
array set 与list 类似
```xml
<!--Properties 注入例子-->
<property name="health">
    <props>
        <prop key="血压">正常</prop>
        <prop key="身高">178</prop>
        <bean id="user3" class="com.xiaoshu.spring.test.User"
        		p:cars-ref="cars" p:username="Titannic">
        </bean>
    </props>
</property>

 <!--Map 注入例子-->
 <property name="scores">
    <map>
        <entry key="数学">
            <value>88</value>
        </entry>
        <entry key="语文">
            <value>99</value>
        </entry>
    </map>
</property>
    
```
(6) 使用p命名空间

Spring 从 2.5 版本开始引入了一个新的 p 命名空间，可以通过 <bean> 元素属性的方式配置 Bean 的属性。
使用 p 命名空间后，基于 XML 的配置方式将进一步简化
```xml
<bean id="user3" class="com.xiaoshu.spring.test.User"
		p:cars-ref="cars" p:username="Titannic">
</bean>
```
（7）继承 bean

Spring 允许继承 bean 的配置, 被继承的 bean 称为父 bean. 继承这个父 Bean 的 Bean 称为子 Bean
子 Bean 从父 Bean 中继承配置, 包括 Bean 的属性配置
子 Bean 也可以覆盖从父 Bean 继承过来的配置
父 Bean 可以作为配置模板, 也可以作为 Bean 实例. 若只想把父 Bean 作为模板, 可以设置 <bean> 的abstract 属性为 true, 这样 Spring 将不会实例化这个 Bean并不是 <bean> 元素里的所有属性都会被继承. 比如: autowire,abstract等.
也可以忽略父 Bean 的 class 属性, 让子 Bean 指定自己的类, 而共享相同的属性配置. 但此时 abstract 必须设为true

依赖 bean

Spring 允许用户通过 depends-on 属性设定 Bean 前置依赖的Bean，前置依赖的 Bean 会在本 Bean 实例化之前创建好

如果前置依赖于多个 Bean，则可以通过逗号，空格或的方式配置 Bean 的名称

```xml
<!-- bean 的配置能够继承吗 ? 使用 parent 来完成继承 -->	
	<bean id="user4" parent="user" p:username="Bob"></bean>
	
	<bean id="user6" parent="user" p:username="维多利亚"></bean>
	
	<!-- depents-on -->	
	<bean id="user5" parent="user" p:userName="Backham" depends-on="user6"></bean>
```