---
title: Ext4 vBox的使用
date: 2016-11-22 22:53:00
tags: [ext]
---

[文章来源:Ext4 vBox的使用](https://blog.csdn.net/u011229848/article/details/53293043)


要使用VBox布局方式，首先的熟悉下一下几个主要属性：

一、align：字符类型，指示组件在容器内的对齐方式。有如下几种属性。

1、left（默认）：排列于容器左侧。

2、center ：控件在容器水平居中。

3、stretch：控件横向拉伸至容器大小

4、stretchmax:控件横向拉伸，宽度为最宽控件的宽。

二、flex:数字类型，指示组件在容器中水平呈现方式，通俗的讲，就是指示组件在容器中的相对宽度。

三、pack : 字符类型，指示组件在容器的位置，有如下几种属性。

1、start（默认）：组件在容器上边

2、center：组件在容器中间

3、end：组件在容器的下边

HTML代码：
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
<body></body>
<script type="text/javascript"> Ext.onReady(function () {
    var currentName;
    var replace = function (config, name) {
        var btns = Ext.getCmp('btns');
        if (name && name != currentName) {
            currentName = name;
            btns.remove(0);
            btns.add(Ext.apply(config));
        }
    };
    var viewport = Ext.create('Ext.Viewport', {
        layout: 'border',
        items: [{
            id: 'btns',
            region: 'west',
            baseCls: 'x-plain',
            split: true,
            width: 150,
            minWidth: 100,
            maxWidth: 250,
            layout: 'fit',
            margins: '5 0 5 5',
            items: {baseCls: 'x-plain', html: '<p style="padding:10px;color:/#556677;font-size:11px;">点击右边的按钮查看效果</p>'}
        }, {
            region: 'center',
            margins: '5 5 5 0',
            layout: 'anchor',
            items: [{
                anchor: '100%',
                baseCls: 'x-plain',
                layout: {type: 'hbox', padding: 10},
                defaults: {margins: '0 5 0 0', pressed: false, toggleGroup: 'btns', allowDepress: false},
                items: [{
                    xtype: 'button', text: 'Spaced / Align: left', handler: function () {
                        replace({
                            layout: {type: 'vbox', padding: '5', align: 'left'},
                            defaults: {margins: '0 0 5 0'},
                            items: [{xtype: 'button', text: 'Button 1'}, {xtype: 'tbspacer', flex: 1}, {
                                xtype: 'button',
                                text: 'Button 2'
                            }, {xtype: 'button', text: 'Button 3'}, {xtype: 'button', text: 'Button 4', margins: '0'}]
                        }, 'spaced');
                    }
                }, {
                    xtype: 'button', text: 'Multi-Spaced / Align: left', handler: function () {
                        replace({
                            layout: {type: 'vbox', padding: '5', align: 'left'},
                            defaults: {margins: '0 0 5 0'},
                            items: [{xtype: 'button', text: 'Button 1'}, {xtype: 'tbspacer', flex: 1}, {
                                xtype: 'button',
                                text: 'Button 2'
                            }, {xtype: 'tbspacer', flex: 1}, {xtype: 'button', text: 'Button 3'}, {
                                xtype: 'tbspacer',
                                flex: 1
                            }, {xtype: 'button', text: 'Button 4', margins: '0'}]
                        }, 'multi spaced - align left');
                    }
                }, {
                    xtype: 'button', text: 'Align: left', handler: function () {
                        replace({
                            layout: {type: 'vbox', padding: '5', align: 'left'},
                            defaults: {margins: '0 0 5 0'},
                            items: [{xtype: 'button', text: 'Button 1'}, {
                                xtype: 'button',
                                text: 'Button 2'
                            }, {xtype: 'button', text: 'Button 3'}, {xtype: 'button', text: 'Button 4'}]
                        }, 'align left');
                    }
                }, {
                    xtype: 'button', text: 'Align: center', handler: function () {
                        replace({
                            layout: {type: 'vbox', padding: '5', align: 'center'},
                            defaults: {margins: '0 0 5 0'},
                            items: [{xtype: 'button', text: 'Button 1'}, {
                                xtype: 'button',
                                text: 'Button 2'
                            }, {xtype: 'button', text: 'Button 3'}, {xtype: 'button', text: 'Button 4'}]
                        }, 'align center');
                    }
                }, {
                    xtype: 'button', text: 'Align: stretch', handler: function () {
                        replace({
                            layout: {type: 'vbox', padding: '5', align: 'stretch'},
                            defaults: {margins: '0 0 5 0'},
                            items: [{xtype: 'button', text: 'Button 1'}, {
                                xtype: 'button',
                                text: 'Button 2'
                            }, {xtype: 'button', text: 'Button 3'}, {xtype: 'button', text: 'Button 4'}]
                        }, 'align stretch');
                    }
                }, {
                    xtype: 'button', text: 'Align: stretchmax', handler: function () {
                        replace({
                            layout: {type: 'vbox', padding: '5', align: 'stretchmax'},
                            defaults: {margins: '0 0 5 0'},
                            items: [{xtype: 'button', text: 'Jamie'}, {
                                xtype: 'button',
                                text: 'Aaron'
                            }, {xtype: 'button', text: 'Tommy'}, {xtype: 'button', text: 'Ed '}]
                        }, 'align stretchmax');
                    }
                }]
            }, {
                anchor: '100%',
                baseCls: 'x-plain',
                layout: {type: 'hbox', padding: '0 10 10'},
                defaults: {margins: '0 5 0 0', pressed: false, toggleGroup: 'btns', allowDepress: false},
                items: [{
                    xtype: 'button', text: 'Flex: Even / Align: center', handler: function () {
                        replace({
                            layout: {type: 'vbox', padding: '5', align: 'center'},
                            defaults: {margins: '0 0 5 0'},
                            items: [{xtype: 'button', text: 'Button 1', flex: 1}, {
                                xtype: 'button',
                                text: 'Button 2',
                                flex: 1
                            }, {xtype: 'button', text: 'Button 3', flex: 1}, {
                                xtype: 'button',
                                text: 'Button 4',
                                flex: 1,
                                margins: '0'
                            }]
                        }, 'align flex even');
                    }
                }, {
                    xtype: 'button', text: 'Flex: Ratio / Align: center', handler: function () {
                        replace({
                            layout: {type: 'vbox', padding: '5', align: 'center'},
                            defaults: {margins: '0 0 5 0'},
                            items: [{xtype: 'button', text: 'Button 1', flex: 1}, {
                                xtype: 'button',
                                text: 'Button 2',
                                flex: 1
                            }, {xtype: 'button', text: 'Button 3', flex: 1}, {
                                xtype: 'button',
                                text: 'Button 4',
                                flex: 3,
                                margins: '0'
                            }]
                        }, 'align flex ratio');
                    }
                }, {
                    xtype: 'button', text: 'Flex + Stretch', handler: function () {
                        replace({
                            layout: {type: 'vbox', padding: '5', align: 'stretch'},
                            defaults: {margins: '0 0 5 0'},
                            items: [{xtype: 'button', text: 'Button 1', flex: 1}, {
                                xtype: 'button',
                                text: 'Button 2',
                                flex: 1
                            }, {xtype: 'button', text: 'Button 3', flex: 1}, {
                                xtype: 'button',
                                text: 'Button 4',
                                flex: 3,
                                margins: '0'
                            }]
                        }, 'align flex + stretch');
                    }
                }, {
                    xtype: 'button', text: 'Pack: start / Align: center', handler: function () {
                        replace({
                            layout: {type: 'vbox', padding: '5', pack: 'start', align: 'center'},
                            defaults: {margins: '0 0 5 0'},
                            items: [{xtype: 'button', text: 'Button 1'}, {
                                xtype: 'button',
                                text: 'Button 2'
                            }, {xtype: 'button', text: 'Button 3'}, {xtype: 'button', text: 'Button 4'}]
                        }, 'align pack start + align center');
                    }
                }, {
                    xtype: 'button', text: 'Pack: center / Align: center', handler: function () {
                        replace({
                            layout: {type: 'vbox', padding: '5', pack: 'center', align: 'center'},
                            defaults: {margins: '0 0 5 0'},
                            items: [{xtype: 'button', text: 'Button 1'}, {
                                xtype: 'button',
                                text: 'Button 2'
                            }, {xtype: 'button', text: 'Button 3'}, {xtype: 'button', text: 'Button 4', margins: '0'}]
                        }, 'align pack center + align center');
                    }
                }, {
                    xtype: 'button', text: 'Pack: end / Align: center', handler: function () {
                        replace({
                            layout: {type: 'vbox', padding: '5', pack: 'end', align: 'center'},
                            defaults: {margins: '0 0 5 0'},
                            items: [{xtype: 'button', text: 'Button 1'}, {
                                xtype: 'button',
                                text: 'Button 2'
                            }, {xtype: 'button', text: 'Button 3'}, {xtype: 'button', text: 'Button 4', margins: '0'}]
                        }, 'align pack end + align center');
                    }
                }]
            }]
        }]
    });
}); </script>
</html>

```
