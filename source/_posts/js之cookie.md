---
title: js之cookie
date: 2018-07-01 19:41:30
tags: 
- javaScript
---
## cookie 概述
`cookie`是服务器保存在浏览器端的一小段文本信息，每个`cookie`的大小一般不超过4KB。服务器给浏览器设置了`cookie`以后，浏览器没错向服务器发送请求时，就会附上`cookie`。每一个`cookie`都有对应的`域名`和`浏览器`。
`cookie`是提供给`HTTP`协议使用的，可以通过`HTTP`协议传送。

<!-- more -->

## cookie 内容
`cookie` 一般包括一下内容
 - cookie的名字
 - cookie的值
 - 到期时间
 - 所属域名	（默认当前域名）
 - 生效的路径 （默认当前网址）

## cookie 作用
一般的`cookie`的用途有：
- 对话： 保存用户登录状态，购物车信息等
- 个性化：保存用户的偏好设置，例如：字体大小，配色等
- 追踪：记录和分析用户的行为

## 生成 cookie
服务器生成`cookie`是通过设置`HTTP`请求头的`Set-Cookie`来设置的。`Set-Cookie`可以有多个。除了可以设置`Cookie`的值，还可以设置附加的`Cookie的属性`,一个`Set-Cookie`字段，可以包括多个属性，没有次序要求。

附加的属性有以下几点：
- Expires=<date> (UTC格式的日期)
- Max-Age=<non-zero-digit> (最大失效时间，定义自设置`cookie`开始后失效的`秒数`)
- Domain=<domain-value> (指定域名，默认指当前的一级域名)
- Path=<path-value> (指定路径)
- Secure (一个带有安全属性的`cookie`,只有在请求使用`HTTPS`和`SSL`协议时，才发送到服务器。)
- HttpOnly (禁止JavaScript访问`cookie`,可以防范`XSS`跨站脚本攻击)

### Set-Cookie 语法
```
Set-Cookie: <cookie-name>=<cookie-value> 
Set-Cookie: <cookie-name>=<cookie-value>; Expires=<date>
Set-Cookie: <cookie-name>=<cookie-value>; Max-Age=<non-zero-digit>
Set-Cookie: <cookie-name>=<cookie-value>; Domain=<domain-value>
Set-Cookie: <cookie-name>=<cookie-value>; Path=<path-value>
Set-Cookie: <cookie-name>=<cookie-value>; Secure
Set-Cookie: <cookie-name>=<cookie-value>; HttpOnly
```
### node.js 设置示例
```
response.setHeader('Set-Cookie',[`email=${user['email']}`,`Max-Age=${60*60*24*7}`]);

```
## 发送 cookie
在服务器给浏览器设置好`cookie`后，浏览器访问服务器都会带上相应的`cookie`（满足限制条件的前提下）。
服务器根据浏览器发送的`cookie`获取用户数据，并返回给用户，实现记住用户的状态等功能。
## 浏览器 访问
浏览器可以通过`document.cookie`访问当前域名下的`cookie`,也可以通过这个属性设置`cookie`。如果服务器设置了`HttpOnly`，那么就无法使用`javascript`来操作`cookie`l。
## 总结
以上就是对在下对`cookie`的简单的理解，要想深入的了解还是需要动手操作的。本文参考来一些网上的开源文章（[阮老师的Cookie](http://javascript.ruanyifeng.com/bom/cookie.html)，[MDN的开源文档](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies)）。
文中有认识不正确的地方，欢迎指正（可以发送邮件到本人邮箱『1398988546@qq.com』），转载请注明文章来源。
