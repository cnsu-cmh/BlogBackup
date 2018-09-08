---
title: ext4学习笔记(二十)[为Ext的UI组件绑定事件]（白鹤翔第一季）
date: 2016-11-19 22:17:49
tags: [ext]
---
[文章来源:ext4学习笔记(二十)[为Ext的UI组件绑定事件]（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/53235630)

```javascript
Ext.onReady(function(){

	//为Ext的UI组件绑定事件
 	//1:直接在组件内部添加listeners配置项 即可
 	var win = Ext.create('Ext.Window',{
 		title:'UI组件的事件示例一' , 
 		width:400 , 
 		height:300 , 
 		renderTo:Ext.getBody() , 
 		listeners:{		//在这个配置项对象中加入事件即可
 			show : function(){
 				alert('我展示出来了...');
 			} , 
 			close:function(){
 				alert('我被关闭了....');
 			} , 
 			render:function(){
 				alert('组件渲染的时候执行的事件..');
 			}, 
 			click:{
 	            element: 'body', //bind to the underlying body property on the panel
 	            fn: function(){ alert('click body'); }
 			}
 		}
 	});
 	win.show();
 
 
 
 	//2：使用组件的引用为组件绑定一些事件
 	var win = Ext.create('Ext.Window',{
 		title:'UI组件的事件示例一' , 
 		width:400 , 
 		height:300 , 
 		renderTo:Ext.getBody() 		//把组件渲染到指定的位置(body)
 	});
 
 	win.on('show',function(){
 		alert('我展示出来了..');
 	});	
 	
 	win.on({
 		'show':function(){
 				alert('show...');		
 		} , 
 		'close':function(){
 				alert('close...');
 		} , 
 		'render':function(){
 				alert('render....');
 		}
 	});
 	win.show();
	
});
```