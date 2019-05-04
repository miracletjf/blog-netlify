---
title: js函数初探
date: 2018-05-20 11:02:49
tags: 
- javaScript
---
## 基本概念
函数的定义就是一段可以反复调用的代码块，可以接受参数，并且可以根据不同参数返回不同的返回值。js的函数属于复杂类型，属于Object类型，但是它有自己的构造函数，且能够使用`typeof`运算符返回`function`。js中不指定函数返回值，默认返回`undefined`。函数的原型链经过`Function.prototype`。

<!-- more -->

## 函数声明
函数声明的方式目前已知的有五种。
- `function fn(a,b){return a+b}` function命令声明具名函数
- `var fn = function(a,b){return a+b}` function命令声明匿名函数，赋值给变量fn
- `var fn = function f(a,b){return a+b}` function命令声明具名函数，且赋值给fn
- `var fn = new Function('a','b','return a+b')` Function构造函数声明匿名函数，赋值给fn
- `var fn = (a,b)=>{return a+b}` 箭头函数声明一个匿名函数，赋值给fn

比较常用的声明方式是第一，第二，第五种。js的函数声明方面还存在着一个比较反常规的地方，那就是函数的`name`属性。请看以下示例：
```
//具名函数
function fn(a,b){return a+b}
console.log(fn.name) //'fn'
//匿名函数赋值给变量fn
var fn = function(a,b){return a+b}
console.log(fn.name) //'fn'
//具名函数赋值给变量fn
var fn = function f(a,b){return a+b}
console.log(fn.name) //'f'
//Function构造函数声明函数，赋值给fn
var fn = new Function('a','b','return a+b')
console.log(fn.name) //'anonymous'（匿名）
//箭头函数声明一个匿名函数，赋值给fn
var fn = (a,b)=>{return a+b}
console.log(fn.name) //'fn'
```
由此看出，js函数的声明与`name`属性也有很强的不一致性，容易让人产生误解。需要刻意的去记忆。
## 函数提升
函数可以向变量一样提升到代码头部。
```
fn(); //undefined
var a = 0;
function fn(){
	console.log(a);
}
```
之所以以上代码没有报错，是因为，整个`function`被提升到代码头部了。以上代码相当于：
```
var a;
function fn(){
	console.log(a);
}
fn(); //undefined
a = 0;
```
## 函数的调用
函数的调用方式一般有三种直接调用，使用`call`,使用`apply`;
### 直接调用
函数直接调用，传入参数，函数内部执行，返回需要返回的值。
```
function fn(){console.log('直接调用')}
fn()//直接调用
```
### call方式调用
`call`方式调用以也是比较常见的调用方式，它接收的第一个参数内部`this`指定的值，若不传即为`undefined`，内部`this`自动转为`window`对象，若使用严格模式`use strict`，则内部`this`为`undefined`。从第二个参数开始为函数所要接收的参数。示例：
```
function fn(a,b){
	console.log(this,a,b);
}
fn.call();	//window undefined undefined
fn.call(undefined,1,2);	//window 1 2
fn.call({ff:'f'},1,2);	//{ff:'f'} 1 2
```
### apply方式调用
`apply`方式调用与`call`基本一致，只是接收的参数有一点区别，第一个参数没差别，都是指定内部`this`变量，第二个参数，`apply`接收一个数组或者伪数组对象。示例：
```
function fn(a,b){
	console.log(this,a,b);
}
fn.apply();	//window undefined undefined
fn.apply(undefined,[1,2]);	//window 1 2
fn.apply({ff:'f'},[1,2]);	//{ff:'f'} 1 2
```
## this
关于`this`是`js`语言设计者布兰登·艾克为了满足当时的需求（长得像`Java`）而加入的一个属性，`this`指代了属性和方法'当前'所在的对象。刚刚提过，使用`call`或者`apply`可以指代调用函数时内部的`this`对象。
### 构造函数中的this
构造函数中的`this`指代了当前的实例。示例：
```
function F(){
	this.name = 'ff';
	this.t = this;
}
```
![构造函数中的this](https://ws1.sinaimg.cn/large/005NgZr8gy1frhqif5hf5j30d3084gls.jpg)
### 函数中的this
函数中的`this`只有在函数被调用时，才有意义。当调用函数时，我们把调用方式改为`call`的形式调用，可以轻松确定。示例：
```
var obj = {
	a: 1,
	b: function(){
		console.log(this)
	}
}
obj.b() // {a:1,b:function(){console.log(this)}}
//等价于 obj.b.call(obj) .call前面的第二个参数开始往前，即为函数内部的this
```
### 其他情况下的this
在其他情况下，this指向当前作用域的对象。示例：
```
var obj = {
	a: '0',
	b: this
}
obj.b // window
var obj = {
	a: '0',
	b: {
		t:this
	}
}
obj.b.t // window
```
## arguments
`arguments`可以接收传给函数的所有参数。`arguments`本身是一个伪数组。示例：
```
function fn(){console.log(arguments)}
fn(1,2,3,'d') // [1,2,3,'d']
```
## 调用栈（call stack）
`call stack` 是指函数在执行时，内部的处理方式。可以帮助理解，函数在执行过程中，怎么一步一步执行，并且返回到之前的位置的。请看简单的[示例](http://latentflip.com/loupe/?code=ZnVuY3Rpb24gYSgpewogICAgY29uc29sZS5sb2coJ2EnKQogICAgYigpCiAgICBjb25zb2xlLmxvZygnLWEnKQp9CmZ1bmN0aW9uIGIoKXsKICAgIGNvbnNvbGUubG9nKCdiJykKfQphKCk%3D!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)
![调用栈](https://ws1.sinaimg.cn/large/005NgZr8gy1frhr1rg9rwj306s09ijr9.jpg)
## 作用域
作用域是一个比较难理解的概念，但是如果结合数据结构去理解，也就直观了很多。目前，js中的作用域只有全局作用域`window`和函数作用域。
1. 作用域是一个树形结构
2. 若当前作用域无变量名所对应的变量，则去父作用域中寻找，直到根节点（全局作用域）。若存在则去值，若不存在，则取`undefined`。这就是作用域的`就近原则`
3. 在作用域中确定某个变量的值，第一步要做的就是`变量提升`，然后再去分析变量的值。

### 典型例题
#### 第一题
```
var a = 1
function f1(){
	console.log(a)
	var a = 2 
}
f1.call() //undefined
```
分析：
1. 变量提升,声明提升
```
var a
function f1(){
	var a
	console.log(a)
	a = 2
}
a = 1
f1.call()
```
2. 分析作用域
因为在`f1()`中`console.log(a)`的作用域是`f1`，所以在内部`console.log(a)`之前的代码中寻找`a`，发现`a`是`undefined`

#### 第二题
```
var a = 1
function f1(){
    var a = 2
    f2.call()
}
function f2(){
    console.log(a)
}
f1.call()	//1
```
分析：
1. 变量提升，提升声明
```
var a
function f1(){
	var a
	a = 2
	f2.call()
}
function f2(){
	console.log(a)
}
a = 1
f1.call()
```
2. 分析作用域
因为`f1()`执行时，内部执行`f2()`,而`f2()`在全局作用域中。这时`f2()`中的`console.log(a)`之前没有变量`a`,沿着作用域链向上寻找，在全局作用域中有变量`a`，那么`f2()`中的`console.log(a)`即为全局的变量`a`。在执行`f2()`之前，`a`已经被赋值为`1`，所以打印出`1`。

#### 第三题
```
//有6个li
var liTags = document.querySelectorAll('li')
for(var i = 0; i<liTags.length; i++){
    liTags[i].onclick = function(){
        console.log(i) // 点击第3个 li 时，打印 2 还是打印 6？
    }
}
```
分析：
1. 变量提升，提升声明
```
var liTags
var i
liTags = document.querySelectorAll('li')
for(i=0;i<liTags.length;i++){
	liTags[i].onclick = function(){
		console.log(i)
	}
}
```
2. 当点击第三li的时候，会执行函数`function(){console.log(i)}`。因为`i`变量在当前函数中并不存在，所以要去父作用域（全局作用域）寻找，找到了全局的`i`,这时循环已经结束，而`i`的值已经变为`liTags.length`。所以打印出`6`

### 闭包
闭包就是一个函数是用来它范围外的变量，那么 `这个函数` + `这个变量` 就组成了 `闭包`。示例：
```
function a(){
	var aa = 'aa'
	function b(){
		bb = aa;
	}
}
```

## 总结
函数是整个js的核心，必须完全弄明白。这对以后的工作和提升会有很大的影响。再者就是数据结构是可以帮助理解抽象化知识，使抽象化的知识在大脑中形成一个更加明确的结构。
本文仅供交流与学习使用，转载请注明出处。
