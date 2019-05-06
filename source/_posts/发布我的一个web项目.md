---
title: 发布我的一个web项目
date: 2018-10-02 22:04:27
tags: 
- 服务器
---

今天终于发布上线了自己的第一个web项目，决定把这个过程记录下来，一方面做个记录，方便后续查看，另一方面，为了纪念这一历史性的时刻。下面就是一个从一个小白到项目发布成功的过程。

<!-- more -->

![Snipaste_2019-05-06_23-22-49.png](https://i.loli.net/2019/05/06/5cd051733d358.png)

## 前期准备
### 申请服务器
首先申请一台服务器，我用的是腾讯云的服务器，用的系统是`centOS 7`
### 登录服务器
下载一个[Bitvise SSH Client](https://www.bitvise.com/ssh-client-download) 软件访问服务器

![登录服务器](https://ws1.sinaimg.cn/large/005NgZr8gy1fvuagy4ib1j30i10i9t9c.jpg)
## nginx

`nginx`是一个异步框架的`web服务器`，本质是一个服务器。更多内容参考[维基百科](https://zh.wikipedia.org/wiki/Nginx)

### 安装于配置 nginx

关于安装与配置`nginx`，在网上找了一篇文档，照着从头做到尾就行了。[CentOS 7 下 Nginx安装以及配置](http://www.souvc.com/?p=1661)

## pm2 持久化链接

`pm2`的作用就是将命令行的程序 持久化为 后台进程，即便窗口关闭，也不会影响到后台进程。因为我的项目中有一个 node server 需要持久化为后台进程。

`pm2` 是基于`node`的,并且使用`npm`安装,所以要下载 `node`,`node`中自带`npm`


### 下载node安装包

```
curl --silent --location https://rpm.nodesource.com/setup_8.x | sudo bash -
```

### 安装node

```
sudo yum -y install nodejs

```

### 切换淘宝的源 

因为 npm 默认是使用国外的库来下载的，在国内网速受限，我们使用淘宝的镜像 命令行如下

```
npm config set registry https://registry.npm.taobao.org

```
使用以下命令查看是否切换成功
```
npm config get registry
```

### 下载安装 pm2

```
npm install pm2 -g
```

### 使用 pm2

把需要持久化的程序命令前 加上`pm2` 例如

```
pm2 node server
```

但是 如果需要参数那就要再加上 `--`,然后再接参数。例如
```
pm2 node server -- 9901
```

## 项目发布 

项目发布虽然简单，但是这个问题的本质一直困扰了我很久。在此之前，常听身边的人讲起发布项目，过程听着都是如何如何复杂。自己今天弄懂了才明白是怎么一回事，也就是将自己要发布的文件夹放到可以服务器监听端口对应的目录下，然后在访问地址定位到文件夹中的文件，仅此而已！

## 遗留问题

关于`nginx`配置多个端口，本地能访问，但是使用腾讯云的公网却访问不了。试了很多方法，都未解决。希望能有前辈能够提点提点，十分感谢。本人邮箱：`1398988546@qq.com`

## 总结

通过这个过程，我意识到亲自动手实践的问题的重要性，也提高了自己对问题搜索与解决的能力。期待自已有更大的进步!