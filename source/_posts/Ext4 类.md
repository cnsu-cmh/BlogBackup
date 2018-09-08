---
title: Ext4 类
date: 2016-11-20 21:59:01
tags: [ext]
---
[文章来源:Ext4 类](http://blog.csdn.net/u011229848/article/details/53244976)

****

**1).声明******

**1.1) 旧的方式**

如果你曾经使用过旧版本的extjs,那么你肯定熟悉使用Ext.extend来创建一个类:

var MyWindow=Ext.extend(Object,{...});

这个方法很容易从现有的类中继承创建新的类.相比直接继承,我们没有好用的API用于类创建的其他方面,诸如:配置、静态方法、混入（Mixins）。呆会我们再来详细的重新审视这些方面。现在，让我们来看看另一个例子：

My.cool.Window = Ext.extend(Ext.Window, { ... });

在这个例子中，我们创建我们的新类，继承Ext.Window，放在命名空间中。我们有两个问题要解决：
1，在我们访问My.cool的Window属性之前,My.cool必须是一个已有的对象.

2,Ext.Window必须在引用之前加载.

第一个问题通常使用Ext.namespace(别名Ext.ns)来解决.该方法递归创建(如果该对象不存在)这些对象依赖.比较繁琐枯燥的部分是你必须在Ext.extend之前执行Ext.ns来创建它们.

Ext.ns('My.cool');

My.cool.Window = Ext.extend(Ext.Window, { ... });

第二个问题不好解决,因为Ext.Window可能直接或间接的依赖于许多其他的类,依赖的类可能还依赖其它类...出于这个原因,在ext4之前,我们通常引入整个ext-all.js,即使是我们只需要其中的一小部分.

**1.2) 新的方式**

在Extjs4中,你只需要使用一个方法就可以解决这些问题:**Ext.define**.以下是它的基本语法:

Ext.define(className, members, onClassCreated);
例如：  

```javascript
Ext.onReady(function(){
	Ext.define('Persion',{
		name:'Cmh',
		constructor:function(name){
			if(name){
				this.name = name;
			}
			return this;
		},
		say:function(language){
			Ext.Msg.alert('alert:',this.name +" say "+language +"!");
		}
	});
	
	var xiaoming = Ext.create('Persion','XiaoMing');
	xiaoming.say("Chinese"); //alert: XiaoMing sya Chinese!
})



```

注意我们使用Ext.create()方法创建了My.sample.Person类的一个新实例.我们也可以使用新的关键字(new My.sample.Person())来创建.然而,建议养成始终用Ext.create来创建类示例的习惯,因为它允许你利用动态加载的优势.

**2).配置**

在ExtJS 4 ,我们引入了一个专门的配置属性,用于提供在类创建前的预处理功能.特性包括:

配置完全封装其他类成员
getter和setter.如果类没有定义这些方法,在创建类时将自动生成配置的属性的getter和setter方法。

同样的,每个配置的属性自动生成一个apply方法.自动生成的setter方法内部在设置值之前调用apply方法.如果你要在设置值之前自定义自己的逻辑,那就重载apply方法.如果apply没有返回值,则setter不会设置值.看下面applyTitle的例子:
```javascript
Ext.onReady(function(){	
	
	Ext.define('MyWindow',{
		isWindow:true,
		config:{
			title:"MyWindow",
			bottomBar:{
				enabled:true,
				height:200,
				resizable:false
			}
		},
		constructor:function(config){
			this.initConfig(config);
			return this;
		},
		
		applyTitle:function(title){
			if(!Ext.isString(title) || title.length ===0){
				Ext.Msg.alert('提示：',"title不能为空");
			}else{
				return title;
			}
		}
		
	});
	
	var myWindow = Ext.create("MyWindow",{
		title:'MyWindow TestOne',
		bottomBar:{
			height:30
		}
	});
	
	Ext.Msg.alert("标题",myWindow.getTitle());//MyWindow TestOne
	
	myWindow.setTitle('MyWindow Test_One');
	
	alert(myWindow.getTitle()); //MyWindow Test_One
});

```
**3.Statics**

静态成员可以使用statics配置项来定义

```javascript
Ext.onReady(function(){
	Ext.define('Photo',{
		statics:{
			size:0
		}
	});	
	alert(Photo.size); //0
	Photo.size = 200;
	alert(Photo.size); //200
	
});


```
详细配置还有文章[Ext.define](http://blog.csdn.net/u011229848/article/details/53014129) [http://blog.csdn.net/u011229848/article/details/53014129](http://blog.csdn.net/u011229848/article/details/53014129)
[](http://blog.csdn.net/u011229848/article/details/53014129)

[](http://blog.csdn.net/u011229848/article/details/53014129)