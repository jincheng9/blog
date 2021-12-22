---
title: "Hugo如何添加新页面"
date: 2021-12-22T20:53:34+08:00
lastmod: 2021-12-22T20:53:34+08:00
draft: false
keywords: [hugo, add page, 新增页面, 新页面]
description: "hugo添加新页面，hugo右上角添加新页面"
tags: [hugo]
categories: [website]
author: "jincheng9"

# You can also close(false) or open(true) something for this content.
# P.S. comment can only be closed
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: false
hiddenFromHomePage: false
# You can also define another contentCopyright. e.g. contentCopyright: "This is another copyright."
contentCopyright: false
reward: false
mathjax: false
mathjaxEnableSingleDollar: false
mathjaxEnableAutoNumber: false

# You unlisted posts you might want not want the header or footer to show
hideHeaderAndFooter: false

# You can enable or disable out-of-date content warning for individual post.
# Comment this out to use the global config.
#enableOutdatedInfoWarning: false

flowchartDiagrams:
  enable: true
  options: ""

sequenceDiagrams: 
  enable: true
  options: ""

---

## 前言

Hugo默认在博客首页右上角有4个页面，分别是Home, Archives, Tags和Categories。

对这4个页面做增删改操作，可以直接操作`config.toml`配置文件。

本文展示如何在Hugo博客网站的右上角添加一个新的`关于`页面。总体逻辑是把根据文章markdown文件自动生成的html文件作为新页面跳转路径。

## 操作

1. 修改`config.toml`，在博客菜单目录配置项后面添加一个新的`关于`页面。 

   ```markdown
   [[menu.main]]             # config your menu              # 配置目录
     name = "首页"
     weight = 10
     identifier = "home"
     url = "/"
   [[menu.main]]
     name = "全部文章"
     weight = 20
     identifier = "archives"
     url = "/post/"
   [[menu.main]]
     name = "标签"
     weight = 30
     identifier = "tags"
     url = "/tags/"
   [[menu.main]]
     name = "分类"
     weight = 40
     identifier = "categories"
     url = "/categories/"
   [[menu.main]]
     name = "关于"
     weight = 40
     identifier = "about"
     url = "/about/" 
   ```

   url表示该页面的url地址。

2. 新增博客markdown文件

   博客markdown文件名称要和url里的路径名称相同，比如上面的url路径名称是`about`，那我们创建的博客的markdown文件名称也叫`about`，命令如下：

   ```bash
   $ hugo new post/about.md
   ```

   把`about.md`里的内容修改为新页面想展示的内容，注意把markdown文件开头的`draft`设置为`false`，因为这个配置项如果是`true`，那执行下面的`hugo`命令就不会生成新页面。

3. 执行`hugo`命令，自动根据markdown文件生成新页面

   ```bash
   $ hugo
   ```

   新页面位于`public\post\about`目录，`about`目录下就一个`index.html`文件。

4. 拷贝新页面到`public`根目录：把上面的`about`目录完整拷贝一份到`public`根目录。

   拷贝后的目录结构包含`public\about\index.html`。

5. 再次修改上面创建的markdown文件，`draft`配置项设置成`true`，执行`hugo`命令。

   这一步操作是为了不让这个文章出现在博客的`首页`和`全部文章`里面，如果你想出现在博客`首页`和`全部文章`里，这一步可以省略。

6. 更新public目录到托管平台，就可以在互联网看到了修改后的效果了。比如我用的GitHub Pages，那就直接把public目录的内容push到GitHub上即可。

   注意：新页面在自己本地电脑访问localhost的域名会报404，推送到托管平台访问是正常的。

![](/img/wechat.png)

