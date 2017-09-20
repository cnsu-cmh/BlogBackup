---
title: 观察者模式（java设计模式）
date: 2016-11-08 09:55:27
tags: [java,设计模式]
---
[文章来源:观察者模式（java设计模式）](http://blog.csdn.net/u011229848/article/details/53078984)


**本文结合了两篇博文而改，希望对你有用**

**观察者模式（Observer Pattern）也叫做发布订阅模式（Publish/subscribe）****。**

**其定义如下：**

定义对象间一种一对多的依赖关系，使得每当一个对象改变状态，则所有依赖于它的对象都会得到通知并被自动更新。

观察者模式理解：这是一种1对多的依赖，首先得有一个被观察者，当被观察者的状态发生变化时，可以自动通知notify观察者，所以叫做观察者模式。观察者模式中，一个被观察者管理所有相依于它的观察者物件，并且在本身的状态改变时主动发出通知。这通常通过呼叫各观察者所提供的方法来实现。此种模式通常被用来实现事件处理系统。

**通用类图如下所示：**

![](观察者模式（java设计模式）/20160902152720873.jpg)
<!--more-->
**包含角色：**

****

**● Subject**抽象被观察者角色：

把所有对观察者对象的引用保存在一个集合中，每个被观察者角色都可以有任意数量的观察者。被观察者提供一个接口，可以增加和删除观察者角色。一般用一个抽象类和接口来实现。

**● Observer抽象观察者角色：**

为所有具体的观察者定义一个接口，在得到主题的通知时更新自己。

**● Concrete Subject具体被观察者角色：**

在被观察者内部状态改变时，给所有登记过的观察者发出通知。具体被观察者角色通常用一个子类实现。

●Concrete Observer具体观察者角色：

该角色实现抽象观察者角色所要求的更新接口，以便使本身的状态与主题的状态相协调。通常用一个子类实现。如果需要，具体观察者角色可以保存一个指向具体主题角色的引用。

通用源代码如下：
```java
import java.util.*;  
  
  
// 被观察者  
public abstract class Subject {  
     //定义一个观察者数组  
     private Vector<Observer> obsVector = new Vector<Observer>();  
     //增加一个观察者  
     public void addObserver(Observer o){  
             this.obsVector.add(o);  
     }  
   //删除一个观察者  
     public void delObserver(Observer o){  
             this.obsVector.remove(o);  
     }  
     //通知所有观察者  
     public void notifyObservers(){  
             for(Observer o:this.obsVector){  
                     o.update();  
             }  
     }  
}  
  
//具体被观察者  
public class ConcreteSubject extends Subject {  
    //具体的业务  
    public void doSomething(){  
            /* 
             * do something 
             */  
            super.notifyObservers();  
    }  
}  
  
//观察者  
public interface Observer {  
     //更新方法  
     public void update();  
}  
  
//具体观察者  
public class ConcreteObserver implements Observer {  
     //实现更新方法  
     public void update() {  
             System.out.println("接收到信息，并进行处理！");  
     }  
}  
  
//测试类  
public class Client {  
    public static void main(String[] args) {  
            //创建一个被观察者  
            ConcreteSubject subject = new ConcreteSubject();  
            //定义一个观察者  
            Observer obs= new ConcreteObserver();  
            //观察者观察被观察者  
            subject.addObserver(obs);  
            //观察者开始活动了  
            subject.doSomething();  
    }  
}  
```
**观察者模式的优点：**

●观察者和被观察者之间是抽象耦合

●可以建立一套完整的触发机制

**缺点：**

观察者模式需要考虑一下开发效率和运行效率问题，一个被观察者，多个观察者，开发和调试就会比较复杂，而且在[Java](http://lib.csdn.net/base/javaee "Java EE知识库")中消息的通知默认是顺序执行，一个观察者卡壳，会影响整体的执行效率。在这种情况下，一般考虑采用异步的方式。

**Java世界中的观察者模式：**

javase的API中提供了**java.util.Observable实现类和java.util.Observer接口**，我们写的每个被观察者只需要extends Observable实现类即可以，而每一个观察者只需要implements Observer接口就OK。

其中，在Observable实现类中Java给我们提供了常用的方法，包括addObserver(Observer o)以及deleteObserver(Observer o)、notifyObserver（）等。

在接口Observer中，有一个抽象方法update( )需要我们自己实现。如下图所示：

![](观察者模式（java设计模式）/20160902154528927.jpg)

详情请查阅API

**适用场景**

1) 当一个抽象模型有两个方面, 其中一个方面依赖于另一方面。将这二者封装在独立的对象中以使它们可以各自独立地改变和复用。
2) 当对一个对象的改变需要同时改变其它对象, 而不知道具体有多少对象有待改变。

