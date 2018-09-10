---
title: ext4 学习笔记(三) Ext.window.Window（白鹤翔第一季）
date: 2016-11-02 00:26:36
tags: [ext]
---
[文章来源:ext4 学习笔记(三) Ext.window.Window（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/53002802)


<br/>Ext.window.Window。对于组件，也就是Ext最吸引开发者的地方，那么我们要真正的使用Ext的组件，首先必须学会阅读API文档。
<br/>xtype：组件的别名 Hierarchy 层次结构 Inherited mixins 混入的类 Requires 该组件需要使用的类
<br/>configs：组件的配置信息 properties：组件的属性methods：组件的方法events：组件的事件
<br/>window组件常用属性和方法讲解:
<br/>configs:<!--more-->
<br/>constrain:布尔值，true为限制窗口只能在其容器内移动，默认值为false，允许窗口在任何位置移动。(另：constrianHeader属性)
<br/>modal:布尔值，true为设置模态窗口。默认为false
<br/>plain：布尔值，true为窗口设置透明背景。false则为正常背景，默认为false
<br/>x、y 设置窗口左上角坐标位置。
<br/>onEsc：复写onEsc函数，默认情况下按Esc键会关闭窗口。
<br/>closeAction：string值，默认值为'destroy'，可以设置'hide'关闭时隐藏窗口
<br/>autoScroll：布尔值，是否需要滚动条，默认false
```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	<button id='btn'>点击弹窗</button>
</body>
<!-- 引入EXT样式  -->
<link href="resources/ext-5.0.1-min/packages/ext-theme-crisp/build/resources/ext-theme-crisp-all.css" rel="stylesheet" />
<!-- 引入EXT all -->
<script src="resources/ext-5.0.1-min/ext-all.js"></script>
<!-- 引入EXT  国际化-->
<script src="resources/ext-5.0.1-min/packages/ext-locale/ext-locale-zh_CN.js"></script>
<script type="text/javascript">

    Ext.onReady(function(){

    	/*实例001：点击一个按钮 ,打开一个新的窗体 window重复创建的问题  */
    	//第一种实现
    	//JQuery code: var btn = $('#btn'); var dombtn = btn.get(0);
      	var btn = Ext.get('btn');		//这个元素是经过Ext包装的一个Ext的Dom对象//alert(btn.dom.value);
    	btn.on('click',function(){
    		if(!Ext.getCmp('mywin')){
    			Ext.create('Ext.window.Window',{
    				id:'mywin' ,		//如果你给组件加了一个id  那么这个组件就会被Ext所管理
    				title:'新窗体' , 
    				height:300 ,
    				width:400 ,
    				renderTo:Ext.getBody() //,
    				//modal:true
    			}).show();		
    		}
    	});  
    	
    	//第二种实现
    	/* var win = Ext.create('Ext.window.Window',{
    				title:'新窗体' , 
    				height:300 ,
    				width:400 ,
    				renderTo:Ext.getBody() ,
    				closeAction:'hide'  //closeAction默认是destroy 
    	});
	
    	Ext.get('btn').on('click',function(){
    			win.show();
    	});  */
    	
    	
    	
    	
    	/* 实例002 : 在组件中添加子组件  ,并进行一系列针对于组件的操作 */
    	
    	//在组件中添加子组件：
     	var win = new Ext.window.Window({
     		id:'myWinItems' ,
    		title:"组件,tbar实例" , 
    		width:'40%' ,
    		height:400 , 
    		renderTo:Ext.getBody() ,
    		draggable:false , 	//不允许拖拽
    		resizable:false , 	//不允许改变窗口大小
    		closable:false, 	//不显示关闭按钮
    		collapsible:true ,	//显示折叠按钮
    		bodyStyle: 'background:#ffc; padding:10px;' , // 设置样式
    		html:'我是window的内容!!' ,
    		//Ext items(array) 配置子组件的配置项
    		items:[{
    			//Ext的组件 给我们提供了一个简单的写法	 xtype属性去创建组件
    			xtype:'panel',
    			width:'100%',
    			height:100 ,
    			html:'我是面板'
    		},
    		new Ext.button.Button({
    			text:'我是按钮' , 
    			handler:function(){
    				Ext.Msg.alert('执行!!');
    			}
    		}),
    		{
    			xtype:'button' , 
    			text:'我是按钮2',
    			handler:function(btn){
    				Ext.Msg.alert('我被点击了');
    				console.info(btn.text);
    			}
    		} 
    		],
    		//表示在当前组件的top位置添加一个工具条
    		tbar:[{			//bbar(bottom) lbar(leftbar)  rbar(rightbar)  fbar(footbar)
    			text:'按钮1' ,
    			handler:function(btn){
    				//组件都会有 up、down 这两个方法(表示向上、或者向下查找) 需要的参数是组件的xtype或者是选择器
    				Ext.Msg.alert(btn.up('window').title);
    			}
    		},{
    			text:'按钮2' , 
    			handler:function(btn){
    				//最常用的方式
    				Ext.Msg.alert(Ext.getCmp('myWinItems').title);
    			}
    		},{
    			text:'按钮3' ,
    			handler:function(btn){
    				//以上一级组件的形式去查找 OwnerCt
    				console.info(btn.ownerCt);
    				Ext.Msg.alert(btn.ownerCt.ownerCt.title);
    			}			
    		}]
    		
    	});
    	win.show();
    	
    });
</script>
</html>
```
<br/>注意：
<br/>（1）在实例1中 要注意的是重复创建，每点一次按钮就创建一个window 第一种方式就是每次点击按钮的时候判断是否存在窗口，存在我就不创建；第二种方式是先把window创建出来，之后点击按钮把时调用window的show()方法显示，但是要注意关闭时默认方式销毁，我们要将其配置成hide,否则下次调用show方法时候已经销毁找不到window而报错；对于第二种方法，窗口过多的时候每次关闭只是hide对内存来说是极度不好的，所以建议使用第一种方法。
<br/>（2）对于实例2，主要是以查找组件为中心，主要是下面三个方法：ownerCt、up/down方法、Ext.getCmp方法（推荐使用）
<br/>说明：
<br/>（1）本文章是针对于ExtJs 4.X ，文章中出现的5版本只是我引入的文件是ExtJs.5.0,并不是文章是基于5版本，文章是4版本的
<br/>（2）由于注释很全，所以文章内容就不写那么详细了，直接贴代码还望读者能够理解
<br/>好了就到这吧，希望对于看文档的你能有所帮助！