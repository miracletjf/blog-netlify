---
title: Ajax与HTTP请求
date: 2018-06-10 12:34:49
tags: 
- javaScrit
---
最近在学习`Ajax`相关的内容。谈到`Ajax`就不得不提`HTTP协议`，`Ajax`就是使用`JavaScript`来发送，一个`HTTP请求`，并且可以接收从服务器返回的`HTTP响应`。下面我们来聊一下`Ajax`和`HTTP`协议。

<!-- more -->

## HTTP 协议
`HTTP`的全称是超文本传输协议`(英文：HyperText Transfer Protocol)`，它是`客户端`与`服务器`请求和响应的标准。`HTTP`使得`客户端`与`服务器`之间可以进行通信，传输与接收数据。更详细的定义可以查看维基百科:[HTTP协议](https://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE)。
我们具体来看一下，`HTTP`的请求与响应。
### HTTP 请求
`HTTP`请求分为四部分，格式如下所示：
```
1 请求的动词 路径 协议/版本
2 key1: value1
2 key2: value2
2 key3: value3
2 ...
2 Content-Type: value
2 Host: www.baidu.com
2 User-Agent: curl/7.57.0
3 (回车)
4 要上传的数据
```
对于第一部分，第三部分，第四部分都好理解，第二部分需要说明一下。第二部分是请求的头信息，用来描述一些元数据，服务器会根据头信息，作出相应的处理方式。
### HTTP 响应
`HTTP`响应也有四部分，格式如下：
```
1 协议/版本号 状态码 状态解释
2 key1: value1
2 key2: value2
2 Content-Length: 2443
2 Content-Type: text/html
2 ...
3 (回车)
4 要下载的内容
```
响应部分也是一样，第一，第三部分不用多解释，第二部分返回响应头信息，客户端根据头信息，作出相应的处理操作。第四部分是请求体，即：服务器根据请求响应的内容。
## Ajax
`Ajax`的全称是`Asynchronous JavaScript and XML`。在没有`Ajax`之前，前端想要向请求后端的数据，可以使用的方式有：
- `form` 表单请求 『缺点：页面会刷新』
- `img` 通过 img 标签，向服务器发送请求 『缺点：只能发送GET,页面没有刷新，只能请求图片』
- `script` 标签请求，传递回调函数给后台，后台把数据放入回调函数中，当作参数，执行。『缺点：只能发送`GET`请求』（这就是`jsonp`）
- ...

以上这些方式要么需要刷新页面，要么只能发送`get`请求。做不到既可以发送除`GET`以外的请求，并且不刷新页面的效果。而`Ajax`就是为了解决这么一个问题出现的，说白了`Ajax`就是让`JavaScript`可以发送`HTTP`请求和接收`HTTP`响应。
### Ajax 的历史
1999年微软就在`IE5.0`上就引入了可以让`JavaScript`发送请求的接口，但是一开始并没有引起重视，直到2004年`Gmail`的发布，才引起广泛的关注。在2005年`Ajax`这个词被提出。后来`Ajax`这个词就成为了`JavaScript`脚本发起`HTTP`通信的代名词。
### Ajax 的使用
`Ajax`一般包括以下步骤
> 1. 创建 `XMLHttpRequest` 实例
> 2. 发出 `HTTP` 请求
> 3. 接收服务器传回的数据
> 4. 处理网页数据 

#### Ajax 请求 前端代码
以下是一个简单的`Ajax`的示例：
```
let xhr = new XMLHttpRequest();
xhr.open('GET','http://jack.com/ajax');
xhr.setRequestHeader('Content-Type','application/x-www-form-urlencode;charset=utf-8');
xhr.onreadystatechange = ()=>{
  if(xhr.readyState === 4){
    if(xhr.status >= 200 && xhr.status < 300){
      let string = xhr.responseText;
      let obj = JSON.parse(string);
      console.log('请求成功');
    } else if(xhr.status >= 400){
      console.log('请求失败');
    }
  }
}
xhr.send('a=9&b=6');
```
上述代码与`HTTP`请求方式相对应
- `xhr.open()` 对应了`HTTP`请求的 `请求方式`、`请求路径`、`请求的域名`
- `xhr.setRequestHeader()` 配置 请求头信息
- `xhr.onreadystatechange()` 当状态发生改变时，判断当前的请求状态以及响应的状态。定义成功与失败的回调函数。
- `xhr.send()` 请求的数据

#### Ajax 响应 node.js后端代码
以下是一个简单的`node.js`服务端处理`HTTP`响应的示例：
```
 if(pathNoQuery === '/ajax'){
    response.statusCode = 200;
    response.setHeader('Access-Control-Allow-Origin','http://testa.com:8890');	//设置允许跨域请求的 源
    response.setHeader('Content-Type','application/json');		//设置返回的数据形式是 JSON
    response.write(`{"name":"miralce","method":"ajax"}`);
    response.end();
  }
```
上述代码跟`HTTP`的响应方式相对应
- `response.statusCode` 设置状态码，告知请求的状态，成功与否
- `response.setHeader` 设置响应的响应头信息
- `response.write` 设置响应体数据，返回给前端的数据
- `response.end()` 响应结束发送

### Ajax 的封装
一般来说直接写`Ajax`代码比较多，不利于书写，所以可能需要对`Ajax`进行封装，利用`jQuer`的思路，在结合`ES6`的`Promise`：
```
window.jQuery.ajax = function({url,method,header,body}){
  return new Promise(function(resolve,reject){
    let xhr = new XMLHttpRequest();
    xhr.open(method,url);
    for(let key in header){
      xhr.setRequestHeader(key,header[key]);
    }
    xhr.onreadystatechange = function(){
      if(xhr.readyState === 4){
        if(xhr.status >= 200 && xhr.status < 300){
          resolve(xhr.responseText);
        }else{
          reject(xhr);
        }
      }
    }
    let bodys = '';
    for(let key in body){
      bodys += `${key}=${body[key]}`; 
    }
    xhr.send(bodys);
  })
}
```
调用的方式
```
  $.ajax({
    url: '/ajax',
    method: 'post',
    header: {
      'Content-Type': 'application/x-www-form-urlencode;charset=utf-8',
      'user': 'miracle'
    },
    body: {
      'name': 'miracle',
      'time': Date.now()
    }
  }).then(resText=>{
      let res = JSON.parse(resText);
      console.log(res);
    },xhr=>{
      console.log(xhr);
    })
```
## 总结 
`Ajax`技术是前端的一个里程碑，它的出现直接改变了人们对前端的认识，对前端的发展有着巨大的推动作用。也是现在的前端开发人员不得不掌握的技能之一。
以上只是本人对`Ajax`和`HTTP`简单的看法，若有写的不正确或者不好的地方，欢迎指正。
本文仅供学习与交流使用，转载请注明出处。