---
title: HexoBlog
date: 2018-04-01 21:13:08
tags:
- hexo
---
# Hexo 搭建个人博客（入门教程）

## 背景介绍
上周刚进饥人谷任务班的学习群时，看到群里有人在聊一个前端blog相关的东西，是借助github来实现的。在此之前自己曾经也有过这么个打算，后来可能是因为国内搜索引擎的原因，费了一些功夫也没有找到什么相关的东西，后来也就抛在脑后了。这次听到了群里的讨论，也就让我再次点燃了我想实现这么一个功能的热情。听到了一个关键的词 git pages，然后就是去google搜索，因为英文比较烂，第一篇能看懂的就是阮一峰前辈的博客了。点进去，按照教程里面讲的，借助jekyll,实现一个简单的demo。因为是demo版本，只能发布一些html，想写一个md发布也没有什么效果。接着受不了默认的没有样式的页面，就去网上找一些好看的主题过来，找到了以后fork下来，还是怎么的，也没有成功。（可能是接触的不多，不知道具体操作）后来群群里问了一下班级的新望老师，因为他用的是Hexo，jekyll不怎么熟也就推荐我去看一下hexo。我去看了hexo的教程以后，觉得简单易懂，于是就改换hexo，搭建blog。下面就是我搭建blog的步骤，中间也遇到了很多的问题，后来也解决了，结果还是很满意的。(本人是windows操作系统，接下的操作都是基于windows的操作)

<!-- more -->

## 安装node
因为node，我之前有安装过。我的电脑里已经有node和npm了。需要下载的可以去[Node官网](https://nodejs.org/en/download/)下载，根据提示一步一步安装。
## 安装 hexo
如果你已经安装好node和npm了，接下来安装hexo。
代开git bash

	$ npm install hexo-cli -g
	
然后输入命令`hexo v`查看是否安装成功，我试了一下我是没有安装成功的（没有成功它会提示`hexo command not foundm`，成功的话它会输出版本信息），然后去Google搜索了一下，接下来就是我尝试的过程了。

- 有一种说法是环境变量没有配置，我就去配置了一下系统环境变量，我安装在`D:\development\node\npm_global\node_modules`目录下，环境变量就是`D:\development\node\npm_global\node_modules\hexo-cli\bin`这个目录。然后关闭git bash 重新打开试了一下，还是不行。（怀疑是系统没有反应过来？）我就重启了电脑，再次尝试，没有成功。。。
- 于是就换另一种解决方法，那就是说可能是node的版本太高的问题。按照文章，我就安装了`nvm` node版本控制工具，切换到低一点的7.2.0版本。重新试了一下，无果。重启一下，无果。再降一个版本，试一遍，无果。这时候已经是凌晨2：00多了，没办法只能睡了，明天还要上班。
- 第二天再次尝试，一直觉得可能是环境变量那里出了什么问题，于是重新删掉，配了一遍，结果还是一样。不知道是怎么想到了，觉得可能是盘的问题？然后又去重新配了一遍环境变量，之前一直是在E盘尝试，现在改去了D盘（node所在盘，hexo所在盘）尝试，然后就成功了。。。再去E盘试一下，也可以了。。。(神奇！或许这是node，或者windows上的一个bug吧，或者不是。。。不过如果你也是我这样子的情况，你可以试一下，说不定可以少走弯路)
 
好吧，终于成功了。一个hexo安装就搞了这么久，但是接下来是一帆风顺的。
## Hexo 搭建本地blog
### 创建目录
	$ hexo miralcetjf.github.io
miracletjf是我的github上的用户名，如果你想部署到github上，最好跟你的github用户名一致。
	$ cd miracletjf.github.io
执行上一步后，hexo会自动在当前生成一个miracletjf.github.io的文件夹，里面有一些自动生成的目录和文件。进入该目录
	$ npm install
成功以后，会在你的目录下新建一个node_modules

### 生成静态页
	$ hexo clean
清楚缓存文件（db.json）和已生成的静态文件（public）
	$ hexo generate
使用Hexo 生成静态文件
### 本地服务器运行
	$ hexo sever
打开浏览器，在地址栏输入localhost://4000
## 发布文章
	$ hexo new helloHexo
然后在source/_posts目录下生成helloHexo.md文件，打开，输入`# hello World`,保存。

	$ hexo clean
	$ hexo generate
	$ hexo sever
打开locallost://4000查看变化
## 在github上部署
在github上新建仓库miracletjf.github.io
## 配置网站
打开_config.yml配置文件，修改以下配置
- title 改成你想要的名字
- author 你的名字
- type: git
- 加一行repo: 仓库地址(用ssh地址)，左边与type对其


	$ npm install hexo-deployer-git --save
安装 git部署插件
	hexo deployer
进入 github上 miracletjf.github.io仓库，打开github pages功能，如果已经打开，直接点击预览链接
## 换主题
1. [hexo主题下载站](https://github.com/hexojs/hexo/wiki/Themes)可以下载主题
2. 找到一个主题，进入github，我用的是比较常用的一个主题，[hexo-theme-next](https://github.com/iissnan/hexo-theme-next)
3. 复制ssh地址
4. 进入本地的blog部署文件夹
5. 进入themes ，`git clone `加上选中的ssh地址
6. 返回上级目录，更改_config.yml第75行theme：hexo-theme-next (你所选主题的名称)，保存
7. `hexo generate` 
8. `hexo deploy`
## 上传源代码
为了保障数据，你可能还需要上传源代码，在git上面新建一个仓库blog-generator空仓库，然后在本地blog仓库主目录下`git init`，把代码同步到github上。每次新增文章，更换主题，都要重新部署发布，并且重新github上更新源码
