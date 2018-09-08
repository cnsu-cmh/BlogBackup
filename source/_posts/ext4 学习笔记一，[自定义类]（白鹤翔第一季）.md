---
title: ext4 学习笔记一，[自定义类]（白鹤翔第一季）
date: 2016-10-30 17:11:06
tags: [ext]
---
[文章来源:ext4 学习笔记一，[自定义类]（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/52972922)


<br/>首先说明一下：

<br/>（1）本文章是针对于ExtJs 4.X ，文章中出现的5版本只是我引入的文件是ExtJs.5.0,并不是文章是基于5版本，文章是4版本的
<br/>（2）由于注释很全，所以文章内容就不写那么详细了，直接贴代码还望读者能够理解  
 
```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>

</body>
<!-- 引入EXT样式  -->
<link href="resources/ext-5.0.1-min/packages/ext-theme-crisp/build/resources/ext-theme-crisp-all.css" rel="stylesheet" />
<!-- 引入EXT all -->
<script src="resources/ext-5.0.1-min/ext-all.js"></script>
<!-- 引入EXT  国际化-->
<script src="resources/ext-5.0.1-min/packages/ext-locale/ext-locale-zh_CN.js"></script>
    <script type="text/javascript">
    /* Start ExtJS 中自定义类 **/
    Ext.define("Person", {  
        name: '',  //属性
        age: 0,  
    	say: function (msg) {  //方法
            Ext.Msg.alert(this.name + " says:", msg+"I'm "+this.age +"years。");  
        },  
        constructor: function (name, age) {   //构造器
            this.name = name;  
            this.age = age;  
        }  
    });  
    
    Ext.define("Friend", {  
        name: '',  //属性
        age: 0,   
        say: function (msg) { //方法 
            Ext.Msg.alert(this.name + " says:", msg+"I'm "+this.age +"years。");  
        },  
        constructor: function (obj) {   //构造器
            Ext.apply(this, obj); //apply 将对象obj的属性复制给当前对象
    }
    });
    
    Ext.onReady(function () {
    	//Ext.MessageBox.XXX 可直接斜Ext.Msg.XXX
    	//注意：这是用的一个DIV  后面的会覆盖前面的
    	
        Ext.MessageBox.alert("提示", "Hello World");
        var tom = new Person("小树",22);
        	tom.say("Hello，ExtJs！");
    	  /* var tom = Ext.create("Person");;
    	  tom.constructor("小树", 23);
    	  tom.say("Hello，ExtJs！") */
        
        var mary = Ext.create("Friend",{name:"mary",age:23});//ext4.X推荐使用
        	mary.say("Hello，ExtJs！");
        
    });
    </script>
</html>
```
 主要配置（创建类详细配置学习）请看 [学习笔记五Ext.define](http://blog.csdn.net/u011229848/article/details/53014129) [http://blog.csdn.net/u011229848/article/details/53014129](http://blog.csdn.net/u011229848/article/details/53014129)

<br/>ext是倾向于面向对象的编程思想，这也是它的一个比较大的特性，4 以后推荐使用的创建类方法是 Ext.create方法，实例中也有说明，new 的方式不是不可以，只是之前版本一直用new。

我们知道，只有在Ext框架全部加载完后才能在客户端的代码中使用Ext，而Ext的onReady正是用来注册在Ext框架及页面的html代码加载完后，所要执行的函数。

调用onReady方法时可以带三个参数，

第一个参数是必须的，表示要执行的函数或匿名函数，

第二参数表示函数的作用域，

第三个参数表示函数执行的一些其它特性，比如延迟多少毫秒执行等，大多数情况下只需要第一个参数即可。

Ext.onReady(function(){alert("2")},this,{delay:5000});
则在页面加载完成后，5秒后会执行上面onReady方法中的函数alert弹出'2'。

谢谢大家观看，更希望能给您带来收获，有什么疑问或者错误希望大家能够提出，再接再厉！