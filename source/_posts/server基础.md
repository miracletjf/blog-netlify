---
title: server入门
date: 2018-04-05 10:13:40
tags: 
- http
---

# server基础入门

----------
要了解server,首先要了解一些网络的名词,基本概率,TCP,IP,端口.然后在了解什么叫做server,具体是怎么工作的.

<!-- more -->

## 基本网络名词

### TCP 传输控制协议(Transmission Control Protocol)

##### TCP 的特点是什么 与 UDP 的区别是什么
>TCP 是可靠的(因为它清楚的了解到数据包的收发状态),面向连接的,相对于UDP较慢;UDP 不可靠,不面向连接,相对于TCP较快.
#### TCP 的三次握手
>TCP在每次连接前,客户端与服务端之间都要进行三次对话才正式开始传输内容,三次对话大概是以下意思

1. 客户端: 我要连接你了,可以吗
2. 服务端: 嗯,我准备好了,连接我吧
3. 客户端: 那我连接了
4. 开始数据传输

### IP 网络协议(Internet Protocol)
只要你在互联网中,你就有会有一个IP.通俗上理解,IP分为[内网IP] 和 [外网IP].内网IP对应你在路由器控制的范围内的IP,外网IP就是你的路由器所对应的Ip地址.

- 你用电信的网,电信给你提供DNS服务
- 你有一个路由器,只要它连接到电信的服务器,那么路由器就会有一个外网IP,但是一般这个IP是不固定的,想要固定的就要多花几千去电信租用IP
- 所有连接到路由器的设备,都在内网中,公用的外网IP是路由器的IP
- 外网的信息都是通过从路由器发送到你的设备的
- 路由器还是一个网关,内网和外网就像是两个隔绝的空间,无法互通,唯一的联通点就是路由器.
- 路由器的ip是192.168.1.1
- 127.0.0.1 指向 localhost 自己的本机
- 0.0.0.0 不代表任何设备

### 端口(Port)
要想访问一个设备,光知道IP是不够的,还必须指定端口,端口是一个编号并不是一个硬件.一个服务器不一定至提供一个服务,比如HTTP服务,有提供FTP服务,还提供SMTP(邮件服务)服务,那么只有一个IP是无法告诉服务器你想有那种服务的.
比如

1. 要提供HTTP服务,你最好使用80服务(能不能使用其他端口?可以,不过不建议你违反约定)
2. 要提供HTTPS服务,你最好使用443端口(能不能使用别的端口?可以,不过不建议你违反约定)
3. 要提供FTP服务,你最好使用21端口(能不能使用别的端口?可以,不过不建议你违反约定)

#### 我怎么知道应该使用什么端口
[维基百科](https://zh.wikipedia.org/wiki/TCP/UDP%E7%AB%AF%E5%8F%A3%E5%88%97%E8%A1%A8#0.E5.88.B01023.E5.8F.B7.E7.AB.AF.E5.8F.A3)
#### 一共有多少个端口
每个机器一共有65535(2^16 - 1)个端口.请0-1023个端口需要管理员权限才可以使用

### 总结
>使用HTTP协议访问另一个IP时,要同时提供IP和端口号,缺一不可

我访问http://qq.com时并没有提供端口号,为什么我依然可以访问
>因为浏览器会帮你加上默认端口 80

## node.js 服务器
在本地建立一个node服务器,新建一个server.js文件,内容如下
    
    var http = require('http');
    var fs = require('fs');
    var url = require('url');
    var port = process.argv[2];
    
    if(!port){
	    console.log('指定端口号好不?\nnode server.js 8898');
	    process.exit(1);
    }
    
    var server = http.createServer(function(request, response){
	    var parsedUrl = url.parse(request.url,true);
	    var path = request.url;
	    var query = '';
	    if(path.indexOf('?') >= 0){
	    	query = path.substring(path.indexOf('?') + 1);
	    }
	    var pathNoQuery = parsedUrl.pathname;
	    var queryObject = parsedUrl.query;
	    var method = request.method;
	    
	    /******** 从这里开始看,上面不要看 ********/
	    
	    console.log('HTTP 路径为\n' + path );
	    if(path == '/style.css'){
		    response.setHeader('Content-Type','text/css; charset=utf-8');
		    response.write('body{background-color: #ddd;}h1{color:red}');
		    response.end();
	    }else if(path == '/main.js'){
		    response.setHeader('Content-Type','text/js; charset=utf-8');
		    response.write('alert("这是js执行的")');
		    response.end();
	    }else if(path == '/'){
		    response.setHeader('Content-Type','text/html; charset=utf-8');
		    response.write('<!DOCTYPE>\n<html>' +
		    '<head><link rel="stylesheet" href="/style.css">' +
		    '</head></body>' +
		    '<h1>你好</h1>' +
		    '<script src="/main.js"></script>' +
		    '<body></html>');
		    response.end();
	    }else {
		    response.statusCode = 404;
		    response.end();
	    }
	    /************ 代码结束,下面不要看 ***********/
    })

    server.listen(port);
    console.log('监听' + port + '成功\n请用在空中转体720度然后用电饭煲打开 http:localhost:' + port);
    

使用 `node server.js` 命令开启服务器,浏览器中打开http://localhost:8898/即可访问

- response.setHeader('Content-Type','text/js; charset=utf-8'),设置第二部分的响应头,控制响应的文本格式
- response.write('文本'),返回的文本内容
- response.end(),响应结束
- response.statusCode = 404 ,设置状态码404 