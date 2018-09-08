---
title: ext4 学习笔记（九）[dom操作一 获取元素]（白鹤翔第一季）
date: 2016-11-07 22:56:41
tags: [ext]
---
[文章来源:ext4 学习笔记（九）[dom操作一 获取元素]（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/53073861)



<BR/>首先，什么是DOM（Document Object Model）
<BR/>W3C对DOM的定义：文档对象模型是一个平台，一个中立于语言的应用程序编程接口（API）,允许程序访问并更改文档的内容、结构和样式。
<BR/>其实DOM是一种通用的模型、不止在我们的HTML中存在，也可以在其他文件中存在，相信你最熟悉的就是XML了吧，其实还有很多...
<BR/>DOM的发展也非常的漫长，版本延续，产生了0级DOM、1级DOM、2级DOM和最新的3级DOM，那么相对成熟的就是从2级DOM以后了。每一次版本更新都有非常实用的变化。
<BR/>节点Node，对于nodeType、nodeName、nodeValue、getAttribute等等一些对加点的定义你一定不会陌生。
<BR/>
<BR/>对于DOM模型的操作，相信一个个熟悉又可爱的名字你一定知道：
<BR/>DOM的访问
<BR/>document.getElementById、document.getElementsByTagName、document.getElementsByName、innerHTML、innerText等等一些方式去访问DOM元素
<BR/>DOM的CRUD
<BR/>createElement、parentNode、childNodes、appendChild、removeChild、replaceChild、inertBefore、firstChild、previousSibling等等一系列操作DOM的方式
<BR/>对于DOM的样式
<BR/>我也相信你非常的了解，只需要给节点添加一个style属性，我们就可以操作该节点的样式，或者触发事件改变样式，又或者根据需求操作DOM变换不同的动画效果等等，这都离不开style属性。
<BR/>
<BR/>好了，对于旧时代的DOM我们暂且放在一边、那么Ext的出现，让以上这些对于DOM的概念变得简单、实用。
<BR/>Ext之DOM
<BR/>Ext中使用了三个核心的工具类对我们掌握的DOM进行了完美的封装，OK，请记住他们的名字:
<BR/>Ext.Element（几乎对DOM的一切进行了彻底封装）
<BR/>Ext.DomHelper（嗯，他是一个强大的操控UI界面的工具类）
<BR/>Ext.DomQuery（用来进行DOM节点查询）
<BR/>
<BR/>Ext.Element常用方法：
<BR/>如果你深深迷恋着Ext，那么你一定知道Ext.Element这个类，4.x版本由于进行了底层的重构，从而让我看到了更加简洁清晰的代码，那就是这个js文件：AbstractElement.js，他里面有俩个顶顶大名的函数，让开发者再次感叹Ext底层的强大。他们就是Ext.get和Ext.fly。嗯！请记住他们的名字！！一个方法使用了缓存机制来提升获取DOM节点的效率，而另一个方法则使用了javascript经典的‘享元模式’来提升效率，从而节约内存，更加低碳化。
<BR/>Ext.get （Ext.Element.get）
<BR/>Ext.fly（Ext.Element.fly）
<BR/>Ext.getDom
<BR/>
<BR/>Ext.dom.Element
<BR/>Ext.get 使用了缓存机制来提升获取DOM节点的效率 Ext.Element
<BR/>首先get方法的描述:
<BR/>//*/*
<BR/>/* 1 首先去Ext.cache缓存里去查找 ，如果缓存里有，直接返回即可
<BR/>/* 2 如果缓存里没有 ，那再去页面上去查找 ， 如果页面里没有，返回null
<BR/>/* 3 如果页面里有，把当前内容加入到缓存里： { id : {data/events/dom} }
<BR/>/* 4 Ext.addCacheEntry加到缓存里的方法
<BR/>/*/
   ```javascript
    var d1 = Ext.get('d1');//Ext.Element 
    console.info(d1.dom.innerHTML);//获取到id为d1的html值
```
<BR/>Ext.fly
<BR/>
<BR/>//*/*
<BR/>/* fly:使用了javascript经典的‘享元模式’来提升效率，从而节约内存，更加低碳化
<BR/>/* 返回的对象：Fly对象 ，当然你可以理解成为返回的就是Ext封装好的Ext.Element对象
<BR/>/*注意点：fly由于内部使用了享元模式 所以 只适合一次操作 ，从而节省内存
<BR/>/*/
```javascript
    var d2 = Ext.fly('d2'); 
    d2.dom.innerHTML = 'AAA'; 
    var d3 = Ext.fly('d3'); 
    d3.dom.innerHTML = 'BBB'; 
    var d2 = Ext.fly('d2'); 
    var d3 = Ext.fly('d3'); 
    d2.dom.innerHTML = 'AAA'; //此时这行代码无效，fly里面没有d2,因为享元模式只适合一次操作， 
                                //第二次进去之后key值是一样的，将d3赋值覆盖了之前的fly, 
    d3.dom.innerHTML = 'BBB';
```
<BR/>Ext.getDom
<BR/>//*/*
<BR/>/* 直接从页面上获取元素的DOM元素
<BR/>/*/

```javascript
    var dom = Ext.getDom('d3'); //HTMLElement dom.innerHTML="CCCC";
```

<BR/>
<BR/>直接获取操作其dom
<BR/>**获取元素的总结：**
<BR/>Ext.get比较消耗内存，尽量避免使用。
<BR/>Ext.fly虽然比较省内存，但是只能被使用一次。
<BR/>Ext.getDom非常适合直接获取页面元素，并返回的就是DOM元素，如果你想操作DOM元素的属性，那这个方法是最好不过的咯
<BR/>
<BR/>对于dom的获取就简单的介绍这些吧，希望看文档的你给予批评指教，谢谢！