---
title: ext4 学习笔记（五）[Ext.define 新建类]（白鹤翔第一季）
date: 2016-11-03 00:05:24
tags: [null]
---
[文章来源:ext4 学习笔记（五）[Ext.define 新建类]（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/53014129)


<br/>说明：
<br/>
<br/>（1）本文章是针对于ExtJs 4.X ，文章中出现的5版本只是我引入的文件是ExtJs.5.0,并不是文章是基于5版本，文章是4版本的
<br/>定义类的方法：define
<br/>对于Ext4.X版本来说，采用了新定义类的define方法，而不是延续旧版本的extend方法，那么对于定义一个新的类。我们来了解下define的使用。
<br/>Ext.define(classname,properties,callback)
<br/>
<br/>classname:要定义的新类的类名，properties：新类的配置对象，callback：回调函数，当类创建完后执行该函数
<br/>对于Ext定义一个新的类，那么我们可以想象到，既然是利用Ext.define去创建类，那么创建的类一定是Ext所特有的类，不同于传统的javascript创建一个类，也就是说我们要对于define方法的第二个参数properties配置项进行配置，需要找到Ext对于类支持的API并进行配置。
<br/>configs:
<br/>extend：用于继承
<br/>alias:类的别名
<br/>
<br/>alternateClassName：备用名，与alias差不多
<br/>requires：需要使用到的类名数组，在动态加载时会根据该属性去下载类，注意需要的类是在当前类之前被加载
<br/>uses：与requires类似 但是被引用的类可以在该类之后才加载
<br/>constructor：构造器属性，一般用来初始化类的配置和调用其父类的方法
<br/>mixins：混入属性，多继承
<br/>config：定义类的配置项，会把config里的每个属性加上get和set方法
<br/>statics：定义静态方法，属性不能被子类继承
<br/>inheritableStatics：与statics类似，但是其属性可被子类继承
<br/>singleton：设置该类为单件模式
<br/>基本介绍就到这，下面进入代码示例：
<br/>
<br/>首先引入ext所需要的样式，js和国际化（文章中都有，这里就省略了，望见谅）
<br/>在Ext中如何去定义一个类？之前文章也说到过define,即 Ext.define(className , properties , callback)
<br/>
<br/>classname:要定义的新类的类名， properties：新类的配置对象， callback：回调函数，当类创建完后执行该函数，回调函数很少用。
```javascript
    Ext.onReady(function(){
    	Ext.define('Person',{
    		//这里是对于这个类的一些配置信息
    		//config属性 就是配置当前类的属性内容,并且会加上get和set方法
    		config:{
    			name:'张三' , 
    			age: 20
    		},
    		//自己定义的方法
    		say:function(){
    			console.info('我是say()方法...');
    		},
    		//给当前定义的类加一个构造器 ,目的就是为了初始化信息
    		constructor:function(config){
    			var me = this ;
//    			for(var attr in config){	//这里可以看到传进来的config
//    				console.info(attr + " : " + config[attr]);
//    			}
    			me.initConfig(config);	// 真正的初始化传递进来的参数
    		}
    	});
    	
    	//Ext.create 实例化一个对象
    	var p = Ext.create('Person',{
    		name:'王五' , 
    		age:30
    	});
    	//因为config已经添加了get，set方法  我们就不再用p.name这种方法获取name值，推荐采用p.getName()
    	console.info(p.getName()+p.getAge());
    	p.say();
    	
    	
    });
```

