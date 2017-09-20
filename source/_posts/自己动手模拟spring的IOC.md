---
title: 自己动手模拟spring的IOC
date: 2016-10-18 09:43:36
tags: [java,spring]
---
[文章来源:自己动手模拟spring的IOC](http://blog.csdn.net/u011229848/article/details/52845821)


我们这里是模拟spring，主要模拟spring中的IOC功能，所以在此我们一样要在service层中定义dao的实例，当然不用new出来，我们就通过spring的IOC把这里的dao层注入进来。不要忘了对dao提供set。Get方法，因为IOC的底层其实就是利用反射机制实现的，他把dao注入进来，其实底层就是通过反射set进来的。

首先我们把我们用的dao、service、entity定义出来：
```java
package org.spring.demo.entity;

public class Student {

	private int id;  
	private String name; 
	private String memo;
	private String address;
	
	public Student(int id, String name, String memo, String address) {
		super();
		this.id = id;
		this.name = name;
		this.memo = memo;
		this.address = address;
	}
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getMemo() {
		return memo;
	}
	public void setMemo(String memo) {
		this.memo = memo;
	}
	public String getAddress() {
		return address;
	}
	public void setAddress(String address) {
		this.address = address;
	}
	
	
}

```
因为 spring 提倡的就是面向接口编程，所以在我们写 dao 层和 service 层具体实现之前，我们先定义接口，让我们的具体实现实现接口。
<!--more-->
```java
package org.spring.demo.service;

import org.spring.demo.entity.Student;

public interface StudentService {

	public void add(Student student);
}

```
```java
package org.spring.demo.service.impl;

import org.spring.demo.dao.StudentDao;
import org.spring.demo.entity.Student;
import org.spring.demo.service.StudentService;

public class StudentServiceImp implements StudentService {

	StudentDao studentDao=null;  
	
	public StudentDao getStudentDao() {  
		return studentDao;  
	}  
	public void setStudentDao(StudentDao studentDao) {  
		this.studentDao = studentDao;  
	} 
	
	@Override
	public void add(Student student) {
		studentDao.add(student);  
		
	}

}

```
```java
package org.spring.demo.dao;

import org.spring.demo.entity.Student;

public interface StudentDao {
	
	public void add(Student student);
}

```

```java
package org.spring.demo.service.impl;

import org.spring.demo.dao.StudentDao;
import org.spring.demo.entity.Student;
import org.spring.demo.service.StudentService;

public class StudentServiceImp implements StudentService {

	StudentDao studentDao=null;  
	
	public StudentDao getStudentDao() {  
		return studentDao;  
	}  
	public void setStudentDao(StudentDao studentDao) {  
		this.studentDao = studentDao;  
	} 
	
	@Override
	public void add(Student student) {
		studentDao.add(student);  
		
	}

}

```
```java
package org.spring.demo.dao;

import org.spring.demo.entity.Student;

public interface StudentDao {
	
	public void add(Student student);
}

```
```java
package org.spring.demo.dao.impl;

import org.spring.demo.dao.StudentDao;
import org.spring.demo.entity.Student;

public class StudentDaoImp implements StudentDao {

	@Override
	public void add(Student student) {
		System.out.println(student.getName()+" is saved");
	}  

	
}

```
下一步我们就是定义我们自己的 ClassPathXmlApplicationContext类了，通过他，在我们new 出他的对象的时候，他来加载配置文件，然后把我们的 dao 操作注入到我们的 service 层，在 spring 中， ClassPathXmlApplicationContext类实现了BeanFactory 接口，在此我们也定义一个 BeanFactory 接口，其实这个接口没什么具体的作用，我们就是为了来模拟 spring 。在定义这个接口和实现类之前，我们先来看一下我们所需的 xml 是怎么编写的，下面我们就具体来看一下 beans.xml 的配置：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans>  
	<bean id="studentDao" class="org.spring.demo.dao.impl.StudentDaoImp" />  
	<bean id="studentService" class="org.spring.demo.service.impl.StudentServiceImp" >  
		<property name="studentDao" bean="studentDao"/>  
	</bean>  
</beans>  
```
看到这，相信大家都能感觉到这个配置文件太简单了，没有spring中那么多繁琐的配置，当然啦，我们这是只是实现其中的一个功能，spring提供了很多那么强大的功能，配置当然繁琐一些了。相信上边的代码不用我解释大家也能看懂了吧。

好了，配置文件我们看完了，下一步我们一起来看一下我们的spring容器——ClassPathXmlApplicationContext具体是怎么实现的，我们首先还是来看一下他的接口定义：
```java
package org.spring.demo.util;

public interface BeanFactory {

	public Object getBean(String id);
}

```
```java
package org.spring.demo.util;

import java.lang.reflect.Method;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.jdom.Document;
import org.jdom.Element;
import org.jdom.input.SAXBuilder;

public class ClassPathXmlApplicationContext implements BeanFactory{

	private Map<String, Object> beans = new HashMap<String, Object>();  
	
	public ClassPathXmlApplicationContext() throws Exception, Exception {  
		SAXBuilder sb = new SAXBuilder();  
		Document doc = sb.build(this.getClass().getClassLoader().getResourceAsStream("beans.xml")); // 构造文档对象  
		Element root = doc.getRootElement(); // 获取根元素HD  
		List list = root.getChildren("bean");// 取名字为disk的所有元素  
		for (int i = 0; i < list.size(); i++) {
			Element element = (Element) list.get(i);  
			String id = element.getAttributeValue("id");  
			String clazz = element.getAttributeValue("class");  
			Object o = Class.forName(clazz).newInstance();  
			System.out.println(id);  
			System.out.println(clazz);  
			beans.put(id, o);  
			for (Element propertyElement : (List<Element>) element.getChildren("property")) {  
				String name = propertyElement.getAttributeValue("name"); // userDAO  
				String bean = propertyElement.getAttributeValue("bean"); // u  
				Object beanObject = beans.get(bean);// UserDAOImpl instance  
				String methodName = "set" + name.substring(0, 1).toUpperCase()+ name.substring(1);  
				System.out.println("method name = " + methodName);  
				Method m = o.getClass().getMethod(methodName, beanObject.getClass().getInterfaces()[0]);  
				m.invoke(o, beanObject);  
			}  
		}  
	}  
	
	@Override
	public Object getBean(String id) {
		return beans.get(id);  
	}

}


```
首先我们定义了一个容器Map<String, Object> beans，这个容器的作用就是用来装我们从配置文件里解析来的一个个bean，为什么要用map类型，我想大家也差不多能猜到吧，我们配置文件中每一个bean都有一个id来作为自己的唯一身份。我们把这个id存到map的key里面，然后value就装我们的具体bean对象。说完这个容器之后，下面我们在来看一下ClassPathXmlApplicationContext的构造方法，这个构造方法是我们spring管理容器的核心，这个构造方法的前半部分是利用的jdom解析方式，把xml里面的bean一个个的解析出来，然后把解析出来的bean在放到我们bean容器里。如果这段代码看不懂的话，那你只好在去看看jdom解析xml了。好了，我们下面在来看一下这个构造的方法，后半部分主要是在对配置文件进行解析出bean的同时去查看一下这个bean中有没有需要注射bean的，如果有的话，他就去通过这些里面的property属性获取他要注射的bean名字，然后构造出set方法，然后通过反射，调用注入bean的set方法，这样我们所需要的bean就被注入进来了。如果这段代码你看不懂的话，那你只能去看一下有关反射的知识了。最后我们就来看一下实现接口的getBean放了，其实这个方法很简单，就是根据提供的bean的id，从bean容器内把对应的bean取出来。

好了，我们所需的东西都定义好了，我们自己模仿的spring把我们所需要的dao层给我们注入进来。
```java
package org.spring.demo;

import org.spring.demo.entity.Student;
import org.spring.demo.service.StudentService;
import org.spring.demo.util.ClassPathXmlApplicationContext;

public class SoringIOCTest {
	public static void main(String[] args) throws Exception {  
		ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext();  
		StudentService service = (StudentService) context.getBean("studentService");  
		Student student = new Student(1,"刘一",null,null); 
		service.add(student);  
	}  
}

```
运行代码，控制台输出：
```
studentDao
org.spring.demo.dao.impl.StudentDaoImp
studentService
org.spring.demo.service.impl.StudentServiceImp
method name = setStudentDao
刘一 is saved
```
好，成功注入进来，到此，我们模仿的spring就到此结束了.