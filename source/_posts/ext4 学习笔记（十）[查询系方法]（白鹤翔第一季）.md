---
title: ext4 学习笔记（十）[查询系方法]（白鹤翔第一季）
date: 2016-11-08 22:57:42
tags: [ext]
---
[文章来源:ext4 学习笔记（十）[查询系方法]（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/53089615)


<br/>查询系方法：
<br/>contains：判断元素是否包含另一个元素
<br/>child：从元素的直接子元素中选择与选择符匹配的元素
<br/>down：选择与选择符匹配的元素的子元素
<br/>first：选择元素第一个子元素
<br/>findParent：查找与简单选择符匹配的元素的父元素
<br/>findParentNode、up：查找与简单选择符匹配的元素的父元素<!--more-->
<br/>is：判断元素是否匹配选择符
<br/>last：选择元素的最后一个子元素
<br/>next：选择元素同层的下一个元素
<br/>prew：选择元素同层的上一个元素
<br/>parent：返回元素的父元素
<br/>Ext.query：根据选择符获取元素
<br/>
<br/>Ext.select：根据选择符获取元素集合

```javascript
	Ext.create('Ext.panel.Panel',{
		title:'我的面板' , 
		width:'100%' , 
		height:400 ,
		renderTo:Ext.getBody(),
		html:'<div id=d1><span id=sp>我是sp的内容</span><div id=d2>我是d2的内容</div></div><input id=inp value=123 /><form id=f1><input name=uname value=bhx /><input name=pwd value=123 /></form>'
	});
	//查询系最常用的方法:
	//Ext.dom.Element   get  fly   getDom
	var d1 = Ext.get('d1');
	var sp = Ext.get('sp');
```

```javascript
	//1: contains：判断元素是否包含另一个元素
	console.info(d1.contains(sp));
	console.info(d1.contains('sp'));
	
	//2: child：从元素的直接子元素中选择与选择符匹配的元素 (返回的只是一个元素，并不能返回数组) ,2个参数 第二个参数是可选的 如果为true表示取得的是原生的HTMLElement元素
	var ch1 = d1.child('span');	//Ext.dom.Element
	console.info(ch.dom.innerHTML);
	var ch2 = d1.child('span',true);	//HTMLElement
	console.info(ch.innerHTML);
	
	//3: down：选择与选择符匹配的元素的子元素//findParentNode、up：查找与简单选择符匹配的元素的父元素
	var ch1 = d1.down('#d2');
	console.info(ch1.dom.innerHTML);
	
	//4: first：选择元素第一个子元素 //last：选择元素的最后一个子元素
	var f1 = d1.first('div');
	console.info(f1.dom.innerHTML);

	//5: findParent：查找与简单选择符匹配的元素的父元素 //parent：返回元素的父元素
	var parent =  sp.findParent('div');
	console.info(parent.innerHTML);
	
	//6: is：判断元素是否匹配选择符
	console.info(d1.is('div'));
	
	//7: next：选择元素同层的下一个元素 //prew：选择元素同层的上一个元素
	var next = sp.next();
	console.info(next.dom.nodeName);

	//8: Ext.query：根据选择符获取元素  (Ext.dom.Element.query)
	var arr = Ext.query('span','d1');	//HTMLElement[]
	Ext.Array.each(arr , function(item){
		console.info(item.innerHTML);
	});
	
	//9: Ext.select/Ext.dom.Element.select：根据选择符获取元素集合
	// 返回的都是元素集合: Ext.dom.CompositeElementLite(HTMLElemennt)/Ext.dom.CompositeElement(Ext.dom.Element)
	// 参数说明： 3个参数 ， 
	//	1：selector 选择器  (不要使用id选择器)
	//  2：返回的集合对象（boolean false:Ext.dom.CompositeElementLite true:Ext.dom.CompositeElement）
	//  3: 指定的根节点开始查找
	var list1 = Ext.select('span',false,'d1');//Ext.dom.CompositeElementLite
	Ext.Array.each(list1.elements,function(el){
			console.info(el.innerHTML);
	});
	var list2 = Ext.select('span',true,'d1');//Ext.dom.CompositeElement
	Ext.Array.each(list2.elements,function(el){
			console.info(el.dom.innerHTML);
	});	
	
```
这一小节笔记比较粗糙，全是代码，但是代码已经很详细；

如果你足够细心，那么你会看到 3 down方法时我里面加了一个/#号，在这里我要说明一下啊，这些查询系最好使用xtype,api文档中明确描述到不应该包含组件id,所以我在down方法里面用了/#id，这里也向大家说一下，平时最好不要这样用，因为这样虽然能能够得到我们想要的，但是及其不易扩展，还是按API所说用组件xtype。