<br/>代码中注释也很详细，在这里我还是说一下，这个config的属性是Ext在 创建类时配置属性，那也就是在Ext.Class下面api中，需要注意的是构造器 constructor，这个方法中用到了initConfig()这个自己去看一下底层代码，在这里不解释，只需要知道是初始化参数的方法。另外就是config提供了set,get方法，那我们获取属性值的时候就不在推荐用实例对象.属性的方法，推荐使用get方法。
<br/>
<br/>初始化没什么好说的，下面我们来说一下继承extend
```javascript
    	//Sup Class
    	Ext.define('Person',{
    		config:{
    			name:'小树'
    		} ,
    		constructor:function(config){
    			var me = this ;
    			me.initConfig(config);
    		}
    	});
    	//Sub Class
    	Ext.define('Boy',{
    		//使用Ext的继承
    		extend:'Person',
    		config:{
    			sex:'男',
    			age:20
    		}
    	});
    	var b = Ext.create('Boy',{
    		name:'张三',
    		age:25
    	});
    	console.info(b.getName()+','+b.getAge+','+b.getSex());
```
<br/>在代码中看到Boy继承了Person，创建Boy的时候拥有了Person里面sex属性，我们看一下javaScript原来的继承写法：
```javascript
<pre name="code" class="javascript">    	//javascript : prototype(原型)  :所有类的实例对象所共享
    	function Person(name){
    		this.name = name; 
    		//this.sayName = sayName ;
    	};
    	//如果方法单独拿出在外则不只是Person的方法，也就不叫继承
//    	function sayName(){
//    		alert(this.name);
//    	};
    	Person.prototype.sayName = function(){
    		console.info(this.name);
    	};
    	
    	var p1  = new Person('张三');
    	p1.sayName();
    	var p2  = new Person('李四');
    	p2.sayName();	
    	console.info(p1.sayName == p2.sayName); 
     
    	//javascript : prototype(原型)  :实现继承
    	//SupClass
    	var PersonClass = function(name){
    		this.name = name; 
    	};
    	//console.info(Person.prototype.constructor);		//原型对象的构造器,默认是当前的类的模板
    	//SupClass prototype object
    	PersonClass.prototype = {
    		constructor:PersonClass ,
    		id:100
    	};
    	
    	//SubClass
    	var Boy = function(name,sex,age){
    		//借用构造函数继承的方式
    		PersonClass.call(this,name);
    		this.sex = sex ;
    		this.age = age ;
    	};
    	
    	//实现原型继承：继承父类的模板和父类的原型对象  
    	//Boy.prototype = new PersonClass();
    	
    	var b = new Boy('李四','男',25);
    	console.info(b.name+','+b.age+','+b.sex);
    	
    	
    	
    	//实现原型继承: 继承了父类的模板和父类的原型对象
    	//Boy.prototype = new Person();
    	//自己实现extend的方法
    	function myextend(sub , sup){
    	        var F = function() {},		//定义一个空函数做为中转函数
    	            subclassProto,			//子类的原型对象
    	            superclassProto = sup.prototype;	//把父类的原型对象 交给了superclassProto变量
    	
    	        F.prototype = superclassProto;	// 做中转的位置：把父类的原型对象 赋值给了 F这个空函数的原型对象
    	        subclassProto = sub.prototype = new F();	//进行原型继承
    	        subclassProto.constructor = sub;		//还原构造器
    	        sub.superclass = superclassProto;		//做了一个保存,保存了父类的原型对象
    			//目的是为了防止你大意了
    	        if (superclassProto.constructor === Object.prototype.constructor) {
    	            superclassProto.constructor = sup;
    	        }	
    	};
    	myextend(Boy ,PersonClass);
    	var b2 = new Boy('刘一','男',23);
    	console.info(b2.name+','+b2.age+','+b2.sex);
```
<br/>原生Js利用的也就是一个构造函数继承方式，但仔细看，Boy.prototype = new PersonClass(); 实例初始化一次，构造器又实例初始化一次 如果我的父类有N个属性，这样对于性能来讲是很差的。看ExtJs的方式，很巧妙：
```javascript
    	//自己实现extend的方法
    	function myextend(sub , sup){
    	        var F = function() {},		//定义一个空函数做为中转函数
    	            subclassProto,			//子类的原型对象
    	            superclassProto = sup.prototype;	//把父类的原型对象 交给了superclassProto变量
    	
    	        F.prototype = superclassProto;	// 做中转的位置：把父类的原型对象 赋值给了 F这个空函数的原型对象
    	        subclassProto = sub.prototype = new F();	//进行原型继承
    	        subclassProto.constructor = sub;		//还原构造器
    	        sub.superclass = superclassProto;		//做了一个保存,保存了父类的原型对象
    			//目的是为了防止你大意了
    	        if (superclassProto.constructor === Object.prototype.constructor) {
    	            superclassProto.constructor = sup;
    	        }	
    	};
    	myextend(Boy ,PersonClass);
    	var b2 = new Boy('刘一','男',23);
    	console.info(b2.name+','+b2.age+','+b2.sex);
```
<br/>创建一个空的函数来中转一次，这样继承父类的原型时并没有直接的原型对象，也就不存在重复初始化参数值，还考虑了我们没有对父类原型继承时自动给我们继承父类原型。
<br/>别名、备用名
```javascript
/**别名  备用名*/
    	Ext.define("User",{
    		config:{
    			name:'小树' , 
    			age:100
    		},
    		alias:'u' ,//起别名	底层代码在Ext.ClassManger
    		alternateClassName:'uu',	//给当前类一个备用名 底层代码在Ext.ClassManger
    		constructor:function(config){
    			var me = this;
    			me.initConfig(config);
    		}
    	});
    	 
    	var u = Ext.create('u');
    	var uu = Ext.create('uu');
    	console.info(u.getName()+","+uu.getAge());
```
<br/>  静态方法，静态属性 statics(子类不能继承) inheritableStatics(子类可以继承)

