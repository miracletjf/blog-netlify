---
title: css常用技巧
date: 2018-04-13 20:11:30
tags: 
- css
---

# css常用布局和居中
本文主要介绍了`css`常用的布局，居中等其他小技巧。

<!-- more -->

## `css`布局
### 左右布局
#### 利用`float`实现左右布局
左右模块设置`float:left;`,并且设置宽度，保证左右模块加起来的和等于`100%`，父级利用`clear`清除浮动。

html代码
```
  <div class="wrap clearfix">
    <div class="left-box">
      这是左边
    </div>
    <div class="right-box">
      这是右边
    </div>
  </div>
```

css代码
```
.clearfix::after {
  content: "";
  display: block;
  clear: both;
}
.wrap {
  padding: 10px;
  background: #ccc;
}
.left-box {
  float: left;
  background: #eaa;
  font-size: 30px;
  width: 40%;
  text-align: center;
  height: 300px;
}
.right-box {
  float: right;
  background: #6a8;
  font-size: 30px;  
  width: 60%;
  text-align: center;
  height: 300px;
}
```
#### 利用`flex`实现左右布局
利用`css3`的弹性布局盒子，实现左右布局。直接在父元素上设置`display:flex;`即可。
html代码
```
  <div class="wrap">
    <div class="left-box">
      这是左边
    </div>
    <div class="right-box">
      这是右边
    </div>
  </div>
```
css代码
```
.wrap {
  display: flex;
  padding: 10px;
  background: #ccc;
}
.left-box {
  background: #eaa;
  font-size: 30px;
  width: 40%;
  text-align: center;
  height: 300px;
}
.right-box {
  background: #6a8;
  font-size: 30px;  
  width: 60%;
  text-align: center;
  height: 300px;
}
```
若其中有一个模块为宽度为定宽，那么只需要在另一个模块上设置`flex: 1;`即可。

### 左中右布局
实现左中右布局，可以使用`float`,`inline-blok`,`flex`等多种方式来实现。
#### 利用`float`实现
利用`float`实现左中右布局，原理跟上文的左右布局类似，只需要添加一个中间模块即可。
html 代码
```
  <div class="wrap clearfix">
    <div class="left-box">
      这是左边
    </div>
    <div class="middle-box">
      这是中间
    </div>
    <div class="right-box">
      这是右边
    </div>
  </div>
```
css代码
```
.clearfix::after {
  content: "";
  display: block;
  clear: both;
}

.wrap {
  display: block;
  padding: 10px;
  background: #ccc;
}
.left-box {
  float: left;
  width: 30%;
  background: #eaa;
  font-size: 30px;
  text-align: center;
  height: 300px;
}
.middle-box {
  float: left;
  width: 40%;
  background: #a8a;
  font-size: 30px;
  text-align: center;
  height: 300px;
}
.right-box {
  float: left;
  width: 30%;
  background: #6a8;
  font-size: 30px;  
  text-align: center;
  height: 300px;
}
```
#### 利用`inline-block`实现左中右居中
理由`inline-block`实现左中右布局，有一个细节要注意一下，需要在父节点上面设置`font-size: 0;`除去标签之间回车引起的空格。还有就是圣杯布局，兼容性好一点。
html带代码
```
  <div class="wrap">
    <div class="left-box">
      这是左边
    </div>
    <div class="middle-box">
      这是中间
    </div>
    <div class="right-box">
      这是右边
    </div>
  </div>
```
css代码
```
.wrap {
  display: block;
  padding: 10px;
  background: #ccc;
  font-size: 0;
}
.left-box {
  display: inline-block;
  width: 30%;
  background: #eaa;
  font-size: 30px;
  text-align: center;
  height: 300px;
}
.middle-box {
  display: inline-block;
  width: 40%;
  background: #a8a;
  font-size: 30px;
  text-align: center;
  height: 300px;
}
.right-box {
  display: inline-block;
  width: 30%;
  background: #6a8;
  font-size: 30px;  
  text-align: center;
  height: 300px;
}
```
#### `flex`实现左中右布局
原理如上文提到的左右布局相似，只需要多添加一个模块即可。
#### 圣杯布局
圣杯布局是为了兼容老版本的ie浏览器使用的手段。功能是左右固定宽度，中间可变。相对来说，圣杯布局的复杂度高一点，但只要理解了，也就很容易搞清楚。
htm代码
```
  <div class="wrap clearfix">
    <div class="middle box">
      <div class="margin-inner">
              这是中间
      </div>
    </div>
    <div class="left box">
      这是左边
    </div>
    <div class="right box">
      这是右边
    </div>
  </div>
```
css代码
```
.clearfix::after {
  content: "";
  display: block;
  clear: both;
}
.box {
  float: left;
  min-height: 300px;
  font-size: 30px;
  text-align :center;
}
.wrap {
  padding: 10px;
  display: block;
  background: #ccc;
  font-size: 0;
}
.left {
  margin-left: -100%;
  width: 200px;
  background: #eaa;
}
.middle {
  width: 100%;
}
.middle .margin-inner {
  margin: 0 220px 0 200px;
  background: #a8a;
  min-height: 200px;
}
.right {
  margin-left: -220px;
  width: 220px;
  background: #6a8;
}
```
## 居中

