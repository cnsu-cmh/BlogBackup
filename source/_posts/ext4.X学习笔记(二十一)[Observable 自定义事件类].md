---
title: ext4.X（白鹤翔第一季）学习笔记(二十一)[Observable 自定义事件类]
date: 2016-11-19 22:21:54
tags: [ext]
---
[文章来源:ext4.X（白鹤翔第一季）学习笔记(二十一)[Observable 自定义事件类]](http://blog.csdn.net/u011229848/article/details/53235773)


```javascript
Ext.onReady(function(){

	//Ext.util.Observable 自定义事件类
	
	//1：最简单的自定义事件：
	var win = Ext.create('Ext.Window',{
		title:'简单的自定义事件' , 
		width:300 , 
		height:200 , 
		renderTo:Ext.getBody() , 
		listeners:{
			show:function(){
				//3:触发自定义事件的时机
				win.fireEvent('myEvent');
			}
		}
	});
	//1: 添加事件类型
	win.addEvents('myEvent');
	//2: 添加事件的监听
	win.on('myEvent' , function(){
		alert('my....event...');
	});
	win.show();
	
	//2: 为自己定义的类去添加事件的支持
	Ext.define('Employee', {
	    mixins: {
	        observable: 'Ext.util.Observable'
	    },
	    constructor: function (config) {
	        this.mixins.observable.constructor.call(this, config);
	        this.addEvents(
	            'fired',
	            'quit'
	        );
	    }
	});
	var newEmployee = new Employee({
	    listeners: {
	        quit: function() {
	            alert(" has quit!");
	        }
	    }
	});
	newEmployee.fireEvent('quit');
	
	
	
	
	// 单次运行监听器的使用，single配置项在组件中的用途

	var win = Ext.create('Ext.Window',{
		title:'单次执行监听器的使用' , 
		width:300 , 
		height:200 , 
		renderTo:Ext.getBody() , 
		listeners:{
			render:function(){
				alert('把组件渲染到body上，整个过程只有一次');
			} , 
			single:true	,	//当前这个事件监听执行一次之后就自动销毁了
			delay:3000 		//延迟执行事件监听
		}
	});
	win.show();
	
	
	//对于事件的挂起和恢复实例
	var btn1 = Ext.create('Ext.button.Button',{
		text:'挂起' ,
		renderTo:Ext.getBody(),
		handler:function(){
			 btn3.suspendEvents();
		}
	});
	var btn2 = Ext.create('Ext.button.Button',{
		text:'恢复' ,
		renderTo:Ext.getBody(),
		handler:function(){
			btn3.resumeEvents();
		}
	});
	var btn3 = Ext.create('Ext.button.Button',{
		text:'按钮' ,
		renderTo:Ext.getBody(),
		listeners:{
			'mouseover':function(){
				alert("执行啦...");
			}
		}
	});	
	
	
	//事件的转发机制	
	var win = Ext.create('Ext.Window',{
		title:'事件的转发' , 
		renderTo:Ext.getBody() , 
		width:300 , 
		height:200 ,
		listeners:{
			myEvent:function(){
				alert('我是自己定义的事件...');
			}
		}
	});
	
	var btn = Ext.create('Ext.Button',{
		text:'按钮' ,
		renderTo:Ext.getBody() , 
		handler:function(){
			btn.fireEvent('myEvent');
		}
	});
	win.show();
	//事件的转发机制： 1 转发给的对象    2 转发的事件类型数组
	win.relayEvents(btn,['myEvent']);
	
	
	
});

```