---
title: ext4 学习笔记（六）[Ext.js方法 ]（白鹤翔第一季）
date: 2016-11-03 22:51:36
tags: [ext]
---
[文章来源:ext4 学习笔记（六）[Ext.js方法 ]（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/53027050)


Ext.js方法详解：
Ext.apply&Ext.applyIf
Ext.extend
typeOf
isEmpty、isIterable、isFunction、isArray...
Iterate

Ext.apply & Ext.applyIf

```javascript
    //Ext.apply就是为对象扩展属性或方法的
	var src = {name:'张三',age:20};	//源对象
	var config = {name:'李四',sex:'男'};		//配置对象
	//Ext.apply(src , config);
	Ext.applyIf(src,config);		//如果当前对象存在属性,我就不进行copy 如果不存在则copy
	// name : '李四'  age : 20  sex:'男'
	for(var attr in src){
		console.info(attr + " : " + src[attr]);
	}
```
上面代码Ext.apply（src,config）是将配置对象的属性赋给源对象，也就是src已经有的属性我给你赋值，没有的我给你扩展，则apply之后src ={name：'李四'，age：20，sex:'男'}

Ext.applyIf(src,config)是说如果源对象已经有配置对象的属性，我就不管，如果没有我就扩展，也就是代码注释中说已经有的属性我不做复制，没有的就复制 applyIf之后src={name='张三',age='20',sex='男'}

```javascript
//Ext.typeOf 和原生的javascript typeof
    	var str = '111';
    	var num = 20;
    	console.info(Ext.typeOf(str));
    	console.info(Ext.typeOf(num));
    	Ext.isArray();
    	var arr = [1,2,3,4];
    	Ext.iterate(arr,function(item){
    		console.info(item);
    	});
```
typeOf这个和原生的差不多，这里就不多说了，Ext.typeOf(str) string Ext.typeOf(num) number

```javascript
//Ext.override
	Ext.define('User',{
		say:function(){
			console.info('say....');
		}		
	});
	var user = Ext.create('User');
	Ext.override(user,{
		say:function(){
			console.info('我是覆盖后的say方法..');
		}
	});
	user.say();
```
override 顾名思义也就是重写覆盖，跟java里面的重写是一样的，只是这里没有父子关系而已，就是覆盖修改方法。

不知道这样解释是否正确，对于extend方法就不再啰嗦了，在创建类的配置中已经说过了这一届就不再说了。