### 水平居中
#### `margin`实现块级元素水平居中
给设置好宽度的块级元素设置的`margin: 0 auto;`实现水平居中
#### `inline-block`实现水平居中
给元素设置`display: inline-block;`,设置父级元素的属性`width: 100%;text-align: center;`
#### `flex`实现水平居中
父级元素设置`display: flex;justify-content: center;`即可
html代码
```
  <div class="wrap">
    <div class="box">
      第一行文字<br>
      第一行文字<br>
      第一行文字<br>
      第一行文字
    </div>
```
css代码
```
.wrap {
  display: flex;
  justify-content: center;
  height: 600px;
  font-size: 30px;
  background: #ccc;
  font-size: 0;
}
.box {
  display: inline-block;
  padding: 10px;
  vertical-align: middle;
  font-size: 30px;
}

```


### 垂直居中
#### 利用`padding`或`line`

- 可以设置上下`padding`
- `line-height`和`height`相等实现。

#### 利用`display: table`
父级元素设置`display: table;`,子元素设置`display: table-cell;vertical-align: middle;`
html代码
```
  <div class="wrap">
    <div class="box">
      第一行文字<br>
      第一行文字<br>
      第一行文字<br>
      第一行文字
    </div>
  </div>
```
css代码
```
.wrap {
  display: table;
  height: 600px;
  font-size: 30px;
  text-align:center;
}
.box {
  padding: 10px;
  display: table-cell;
  vertical-align: middle;
  background: #ccc;
  font-size: 30px;
}

```
#### 利用伪元素实现
设置伪元素高度100%,宽度为0，然后利用垂直对齐属性`vertical-align: middle;`实现垂直居中。
html代码
```
  <div class="wrap">
    <div class="box">
      第一行文字<br>
      第一行文字<br>
      第一行文字<br>
      第一行文字
    </div>
  </div>
```
css代码
```
.wrap {
  display: block;
  height: 600px;
  font-size: 30px;
  background: #ccc;
  font-size: 0;
}
.wrap::before {
  content: '';
  display: inline-block;
  vertical-align: middle;
  width: 0;
  height: 100%;
}
.box {
  padding: 10px;
  display: inline-block;
  vertical-align: middle;
  font-size: 30px;
}

```
注意细节，需要把父级元素`font-size`设置为`0`。如果想要兼容ie8及以下，需要把`::before`改为`:before`。
#### 利用定位给已知高度的元素设置垂直居中
父级元素设置`position: relative;`，子元素设置`positon:absolute;`,在利用`margin`实现垂直居中
html代码
```
  <div class="wrap">
    <div class="box"></div>
  </div>
```
css代码
```
.wrap {
  position: relative;
  height: 600px;
  font-size: 30px;
  background: #ccc;
  font-size: 0;
}
.box {
  position: absolute;
  top: 50%;
  margin-top: -90px;
  width: 150px;
  height: 180px;
  font-size: 30px;
  background: #987;
}
```
#### 利用`flex`实现垂直居中
父级元素设置`flex`和`align-items: center;`即可
html代码
```
<div class="wrap">
    <div class="box"></div>
  </div>
```
css代码
```
.wrap {
  display: flex;
  align-items: center;
  height: 600px;
  font-size: 30px;
  background: #ccc;
  font-size: 0;
}
.box {
  top: 50%;
  width: 150px;
  height: 180px;
  font-size: 30px;
  background: #987;
}
```
## 其他小技巧
### 使用`margin`实现水平和垂直同时居中。
给元素设置绝对定位，左右上下距离设置为`0`，再利用`margin： auto;`使得上下左右距离均匀分配。

html代码
```
 <div class="box"></div>
```
css代码
```
.box {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin: auto;
  width: 150px;
  height: 180px;
  font-size: 30px;
  background: #987;
}

```
### 文字间距
写页面时，经常遇到需要扩大文字间间距的情况，尤其是标题，如果用空格去填充那就太麻烦了，代码也不够优雅。好在`css`提供了扩大文字间距的属性`letter-spacing`。
![文字并没有居中](https://ws1.sinaimg.cn/large/005NgZr8gy1fqcfj08ydsj303g02kmwx.jpg)
但是由于最后一个字符后也跟了一个间距，导致文字并没有完全居中。那么现在要使用`css`提供的第二个间距相关的属性`text-indent`，可以控制首行文本前的间距。设置与`letter-spacing`的值相同即可。
![text-indent配合letter-spacing](https://ws1.sinaimg.cn/large/005NgZr8gy1fqcfnqmablj304103gt8h.jpg)

代码如下：
```
  letter-spacing: 0.5em;
  text-indent: 0.5em;
```
