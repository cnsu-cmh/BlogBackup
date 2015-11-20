---
title: 策略模式(java设计模式)
date: 2016-11-09 17:56:49
tags: [java,设计模式]
---
[文章来源:策略模式(java设计模式)](http://blog.csdn.net/u011229848/article/details/53102519)


版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/u011229848/article/details/53102519

**我们先看实例，再来解读策略模式**

****

实例中用的小鸭子模拟器，首先说一下实例的思想，我们把鸭子分成不同类型，不管灰鸭子，黄鸭子都是鸭子，我们继承鸭子，但是有些鸭子会飞，有些鸭子还不会飞，有的鸭子嘎嘎叫，有的鸭子咯咯叫...这些鸭子的行为不是具体的，就好比旱鸭子也有学会游泳的一天，呵呵呵呵，好了，具体上代码：

飞行行为，不只是鸭子会飞行哦
```java
package strategy;

public interface FlyBehavior {

	/**
	 * 所有飞行行为类必须实现的接口
	 */
	public void fly();
}

```
实现飞的行为实例： 会飞与不会飞

```java
package strategy;

public class FlyWithWings implements FlyBehavior {

	/**
	 * 这个是飞行行为的实现，给真的会飞的鸭子使用
	 */
	@Override
	public void fly() {
		System.out.println("I'm flying!");
	}

}

```
```java
package strategy;

public class FlyNoWay implements FlyBehavior {

	/**
	 * 这个是飞行行为的实现，给不会飞的鸭子使用
	 */
	@Override
	public void fly() {
		System.out.println("I'm can't flying!");
	}

}

```

同理，叫的行为

```java
package strategy;

public interface QuackBehavior {
	/**
	 * 所有喊叫行为类必须实现的接口
	 */
	public void quack();
}

```
叫的具体实现
```java
package strategy;

public class Quack implements QuackBehavior {

	@Override
	public void quack() {
		System.out.println("嘎嘎");

	}

}

```

```java
package strategy;

public class SqueakQuack implements QuackBehavior {

	@Override
	public void quack() {
		System.out.println("咯咯");

	}

}

```
然后是我们的主角鸭子

```java
package strategy;

public abstract class Duck {

	/*FlyBehavior flyBehavior;
	QuackBehavior quackBehavior;*/
	
	public Duck() {
	}
	
	public abstract void display();
	
	public void performFly(FlyBehavior flyBehavior){
		flyBehavior.fly();
	}
	
	public void performQuack(QuackBehavior quackBehavior){
		quackBehavior.quack();
	}
	
	public void swing(){
		System.out.println("All ducks can swing");
	}
}

```
不同的鸭子

```java
package strategy;

public class ModelDuck extends Duck {

	@Override
	public void display() {
		System.out.println("I'm real model duck");

	}

}

```

```java
package strategy;

public class MallardDuck extends Duck {

	@Override
	public void display() {
		System.out.println("I'm real Mallard duck");

	}

}

```
看效果，测试类

```java
package strategy;

public class Test {

	public static void main(String[] args) {
		Duck mallardDuck = new MallardDuck();
		mallardDuck.display();
		FlyWithWings flyBehavior = new FlyWithWings();
		mallardDuck.performFly(flyBehavior);
		mallardDuck.performQuack(new Quack());
		mallardDuck.swing();
		
		Duck modelDuck = new ModelDuck();
		modelDuck.display();
		FlyNoWay flyBehavior2 = new FlyNoWay();
		modelDuck.performFly(flyBehavior2);
		modelDuck.performQuack(new SqueakQuack());
		modelDuck.swing();
	}
}

```

I'm real Mallard duck
I'm flying!
嘎嘎
All ducks can swing
I'm real model duck
I'm can't flying!
咯咯
All ducks can swing
```
好了，只看代码是不知道所以然的，我们来看一下策略模式，再根据你理解的再解读代码，我想会瞬间变得更清晰的。

策略模式 :策略模式定义了一系列的算法，并将每一个算法封装起来，而且使它们还可以相互替换。策略模式让算法独立于使用它的客户而独立变化.

**组成：**

---抽象策略角色： 策略类，通常由一个接口或者抽象类实现。

---具体策略角色：包装了相关的算法和行为。

---环境角色：持有一个策略类的引用，最终给客户端调用。

**概念:**

****

（原文：The Strategy Pattern defines a family of algorithms,encapsulates each one,and makes them interchangeable. Strategy lets the algorithm vary independently from clients that use it.）

Context(应用场景):
1、需要使用ConcreteStrategy提供的算法。

2、 内部维护一个Strategy的实例。
3、 负责动态设置运行时Strategy具体的实现算法。

4、负责跟Strategy之间的交互和数据传递。
Strategy(抽象策略类)：

1、 定义了一个公共接口，各种不同的算法以不同的方式实现这个接口，Context使用这个接口调用不同的算法，一般使用接口或抽象类实现。
ConcreteStrategy(具体策略类)：

2、 实现了Strategy定义的接口，提供具体的算法实现。
****

****

应用场景

1、 多个类只区别在表现行为不同，可以使用Strategy模式，在运行时动态选择具体要执行的行为。

2、 需要在不同情况下使用不同的策略(算法)，或者策略还可能在未来用其它方式来实现。
3、 对客户隐藏具体策略(算法)的实现细节，彼此完全独立。 **优缺点**

优点：

1、 策略模式提供了管理相关的算法族的办法。策略类的等级结构定义了一个算法或行为族。恰当使用继承可以把公共的代码转移到父类里面，从而避免重复的代码。
2、 策略模式提供了可以替换继承关系的办法。继承可以处理多种算法或行为。如果不是用策略模式，那么使用算法或行为的环境类就可能会有一些子类，每一个子类提供一个不同的算法或行为。但是，这样一来算法或行为的使用者就和算法或行为本身混在一起。决定使用哪一种算法或采取哪一种行为的逻辑就和算法或行为的逻辑混合在一起，从而不可能再独立演化。继承使得动态改变算法或行为变得不可能。

3、 使用策略模式可以避免使用多重条件转移语句。多重转移语句不易维护，它把采取哪一种算法或采取哪一种行为的逻辑与算法或行为的逻辑混合在一起，统统列在一个多重转移语句里面，比使用继承的办法还要原始和落后。
缺点：

1、客户端必须知道所有的策略类，并自行决定使用哪一个策略类。这就意味着客户端必须理解这些算法的区别，以便适时选择恰当的算法类。换言之，策略模式只适用于客户端知道所有的算法或行为的情况。
2、 策略模式造成很多的策略类，每个具体策略类都会产生一个新类。有时候可以通过把依赖于环境的状态保存到客户端里面，而将策略类设计成可共享的，这样策略类实例可以被不同客户端使用。换言之，可以使用享元模式来减少对象的数量。

****

**好了，就到这，希望读者能够多多指点批评，谢谢！**

****

****