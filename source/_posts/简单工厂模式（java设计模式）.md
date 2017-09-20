---
title: 简单工厂模式（java设计模式）
date: 2016-11-11 15:18:14
tags: [java,设计模式]
---
[文章来源:简单工厂模式（java设计模式）](http://blog.csdn.net/u011229848/article/details/53129044)


版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/u011229848/article/details/53129044

**简单工厂模式：**

****简单工厂模式是属于创建型模式，又叫做静态工厂方法（Static Factory Method）模式，但不属于23种GOF设计模式之一。简单工厂模式是由一个工厂对象决定创建出哪一种产品类的实例。简单工厂模式是工厂模式家族中最简单实用的模式，可以理解为是不同工厂模式的一个特殊实现。

****

**简单工厂模式的UML图：**

简单工厂模式中包含的角色及其相应的职责如下：

工厂角色（Creator）：这是简单工厂模式的核心，由它负责创建所有的类的内部逻辑。当然工厂类必须能够被外界调用，创建所需要的产品对象。

抽象（Product）产品角色：简单工厂模式所创建的所有对象的父类，注意，这里的父类可以是接口也可以是抽象类，它负责描述所有实例所共有的公共接口。

具体产品（Concrete Product）角色：简单工厂所创建的具体实例对象，这些具体的产品往往都拥有共同的父类。

![](简单工厂模式（java设计模式）/2010062300571970.png)
<!--more-->
**简单工厂模式深入分析**：

简单工厂模式解决的问题是如何去实例化一个合适的对象。

简单工厂模式的核心思想就是：有一个专门的类来负责创建实例的过程。

具体来说，把产品看着是一系列的类的集合，这些类是由某个抽象类或者接口派生出来的一个对象树。而工厂类用来产生一个合适的对象来满足客户的要求。

如果简单工厂模式所涉及到的具体产品之间没有共同的逻辑，那么我们就可以使用接口来扮演抽象产品的角色；如果具体产品之间有功能的逻辑或，我们就必须把这些共同的东西提取出来，放在一个抽象类中，然后让具体产品继承抽象类。为实现更好复用的目的，共同的东西总是应该抽象出来的。

在实际的的使用中，抽闲产品和具体产品之间往往是多层次的产品结构，如下图所示：

![](简单工厂模式（java设计模式）/201006/2010062300583042.png)

**简单工厂模式代码实现：**
```java
package simplefactory;

public interface People {

	final static String Chinese="Chinese",American="American";
	
	public void say();
}

```
```java
package simplefactory;

public class American implements People{

	@Override
	public void say() {
		System.out.println("Speak English！");
	}

}
```
```java
package simplefactory;

public class Chinese implements People{

	@Override
	public void say() {
		System.out.println("说汉语！");
	}

}

```
```java
package simplefactory;

public class PeopleFactory {

	static People creat(String type){
		if(type.equals(People.Chinese)){
			return new Chinese();
		}else if(type.equals(People.American)){
            return new American();
      }
		return null;
	}
}

```
```java
package simplefactory;

public class Test {
	
	public static void main(String[] args) {
		People p = PeopleFactory.creat(People.Chinese);
		p.say();//说汉语！
		
		p = PeopleFactory.creat(People.American);
		p.say();//Speak English！
	}
	
	
}

```