```javascript
/**statics(子类不能继承) inheritableStatics(子类可以继承) 给当前类定义静态方法或属性*/
    	Ext.define('Person',{
    		config:{
    			name:'我是父类'
    		},
    		statics:{	//静态的方法或属性
    			static_id:'我是Person的id,不能被子类所继承!!'
    		},
    		inheritableStatics:{	//静态的方法或属性
    			inheritableStatics_id:'我是Person的id,我可以被子类继承!!'
    		},
    		constructor:function(config){
    			var me = this;
    			me.initConfig(config);
    		}
    	});
    	
    	//一定注意:!!!!!//实例对象是无法使用静态属性或方法的
    	//var p = Ext.create('Person');
    	//console.info(p.static_id);	
    	//用类名去使用静态属性:!!!! 不能p.static_id
//    	console.info(Person.static_id);  
//    	console.info(Person.inheritableStatics_id);
    	
    	Ext.define('User',{
    		extend:'Person' , 
    		config:{
    			age:20
    		}
    	});
    	console.info(User.static_id);   // underfine
    	console.info(User.inheritableStatics_id);
```
<br/>混合配置 多继承（java中不能多继承，这里就是继承多个的意思，表述有问题请见谅）

```javascript
/**mixins 混合的配置项,可以多继承的配置*/
    	Ext.define("Sing",{
    		canSing:function(){
    			console.info('cansing...');
    		}
    	});
    	Ext.define("Say",{
    		canSay:function(){
    			console.info('cansay...');
    		}
    	});	
    	Ext.define('User',{
    		mixins:{
    			sing:"Sing" , 
    			say:"Say"
    		}
    	});
    	
    	var u = Ext.create("User");
    	u.canSay();
    	u.canSing();
```
<br/>requires 和 uses 以及 singleton
<br/>需要Ext或者是其他的类做支持
<br/>
<br/>requires加载需要的类时机是：当前类初始化之前被加载
<br/>requires:['Ext.window.Window','Ext.button.Button'],
<br/>uses加载需要的类时机是：当前类初始化之后被加载
<br/>uses:['Ext.form.Panel','Ext.grid.Panel'],
<br/>当前的类就被当做一个单例对象 （单例就是只会初始化一次，new多少次都只有一个实例）
<br/>singleton:true
<br/>
<br/>OK!这一节就到这吧，共勉！