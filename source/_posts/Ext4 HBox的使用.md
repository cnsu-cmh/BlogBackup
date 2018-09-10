---
title: Ext4 HBox的使用
date: 2016-11-22 22:50:28
tags: [ext]
---

要使用HBox布局方式，首先的熟悉下一下几个主要属性：

一、align：字符类型，指示组件在容器内的对齐方式。有如下几种属性。

1、top（默认）：排列于容器顶端。

2、middle：垂直居中排列于容器中。

3、stretch：垂直排列且拉伸义填补容器高度

4、stretchmax:垂直拉伸，并且组件以最高高度的组件为准。

二、flex:数字类型，指示组件在容器中水平呈现方式，通俗的讲，就是指示组件在容器中的相对宽度。
<!--more-->
三、pack : 字符类型，指示组件在容器的位置，有如下几种属性。

1、start（默认）：组件在容器左边

2、center：组件在容器中间

3、end：组件在容器的右边

实例代码：
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Insert title here</title> <!-- 引入EXT样式 -->
    <link href="../resources/ext-5.0.1-min/packages/ext-theme-crisp/build/resources/ext-theme-crisp-all.css"
          rel="stylesheet"/> <!-- 引入EXT all -->
    <script src="../resources/ext-5.0.1-min/ext-all.js"></script> <!-- 引入EXT 国际化-->
    <script src="../resources/ext-5.0.1-min/packages/ext-locale/ext-locale-zh_CN.js"></script>
</head>
<body>
<div id="d1"></div>
<div id="d2"></div>
<div id="d3"></div>
</body>
<script type="text/javascript"> Ext.onReady(function () {
    var d1 = Ext.create('Ext.Panel', {
        title: 'HBox 顶对齐，且组件在容器的左边',
        frame: true,
        width: 600,
        height: 100,
        items: [{
            anchor: '100%',
            layout: {type: 'hbox', padding: '10', pack: 'start', align: 'top'},
            defaults: {margins: '0 5 0 0'},
            items: [{xtype: 'button', text: 'Button 1'}, {xtype: 'button', text: 'Button 2'}, {
                xtype: 'button',
                text: 'Button 3'
            }, {xtype: 'button', text: 'Button 4'}]
        }]
    })
    d1.render('d1');
    var d2 = Ext.create('Ext.Panel', {
        title: 'HBox 垂直对齐，且组件在容器的右边',
        frame: true,
        width: 600,
        height: 100,
        items: [{
            anchor: '100%',
            layout: {type: 'hbox', padding: '10', align: 'middle', pack: 'end'},
            defaults: {margins: '0 5 0 0'},
            items: [{xtype: 'button', text: 'Button 1'}, {xtype: 'button', text: 'Button 2'}, {
                xtype: 'button',
                text: 'Button 3'
            }, {xtype: 'button', text: 'Button 4'}]
        }]
    })
    d2.render('d2');
    var d3 = Ext.create('Ext.Panel', {
        title: 'HBox 垂直水平居中，并且所有控件高度为最高控件的高度',
        frame: true,
        width: 600,
        height: 100,
        items: [{
            anchor: '100%',
            layout: {type: 'hbox', padding: '5', align: 'stretchmax', pack: 'center'},
            defaults: {margins: '0 5 0 0'},
            items: [{xtype: 'button', text: 'Small Size'}, {
                xtype: 'button',
                scale: 'medium',
                text: 'Medium Size'
            }, {xtype: 'button', scale: 'large', text: 'Large Size'}]
        }]
    })
    d3.render('d3');
}) </script>
</html>

```

