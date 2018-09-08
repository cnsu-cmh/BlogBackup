---
title: ext4 js 模拟EventManager（白鹤翔第一季）
date: 2016-11-19 21:59:38
tags: [ext]
---
[文章来源:ext4 js 模拟EventManager（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/53234970)


EventManager封装浏览器自带的事件 ，并且解决了浏览器的差异问题
```javascript
var MyExt = {};
// 封装浏览器自带的事件 屏蔽浏览器之间的差异
MyExt.EventManager = {
	
	//添加监听element, eventName, fn, useCapture
	addListener:function(el , ename , fn , useCapture){
		if(el.addEventListener){
			el.addEventListener(ename,fn,useCapture);
		} else if(el.attachEvent){
			el.attachEvent('on' + ename , fn);
		}
	},
	//移除监听
	removeListener:function(el , ename , fn , useCapture){
		if(el.removeEventListener){
			el.removeEventListener(ename,fn,useCapture);
		} else if(el.detachEvent){
			el.detachEvent('on' + ename , fn);
		}
	}
	//w3c === event    / ie  window.event
	//getEvent:function(){}
	//鼠标事件 
	//键盘事件
	//滚轮事件...
};

MyExt.EventManager.on = MyExt.EventManager.addListener;
MyExt.EventManager.un = MyExt.EventManager.removeListener;

window.onload = function(){
	var btn = document.getElementById('btn');
	MyExt.EventManager.on(btn,'click',function(){
		alert('我执行了..');
	}, false);
	MyExt.EventManager.on(btn,'click',function(){
		alert('我又执行了..');
	}, false);	
};
```