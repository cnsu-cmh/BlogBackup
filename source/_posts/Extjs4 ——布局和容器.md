---
title: Extjs4 ——布局和容器
date: 2016-11-20 23:14:02
tags: [ext]
---
[文章来源:Extjs4 ——布局和容器](http://blog.csdn.net/u011229848/article/details/53247752)


extjs4.0布局及容器系统(Layouts and Containers)是Ext JS中最强大的部分之一。它负责控制你应用程序中每个组件的尺寸和定位。本文内容包括了如何运用布局的基础。

extjs4.0布局和容器（Layouts and Containers）是Ext JS中最强大的部分之一。它负责控制你应用程序中每个组件的尺寸和定位。本文内容包括了如何运用布局的基础。

**一、容器**

一个Ext JS应用程序的图形用户界面(UI)是由许多组件（查看组件指南([Components Guide](http://dev.sencha.com/deploy/ext-4.0.7-gpl/docs/index.html#/guide/components))获取更多关于组件的信息）构成的。容器是一种可以容纳一些其他组件的特殊类型组件。
<!--more-->
典型的Ext JS应用程序是由一些嵌套的组件构成不同的层来组成的。如下：

![]()

最通用的容器就是[Panel](http://dev.sencha.com/deploy/ext-4.0.7-gpl/docs/index.html#!/api/Ext.panel.Panel)。让我们看下如何创建一个容器以允许一个Panel包含其他的组件：

```javascript
Ext.onReady(function(){
	Ext.create('Ext.panel.Panel', { 
	    renderTo: Ext.getBody(), 
	    width: 400, 
	    height: 300, 
	    title: 'Container Panel', 
	    items: [ 
	        { 
	            xtype: 'panel', 
	            title: 'Child Panel 1', 
	            height: 100, 
	            width: '75%' 
	        }, 
	        { 
	            xtype: 'panel', 
	            title: 'Child Panel 2', 
	            height: 100, 
	            width: '75%' 
	        } 
	    ] 
	});
});
```
 
我们刚刚创建了一个Panel，并把它渲染到DOM的body中，然后我们使用[items](http://dev.sencha.com/deploy/ext-4.0.7-gpl/docs/index.html#!/api/Ext.container.Container-cfg-items)配置项添加两个子Panel到我们的Panel容器中。
 **二、布局** 每个容器都拥有一个布局来管理它子组件的尺寸和定位。 下面我们将讨论如何使用指定类型的布局来配置容器，以及布局系统是如何使每个组件都保持协调的。 使用布局：上面例子中我们没有为Panel容器指定布局。可以看到子Panel是一个接着一个被放置的，正如DOM中标准的块元素一样。 我们刚刚创建了一个Panel，并把它渲染到DOM的body中，然后我们使用[items](http://dev.sencha.com/deploy/ext-4.0.7-gpl/docs/index.html#!/api/Ext.container.Container-cfg-items)配置项添加两个子Panel到我们的Panel容器中。   之所以这样是因为所有容器的默认布局为自动布局。自动布局并不为子元素指定任何特殊的定位和尺寸规则。 例如，我们假定想让两个子Panel并排放置，并且每个占据父容器宽度的50%，我们可以简单 地使用一个列布局([Column Layout](http://dev.sencha.com/deploy/ext-4.0.7-gpl/docs/index.html#!/api/Ext.layout.container.Column))，通过在父容器中配置layout选项来实现。  
Ext.onReady(function(){ Ext.create('Ext.panel.Panel', { renderTo: Ext.getBody(), width: 400, height: 200, title: 'Container Panel', layout: 'column', items: [ { xtype: 'panel', title: 'Child Panel 1', height: 100, width: '50%' }, { xtype: 'panel', title: 'Child Panel 2', height: 100, width: '50%' } ] }); });
   Ext JS拥有一整套可用的布局，几乎容纳了你能想象到的任何一种布局。可以查看布局的例子(Layout Examples)以了解那些布局是可行的。  **布局系统是如何工作的呢？** 父容器的布局负责其所有子元素的初始定位和尺寸大小。框架内部调用了容器的[doLayout](http://dev.sencha.com/deploy/ext-4.0.7-gpl/docs/index.html#!/api/Ext.container.Container-method-doLayout)方法， 该方法触发布局为父容器的所有子元素计算得到正确的尺寸和定位并更新DOM。doLayout方法 是递归调用的，所以父容器的任何子元素同样也将会调用它们的doLayout方法。这种调用将持续到到达组件层次的末端。通常你一般不会在你的应用程序代码中调用doLayout方法，因为框架已为你调用了。 父容器的改变尺寸时，或当添加或删除子组件的items时，重新布局将被触发。通常，我们仅依赖框架为我们管理以更新布局，但是有时我们想阻止框架自动更新布局，这样我们一次可以批量 地处理多个操作，完成时，我们手动地触发布局。 为了到达这个目的，我们在容器中使用延缓布局([suspendLayout)](http://dev.sencha.com/deploy/ext-4.0.7-gpl/docs/index.html#!/api/Ext.container.Container-cfg-suspendLayout)标志以阻止它自动布局，当我们执行那些通常会触发布局的操作时（例如添加或移除items）。 当我们做完这些操作时，我们必须把延缓布局标志关闭，并且手动地调用容器的doLayout方法以触发布局：  
```javascript
Ext.onReady(function(){
	Ext.create('Ext.panel.Panel', { 
	    renderTo: Ext.getBody(), 
	    width: 400, 
	    height: 200, 
	    title: 'Container Panel', 
	    layout: 'column', 
	    items: [ 
	        { 
	            xtype: 'panel', 
	            title: 'Child Panel 1', 
	            height: 100, 
	            width: '50%' 
	        }, 
	        { 
	            xtype: 'panel', 
	            title: 'Child Panel 2', 
	            height: 100, 
	            width: '50%' 
	        } 
	    ] 
	}); 
});
```
  **组件布局** 如容器的布局定义了一个容器如何设定尺寸和定位它的组件items，组件的布局同样也定义了 其如何设定尺寸和定位它内部的子items。 组件的布局通过使用组件布局([componentLayout](http://dev.sencha.com/deploy/ext-4.0.7-gpl/docs/index.html#!/api/Ext.container.Container))来设置配置选项。一般你不需要使用该配置项，除非你正在编写一个自定义的组件，因为所有提供的组件其内部元素的大小调整和定位都拥有它们自己的布局管理器。 大多数组件使用自动布局([Auto Layout](http://dev.sencha.com/deploy/ext-4.0.7-gpl/docs/index.html#!/api/Ext.layout.container.Auto))，但是更多复杂的组件需要自定义的组件布局方式（例如一个有header，footer，toolbars的Panel）

关于更多详细Ext布局请参考 [EXT4布局详解](http://blog.csdn.net/u011229848/article/details/53266452)