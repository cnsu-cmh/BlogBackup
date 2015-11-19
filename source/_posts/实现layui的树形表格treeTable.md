---
title: 实现layui的树形表格treeTable
date: 2018-08-18 20:28:07
tags: [layui]
---
[文章来源:实现layui的树形表格treeTable](http://blog.csdn.net/u011229848/article/details/81812479)


实现layui的树形表格treeTable，layui中有 可以使用 'tree'，'table' , 进而使用layui.treeGird实现
```javascript
 layui.treeGird({
        elem: '#demo', //传入元素选择器
        nodes: data,//返回json数据，组装为tree结构
        layout: layout //table显示列定义
    });
```
今天推荐一个对对layui数据表格进行扩展[https://github.com/whvcse/treetable-lay](https://github.com/whvcse/treetable-lay)，作者资料写的已经非常完整，实例也很完整，具体就不细说了，重点是不需要专门组装数据，只需查询出list即可。