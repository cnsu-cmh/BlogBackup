---
title: Java 之 Annotation(注解)介绍
date: 2016-11-10 00:56:50
tags: [java]
---
[文章来源:Java 之 Annotation(注解)介绍](http://blog.csdn.net//u011229848//article/details/53114684)


## 什么是 Annotation？ （注解 or 注释）

Annotation, 准确的翻译应该是 -- 注解。 和注释的作用完全不一样。

Annotation 是JDK5.0及以后版本引入的一个特性。 与类、接口、枚举是在同一个层次，可以成为[Java](http://lib.csdn.net/base/javaee "Java EE知识库") 的一个类型。

语法是以@ 开头

简单来说，

注释是程序员对源代码的类，方法，属性等做的一些记忆或提示性描述(比如这个方法是做什么用的)，是给人来看的。

注解则是Java 编译器可以理解的部分，是给编译器看的。

举个简单的例子来看一下注解的使用和作用。

@Override 是比较常见的Java 内置注解，它的作用就是在编译代码的时候检查子类中定义的方法是否正确。

```java
package annotation;  
  
public abstract class Animal {  
    public abstract void eat();  
}  

  
public class Cat extends Animal{  
  
    @Override  
    public void eat(String food) {    
    }  
}  
```

 这里在子类Cat中 eat 方法被注解为覆写父类的方法， 但是却比父类方法多出一个参数。

如果是在Eclipse 在编辑的话， 直接就会有红色叉叉提示。(代码编译会通不过)。

如果去掉@Override的注解的话， 编译没问题， 但是Cat 中eat方法就是这个类的一个新的方法了，而不是从父类继承的了。

## []()常见的Java 内置注解

包含@Override ， 还有哪些常见的Java内置注解？

1. @Deprecated

注解为不建议使用，可以用在 方法和类上。

基本上这种方法和类都是因为升级或性能上面的一些原因废弃不建议使用，但是为了兼容或其他原因，还必须保留。

所以就打上这个注解。

在Java 本身的API中就有很多这样的例子， 方法打上了这个注解，进到Source code 会看到替代的新的方法是哪个。

在eclipse 中编写code时，添加此注解的方法在声明和调用的地方都会加上删除线。

2.@Override

3.@SuppressWarnings

忽略警告。

如果你的code在转型或其他的部分有一些警告的话，但是你又想忽略这些警告，就可以使用这个注解了。

1)deprecation 使用了不赞成使用的类或方法时的警告
2)unchecked 执行了未检查的转换时警告
3)fallthrough 当使用switch操作时case后未加入break操作，而导致程序继续执行其他case语句时出现的警告
4)path 当设置一个错误的类路径、源文件路径时出现的警告
5)serial 当在可序列化的类上缺少serialVersionUID定义时的警告
6)fianally 任何finally子句不能正常完成时警告
7)all 关于以上所有情况的警告

## []()自定义注解

除了Java本身提供的内置注解， Java 还提供了定制自定义注解的功能。

定义的方式就是使用注解定义注解， 用来定义注解的注解称为元注解。

主要的元注解有以下四个：@Target ；@Retention；@Documented；@Inherited

1. @Target 表示该注解用于什么地方，使用在类上，方法上，或是属性等

可能的 ElemenetType 参数包括：
ElemenetType.CONSTRUCTOR 构造器声明
ElemenetType.FIELD 域声明（包括 enum 实例）
ElemenetType.LOCAL_VARIABLE 局部变量声明
ElemenetType.METHOD 方法声明
ElemenetType.PACKAGE 包声明
ElemenetType.PARAMETER 参数声明
ElemenetType.TYPE 类，接口（包括注解类型）或enum声明

2. @Retention 表示在什么级别保存该注解信息

可选的 RetentionPolicy 参数包括：
RetentionPolicy.SOURCE 注解将被编译器丢弃
RetentionPolicy.CLASS 注解在class文件中可用，但会被VM丢弃
RetentionPolicy.RUNTIME VM将在运行期也保留注释，因此可以通过反射机制读取注解的信息。

3. @Documented ，产生doc时，是否包含此注解

将此注解包含在 javadoc 中

4. @Inherited

允许子类继承父类中的注解

看一些简单定义的例子：

```java
package annotation;  
  
import java.lang.annotation.Documented;  
import java.lang.annotation.ElementType;  
import java.lang.annotation.Inherited;  
import java.lang.annotation.Retention;  
import java.lang.annotation.RetentionPolicy;  
import java.lang.annotation.Target;  
  
@Target(ElementType.METHOD)  
public @interface MyAnnotation {  
    String value();  
}  
  
@Retention(RetentionPolicy.SOURCE)   
@interface MyAnnotation1 { }   
  
@Retention(RetentionPolicy.CLASS)   
@interface MyAnnotation2 {}   
  
@Retention(RetentionPolicy.RUNTIME)   
@interface MyAnnotation3 {}  
  
  
@Documented  
@interface MyAnnotation4 {}  
              
@Inherited  
@interface MyAnnotation5 { }  
```
使用例子：
```java
package annotation;  
  
import java.lang.annotation.Annotation;  
  
@MyAnnotation3  
public class TestAnnotation {  
    public static void main(String[] args) {  
        // TODO Auto-generated method stub  
        Annotation annotation = TestAnnotation.class.getAnnotation(MyAnnotation3.class);  
        System.out.println(annotation.toString());  
    }  
  
}  
```
打印出结果： @annotation.MyAnnotation3()

以上例子如果替换使用 MyAnnotation1 和 MyAnnotation2 的话， 则取到的annotation的值为空，这就是RetentionPolicy 不同的差别。

## []()Annotation的作用

介绍到此，可以总结一下Annotation的作用了。
基础的大致可以分为三类:
1. 编写文档
2. 代码分析
3. 编译检查

但是，开源框架对其赋予了更多的作用
比如：
Hibernate,注解配置，
```java
@Column("aa")  
private String xx;  
```
 这个类似于XML配置，简化程序中的配置
相对与把一部分元数据从XML文件移到了代码本身之中，在一个地方管理和维护。
内部如何实现的？ -- java 反射机制，类似与以上例子