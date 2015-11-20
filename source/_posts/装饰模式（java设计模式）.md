---
title: 装饰模式（java设计模式）
date: 2016-11-10 16:48:45
tags: [java,设计模式]
---
[文章来源:装饰模式（java设计模式）](http://blog.csdn.net/u011229848/article/details/53115674)


版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/u011229848/article/details/53115674

**装饰模式：**

在不必改变原类文件和使用继承的情况下，动态地扩展一个对象的功能。

它是通过创建一个包装对象，也就是装饰来包裹真实的对象。

**装饰模式的特点：**

（1） 装饰对象和真实对象有相同的接口。这样客户端对象就能以和真实对象相同的方式和装饰对象交互。

（2） 装饰对象包含一个真实对象的引用（reference）
（3） 装饰对象接受所有来自客户端的请求。它把这些请求转发给真实的对象。

（4） 装饰对象可以在转发这些 **请求以前或以后增加一些附加功能。这样就确保了在运行时，不用修改给定对象的结构就可以在外部增加附加的功能。在面向对象的设计中，通常是通过继承来实现对给定类的功能扩展。**

**适用性：**

1. 需要扩展一个类的功能，或给一个类添加附加职责。

2. 需要动态的给一个对象添加功能，这些功能可以再动态的撤销。
3. 需要增加由一些基本功能的排列组合而产生的非常大量的功能，从而使继承关系变的不现实。

4. 当不能采用生成子类的方法进行扩充时。一种情况是，可能有大量独立的扩展，为支持每一种组合将产生大量的子类，使得子类数目呈爆炸性增长。另一种情况可能是因为类定义被隐藏，或类定义不能用于生成子类。
**优点：**

1. Decorator模式与继承关系的目的都是要扩展对象的功能，但是Decorator可以提供比继承更多的灵活性。

2. 通过使用不同的具体装饰类以及这些装饰类的排列组合，设计师可以创造出很多不同行为的组合。
**缺点：**

1. 这种比继承更加灵活机动的特性，也同时意味着更加多的复杂性。

2. 装饰模式会导致设计中出现许多小类，如果过度使用，会使程序变得很复杂。
3. 装饰模式是针对抽象组件（Component）类型编程。但是，如果你要针对具体组件编程时，就应该重新思考你的应用架构，以及装饰者是否合适。当然也可以改变Component接口，增加新的公开的行为，实现“半透明”的装饰者模式。在实际项目中要做出最佳选择。 **设计原则：**

1. 多用组合，少用继承。

利用继承设计子类的行为，是在编译时静态决定的，而且所有的子类都会继承到相同的行为。然而，如果能够利用组合的做法扩展对象的行为，就可以在运行时动态地进行扩展。
2. 类应设计的对扩展开放，对修改关闭 ****

**包含角色：**
（1）抽象构件（Component）角色：给出一个抽象接口，以规范准备接收附加责任的对象。
（2）具体构件（Concrete Component）角色：定义一个将要接收附加责任的类。
（3）装饰（Decorator）角色：持有一个构件（Component）对象的实例，并实现一个与抽象构件接口一致的接口。
（4）具体装饰（Concrete Decorator）角色：负责给构件对象添加上附加的责任。

**实例；**

星巴克咖啡店收银系统，假设咖啡有深焙，综合，浓缩，低糖咖啡，调料有牛奶，豆浆，摩卡，他们都有不同的价格，那怎么来计算咖啡的价格呢？
```java
package decorator;

/**
 * 饮料类
 */
public abstract class Beverage {

	//饮料描述
	String description = "Unknow Becerage";

	//获得饮料描述
	public String getDescription() {
		return description;
	}

	//饮料的价格   这个价格我们需要放到子类中实现   
	public abstract double cost();
}

```
```java
package decorator;


/**
 * 深焙咖啡
 */
public class DarkRoast extends Beverage {

	/**
	 * DarkRoast继承了Beverage  
	 * 在这里使用构造器来设置饮料的描述
	 */
	public DarkRoast() {
		description = "Dark Roast Coffee";
	}

	
	/**
	 * 现在我们只管深焙咖啡的价格   不需要关心调料的价格
	 */
	@Override
	public double cost() {
		return 0.99;
	}

}
```
```java
package decorator;


/**
 * 低糖咖啡
 */
public class Decat extends Beverage {

	/**
	 * Decat继承了Beverage  
	 * 在这里使用构造器来设置饮料的描述
	 */
	public Decat() {
		description = "Decat Coffee";
	}

	
	/**
	 * 现在我们只管低糖咖啡的价格   不需要关心调料的价格
	 */
	@Override
	public double cost() {
		return 1.99;
	}

}

```
```java
package decorator;


/**
 * 浓缩咖啡
 */
public class Espresso extends Beverage {

	/**
	 * Espresso继承了Beverage  
	 * 在这里使用构造器来设置饮料的描述
	 */
	public Espresso() {
		description = "Espresso Coffee";
	}

	
	/**
	 * 现在我们只管浓缩咖啡的价格   不需要关心调料的价格
	 */
	@Override
	public double cost() {
		return 1.99;
	}

}

```
```java
package decorator;


/**
 * 综合咖啡
 */
public class HouseBlend extends Beverage {

	/**
	 * HouseBlend继承了Beverage  
	 * 在这里使用构造器来设置饮料的描述
	 */
	public HouseBlend() {
		description = "House Blend Coffee";
	}

	
	/**
	 * 综合咖啡的价格   不需要关心调料的价格
	 */
	@Override
	public double cost() {
		return 0.89;
	}

}

```

```java
package decorator;

/**
 * 调料类
 * 这里我们继承  也就是Condiment要能够取代 Beverage
 */
public abstract class Condiment  extends Beverage{
	
	public abstract String getDescription();
}

```
```java
package decorator;

/**
 * 牛奶是一个装饰者，所以要扩展自调料，
 * 调料扩展与饮料
 */
public class Milk extends Condiment {

	//饮料  用一个实例变量来记录饮料，也就是被装饰者
	Beverage beverage; 
	
	//用构造器让被装饰者（饮料）被记录到实例变量里面
	public Milk(Beverage beverage) {
		this.beverage = beverage;
	}

	//这里获得饮料的描述，但是我们不只是描述饮料是什么咖啡，我们还要添加调料的描述
	@Override
	public String getDescription() {
		return beverage.getDescription()+", Milk";
	}

	//我们计算带有牛奶的某一类咖啡价格
	@Override
	public double cost() {
		return beverage.cost()+0.10;
	}

}

```

```java
package decorator;

/**
 * 摩卡是一个装饰者，所以要扩展自调料，
 * 调料扩展与饮料
 */
public class Mocha extends Condiment {

	//饮料  用一个实例变量来记录饮料，也就是被装饰者
	Beverage beverage; 
	
	//用构造器让被装饰者（饮料）被记录到实例变量里面
	public Mocha(Beverage beverage) {
		this.beverage = beverage;
	}

	//这里获得饮料的描述，但是我们不只是描述饮料是什么咖啡，我们还要添加调料的描述
	@Override
	public String getDescription() {
		return beverage.getDescription()+", Mocha";
	}

	//我们计算带有摩卡的某一类咖啡价格
	@Override
	public double cost() {
		return beverage.cost()+0.20;
	}

}
```
```java
package decorator;

/**
 * 豆浆是一个装饰者，所以要扩展自调料，
 * 调料扩展与饮料
 */
public class Soy extends Condiment {

	//饮料  用一个实例变量来记录饮料，也就是被装饰者
	Beverage beverage; 
	
	//用构造器让被装饰者（饮料）被记录到实例变量里面
	public Soy(Beverage beverage) {
		this.beverage = beverage;
	}

	//这里获得饮料的描述，但是我们不只是描述饮料是什么咖啡，我们还要添加调料的描述
	@Override
	public String getDescription() {
		return beverage.getDescription()+", Soy";
	}

	//我们计算带有豆浆的某一类咖啡价格
	@Override
	public double cost() {
		return beverage.cost()+0.15;
	}

}
```

测试：
```java
package decorator;

public class Test {

	public static void main(String[] args) {
		
		//先来一杯浓缩咖啡
		Beverage beverage = new Espresso();
		System.out.println(beverage.getDescription()+" $"+beverage.cost());
	
		//来一杯深焙咖啡
		Beverage beverage2 = new DarkRoast();
		//加双份摩卡
		beverage2 = new Mocha(beverage2);
		beverage2 = new Mocha(beverage2);
		//加一份牛奶
		beverage2 = new Milk(beverage2);
		//加一份豆浆
		beverage2 = new Soy(beverage2);
		System.out.println(beverage2.getDescription()+" $"+beverage2.cost());
		
	
	}
	
}

```
结果

```
Espresso Coffee $1.99
Dark Roast Coffee, Mocha, Mocha, Milk, Soy $1.64
```
具体代码实现我们也看了，那如果说现在星巴克提出，我现在分大中小三种杯，每一种被子对应的调料价格不一样怎么扩展 呢？

就比如说我现在大、中、小杯子加牛奶的价格分别收2.00,1.50，1.00元，怎么办？

我们不妨在饮料里面添加一个属性杯子类型，然后在配料中分别根据不同类型添加价格：
```java
package decorator;

/**
 * 饮料类
 */
public abstract class Beverage {

	static String L="L",M="M",S="S";
	
	//饮料描述
	String description = "Unknow Becerage";

	//杯子类型 
	String size; 
	
	//获得饮料描述
	public String getDescription() {
		return description;
	}

	//饮料的价格   这个价格我们需要放到子类中实现   
	public abstract double cost();

	public String getSize() {
		return size;
	}

	public void setSize(String size) {
		this.size = size;
	}
	
}

```
```java
package decorator;

/**
 * 牛奶是一个装饰者，所以要扩展自调料，
 * 调料扩展与饮料
 */
public class Milk extends Condiment {

	//饮料  用一个实例变量来记录饮料，也就是被装饰者
	Beverage beverage; 
	
	//用构造器让被装饰者（饮料）被记录到实例变量里面
	public Milk(Beverage beverage) {
		this.beverage = beverage;
	}

	//这里获得饮料的描述，但是我们不只是描述饮料是什么咖啡，我们还要添加调料的描述
	@Override
	public String getDescription() {
		return beverage.getDescription()+", Milk";
	}
	
	public String getSize(){
		return beverage.getSize();
	}

	//我们计算带有牛奶的某一类咖啡价格
	@Override
	public double cost() {
		double cost = beverage.cost();
		if(getSize().equals(Beverage.L)){
			cost += 2.00;
		}else if(getSize().equals(Beverage.M)){
			cost += 1.50;
		}else if(getSize().equals(Beverage.S))
			cost += 1.00;
		
		return cost;
	}

}

```

```java
package decorator;

public class Test {

	public static void main(String[] args) {
		
		Beverage beverage3 = new HouseBlend();
		beverage3.setSize(Beverage.M);
		beverage3 = new Milk(beverage3);
		System.out.println(beverage3.getSize()+","+beverage3.getDescription()+" $"+beverage3.cost());
	}
	
}

```

```
M,House Blend Coffee, Milk $2.39
```
好了，装饰模式就到这吧，希望读者能多多批评指教，关于更多装饰模式可以看 java IO 流 ，它是典型的装饰模式。 谢谢！

****
1. Decorator模式与继承关系的目的都是要扩展对象的功能，但是Decorator可以提供比继承更多的灵活性。

2. 通过使用不同的具体装饰类以及这些装饰类的排列组合，设计师可以创造出很多不同行为的组合。