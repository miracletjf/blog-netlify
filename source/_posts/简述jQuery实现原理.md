---
title: 简述jQuery实现原理
date: 2018-05-23 00:12:50
tags: 
- jQuery
---
## 背景介绍
`jQuery`可以说是流行最广，使用量最多的一个`js库`了。它极大的提高了人们工作的效率，解决了困扰人们很久的`dom`不一致性所带来的各种问题。`jQuery`最大的贡献就是在`dom`操作这方面，当然它还提供其他方面的操作。下面我们来通过一个小的例子简单介绍一下`jQuery`的实现原理。

<!-- more -->

## 内部结构
`jQuery`内部其实是一个伪数组对象，对象上还绑定一些自带的属性，比如说`context`,`selector`等。最重要的是`jQuery`的原型，接下来我们讲。
## 构造函数
把`jQuery`写成构造函数，把所有的共用属性写在`JQuery.prototype`上，方便每个示例的调用，也节省了大量的内存空间。如果是非构造函数形式调用，那么就`new`一个实例并返回。
```
  window.jQuery = function(nodes){
    //判断是否使用 new 关键字
    if(this instanceof jQuery){
      this.init(nodes);
    }else {
      return new jQuery(nodes);
    }
  };
```
## 获取节点
`jQuery`基于原生的`Dom`属性与方法封装了自己的选择器方法。通过选择器方法，可以把原生节点，选择器字符串转换为`jQuery`内部节点。然后对节点进行操作。由于选择器方法比较复杂，我们就用现有的`document.querSelectorAll()`来代替，实现的效果相同。在构造函数初始化的时候就去执行这个方法。
```
  //初始化函数，把nodes节点传给this
  jQuery.prototype.init = function(nodes){
    var _this = this;
    _this.length = 0;
    if(typeof nodes === 'string'){
      nodes = document.querySelectorAll(nodes);
    }
    for(var i=0;i<nodes.length;i++){
      _this[_this.length] = nodes[i];
      _this.length++;
    }
  };
```
## addClass方法
`jQuery`有一个`addClass`方法，我们这里模拟一下。不过只是一个简易版，了解一下原理即可。首先遍历所有实例中的节点对象，然后每个执行`dom`原生提供的`classList.add()`。
```
  //添加class
  jQuery.prototype.addClass = function(className){
    var _this = this;
    for(var i = 0;i < _this.length;i++){
      _this[i].classList.add(className)
    }
  };
```
## setText方法
`setText`方法和`addClass`方法原理类似，都要遍历实例中的节点对象，然后每个执行一遍`dom`提供的`textContent`的方法。
```
  //添加文字
  jQuery.prototype.setText = function(text){
    var _this = this;
    for(var i = 0;i < _this.length;i++){
      _this[i].textContent = text;
    }
  };
```
## window.$
为了使用户用的方便，最好能像`jQuery`一样，直接使用`$`就可以，只需要设置`window.$ = jQuery`即可。刚刚已经讲过内部已经判断过是否是通过`new` 来构造实例的，如果不是，内部`return new jQuery`。
## 调用测试
通过以下代码调用，基本都实现功能。如果想要实现链式调用，直接在每个方法结尾`return this`即可。
```
    $('div').addClass('red');
    $('div').setText('h1');
```
## 总结
`jQuery`内部提供了大量的`api`,如果有兴趣，可以自己多实现一些方法。本文只是简单介绍一下实现原理与结构。本文内容仅供学习与交流使用，转载请注明出处。
