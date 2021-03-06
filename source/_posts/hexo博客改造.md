---
title: hexo博客改造
date: 2019-05-07 23:33:46
tags:
  - hexo
categories:
  - [笔记, fff]
  - [测试]
---

最近有机会抽出时间把`hexo`好好改造了一下。（刚开始只是想解决一下，首页文章总是展示全文和设置标签没有出现的bug）

然后就开始动工网上各种查资料，下面把目前实现的功能和改造记录一下，希望可以帮到那些刚接触`hexo`的朋友。

本人用的是`next`主题，所以本文仅针对`next`主题配置。

<!-- more -->

## 外观

解释一下两个基本概念

* `站点配置文件` 指的是`hexo`根目录下的`_config.yml`文件
* `主题配置文件` 指的是主题包文件夹下，在本文也就是`next`文件夹下的`_config.yml`文件

### 设置查看更多

这个是最简单的，简单到很少人提及。默认情况下`next`会把多篇文章顺序列在首页，并且会把所有的内容全部列出，这就导致了首页不够简洁。之前一直受这个问题困扰，但是也没有抽出时间把它彻底解决（每次都到网上随便搜一下，没有找到直接的解决方案就被搁置了）。就在前两天，觉得有必要把自己的博客好好拾掇拾掇了，于是抱着要彻底干掉它的想法，去搜了一下，最终在一个不起眼的地方看到了它。

> 解决方案就是在文章中插入一个像注释一样的标签,标签后的内容将被隐藏，且在首页还会出现一个阅读更多的提示按钮（完美）。

``` html
<!-- more -->
```

### 主题配置

`next`一共提供了四种风格的主题：`Muse`,`Mist`,`Pisce`,`Gemini`。只需要更改`主题配置文件`中的`sheme`字段即可。

> ps: 字段名分号后要接一个空格，再填写值

``` yml
sheme: Gemini
```

### 设置菜单

编辑`主题配置文件`，修改`menu`字段。具体参照以下代码

``` yml
# 需要什么功能，只需要把对应的菜单注释取消掉即可
menu:
  home: /                     # 主页
  archives: /archives         # 归档
  #about: /about              # 关于
  #categories: /categories    # 分类
  tags: /tags                 # 标签
  #commonweal: /404.html      # 404 页面
```

> ps: 配置菜单需要事先配置好功能页，具体功能也配置可以参照下文 分类页和标签页

### 设置头像

编辑`主题配置文件`，修改`avatar`字段，值设置为头像的链接地址即可。

### 作者昵称

编辑`站点配置文件`，设置`author`为你的昵称即可。

### 站点描述

编辑`站点配置文件`，设置`description`字段为你的站点的描述。一句话签名。

## 功能

配置一下常见的功能

### 文章分类

#### 新建分类页

新建一个页面，命名为`categories`。
  
``` bash
hexo new page categories
```

#### 编辑分类页

编辑刚建的页面， 即：`/source/categories/index.md`。在顶上配置项添加`type`字段，值为`categories`

``` md
---
title: 分类
date: 2014-12-22 12:39:04
type: "categories"
---
```

#### 菜单配置

菜单配置，编辑`主题配置文件`，修改`menu`字段，把`categories`字段取消注释即可。具体操作可以查看上文`设置菜单`条目

#### 页面配置

以上我们只是新建了一个分类页，并没有给具体的页面配置类别。配置类别页，只需要在具体页面顶部配置区域，添加`categories`字段，值为当期页面的分类，例如：`- hexo`。具体参照以下示例

``` md
---
title: hexo主题配置
date: 2018-04-13 20:11:30
categories:
- hexo
---
```

#### 修改模板

我们在使用`hexo new page`时，其实都是按照一个已经定好的模板去生成的。我们可以修改这个模板，让生成新页面时自动带上`categories`字段。该文件就是`/scaffolds/post.md`，具体可以参考以下代码

``` md
---
title: {{ title }}
date: {{ date }}
categories:
---
```

### 文章标签

文章标签添加方法与分类页一致，只需要把`categories`改为`tags`即可。下面有一个示例可以看一下

``` md
---
title: hexo主题配置
date: 2018-04-13 20:11:30
categories: 
- hexo
tags:
- hexo
- md
---
```

### 百度统计

使用百度统计，可以清楚的了解到自己的站点被访问的情况，其中包括被访问的次数，访问者的网站的`ip`个数，访问者所在地区，访问的入口等一些用户可能需要关注的信息。这些都可以通过百度统计观测到，下面我来说一下配置的方法

1. 注册/登录[百度统计](https://tongji.baidu.com)，新建网站列表，查看新建的列表并定位到代码获取页面
2. 复制`hm.js?`后面的那串`统计脚本id`，参照对比下图:
![复制](https://i.loli.net/2019/05/11/5cd6e57e51199.png)
3. 编辑`主题配置文件`，修改字段`baidu_analytics`，值为刚刚让你复制的那个`统计脚本id`
4. 致此，你已经完成了，网站的`百度统计`接入。

### 阅读次数（leancloud）

上文配置了`百度统计`的功能，为了让我们更好的监测整个站点的访客，用户来源等站点层面的数据统计。而`阅读`次数则是针对每篇博文的用户浏览数进行统计的，从而更好的了解到自己的哪篇文章更受读者们的喜爱。接下来开始配置：

#### 使用leancloud

首先我们统计数据要用到数据库，我们选择使用[leancloud](https://leancloud.cn/),没有账号就先注册一个，流程很简单就不赘述了。

#### 创建应用

1. 进入控制台
2. 点击创建应用，应用名称自定义即可，我们自己用就用一个开发版的就行（反正没有很多的访问量），有需求的可以使用商用版
![创建应用](https://i.loli.net/2019/05/11/5cd6ea05a213a.png)
3. 创建`Class`，命名必须为`Counter`，必须为`Counter`，必须为`Counter`(说三遍！)。设置数据条目的默认`ALC权限`，选择`无限制`！
![创建Class](https://i.loli.net/2019/05/11/5cd6ec1fcd4d6.png)
4. 获取`AppId`以及`AppKey`，点击左侧菜单栏`设置`，然后再点击`应用key`,再这里我们就可以看到我们要的这两个`key`了(这两个key一定不能公开分享出去，不然别人也就有机会使用你的数据库来读写数据了，这是非常危险的行为)
![AppId&&AppKey](https://i.loli.net/2019/05/11/5cd6ecd152b1c.png)

#### 配置本地主题配置文件

打开本地`主题配置文件`，找到一下字段，更改对应的`app_id`和`app_key`为刚刚我们打开页面的对应值

``` yml
leancloud_visitors:
  enable: true
  app_id: #AppId
  app_key: #AppKey
```

这个时候我们已经完成了，对文章阅读次数统计的功能，重新发布后，打开文章即可在文章标题下面看到效果。

有一个注意点：记录文章访问量的唯一标识符为`文章标题`和`发布日期`两个字段，如果更改其中任何一个，都会使`统计数`变为`0`

### 搜索（Algolia）

详细的配置请参考：[第三方搜索服务-Algolia](http://theme-next.iissnan.com/third-party-services.html#algolia-search)

关于上面的配置，可能会出现点击搜索没有反应的问题。你得参考[这里解决](https://github.com/iissnan/theme-next-docs/issues/162),看点赞最高的那一个的回答。

## 升级HTTPS