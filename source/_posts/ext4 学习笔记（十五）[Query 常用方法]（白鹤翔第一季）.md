---
title: ext4 学习笔记（十五）[Query 常用方法]（白鹤翔第一季）
date: 2016-11-16 23:07:05
tags: [ext]
---
[文章来源:ext4 学习笔记（十五）[Query 常用方法]（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/53192905)


<br/>Ext.dom.Query 嗯，这个类一共提供了8个方法供开发人员去使用。
<br/>要说最常用的方法，无非就是Ext.query这个方法，之前我们已经简单接触过了这个方法，下面是此方法的详细使用规则：
<br/>1.基本选择器（简单选择器） id选择器 css的类选择器 标签选择器
<br/>2属性选择器
<br/>3.伪类选择器（也可以说是相当于JQ过滤选择器）
<br/>
<br/>Ext.query基本使用形式：
<br/>Ext.query('span') 返回整个文档的span标签
<br/>Ext.query('span' , 'root') 根据跟节点进行查询
<br/>Ext.query('/#id')根据id进行查询，但返回数组
<br/>Ext.query('.class')根据样式进行查询，返回数组
<br/>Ext.query('div span')根据标签进行包含选择器过滤
<br/>Ext.query('/*')匹配所有元素
<br/>Ext.query('input[value/*=val]') 进行一个属性的选择匹配
<br/>Ext.query('E>F')进行一个层次查找父节点为E的F节点
<br/>Ext.dom.Query其他方法：
<br/>compile：将选择符或xpath编译成一个可重复使用的函数
<br/>filter：使用简单选择符过滤元素数组
<br/>is：判断元素是否匹配简单选择符
<br/>jsSelect：根据选择符选择元素
<br/>

```javascript
    Ext.onReady(function(){

	
	//Ext.DomQuery
	Ext.create('Ext.Panel',{
		title:'Ext.DomQuery示例',
		width:500 , 
		height:400 , 
		renderTo:Ext.getBody(),
		html:'<ul><li>item1</li><li>item2</li></ul><div id=d1><span id=sp>我是sp内容</span><span class=mycolor>我是第二个span</span></div>'
	});
	
	
	//Ext.DomQuery.select ==  Ext.query  //返回内容:HTMLElement[]
	///Ext.DomQuery.jsSelect：根据选择符选择元素	（这个方法和Ext.DomQuery.select差不多）
	//我把他分为三大类：
	//基本选择器 id选择器 css的类选择器 标签选择器（简单选择器）
	//属性选择器、伪类选择器（也可以说是相当于JQ过滤选择器）（复杂选择器）
	
	
	
	//Ext.query('span')   	返回整个文档的span标签
 	//1 Ext.query('span' , 'root') 	根据跟节点进行查询
 	var arr = Ext.query('span' , 'd1');
 	Ext.Array.each(arr,function(el){
 		console.info(el.innerHTML);
 	});
 	//2 Ext.query('#id')		根据id进行查询，但返回数组	
 	var arr = Ext.query('#d1');
 	Ext.Array.each(arr,function(el){
 		console.info(el.innerHTML);
 	});	
 
 	//3 Ext.query('.class')	根据样式进行查询，返回数组
 	var arr = Ext.query('.mycolor');
 	Ext.Array.each(arr,function(el){
 		console.info(el.innerHTML);
 	});	
 	//Ext.query('*')	匹配所有元素
 	console.info(Ext.query('*'));
 
 
 	//复杂选择器：
 	//1 :Ext.query('div span')	根据标签进行包含选择器过滤
 	var arr = Ext.query('div[id=d1] span');
 	Ext.Array.each(arr,function(el){
 		console.info(el.innerHTML);
 	});
 
 	//1.1:Ext.query('E>F')		进行一个层次查找父节点为E的F节点
 	var arr = Ext.query('div>span');		//Xpath:div/span 查找xml文件比较实用
 	Ext.Array.each(arr,function(el){
 		console.info(el.innerHTML);
 	});	
 
 	//2 :属性选择器Ext.query('E[attr=val]') 	进行一个属性的选择匹配
 	var arr = Ext.query('div[id*=d]');
 	Ext.Array.each(arr,function(el){
 		console.info(el.id);
 	});	
 
 	//3: 伪类选择器
 	//E:first-child
 	var arr = Ext.query('li:first-child');	
 	Ext.Array.each(arr,function(el){
 		console.info(el.innerHTML);
 	});		
 
 
 	//Ext.DomQuery其他方法：
 
 	//1：compile：将选择符或xpath编译成一个可重复使用的函数
 	var fn = Ext.DomQuery.compile('span');
 	console.info(fn);
 	var arr = fn(Ext.getDom('d1'));
 	Ext.Array.each(arr,function(el){
 		console.info(el.innerHTML);
 	});	
 	//2: filter：使用简单选择符过滤元素数组
 	//参数说明 HTMLElement[] el, String selector, Boolean nonMatches
 	var arr = document.getElementsByTagName('div');
 	var filterarr = Ext.DomQuery.filter(arr,'div[id=d1]',false);
 	Ext.Array.each(filterarr,function(el){
 		console.info(el.id);
 	});		
	//is：判断元素是否匹配简单选择符
	console.info(Ext.DomQuery.is(Ext.getDom('d1'),'div[id]'));
	
	
});
```