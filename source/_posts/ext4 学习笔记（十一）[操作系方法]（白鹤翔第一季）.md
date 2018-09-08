---
title: ext4 学习笔记（十一）[操作系方法]（白鹤翔第一季）
date: 2016-11-10 13:51:26
tags: [ext]
---
[文章来源:ext4 学习笔记（十一）[操作系方法]（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/53114958)


DOM操作系方法：
appendTo：将当前元素追加到指定元素中
appendChild：在当前元素中追加元素
createChild：在元素中插入由DomHelper对象创建的元素ext
inertAfter：将元素插入到指定元素之后
inertBefore：将元素插入到指定元素之前
inertSibling：在当前元素前或后插入（或创建）元素（同层）。
insertHtml：在当前元素内插入HTML代码
remove：移除当前元素
replace：使用当前元素替换指定元素
replaceWith：使用创建的元素替换当前的元素

wrap：创建一个元素，并将当前元素包裹起来。

```javascript
//操作dom系的方法：

	//1:appendTo：将当前元素追加到指定元素中（这2个元素都必须存在document里）
	sp.appendTo(Ext.get('d2'));
	sp.appendTo('d2');
	//2:appendChild：在当前元素中追加元素
	sp.appendChild('d2');
	//3:createChild：在元素中插入由DomHelper对象创建的元素
	sp.createChild({
		tag:'ol' ,		//orderlist  unorderlist
		children:[
			{tag:'li' ,html:'item1'},
			{tag:'li' ,html:'item2'}
		]
	});
	
	//4:inertAfter：将元素插入到指定元素之后
	//5:inertBefore：将元素插入到指定元素之前 
	//6:inertSibling：在当前元素前或后插入（或创建）元素（同层）。
	//7:insertHtml：在当前元素内插入HTML代码
	//8:replace：使用当前元素替换指定元素
	//9:replaceWith：使用创建的元素替换当前的元素	
	//10:remove：移除当前元素
	
	sp.remove();
	//11:wrap：创建一个元素，并将当前元素包裹起来。
	sp.wrap('<h1></h1>');
```