---
title: window.open页面关闭后刷新父页面
date: 2016-11-21 13:41:16
tags: [java]
---
[文章来源:window.open页面关闭后刷新父页面](http://blog.csdn.net/u011229848/article/details/53258712)


经常会遇到这么一个问题，window.open打开一个页面，但是页面操作完关闭的时候，父页面因为没有获取到关闭而不刷新，下面给大家介绍一个封装的方法：
```javascript
/** 
 * 监听打开的弹窗，关闭后刷新页面 
 */  
function openWin(url,text,winInfo){  
    var winObj = window.open(url,text,winInfo);  
    var loop = setInterval(function() {       
        if(winObj.closed) {      
            clearInterval(loop);      
            //alert('closed');      
            parent.location.reload();   
        }      
    }, 1);     
}  
```
 再开的时候就可以用openWin()方法了。