3) 当一个对象必须通知其它对象，而它又不能假定其它对象是谁。换言之, 你不希望这些对象是紧密耦合的。
**应用**

珠宝商运送一批钻石，有黄金强盗准备抢劫，珠宝商雇佣了私人保镖，警察局也派人护送，于是当运输车上路的时候，强盗保镖警察都要观察运输车一举一动，
抽象的观察者

```java
public interface Watcher  {  
     public void update();  
}  
```

抽象的被观察者，在其中声明方法（添加、移除观察者，通知观察者）：
```java
public interface Watched {  
     public void addWatcher(Watcher watcher);  
  
     public void removeWatcher(Watcher watcher);  
  
     public void notifyWatchers();  
}  
```
 具体的观察者

保镖
```java
public class Security implements Watcher {  
     @Override  
     public void update()  
     {  
          System.out.println("运输车有行动，保安贴身保护");  
     }  
}  
```
强盗
```java
public class Thief implements Watcher  {  
     @Override  
     public void update()  
     {  
          System.out.println("运输车有行动，强盗准备动手");  
     }  
}  
```
警察
```java
public class Police implements Watcher {  
     @Override  
     public void update()  
     {  
          System.out.println("运输车有行动，警察护航");  
     }  
}  
```
具体的被观察者
```java
public class Transporter implements Watched  {  
      private List<Watcher> list = new ArrayList<Watcher>();  
   
      @Override  
      public void addWatcher(Watcher watcher)  
      {  
           list.add(watcher);  
      }  
   
      @Override  
      public void removeWatcher(Watcher watcher)  
      {  
           list.remove(watcher);  
      }  
   
      @Override  
      public void notifyWatchers(String str)  
      {  
           for (Watcher watcher : list)  
           {  
                watcher.update();  
           }  
      }  
   
 }  
```
测试类
```java
public class Test {  
     public static void main(String[] args)  
     {  
          Transporter transporter = new Transporter();  
  
          Police police = new Police();  
          Security security = new Security();  
          Thief thief = new Thief();  
  
          transporter.addWatcher(police);  
          transporter.addWatcher(security);  
          transporter.addWatcher(security);  
  
          transporter.notifyWatchers();  
     }  
}  
```
我推你拉
例子中没有关于数据和状态的变化通知，只是简单通知到各个观察者，告诉他们被观察者有行动。

观察者模式在关于目标角色、观察者角色通信的具体实现中，有两个版本。
一种情况便是目标角色在发生变化后，仅仅告诉观察者角色“我变化了”，观察者角色如果想要知道具体的变化细节，则就要自己从目标角色的接口中得到。这种模式被很形象的称为：拉模式——就是说变化的信息是观察者角色主动从目标角色中“拉”出来的。

还有一种方法，那就是我目标角色“服务一条龙”，通知你发生变化的同时，通过一个参数将变化的细节传递到观察者角色中去。这就是“推模式”——管你要不要，先给你啦。
这两种模式的使用，取决于系统设计时的需要。如果目标角色比较复杂，并且观察者角色进行更新时必须得到一些具体变化的信息，则“推模式”比较合适。如果目标角色比较简单，则“拉模式”就很合适啦。

观察者模式中，一个被观察者管理所有相依于它的观察者物件，并且在本身的状态改变时主动发出通知。这通常通过呼叫各观察者所提供的方法来实现。此种模式通常被用来实现事件处理系统。