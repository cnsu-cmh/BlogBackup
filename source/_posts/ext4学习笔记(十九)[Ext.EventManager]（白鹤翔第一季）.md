---
title: ext4学习笔记(十九)[Ext.EventManager]（白鹤翔第一季）
date: 2016-11-19 22:14:52
tags: [ext]
---
[文章来源:ext4学习笔记(十九)[Ext.EventManager]（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/53235480)

Ext.EventManager:把浏览器自带的事件做个封装 ，目的是屏蔽浏览器之间的差异问题
on添加事件监听 un移除事件监听

```javascript
Ext.onReady(function(){

	
	//Ext.EventManager:把浏览器自带的事件做个封装 ，目的是屏蔽浏览器之间的差异问题
	// on添加事件监听   un移除事件监听
	
	var inp = document.createElement('input');	//Ext.DomHelper.createDom
	inp.type = 'button';
	inp.id = 'inp1';
	inp.value = '点击';
	document.body.appendChild(inp);
	//----------------------------------------
	
	
	var einp = Ext.get('inp1');
 	Ext.EventManager.on(einp,'click',function(){
 		alert('我执行了..');
 	});
 	Ext.EventManager.on(einp,'click',function(){
 		alert('我又执行了..');
 	});	
 
 	Ext.EventManager.on(einp,{
 		'click':function(){alert('单机!')} , 
 		'mouseout':function(){alert('移除!')}
 	});
 
 	//不推荐 
 	Ext.EventManager.on(einp,{
 		'click':{
 			fn:function(){
 				alert('执行1');
 			}
 		} , 
 	    'mouseout':{
 			fn:function(){
 				alert('执行2');
 			}
 		}
 	});
	
});
```