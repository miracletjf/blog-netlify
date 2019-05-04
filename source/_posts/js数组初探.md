---
title: js数组初探
date: 2018-05-19 20:06:53
tags: 
- javaScript
---
## 基本概念
参照阮一峰老师的说法
>数组是按次序排列的一组值。每个值的位置都有编号（从0开始），整个数组用方括号表示。

<!-- more -->

## js中的数组
js中的数组其实并不是标准意义上的数组，其实是一种特殊的`hash表`，原型链中有`Array.prototype`。由于js中的数组数据在内存中并不是连续的，所以很容易对数组进行删除，新增，修改等操作。
```
//下面这种写法，等同于于一个js数组
var arr = {
	'0':1,
	'1':2,
	'2':3,
	'length':3
}
arr.__proto__ = Array.prototype;
```
## 数组的声明
1. `var arr = Array(3)` 声明一个数组长度为3的数组
2. `var arr = Array(1,2,3)` 声明一个值为1,2,3的数组，即[1,2,3]
3. `var arr = new Array(3)`声明一个数组长度为3的数组
4. `var arr = new Array(1,2,3)`声明一个值为1,2,3的数组，即[1,2,3]
5. `var arr = [1,2,3]`声明一个数组[1,2,3] 

通过以上代码了解到，加不加关键字`new`对声明数组无影响。
由于`Array`方法存在很强的不一致性，一般不用这种方式，常规的方式是最后一种声明方式。
## 常用的api
`Array.prototype`提供了许多`api`，其中常见的有：
- `Array.prototype.concat()`连接两个数组
- `Array.prototype.slice()`获取子数组
- `Array.prototype.sort()`对数组排序(注意：此方法会改变原数组，因为sort内部使用的是快排，属于原地排序方法)
- `Array.prototype.push()`在数组尾部添加数据
- `Array.prototype.pop()`弹出数组尾部数据
- `Array.prototype.shift()`弹出数组第一个数据
- `Array.prototype.unshift()`在数组开头位置添加一个数据
- `Array.prototype.splice()`删除数组中的数据
- `Array.prototype.join()`连接数组中的所有数据，并且放回组成的字符串
- `Array.prototype.indexOf()`返回与值匹配的数组的下标，若没有返回-1
- `Array.prototype.forEach()`遍历数组，无返回值
- `Array.prototype.map()`遍历数组，有返回值
- `Array.prototype.filter()`过滤数组元素
- `Array.prototype.reduce()`对数组中的每个元素应用一个函数，将其减少为单个值

以下重点介绍一下`sort`,`forEach`,`map`,`filter`,`reduce`,`join`
### Array.prototype.sort() 排序
`sort`方法接收一个函数参数，函数接两个参数，代表当前值与下一个值，若返回一个正数，交换两个数的位置，否则不变。示例：
```
var arr = [3,2,1,4,5]
arr.sort(function(a,b){ //a当前值，b下一个值
  return a-b;
})  // [1,2,3,4,5]
```
### Array.prototype.forEach 遍历数组
`forEach`方法接收一个函数为参数，函数接收三个参数（value,index,arr）,一般只用到前面两个参数，无返回值。示例：
```
var arr = [3,2,1,4,5]
var res = arr.forEach(function(value,index){
  console.log(value,index);
  return value*value;
})
//3 0
//2 1
//3 2
//4 3
//5 4
console.log(res) //undefined
```
### Array.prototype.map 遍历数组，返回每次返回值所组成的数组
`map`方法与`forEach`方法基本一致，只是有一个返回值的差别。示例：
```
var arr = [3,2,1,4,5]
var res = arr.map(function(value,index){
  console.log(value,index);
  return value*value;
})
//3 0
//2 1
//3 2
//4 3
//5 4
console.log(res) //[9, 4, 1, 16, 25]
```
### Array.prototype.filter 过滤器
`filter`方法用于过滤数据，接收一个函数参数，函数接收三个参数（value,index,array），若返回值为true，把value添加到返回数组中。示例：
```
var arr = [3,2,1,4,5]
var res = arr.filter(function(value){
  return value%2===0;
})
console.log(res) //[2,4]
```
### Array.prototype.reduce 缩减，压缩
`reduce`方法对数组中的每个元素（从左到右）应用一个函数，将其减少为单个值。接收两个参数，第一个参数是函数，第二个是起始值（可省略）。函数可接受四个参数，一般情况下只用前面两个（累计值，当前值），若省略第二个参数，则函数累计值从下标0开始。示例：
```
//reduce实现累加
var arr = [3,2,1,4,5]
var res = arr.reduce(function(a,b){
  return a+b;
}，0)
console.log(res) //15
//求最大值
var max = arr.reduce(function(a,b){
  return a>b?a:b;  
})
console.log(max) //5
```
### Array.prototype.join 连接数据，返回字符串
`join`方法可以将数组转换成字符串，中间用给定的连接符，默认连接符是`,`。示例：
```
var arr = [3,2,1,4,5]
console.log(arr.join('-'))	//3-2-1-4-5
```
### Array.prototype.slice 获取子数组
`slice`方法接收两个参数(起始位置，结束位置的后一个位置)，默认从0开始。返回子数组。示例：
```
var arr = [3,2,1,4,5]
var sub1 = arr.slice();  //[3,2,1,4,5]
var sub2 = arr.slice(1,2); //[2]
```
## 伪数组
关于js中的伪数组，常见的有`NodeList`类型和`arguments`。所谓的伪数组就是可以通过`下标`取值和获取`length`的值，但是并不可以使用数组中的`api`，即原型链中没有`Array.prototype`。一般情况，为了使用数组的的`api`需要把伪数组转为数组，一般使用`slice`将伪数组转为数组。例如：`Array.prototype.slice.call(arguments)`
## 总结
本文简单介绍了数组的常见`api`,并没有很详细的列举所有的`api`。也当做学习笔记来使用，仅供学习与交流。转载请注明出处。