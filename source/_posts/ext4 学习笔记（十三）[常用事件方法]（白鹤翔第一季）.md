---
title: ext4 学习笔记（十三）[常用事件方法]（白鹤翔第一季）
date: 2016-11-15 23:26:24
tags: [ext]
---
[文章来源:ext4 学习笔记（十三）[常用事件方法]（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/53179924)


<br/>常用事件方法：
<br/>addKepMap：为元素创建一个KeyMap对象
<br/>addKeyListener：为KeyMap绑定事件
<br/>常用事件
<br/>on：绑定事件
<br/>un：移除事件
<br/>click：单机事件
<br/>blur：失去焦点事件
<br/>focus：获得焦点事件
<br/>
<br/>其他方法：
<br/>center：使元素居中
<br/>clean：清理空白的文本节点
<br/>createShim：为元素创建一个iframe垫片保证选择或其他对象跨域时可见
<br/>getLoader：返回ElementLoader对象
<br/>highlight 高亮显示特效
<br/>show 、hide显示隐藏元素
<br/>ghost 元素移动特效
<br/>fadeIn、fadeOout淡入淡出
<br/>slideIn、slideOut向上向下滑动
<br/>
<br/>getValue：如果元素有value属性，返回其值
<br/>normalize：将CSS属性中的连接符号去掉，例如将“font-size”转为fontSize这样。
<br/>load：直接调用ElementLoader的load方法为元素加载内容
<br/>mask：遮罩当前元素，屏蔽用户操作。
<br/>unmask：移除遮罩
<br/>repaint：强迫浏览器重新绘画元素
<br/>serializeForm：序列化为URL编码的字符串
<br/>update：更新元素的innerHTML属性
<br/>unselectable：禁用文本选择
```javascript
	//一：为元素添加事件
	//1 : addKepMap：为元素创建一个KeyMap对象
	var inp = Ext.get('inp');
	inp.addKeyMap({		//Ext.util.KeyMap ====>Class
		key:Ext.EventObject.A ,			//Ext.EventObject 
		ctrl:true ,
		fn:function(){
			console.info('按ctrl+A ，执行!!');
		} , 
		scope:this
	});
	//2 : addKeyListener：为KeyMap绑定事件
	//参数说明： String/Number/Number[]/Object key, Function fn, [Object scope]
	var inp = Ext.get('inp');
	inp.addKeyListener({
		key:Ext.EventObject.X ,	
		ctrl:false 
	},
	function(){
		console.info('x执行了..');
	},
	this);

	//二：元素绑定常用事件
	var inp = Ext.get('inp');
	inp.on('click',function(){
		console.info('执行了...');
	});
	inp.un('click');
	inp.focus();
	
	
	//三:其他重要且常用的方法：
	var inp = Ext.get('inp');
	var sp = Ext.get('sp');
	//1: center：使元素居中
	inp.center('d1');
	//2: clean：清理空白的文本节点
	//3: createShim：为元素创建一个iframe垫片保证选择或其他对象跨域时可见
	//4: getLoader：返回ElementLoader对象//11: load：直接调用ElementLoader的load方法为元素加载内容
	var loader = inp.getLoader(); //ElementLoader
	loader.load({
		url:'base/004_base06_dom2_loader.jsp' , 
		renderer:function(loader ,response){
			//把对象转换成字符串表示形式：Ext.encode 
			//把一个字符串转换成javascript对象： Ext.decode
			var obj = Ext.decode(response.responseText);
			Ext.getDom('inp').value = obj.name ;
		}
	});
	//5: highlight 高亮显示特效
	sp.highlight();
	
	//6: show 、hide显示隐藏元素  	//fadeIn、fadeOout淡入淡出
		var d2 = Ext.get('d2');
		d2.setStyle('width','100px');
		d2.setStyle('height','100px');
		d2.setStyle('backgroundColor','red');
		d2.show({duration: 2000});
		d2.hide({duration: 2000});
	
	//7: ghost  元素移动特效
		d2.ghost('b', { duration: 2000 }); // r/b/l/t
	
	//8: slideIn、slideOut向上向下滑动
	d2.slideIn('b',{duration: 2000});
	d2.slideOut('r',{duration: 2000});
	
	//9: getValue：如果元素有value属性，返回其值
	console.info(inp.getValue());
	
	//10: normalize：将CSS属性中的连接符号去掉，例如将“font-size”转为fontSize这样。
	
	//11 :mask：遮罩当前元素，屏蔽用户操作。 unmask：移除遮罩
	Ext.getBody().mask('请稍等..');
	//	window.setTimeout(function(){
	//		Ext.getBody().unmask();
	//	},2000);
	Ext.defer(function(){
		Ext.getBody().unmask();
	},2000);
	
	//12: repaint：强迫浏览器重新绘画元素
	
	//13: serializeForm：序列化为URL编码的字符串
	console.info(Ext.dom.Element.serializeForm('f1'));
	
	//14: update：更新元素的innerHTML属性
	
	//15: unselectable：禁用文本选择	
	inp.unselectable();
```