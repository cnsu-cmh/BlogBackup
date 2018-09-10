---
title: ext4 学习笔记（七）[ExtJS扩展原生Javascript]（白鹤翔第一季）
date: 2016-11-05 23:46:35
tags: [ext]
---
[文章来源:ext4 学习笔记（七）[ExtJS扩展原生Javascript]（白鹤翔第一季）](http://blog.csdn.net/u011229848/article/details/53048487)



Ext对于原生的javascript对象进行了一系列的扩展，我们把他们掌握好，更能深刻的体会Ext的架构，从而对我们的web开发更好的服务，源码位置，我们可以从开发包的extjs-4.1.1\src\core\src\lang\这个位置找到这几个扩展的js源码：
<!--more-->
<br/>Ext.Object
<br/>Ext.Number
<br/>Ext.String
<br/>Ext.Array
<br/>Ext.Function
<br/>Ext.Date
<br/>Ext.Error
<br/>
<br/>下面我们就来看一下常用的扩展类里面的常用方法：
<br/>
<br/>Ext.Object
<br/>
<br/>1:chain 把当前传入的对象 当成新创建对象的原型
<br/>
```javascript
    var obj = {
    	name:'bsjxt',
    	age:10
    };
    var result = Ext.Object.chain(obj);
    alert(result.name); //'bsjxt'
    alert(result.age); //10
    alert(result.hasOwnProperty('name')); //false 说明是对象的原型，并不是他的属性
```
<br/>2:each 变量当前对象 然后毁掉函数中暴露出三个属性 key、value、self 如果回调函数返回false则停止迭代
```javascript
var obj = {
	name:'张三' , 
	age:20 ,
	sex:'男'
};
Ext.Object.each(obj,function(key , value , self){
	alert(key + ' : ' + value); //name.age,sex都alert
});
	
Ext.Object.each(obj,function(key , value , self){
	alert(key + ' : ' + value);//只alert name
	if(age == 20){
		return false ;
	}
});
```
<br/>3:fromQueryString将字符串转换成对象 这里字符串格式一定是 $ = 拼接的
```javascript
var str = "name=bjsxt&age=10";
var obj = Ext.Object.fromQueryString(str);
console.info(Ext.encode(obj)); // {"name":"bjsxt","age":"10"}
```
4 :toQueryString 将对象换成字符串转
```javascript
var obj = {
	name:'张三' , 
	age:20
};
var str = Ext.Object.toQueryString(obj);
console.info(str); 
```
<br/>5 :toQueryObjects 将一个
<br/>
<br/>name
<br/>-
<br/>
<br/>value
<br/>对转换为一个对象数组，支持内部结构的转换，对构造查询字符串非常有用
```javascript
var objects = Ext.Object.toQueryObjects('hobbies', ['reading', 'cooking', 'swimming']);

// objects此时等于:
[
    { name: 'hobbies', value: 'reading' },
    { name: 'hobbies', value: 'cooking' },
    { name: 'hobbies', value: 'swimming' },
];

var objects = Ext.Object.toQueryObjects('dateOfBirth', {
    day: 3,
    month: 8,
    year: 1987,
    extra: {
        hour: 4
        minute: 30
    }
}, true); // 递归

// objects此时等于:
[
    { name: 'dateOfBirth[day]', value: 3 },
    { name: 'dateOfBirth[month]', value: 8 },
    { name: 'dateOfBirth[year]', value: 1987 },
    { name: 'dateOfBirth[extra][hour]', value: 4 },
    { name: 'dateOfBirth[extra][minute]', value: 30 },
];
```
<br/>Ext.Number
<br/>
<br/>[1.constrain]()(Number number, Number min, Number max ) : Number
<br/>
<br/>检查给定的数值是否在约束的范围内。 如果再范围内就返回此数值。否则，如果大于最大值则返回最大值，如果小于最小值则返回最小值。注意本方法不改变给定的数值本身。
<br/>
<br/>2.[randomInt]()(Number from, Number to ) : Number
<br/>
<br/>在from与to之间的随机一个int数字
<br/>
<br/>3.[toFixed]()(Number value, Number precision )
<br/>
<br/>将vslue四舍五入精确到precision位
```javascript
console.info(Ext.Number.constrain(21, 10 , 20)); //20
console.info(Ext.Number.constrain(5, 10 , 20)); //10
console.info(Ext.Number.randomInt(1,100));  //1-100随机
console.info(Ext.Number.toFixed(3.1415926,5)); //3.14159
```
<br/>Ext.String
<br/>
<br/>1.capitalize 字符串首字母大写
<br/>
<br/>2.ellipsis 字符串直显示n位，n包含三个点，其余字符串省略，用点表示
```javascript
console.info(Ext.String.capitalize('xiaoshu')); //Xiaoshu
console.info(Ext.String.ellipsis('www.bjsxt.com',8)); //www.b...
	
```
<br/>Ext.Array
<br/>
<br/>[1：clean]()( Array array ) : Array
<br/>
<br/>过滤掉数组里的空值，空值的定义见 [Ext.isEmpty]()
```javascript
	var arr = [1,2,null,3,''];
	console.info(Ext.Array.clean(arr)); ///[1,2,3]
```
<br/>2:difference返回 A-B的差异集合，从A中减去所有B中存在的元素 ...
```javascript
	var arr1 = [1,2,3];
	var arr2 = [2,5,6];
	console.info(Ext.Array.difference(arr1,arr2)); //[1,3]
```
<br/>3:each
```javascript
var arr = [1,2,3,4];
	Ext.Array.each(arr,  function(item){
			if(item == 4){
				return false ; 
			}
			console.info(item); //1,2,3
			//当函数内部返回false的时候会停止迭代
	});

```
<br/>4:erase 移除数组中的多个元素。这个功能相当于Array的splice方法。
```javascript
	var arr = [1,2,3,4,5];
	console.info(Ext.Array.erase(arr , 2 , 2)); //[1,2,5]
```
<br/>5:every 在数组的每个元素上执行指定函数，直到函数返回一个false值如果某个元素上返回了false值，本函数立即返回false否则函数返回true ...
```javascript
	var arr = [1,2,5,6,9,10];
	var flag = Ext.Array.every(arr, function(item){
		if(item >=7){
			return false ; 
		}else {
			return true;	
		}
	});
	console.info(flag); //false
```
<br/>6:filter 按照指定方法过滤你的数组
```javascript
	var arr = [1,2,3,4,10,18,23];
	var newarr = Ext.Array.filter(arr,function(item){
		if(item > 10){
			return false ; 
		} else {
			return true ;
		}
	});
	console.info(newarr);//[1,2,3,4,10]

```
<br/>7：include 把一个元素插入到数组，如果它不存在于这个数组 ... 
```javascript
	var arr = [1,2,3,4];
	Ext.Array.include(arr , 2);
	console.info(arr);//[1,2,3,4,]
	Ext.Array.include(arr , 5);
	console.info(arr);//[1,2,3,4,5]
```
<br/>8：unqiue 返回一个去掉重复元素的新数组 ... js去重方法多样，我这里只是其一
```javascript
	var arr = [1,2,3,4,5,5,4,3,2,1,1,21,23,3,3,4];
	console.info(Ext.Array.unique(arr));
	
	
	//利用js对象的特性去掉数组的重复项  obj的key是不能重复的
	var obj = {};
	for(var i = 0 , len = arr.length ; i <len ; i++){
			obj[arr[i]] = true ;//去掉数组的重复项了 
	}
	//console.info(Ext.encode(obj));
	var uniquearr = [];
	for(var i in obj){
		if(obj.hasOwnProperty(i)){
			uniquearr.push(i);
		}
	}
	console.info(uniquearr); //["1", "2", "3", "4", "5", "21", "23"]
```
<br/>
<br/>[Ext.]()[Function]()
<br/>
<br/>[1:alias 起别名

```javascrpt
	var obj = {
		name:'xiaoshu',
		say:function(){
			console.info(this.name);
		}
	};
	var objsay = Ext.Function.alias(obj , 'say');
	objsay();
```
<br/>2:bind 绑定作用域的 就相当于 call、apply
```javascript
var color = 'red';
	var obj = {
		color:'blue'
	};
	function showColor(){
		console.info(this.color);
	}
	Ext.Function.bind(showColor,obj)();
```

<br/>3:defer 定时器 相当于window.setTimeout
```javascript
function task(){
	console.info('执行!');		
};
Ext.Function.defer(task,3000);
```
Ext.Date
<br/>
<br/>[1：between 2.format 3：parse]()
```javascript
	//between 
	console.info(Ext.Date.between(new Date(2013,07,15) ,new Date(2013,07,03),new Date(2013,07,08))); // false
	
	//format
	console.info(Ext.Date.format(new Date() , 'Y-m-d H:i:s')); //2016-11-05 23:42:42
	//parse 
	console.info(Ext.Date.parse('2010-07-05 21:22:22' , 'Y-m-d H:i:s').toLocaleString()); //2010/7/5 下午9:22:22

```
<br/>Ext.Error 基本不用，除非插件里面做提示，只是在控制台显示
<br/>Ext.Error.raise('you are wrong...');
<br/>常用的差不多也在这，这些东西API里面全都由，其他的就自己查一下API吧，望理解，谢谢！