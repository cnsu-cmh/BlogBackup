---
title: ext4 学习笔记（十四）[DomHelper常用方法]（白鹤翔第一季）
date: 2016-11-16 22:43:52
tags: [ext]
---
[文章来源:ext4 学习笔记（十四）[DomHelper常用方法]（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/53192709)


<br/>Ext为了更加的方便我们去操作DOM元素，特提供了DomHelper这个辅助的工具类。下面我们就一起学习下DomHelper
<br/>首先从API来看，这个类暴露出的public方法并不是特别多。仅仅13个方法而已。如果想生成dom节点，在这里不建议使用原生的方法去生成dom节点，原因是代码量比较大的时候性能比较低、其二是自己组装HTML字符串比较麻烦。在Ext里，DomHelper对象类似一个元素生成器，用于解决此类问题。一般的javascript框架底层都会有生成html代码的类似功能函数。
DomHelper常用方法：
<br/>createHtml或markup方法<br/>createDom方法<br/>append方法<br/>insertHTML方法<br/>overwrite方法<br/>createTemplate方法<br/>applyStyles方法
<!--more-->
```javascript
Ext.onReady(function(){

	//准备工作
	Ext.create('Ext.panel.Panel',{
		title:'DomHelper-元素生成器的使用',
		width:'90%' , 
		height:400 ,
		renderTo:Ext.getBody(),
		html:'<div id=d1>我是d1</div>'
	});
	
	
	//DomHelper
	//1: createHtml或markup方法
	//配置项说明：
	//tag 元素的名称  
	//children/cn表示子元素 
	//cls表示样式  
  	//html:文本内容
  	var html = Ext.DomHelper.createHtml({
  		tag:'ol' ,
  		cn:[
  			{tag:'li',html:'item1'},
  			{tag:'li',html:'item2'}
  		]
  	});
  	console.info(html);
  
  	var html = Ext.DomHelper.createHtml({
  		tag:'div' , 
  		children:[
  			{tag:'a' ,html:'bjsxt网站' , href:'www.bjsxt.cn'},
  			{tag:'input' , value:'点击' , type:'button' }
  		]
  	});
  	console.info(html);
  
  	//2: createDom方法 (这个方法是生成原生的dom节点，不推荐使用)
  	var dom = Ext.DomHelper.createDom({
  		tag:'div' ,
  		html:'我是div'
  	});
  	console.info(dom);
  
  	//3: append方法
  	Ext.DomHelper.append('d1',{
  		tag:'ul' , 
  		cn:[{tag:'li',html:'1111'},{tag:'li',html:'2222'}]
  	});
  	Ext.DomHelper.append('d1','<span>我是span内容</span>');
  
  	//4:insertHtml方法 //这个方法就是为了操作原生的node节点的
  	//参数说明：String where, HTMLElement/TextNode el, String html
  	Ext.DomHelper.insertHtml('beforeBegin',Ext.getDom('d1'),'<h1>我是标题!!</h1>')
  
  	//5:overwrite方法
  	Ext.DomHelper.overwrite('d1',{tag:'span',html:'我是覆盖的span'});
  	Ext.DomHelper.overwrite('d1','aaaaa');
  
  	//6:createTemplate方法
  	var tp = Ext.DomHelper.createTemplate('<h1>{text}</h1><h2>{text2}</h2>'); //return Ext.Template
  	tp.overwrite('d1',{text:'我是被替换的内容!!',text2:'我也是替换的内容!!'});
  
  	//7:applyStyles方法	
  	Ext.DomHelper.applyStyles('d1',{
  		width:'100px',
  		height:'100px',
  		backgroundColor:'green'
  	});
	
});
```