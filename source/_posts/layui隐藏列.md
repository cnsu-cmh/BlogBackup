---
title: layui隐藏列
date: 2018-08-17 18:09:33
tags: [layui]
---
[文章来源:layui隐藏列](http://blog.csdn.net//u011229848//article/details/81781542)


layui数据表格中隐藏列方式，如果直接在 {field: 'id', title: 'ID', <font color="red">style:'display:none;'</font>},导致thead中th中ID仍然在，

![image](layui隐藏列/20180817180344707.png)我们要做的是表格头中ID也需要隐藏。

、

官网简单demo如下：
```javascript
layui.use('table', function(){
  var table = layui.table;
  
  table.render({
    elem: '#test'
    ,url:'/demo/table/user/'
    ,cellMinWidth: 80 //全局定义常规单元格的最小宽度，layui 2.2.1 新增
    ,cols: [[
      {field:'id', width:80, title: 'ID', sort: true}
      ,{field:'username', width:80, title: '用户名'}
      ,{field:'sex', width:80, title: '性别', sort: true}
      ,{field:'city', width:80, title: '城市'}
      ,{field:'sign', title: '签名', width: '30%', minWidth: 100} //minWidth：局部定义当前单元格的最小宽度，layui 2.2.1 新增
      ,{field:'experience', title: '积分', sort: true}
      ,{field:'score', title: '评分', sort: true}
      ,{field:'classify', title: '职业'}
      ,{field:'wealth', width:137, title: '财富', sort: true}
    ]]
  });
});
```
添加一行配置即可：
```javascript
done: function () { 
    $("[data-field='id']").css('display','none');
}
```

```javascript
layui.use('table', function(){
  var table = layui.table;
  
  table.render({
    elem: '#test'
    ,url:'/demo/table/user/'
    ,cellMinWidth: 80 //全局定义常规单元格的最小宽度，layui 2.2.1 新增
    ,cols: [[
      {field:'id', width:80, title: 'ID', sort: true}
      ,{field:'username', width:80, title: '用户名'}
      ,{field:'sex', width:80, title: '性别', sort: true}
      ,{field:'city', width:80, title: '城市'}
      ,{field:'sign', title: '签名', width: '30%', minWidth: 100} //minWidth：局部定义当前单元格的最小宽度，layui 2.2.1 新增
      ,{field:'experience', title: '积分', sort: true}
      ,{field:'score', title: '评分', sort: true}
      ,{field:'classify', title: '职业'}
      ,{field:'wealth', width:137, title: '财富', sort: true}
    ]]
    ,done: function () {
            $("[data-field='id']").css('display','none');
        }
  });
});
```
效果如下：

![image](layui隐藏列/20180817180803609.png)