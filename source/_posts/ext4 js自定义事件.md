---
title: ext4 js自定义事件
date: 2016-11-17 23:13:54
tags: [ext]
---

[文章来源:ext4 js自定义事件（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/53208240)


<br/>// 要利用观察者模式 去实现自定义的事件
<br/>//1：由于浏览器他自己能定义内置的事件(click/blur...)
<br/>// 我们也应该有一个类似于浏览器这样的类，这个类 自己去内部定义一些事件(自定义事件)
<br/>var Observable = function(){
<br/>//承装自己所定义的事件类型的
<br/>this.events = ['start','stop'];
<br/>//我们应该设计一种数据类型，这种数据类型就可以去维护自定义事件类型 和 和相关绑定函数的关系,结构如下所示：
<br/>//'start':[fn1 ,fn2....] ,
<br/>//'stop':[fn1,fn2]
<br/>this.listeners = {
<br/>};
<br/>};
<br/>//2:添加新的自定义事件类型：
<br/>Observable.prototype.addEvents = function(eventname){
<br/>this.events.push(eventname);
<br/>};
<br/>//3:为自己的事件类型绑定响应的函数(添加事件监听)
<br/>Observable.prototype.addListener = function(eventname,fn){
<br/>//做一个容错的处理
<br/>if(this.events.indexOf(eventname) == -1){
<br/>this.addEvents(eventname);
<br/>}
<br/>//到这一步 ，必然存在这个事件类型了
<br/>var arr = this.listeners[eventname];
<br/>//如果当前这个函数数组不存在，那么我们要为这个事件类型绑定新添加的函数
<br/>if(!arr){
<br/>arr = [fn];
<br/>} else { //如果存在 当前这个事件类型所对应的函数的数组不为空
<br/>if(arr.indexOf(fn) == -1){
<br/>arr.push(fn);
<br/>}
<br/>}
<br/>//重新维护一下事件类型 和所绑定的函数数组的关联关系
<br/>this.listeners[eventname] = arr ;
<br/>};
<br/>//4:移除事件监听
<br/>Observable.prototype.removeListener = function(eventname,fn){
<br/>//如果你要移除的事件类型，在我的对象里没有被定义
<br/>if(this.events.indexOf(eventname) == -1){
<br/>return ;
<br/>}
<br/>//到这一步 就是你要移除的事件类型 是我当前对象里面存在的
<br/>var arr = this.listeners[eventname];
<br/>if(!arr){
<br/>return ;
<br/>}
<br/>//到这一步 证明arr里面是有绑定函数的
<br/>//判断 如果当前fn函数 在我的函数数组里存着 就移除
<br/>if(arr.indexOf(fn) != -1){
<br/>arr.splice(arr.indexOf(fn),1);
<br/>}
<br/>};
<br/>//5:如何让事件触发: 就是调用 这个事件类型所对应的所有的函数执行即可
<br/>Observable.prototype.fireEvent = function(eventname){
<br/>//如果当前没有传递事件类型名称或者当前传递的事件类型不存在我的对象里，直接返回
<br/>if(!eventname || (this.events.indexOf(eventname) == -1)){
<br/>return ;
<br/>}
<br/>//到这一步 一定存在这个事件
<br/>var arr = this.listeners[eventname];
<br/>if(!arr){
<br/>return ;
<br/>}
<br/>for(var i = 0 , len = arr.length ; i < len ; i ++){
<br/>var fn = arr[i];
<br/>fn.call(fn,this);
<br/>}
<br/>};
<br/>//javascript的习惯给原型对象的方法 起一个简单的名字 方便开发者去使用
<br/>Observable.prototype.on = Observable.prototype.addListener;
<br/>Observable.prototype.un = Observable.prototype.removeListener;
<br/>Observable.prototype.fr = Observable.prototype.fireEvent;
<br/>//Observable 浏览器：
<br/>var ob = new Observable(); //被观察者
<br/>// 子类 继承Observable//观察者
<br/>var fn1 = function(){
<br/>alert('fn1....');
<br/>};
<br/>ob.on('start',fn1);
<br/>var fn2 = function(){
<br/>alert('fn2....');
<br/>};
<br/>ob.on('start',fn2);
<br/>//移除监听
<br/>ob.un('start',fn1);
<br/>ob.fr('start');
<br/>//ob.fr('stop');
<br/>ob.on('run',function(){
<br/>alert('run....');
<br/>});
<br/>ob.fr('run');
<br/>//Ext.util.Observable 类 是为了为开发者提供一个自定义事件的接口
<br/>//Ext.util.Observable
<br/>//观察者模式：(报社、订阅者) 被观察者、观察者
<br/>//Ext.util.Observable 被观察者
<br/>//所有继承(混入)Ext.util.Observable类的对象(子类) 观察者