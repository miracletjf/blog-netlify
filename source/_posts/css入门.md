---
title: css入门
date: 2018-04-12 20:00:24
tags: 
- css
---
# css入门---使用细节

## 使用工具

想要使用`css`实现设计图的效果，首先你得趁手的工具，在这里仅提供个人使用的工具，仅供参考。

- [FastStone Capture](http://www.faststone.org/FSCapturerDownload.htm) 功能强大且体积小。截图，取色，编辑，尺子等功能，都有很实用的价值。
- `world` 当页面上使用了一些特殊字体，你可以打开`world`，选择相似的字体替代，也可以实现差不多的效果。

<!-- more -->

## 文档流

关于`css`中的文档流，只需要记住以下几点即可：

- 文档流指的是文档内流动方向
	- 内联元素的流动方向，从左向右
	- 块级元素的流动方向，从上到下
- `div`的高度由其内部文档流元素的高度的总和决定。

## span相关

- `span`有`border`，并且换行。
- `word-break: break-all;`会断开单词
- `word-break: break-word;`以单词为单位，断开

## 字体的高度

- `font-size` 代表文字最高点到文字最低点之间的距离
- 每个字体的默认行高，都是设计好的，存在着差异。
- 设置`line-height`要大于字体默认行高，才会起效。

## 定位`position`

css的定位总共有五种，其中包括一种实验样式`sticky`

- `static` 默认属性，使用正常布局
- `relative` 相对定位，`z-index`可以生效，辅助`absolute`
- `absolute` 绝对定位，固定于最近的非`static`的祖先元素的位置。脱离文档流
- `fixed` 固定定位，指定元素相对于视口的位置。脱离文档流。
- `sticky` 粘滞定位，该属性是相对定位与绝对定位的混合。当元素达到特定的条件后，由相对定位转变为固定定位。事例:`#st{position: sticky;top:10px;}`当元素与视口的距离小于10px时，元素由相对定位，转变为固定定位。

## 设置div居中

设置固定宽度的`div`元素居中。`margin-left: auto;margin-right: auto;`

## `display: inline-block;`

元素设置了`display: inline-block;`属性，元素的上下`margin`不会与其他元素的`margin`合并。

## css三角形

利用`css`的`border`属性，可以做出一个三角形。
```
    <span class="triangle"></span>
    
    .triangle {
    	display: block;
    	width: 0;
    	border: solid 10px transparent;
    	border-left-color: #ff0000;
    }
```