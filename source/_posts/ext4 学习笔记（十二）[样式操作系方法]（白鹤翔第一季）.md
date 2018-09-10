---
title: ext4 学习笔记（十二）[样式操作系方法]（白鹤翔第一季）
date: 2016-11-10 14:03:28
tags: [ext]
---
[文章来源:ext4 学习笔记（十二）[样式操作系方法]（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/53115087)


<br/>**样式操作系方法：**
<br/>addCls：增加CSS样式到元素，重复的样式会自动过滤
<br/>applyStyles：设置元素的style属性
<br/>setStyle：为元素设置样式
<br/>getStyle：返回元素的当前样式和计算样式
<br/>getStyleSize：返回元素的样式尺寸
<br/>setOpacity：设置不透明度
<br/>clearOpacity：；清理不透明度设置
<br/>getColor：返回CSS颜色属性的值，返回值为6位数组的16进制颜色值
<!--more-->
<br/>boxWrap：使用box.Markup定义的HTML代码包装元素
<br/>
<br/>addClsOnClick添加样式当点击该元素的时候
<br/>addClsOnOver添加样式当鼠标移动到元素上的时候
<br/>getMargin：返回值具有top、left、right、bottom属性的对象，属性值为响应的元素margin值。
<br/>removeCls：删除元素的样式
<br/>replaceCls：替换元素的样式
<br/>set：设置元素属性
<br/>radioCls：为当前元素添加样式，并删除其兄弟节点的元素
<br/>unituzeBox：将表示margin大小的对象转换为字符串
```javascript
	<span style="font-family:KaiTi_GB2312;">//操作样式系的方法：
	//1：addCls：增加CSS样式到元素，重复的样式会自动过滤
	sp.addCls('red');
	//2:applyStyles：设置元素的style属性
	sp.applyStyles('backgroundColor:blue');
	sp.applyStyles({backgroundColor:'yellow'});
	//3:setStyle：为元素设置样式
	sp.setStyle('backgroundColor','green');
	sp.setStyle('fontSize','40px');
	//4:getStyle：返回元素的当前样式和计算样式
	alert(sp.getStyle('fontSize'));
	alert(Ext.encode(sp.getStyle(['fontSize','backgroundColor'])));
	//5:getStyleSize：返回元素的样式尺寸
	alert(Ext.encode(sp.getStyleSize()));
	//6:setOpacity：设置不透明度
	var d2 = Ext.get('d2');
	d2.setStyle('backgroundColor','red');
	d2.setStyle('width','200px');
	d2.setStyle('height','200px');
	d2.setOpacity(.3);	// 0~1
	//7:addClsOnClick添加样式当点击该元素的时候
	var d2 = Ext.get('d2');
	d2.addClsOnClick('red');
	//8:addClsOnOver添加样式当鼠标移动到元素上的时候
	var d2 = Ext.get('d2');
	d2.addClsOnOver('red');	
	//9:getMargin：返回值具有top、left、right、bottom属性的对象，属性值为响应的元素margin值。
	var d2 = Ext.get('d2');
	alert(d2.getMargin('b')); //r l t b
	alert(Ext.encode(d2.getMargin()));
	//10:removeCls：删除元素的样式	
	var d2 = Ext.get('d2');
	d2.addCls('red');			//String/String[] className
	d2.removeCls('red');		//String/String[] className
	
	//11:尺寸、定位
	var d2 = Ext.get('d2');
	alert(Ext.encode(d2.getSize()));
	alert(d2.getX());
	alert(Ext.encode(d2.getXY()));
	sp.moveTo(100,100);</span>

```

<br/>**对齐操作系方法：**
<br/>alignTo：将当前元素对齐到另外一个元素。定位位置的选择是基于所对齐的元素的位置（9个定位点tl、t、tr、l、c、r、bl、b、br）。
<br/>anchorTo：当窗口调整大小时，将当前元素锚到指定元素并重新调整
<br/>removeAnchor：移除当前元素的任何锚定位
<br/>
<br/>**尺寸大小操作系方法：**
<br/>setHeight：设置元素宽度
<br/>setWidth：设置元素高度
<br/>setSize：设置元素大小
<br/>cilp：存储元素当前的overflow设置并裁剪溢出。
<br/>unlip：在clip被调用前将裁剪值（溢出）还原为原始值
<br/>getDocumentWidth：返回文档宽度
<br/>getDocumentHeight：返回文档高度
<br/>getFrameWidth：返回合计了padding和border的宽度
<br/>getHeight：返回offsetHeight值
<br/>getWidth：返回offsetWidth值
<br/>getPadding：返回padding的宽度
<br/>getSize：返回元素的大小
<br/>
<br/>getTextWidth：返回文本宽度
<br/>getViewportHeight：返回窗口的可视高度
<br/>getViewportWidth：返回窗口的可视宽度
<br/>getViewSize：返回元素可以用来放置内容的区域大小
<br/>getBorderWidth：返回边界宽度
<br/>getComputedWidth：返回计算出来的CSS宽度
<br/>getComputedHeight：返回计算出来的CSS高度
<br/>isBorderBox：主要用于检测盒子模型，与IE6、7有关
<br/>
<br/>**定位系方法：**
<br/>clearPositioning：当文档加载完成后，清理定位回到默认值
<br/>fromPoint：返回在建瓯的自拍呢的顶层元素
<br/>getBottom：返回右下角的Y坐标
<br/>getBox：返回一个包含元素位置的对象，对象包括元素左上角的坐标值、右下角的坐标值、宽度和高度。
<br/>getCenterXY：返回元素的当前坐标
<br/>getLeft：返回一个包含元素位置的对象
<br/>getPositioning：返回一个包含CSS位置属性的对象
<br/>getRegin：返回元素所在区域
<br/>getRight：返回元素的右边X坐标
<br/>getTop：返回元素顶部Y坐标
<br/>getViewRegion：返回元素的内容区域
<br/>getX：返回元素当前的X坐标
<br/>getY：返回元素当前的Y坐标
<br/>
<br/>getXY：返回元素当前的XY坐标
<br/>move：移动元素
<br/>moveTo：将元素移动到指定的XY坐标上
<br/>position：初始化元素的位置
<br/>setBottom：设置元素的bottom样式
<br/>setBounds：设置元素的位置和大小
<br/>setBox：设置元素的位置大小
<br/>setLeft：设置元素坐标的X坐标
<br/>setRight：设置元素right的样式值
<br/>setLeftTop：设置元素左上角坐标
<br/>setLocation：设置元素位置
<br/>setTop：设置元素的顶部Y坐标
<br/>setX、setY、setXY：设置元素的X、Y、XY坐标位置
<br/>translatePoints：转换元素的页面坐标为CSS的left和top值
<br/>
<br/>**滚动系方法：**
<br/>getScroll：返回元素当前滚动条的位置
<br/>isScrollable：如果元素允许滚动，则返回true
<br/>scroll：滚动到指定位置
<br/>scrollIntoView：将元素滚动到指定容器的可视区域
<br/>scrollTo：将元素滚动到指定的位置