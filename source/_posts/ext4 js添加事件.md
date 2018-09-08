---
title: ext4 js添加事件（白鹤翔第一季）
date: 2016-11-17 22:39:42
tags: [ext]
---
[文章来源:ext4 js添加事件（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/53207918)


<br/>原始的事件模型，也就是0级DOM事件模型。非常通俗的说，就是给HTML元素里加一个事件，这种方法有一个硬伤，就是不能同时对元素注册多个处理函数。
<br/>W3C的专家也知道这样不好，那么2级DOM事件模型也就产生了，也就是所谓的标准事件模型。API如下定义：
<br/>element.addEventListener(type,listener,useCapture);
<br/>element.removeEventListener(type,listener,useCapture);
<br/>W3C的专家把事件分成了3个阶段，也就是所谓的事件流：（1）捕获 （2）目标 （3）冒泡。
```javascript
// 1: 在早些时候,是不存在事件这个概念的,开发者用javascript2个函数去模拟事件的机制(window.setTimeout/window.setInterval)
// 2: 由于很多原因 比如效率太低, 人们开始发明了 最原始的 0级dom事件模型 为元素添加一个事件  在事件上绑定一个处理函数  
//    注意：这种模型有一个致命的硬伤：就是不能为元素的事件 添加多个处理函数```
 window.onload = function(){
 	var inp = document.createElement('input');
 	inp.id = 'inp1';
 	inp.value = '点击' ; 
 	inp.type = 'button' ; 
 	inp.onclick = function(){  //这里方法将被下面onclick事件覆盖
 		alert('执行了..');
 	};
    inp.onclick = function(){
 		//alert(this == window);
 		alert(this == inp);
 		alert('我也执行了..');
 	}; 
 	document.body.appendChild(inp);
 };
 
 function test1(){alert(this == window);};
 function test2(){alert(22);};

// 3: 在0级dom事件模型有了以后，才成立了 w3c，觉得0级事件模型 不好 退出了 2级事件模型  (标准dom事件模型)
//element.addEventListener(type,listener,useCapture);
//element.removeEventListener(type,listener,useCapture);
 window.onload = function(){
 	var inp = document.createElement('input');
 	inp.id = 'inp1';
 	inp.value = '点击' ; 
 	inp.type = 'button' ;
 	// type :事件类型    listener :这个事件的绑定函数    useCapure(boolean):(事件传播：true=捕获 / false=冒泡)
 	inp.addEventListener('click' ,test1 , false );
 	inp.removeEventListener('click' ,test1 , false );
 	inp.addEventListener('click' ,test2 , false );
 	document.body.appendChild(inp);
 	//IE浏览器 678事件用：attachEvent();  detachEvent();  IE9/IE10已经支持w3c的标准了
 	var inp = document.createElement('input');
 	inp.id = 'inp1';
 	inp.value = '点击' ; 
 	inp.type = 'button' ;	
 	inp.attachEvent('onclick' ,test1);
 	inp.detachEvent('onclick' ,test1);
 	inp.attachEvent('onclick' ,test2);
 	document.body.appendChild(inp);
 };
 
 
 
//  对于事件的传播机制： w3c：1 捕获    2 目标(命中)  3 冒泡
//  w3c 提供了一个关键字 event 事件对象  / ie: 6.7.8  window.event
 window.onload = function(){
 	
 	var inp = document.createElement('input');
 	inp.id = 'inp1';
 	inp.value = '点击';
 	inp.type = 'button';
 	inp.addEventListener('click',function(event){
 		alert('input执行...');
 		event.stopPropagation();	//阻止冒泡事件的发生
 	},false);
 	
 	var div = document.createElement('div');
 	div.addEventListener('click',function(){
 		alert('div执行...');
 	},false);
 	
 	document.body.addEventListener('click',function(){
 		alert('body执行...');
 	},false);
 	
 	div.appendChild(inp);
 	document.body.appendChild(div);
 
 };
 ```