---
title: html常用标签介绍
date: 2018-04-07 21:38:55
tags:
- html
---

# html 常用标签介绍

----------

HTML标签的数量是有很多的，每个标签都有自己的用途。这就是HTML语义化的一方面。方便理解，运用。具体的每个标签的含义可以查看我的上篇文档[html简介](https://miracletjf.github.io/2018/04/06/html%E7%AE%80%E4%BB%8B/)下面介绍一些比较常用标签，但是他们又有一些小的技巧是我们平时不容易发现的。

<!-- more -->

## iframe标签
- iframe标签自带一个`frameborder="1"`的属性，我们使用时应该把它给去掉，用`frameborder="0"`覆盖。
- `name` 属性，定义自身窗口的名称，配合`a`标签的`target`属性，可以实现点击`a`标签，在指定的`frame`窗口中打开的功能。
- `src`属性，定义了`iframe`窗口要展示的网页地址。
- `sandbox`可以对`iframe`启动额外的限制条件。如果属性值为空，表示限制条件全部启用，若不为空，即用空格分隔的一系列指定的字符串。比如说允许窗口执行JavaScript,只有这样写`sandbox="allow-scripts"`
## a标签
- `target` 属性代表指定位置打开链接，对应的值有
	- `_blank` 在新窗口打开链接
	- `_self` 在自身窗口打开链接，默认值
	- `_parent` 在父窗口打开链接
	- `_top` 在顶级窗口打开链接
- `download` 下载链接地址的内容（有的网站会禁止你下载的）。
- `href` 代表链接地址
	- `http://www.baidu.com` 打开指定链接地址
	- `//www.baidu.com` 用与当前网页相同的协议打开链接地址，有`http`，`https`，一般ftp方式打开
	- `/xxx.html` 直接打开在域名后接/xxx.html的链接地址
	- `#sss` 跳转到锚点位置
	- `?name=666` 添加查询参数，并跳转
## form标签
form标签其实也是在跳转链接，只不过form跳转链接是发起的是post请求，并且可以携带第四部分的参数到服务器。

- `method` 指定发起的请求的方式一般指定为`post`
- `form` 提交也可以携带查询参数
- 只要是`http`协议提交数据，密码在网络中就是明文传递的，不安全。推荐使用`https`，传递机密数据。
## button标签
- 如果`form`标签中只用一个`<button>`标签，并且标签的`type`属性未指定，那么`button`按钮，自动升级为`submit`按钮
- 指定`input`的`type`类型为`submit`也可以提交`form`
## label标签
- 把`input`标签包裹在`label`中，可以建立`label`与`input`的联系。点击`label`中的内容，可以触发`input`点击效果。
## textarea标签
- css设置`resize:none;`可以禁用可拖拽功能
## table标签
- `<thead>` 表格头部
- `<tbody>` 表格主体
- `<tfoot>` 表格底部
- `<tr>` table row 表格行
- `<th>` table header 表头
- `<td>` table data 表格数据
- `<colgroup>` 表格列组，用来定义列表中的一组列表。
- `<col>` 表示表格中的列，属性`bgcolor`可以订阅某列的背景色。