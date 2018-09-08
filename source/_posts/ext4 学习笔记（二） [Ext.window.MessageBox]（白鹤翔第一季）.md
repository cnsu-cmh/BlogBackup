---
title: ext4 学习笔记（二） [Ext.window.MessageBox]（白鹤翔第一季）
date: 2016-10-31 23:38:10
tags: [ext]
---
[文章来源:ext4 学习笔记（二） [Ext.window.MessageBox]（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/52988728)


<BR/>首先说明一下：
<BR/>
<BR/>（1）本文章是针对于ExtJs 4.X ，文章中出现的5版本只是我引入的文件是ExtJs.5.0,并不是文章是基于5版本，文章是4版本的
<BR/>（2）由于注释很全，所以文章内容就不写那么详细了，直接贴代码还望读者能够理解
<BR/>
<BR/>Ext.onReady：这个方法是Ext的准备函数，也就是Ext相关的代码都会在这个函数里书写，它比较类似于window的onload方法，但是注意其执行时机是在页面的DOM对象加载完毕之后立即执行。
<BR/>Ext.window.MessageBox：Ext.MessageBox是一个工具类，提供了各种风格的信息提示对话框，也可以简写为Ext.Msg,这在Ext中很常见,很多组件或类都可以使用简写形式。
<BR/>
<BR/>下面是关于Ext.window.MessageBox 的几个简单小组件实例。
<BR/>
<BR/>alert，confirm，prompt，wait，show方法
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

  //Ext.onReady 准备函数 类似于window.onload
    Ext.onReady(function(){
    	
     	//提示信息
    	Ext.MessageBox.alert('我是标题!' , 'Hello World!' , function(){
    		console.info(this);
    		alert('我是回调函数!');
    	} , this);
     	//这里充分说明 Ext.Msg.alert 不是浏览器的alert  只是一个DIV
    	Ext.Msg.alert('提示信息','ExtJS!!!');
    	alert("我来了，前面的Ext.Msg.alert()你是冒牌的alert");

    	
    	//询问确认框
    	Ext.Msg.confirm('提示信息','确认删除该条记录么?',function(op){
    			// yes  on
    			if(op == 'yes'){
    				alert('确认了!');
    			} else {
    				alert('取消了!');
    			}
    	});


    	//输入框
    	//String title, String msg, [Function fn], [Object scope], [Boolean/Number multiline], [String value]
    	Ext.Msg.prompt('我是标题!','请输入姓名:' , function(op , val){
    		//op  ok cancel
    		console.info(op);
    		console.info(val);
    	} , this , true , '张三');

    	//等待框 
    	Ext.Msg.wait('提示信息','我是标题' ,{
    		   interval: 1000, 			//循环定时的间隔
    		   duration: 20000,			//总时长
    		   increment: 5,			//执行进度条的次数
    		   text: 'w...',	//进度条上的文字
    		   scope: this,
    		   fn: function(){
    				alert('更新成功!');
    		   },
    		   animate:true			//更新渲染时提供一个渐进动画效果
    	}); 
    	
    	
    	
   		/* var progressBar = new Ext.ProgressBar({
   			width: 800,
   			renderTo: Ext.getBody()
   		});*/ 
   	
   		//此方式于上一种方式都是创建，  4.X推荐使用Ext.create()方式
    	var progressBar = Ext.create('Ext.ProgressBar', {
    	   renderTo: Ext.getBody(),
    	   width: 800
    	});
   		
   		progressBar.wait({
   			duration: 10000,
   			interval: 1000,
   			increment: 10,
   			text: 'waiting...',
   			scope: this,
   			fn: function() {
   				Ext.MessageBox.alert('Information', '更新完毕');
   			},
   			animate:true
   		}); 

    		
    	//show方法
    	Ext.Msg.show({
    		title:'我是自定义的提示框!!' , 
    		msg:'我是内容!!' , 
    		width:300 , 
    		height:300 ,
    		buttons:Ext.Msg.YESNOCANCEL ,
    		icon:Ext.Msg.WARNING		// ERROR INFO QUESTION  WARNING
    	}); 
    });
    </script>
</html>
```
<BR/>文章中充分说明 Ext 的MessageBox 是封装的一个div
<BR/>文中等待框Ext.Msg.wait（）在实例中配置失效，出现效果但是动画于时间，次数配置失败，在4.1.1下正常，文章中我引入的是5.0的包，具体暂时还没弄清楚原因，建议使用下一种方式，renderTo 可以自己定义div，如果您知道原因还希望指点，谢谢。O(∩_∩)O