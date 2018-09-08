---
title: ext4 学习笔记（四） [Ext.windowGroup]（白鹤翔第一季）
date: 2016-11-02 00:34:18
tags: [ext]
---
[文章来源:ext4 学习笔记（四） [Ext.windowGroup]（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/53002900)

windowGroup对象 操作window组

重点分析：该实例主要目的针对于特殊需求进行具体的实现，利用windowGroup去操作多个窗体同步执行某些任务，这有点类似于javascript里的组合模式，原理就是上级负责执行一个动作但并不真正去执行，而是分别传递给所有的下级组件去具体执行。

```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
</body>
<!-- 引入EXT样式  -->
<link href="resources/ext-5.0.1-min/packages/ext-theme-crisp/build/resources/ext-theme-crisp-all.css" rel="stylesheet" />
<!-- 引入EXT all -->
<script src="resources/ext-5.0.1-min/ext-all.js"></script>
<!-- 引入EXT  国际化-->
<script src="resources/ext-5.0.1-min/packages/ext-locale/ext-locale-zh_CN.js"></script>
<script type="text/javascript">

    Ext.onReady(function(){

    	var wingroup = new Ext.WindowGroup();
    	for(var i = 1 ; i <=5;i++){
    		var win = Ext.create('Ext.Window',{
    			title:'第' + i + '个窗口' , 
    			id:'win_' + i , 
    			width:300 , 
    			height:300 ,
    			renderTo:Ext.getBody()
    		});
    		win.show();
    		wingroup.register(win);		//把窗体对象注册给ZindexManger
    	}
    	
    	var btn1 = Ext.create('Ext.button.Button',{
    		text:'全部隐藏' , 
    		renderTo:Ext.getBody(),
    		handler:function(){
    			wingroup.hideAll();		//隐藏所有被管理起来的window组件
    		}
    	});
    	
    	var btn2 = new Ext.button.Button({
    		text:'全部显示' , 
    		renderTo:Ext.getBody(),
    		handler:function(){
    			wingroup.each(function(cmp){
    				cmp.show();
    			});
    		}		
    	});
    	
    	var btn3 = new Ext.button.Button({
    		text:'把第三个窗口显示在最前端' , 
    		renderTo:Ext.getBody(),
    		handler:function(){
    			wingroup.bringToFront('win_3'); //把当前的组件显示到最前端
    		}		
    	});	
    	
    	
    	var btn4 = new Ext.button.Button({
    		text:'把第五个窗口显示在最末端' , 
    		renderTo:Ext.getBody(),
    		handler:function(){
    			wingroup.sendToBack('win_5');	//把当前的组件显示到最后
    		}		
    	});	
    });
</script>
</html>
```
本文章主要注意register，hideAll，each 方法 详细笔记在代码中已经注释 ，这里就不再重复，望谅解。

说明：

（1）本文章是针对于ExtJs 4.X ，文章中出现的5版本只是我引入的文件是ExtJs.5.0,并不是文章是基于5版本，文章是4版本的
（2）由于注释很全，所以文章内容就不写那么详细了，直接贴代码还望读者能够理解
好了就到这吧，希望对于看文档的你能有所帮助！