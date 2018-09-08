---
title: ext4 学习笔记（八）[动态加载Js]（白鹤翔第一季）
date: 2016-11-07 22:38:36
tags: [ext]
---
[文章来源:ext4 学习笔记（八）[动态加载Js]（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/53073246)


首先回顾一下Ext创建类，我们先创建一个window.Window组件
```javascript
    Ext.create('Ext.window.Window',{
		title:'我是一个窗口',
		height:300 , 
		width:400 ,
		constrain:true , 
		modal:true , 
		html:'我是窗体的内容!!!!' , 
		renderTo:Ext.getBody()
	}).show();
```
接下来我们自定义一个类： 
```javascript
    Ext.define('MyWindow' , {
		extend:'Ext.window.Window' , //继承Ext的window类
		title:'我是一个窗口',
		height:300 , 
		width:400 ,
		constrain:true , 
		modal:true , 
		html:'我是窗体的内容!!!!' , 
		renderTo:Ext.getBody()		
	});
```
我们这个时候创建调用自定义类：
```javascript
    var w1 = Ext.create('MyWindow');
	var w2 = Ext.create('MyWindow',{
		title:'我是w2'
	});
	var w3 = Ext.create('MyWindow',{
		itle:'我是w3'
	});
	w1.show();
	w2.show();
	w3.show();
```

大家可以看到我们调用自己定义的这个MyWindow很方便，但是如果这个MyWindow想用在整个项目，是不是我们需要把这个js单独提出，正常方式下引入这个js就可以，但是这样如果js很多，而且项目都加载也很庞大，所以我们来看一下Ext给我们提供的一种只有调用这个组件时才加载这个js的方式：
第一步：在js/extjs/添加文件夹 (ux)，在这个ux文件夹下 建立自己的组件所对应的js文件
第二步：在js/extjs/ux下编写自己的扩展的组件
 注意：这里路径是从项目根目录开始算，而且创建的类名一定要严格遵循包名层次路径编写   
```javascript
//define的类名，一点要严格按照包层次路径去编写
Ext.define('js.extjs.ux.MyWindow',{
		extend:'Ext.window.Window' , //继承Ext的window类
		title:'我是动态加载进来的组件',
		height:300 , 
		width:400 ,
		constrain:true , 
		modal:true , 
		html:'我是窗体的内容!!!!' , 
		renderTo:Ext.getBody()	
});
```
第三步：启用ext动态加载的机制 并设置要加载的路径 paths可配置多个
```javascript
    Ext.Loader.setConfig({
		enabled:true ,
		paths:{
			myux:'js/extjs/ux'
		}
	});
```

第四步：创建类的实例并使用：
```javascript
Ext.create('js.extjs.ux.MyWindow').show();
```
在这里我们如果卟使用这个类，js/extjs/ux下的MyWindow.js文件不会被加载哦 这就是Ext提供的动态加载js方式，
需要注意的地方就是一定要按照包层次路径编写。

好了，这个比较简单，以后还会涉及，现在就暂时先到这里吧。