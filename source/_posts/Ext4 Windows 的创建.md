---
title: Ext4 Windows 的创建
date: 2016-11-21 23:26:51
tags: [ext]
---
[文章来源:Ext4 Windows 的创建](http://blog.csdn.net/u011229848/article/details/53268721)

Extjs4,创建 Ext组件有了新的方式，就是 Ext.create(....)，而且可以使用动态加载 JS的 方式来加快组件的渲染，我们再也不必一次加载已经达到 1MB的 ext-all.js了，本文介绍如 何在 EXTJS4中创建一个 window.
```javascript

<!DOCTYPE html >  
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>窗口实例</title>
<!-- 引入EXT样式  -->
<link href="../resources/ext-5.0.1-min/packages/ext-theme-crisp/build/resources/ext-theme-crisp-all.css" rel="stylesheet" />
<!-- 引入EXT all -->
<script src="../resources/ext-5.0.1-min/ext-all.js"></script>
<!-- 引入EXT  国际化-->
<script src="../resources/ext-5.0.1-min/packages/ext-locale/ext-locale-zh_CN.js"></script>
<script type="text/jscript">  
	Ext.onReady(function(){    
		Ext.create('Ext.window.Window',{      
			width:400,      
			height:230,      //X,Y标识窗口相对于父窗口的偏移值。      
			x:10,      
			y:10,      
			plain: true,      //指示标题头的位置,分别为 top,bottom,left,right,默认为top      
			headerPosition: 'left',      
			title: 'ExtJS4 Window的创建，头在左'    
		}).show();        
	
		Ext.create('Ext.window.Window',{      
			width:400,      
			height:230,      
			x:500,      
			y:10,      
			plain: true,      //指示标题头的位置,分别为 top,bottom,left,right,默认为top      
			headerPosition: 'right',      
			title: 'ExtJS4 Window的创建，头在右'    
		}).show();        
		
		Ext.create('Ext.window.Window',{      
			width:400,      
			height:230,      
			x:10,      
			y:300,      
			plain: true,      //指示标题头的位置,分别为 top,bottom,left,right,默认为top      
			headerPosition: 'bottom',      
			title: 'ExtJS4 Window的创建，头在底'    
		}).show();    
		
		var win = Ext.create('Ext.window.Window',{      
			width:400,      
			height:230,      
			x:500,      
			y:300,      
			plain: true,      //指示标题头的位置,分别为 top,bottom,left,right,默认为top      
			headerPosition: 'top',      
			title: 'ExtJS4 Window的创建'    
		});    
		win.show();  
	});  
	
</script>  
</head>  
<body> 

</body>  
</html>
```

很多书籍对于4版本创建的时候用的不是Ext.window.Window,而是直接用的Ext.Window，这里个人建议还是带着包名，写成Ext.window.Window,因为有些浏览器有时候很奇怪，用Ext.Window会导致创建失败。
关于window的更多配置请看 [Ext4学习笔记Ext.window.Window](http://blog.csdn.net/u011229848/article/details/53002802)