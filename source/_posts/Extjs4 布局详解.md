---
title: Extjs4 布局详解
date: 2016-11-21 22:53:19
tags: [ext]
---
[文章来源:Extjs4 布局详解](http://blog.csdn.net/u011229848/article/details/53266452)


**很抱歉，之前编辑的时候只是截图直接粘贴上的，编辑时能看到界面截图，后来无意发现图片没有了，如你阅读有不便之处还望原谅。**

****

**1 Fit 布局**

在 Fit布局中，子元素将自动填满整个父容器。注意：在 fit布局下，对其子元素设置 宽度是无效的。如果在 fit布局中放置了多个组件，则只会显示第一个子元素。典型的案例 就是当客户要求一个 window或 panel中放置一个 GRID组件，grid组件的大小会随着父容 器的大小改变而改变。 示例代码
```javascript
Ext.onReady(function(){
	Ext.create('Ext.panel.Panel', { 
		renderTo: Ext.getBody(), 
	    title: 'First Fit Panel', 
	    layout: 'fit', 
	    html:"fit撑满父元素",
	    height:300
	});
	
});

```
**2 Border 布局**

border布局：border布局也称边界布局，他将页面分隔为 west,east,south,north,center这 五个部分，我们需要在在其 items中指定使用 region参数为其子元素指定具体位置。 注意：north和 south部分只能设置高度（height），west和 east部分只能设置宽度 （width）。north south west east区域变大，center区域就变小了。 参数 split:true 可以调整除了 center四个区域的大小。 参数 collapsible:true 将激活折叠功能， title必须设置，因为折叠按钮是出现标题部分 的。 center 区域是必须使用的，而且 center 区域不允许折叠。Center区域会自动填充其 他区域的剩余空间。尤其在 Extjs4.0中，当指定布局为 border时，没有指定 center区域 时，会出现报错信息。 示例代码：

```javascript
Ext.onReady(function(){
	 Ext.create('Ext.panel.Panel', {
		 title:"border布局Panel",
		 width: 1024,           
		 height: 720,           
		 layout: 'border',           
		 items: [
		         {               
					 region: 'south',               
					 xtype: 'panel',               
					 height: 20,               
					 split: false,              
					 html: '欢迎登录!',              
					 margins: '0 5 5 5'           
			 	},
			 	{               
				 	title: 'West Region is collapsible',               
					 region:'west',               
					 xtype: 'panel',               
					 margins: '5 0 0 5',               
					 width: 200,               
					 collapsible: true,               
					 id: 'west-region-container',               
					 layout: 'fit'           
				 },
				 {               
					 title: 'Center Region',               
					 region: 'center',               
					 xtype: 'panel',               
					 layout: 'fit',               
					 margins: '5 5 0 0',               
					 html:'在Extjs4中，center区域必须指定，否则会报错。'           
				 }
			],           
		renderTo: Ext.getBody()       
	 });
 })
```

**3 Accordion 布局**

accordion布局：accordion布局也称手风琴布局，在 accordion布局下，在任何时间 里，只有一个面板处于激活状态。其中每个面边都支持展开和折叠。注意：只有 Ext.Panels 和所有 Ext.panel.Panel 子项，才可以使用 accordion布局。 示例代码：

```javascript
Ext.onReady(function(){
	 Ext.create('Ext.panel.Panel', {          
		 title: 'Accordion Layout',              
		 width: 300,               
		 height: 300,          
		 x:20,          
		 y:20,          
		 layout:'accordion',              
		 defaults: {              
			 bodyStyle: 'padding:15px'          
		 },          
		 layoutConfig: {                     
			 titleCollapse: false,                      
			 animate: true,                      
			 activeOnTop: true               
		 },              
		 items: [{                      
			 title: 'Panel 1',                      
			 html: 'Panel content!'               
		 },
		 {                      
			 title: 'Panel 2',                      
			 html: 'Panel content!'              
		 },
		 {              
			 title: 'Panel 3',                      
			 html: 'Panel content!'              
		 }],               
		renderTo: Ext.getBody()      
	})
}); 
```
****

**4 Card 布局**

Card布局：这种布局用来管理多个子组件，并且在任何时刻只能显示一个子组件。这 种布局最常用的情况是向导模式，也就是我们所说的分布提交。Card布局可以使用 layout:'card'来创建。注意：由于此布局本身不提供分步导航功能，所以需要用户自己开发 该功能。由于只有一个面板处于显示状态，那么在初始时，我们可以使用 setActiveItem功 能来指定某一个面板的显示。当要显示下一个面板或者上一个面板的时候，我们可以使用 getNext()或 getPrev()来得到下一个或上一个面板。然后使用 setDisabled方法来设置面板的 显示。另外，如果面板中显示的是 FORM布局，我们在点击下一个面板的时候，处理 FORM中提交的元素，通过 AJAX将表单中的内容保存到数据库中或者 SESSION中。 下面的示例代码展示了一个基本的 Card布局，布局中并没有包含 form元素，具体情 况，还要根据实际情况进行处理：

```javascript
Ext.onReady(function(){
	Ext.create('Ext.panel.Panel', {               
		title: 'Card布局示例',               
		width: 300,                
		height: 202,               
		layout: 'card',                
		activeItem: 0,           
		x:30,           
		y:60,           
		bodyStyle: 'padding:15px',               
		defaults: {border: false},           
		bbar: [{              
	        id: 'move-prev',                           
	        text: 'Back',                            
	        handler: function(btn) {                   
	        	navigate(btn.up("panel"), "prev");                           
	       	},               
       		disabled: true           
       	},           
       	'->',           
       	{               
       		id: 'move-next',                           
       		text: 'Next',                            
       		handler: function(btn) {                   
       			navigate(btn.up("panel"), "next");               
			}           
       	}],           
       	items: [{               
       		id: 'card-0',               
       		html: '<h1>Welcome to the Wizard!</h1><p>Step 1 of 3</p>'               
       		},           
       		{               
       			id: 'card-1',               
       			html: '<p>Step 2 of 3</p>'           
     		},           
     		{               
     			id: 'card-2',               
     			html: '<h1>Congratulations!</h1><p>Step 3 of 3 - Complete</p>'                
    		}],               
    	renderTo: Ext.getBody()       
	});
	
	 var navigate = function(panel, direction){                
		 var layout = panel.getLayout();               
		 layout[direction]();                
		 Ext.getCmp('move-prev').setDisabled(!layout.getPrev());                
		 Ext.getCmp('move-next').setDisabled(!layout.getNext());       
	};   
	
});
```
**5 Anchor 布局**

anchor布局将使组件固定于父容器的某一个位置，使用 anchor布局的子组件尺寸相对 于容器的尺寸，即父容器容器的大小发生变化时，使用 anchor布局的组件会根据规定的规 则重新渲染位置和大小。 AnchorLayout布局没有任何的直接配置选项（继承的除外），然而在使用 AnchorLayout布局时，其子组件都有一个 anchor属性，用来配置此子组件在父容器中所处 的位置。 anchor属性为一组字符串，可以使用百分比或者是-数字来表示。配置字符串使用空格 隔开，例如 anchor:'75% 25%'，表示宽度为父容器的 75%，高度为父容器的 25% anchor:'-300 -200'，表示组件相对于父容器右边距为 300，相对于父容器的底部位 200 anchor:'-250 20%'，混合模式，表示组件党对于如容器右边为 250，高度为父容器的 20% 示例代码
            
```javascript
Ext.onReady(function(){
	Ext.create('Ext.panel.Panel',{
		width:500,
		height:400,
		title:'Anchor布局',
		layout:'anchor',
		x:60,
		y:80,
		renderTo:Ext.getBody(),
		items:[{
				xtype:'panel',
				title:'75%---25%',
				anchor:'75% 25%'
			},
			{
				xtype:'panel',
				title:'-300---(-200)',
				anchor:'-300 -200'
			},
			{
				xtype:'panel',
				title:'-300---(25%)',
				anchor:'-200 25%'
			}
		]
	});
});
```

**6 Absolute 布局**

Absolute布局继承 Ext.layout.container.Anchor 布局方式，并增加了 X/Y配置选项对子 组件进行定位，Absolute布局的目的是为了扩展布局的属性，使得布局更容易使用。

```javascript
Ext.onReady(function(){
	Ext.create('Ext.form.Panel',{
		width:500,
		height:300,
		title:'absolute布局',
		layout:'absolute',
		x:60,
		y:80,
		items:[{
				xtype:'label',
				text:'name',
				x:10,
				y:10
			},
			{
				name:'name',
				anchor:'90% 9%',
				x:100,
				y:10
			},
			{
				xtype:'label',
				text:'password',
				x:10,
				y:60
			},
			{
				name:'PWD',
				anchor:'90% 23%',
				x:100,
				y:60
			},
			{
				y:120,
				x:10,
				xtype:'textareafield',
				name:'textareafield',
				anchor:'90% 25%'
			}
		],
		renderTo:Ext.getBody()
	});
});

```

**7 Column 布局**

****

Column布局一般被称为列布局，这种布局的目的是为了创建一个多列的格式。其中 每列的宽度，可以为其指定一个百分比或者是一个固定的宽度。Column布局支持一个 columnWidth属性，在布局过程中，使用 columnWidth指定每个面板的宽度。 注意：使用 Column布局布局时，其子面板的所有 columnWidth值加起来必须介于 0~1之间或者是所占百分比。他们的总和应该是 1。 另外，如果任何子面板没有指定 columnWidth值，那么它将占满剩余的空间。 示例代码：

```javascript
Ext.onReady(function(){
	Ext.create('Ext.panel.Panel',{
		width:500,
		height:400,
		title:'column布局',
		layout:'column',
		x:60,
		y:80,
		items:[
		       {
					title:'Column 1',
					columnWidth: .25,
					xtype:'panel'
				},
				{
					title:'Column 2',
					columnWidth: .55,
					xtype:'panel'
				},
				{
					title:'Column 3',
					columnWidth: .20,
					xtype:'panel'
				}
		 	],
		renderTo:Ext.getBody()
	});
});
```

```javascript
Ext.onReady(function(){
	Ext.create('Ext.panel.Panel',{
		width:500,
		height:400,
		title:'column布局',
		layout:'column',
		x:60,
		y:80,
		items:[
		       {
					title:'Column 1',
					width: '25%',
					xtype:'panel'
				},
				{
					title:'Column 2',
					width: '55%',
					xtype:'panel'
				},
				{
					title:'Column 3',
					width: '20%',
					xtype:'panel'
				}
		 	],
		renderTo:Ext.getBody()
	});
});
```

之前我是引入的ext5，故样式跟上不一样，在5里面Column布局直接使用width设置如下图，使用columnWidth如column实例图一，在4里面直接用width如上图
