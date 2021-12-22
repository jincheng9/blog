---
title: "Hugo如何在markdown里引用本地图片"
date: 2021-12-22T10:42:18+08:00
lastmod: 2021-12-22T10:42:18+08:00
draft: false
keywords: [hugo markdown 本地图片 local images]
description: "Hugo如何在markdown里引用本地图片"
tags: [hugo, markdown, images]
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

在Hugo上写文章的时候，肯定会遇到要插入图片的情况。

我们在`content/post`目录下的markdown文件里应该怎么引用图片，才能让网站能正常显示图片呢？

这里我们其实有2种选择：

1. 使用图床：图床其实就是图片存储空间，我们可以把要插入在markdown的图片先上传到图床里，生成图片的永久链接，然后在markdown里引用这个图片链接即可。
   1. 优点：文章可以不用修改，直接复制到多个平台，而不用担心图片加载会出问题，因为图片链接是固定永久的。
   2. 缺点：免费的图床有上传数量和容量限制，还可能停用。付费的图床嘛，有钱就可以任性。。。
2. 使用本地图片：直接在markdown里引用本地的图片
   1. 优点：免费，图片和文章在一起，不用担心图片丢失
   2. 缺点：文章复制到其它平台的时候，对于图片的引用要手工修改，比较繁琐

这篇文章主要讲下如何基于Hugo，在文章对应的markdown文件里引用本地图片。

## 操作

我们先讲下Hugo的一个实现逻辑：

1. Hugo博客的根目录有一个`static`目录，这个static目录就是用来存放一些静态文件，比如图片、css、js文件等。
2. 执行`hugo`命令的时候，会把`static`目录下的子目录或文件复制到`public`目录下。比如我在`static`下添加了一个`img`子目录，并且在`img`子目录放了图片，那执行`hugo`命令后，就会把`static\img`文件的内容拷贝到`public\img`里面。
3. 大家都知道Hugo博客网站展示的其实是`public`下的内容，因此markdown文章里引用图片的时候，得引用pubic下的图片才可以。

具体操作非常简单，分2步：

1. 在`static`目录下创建`img`子目录，把markdown要使用的图片放在`static\img`目录里。

2. 在markdown文件里，按照如下格式引用图片(这里假设图片名称叫wechat.png)。这样最终public目录下生成的静态页面就可以引用到`public\img`下的图片了。

   ```markdown
   ![](/img/wechat.png)
   ```

执行`hugo`命令，启动`hugo server` ，就可以在本地看到文章里的图片了，比如下方的公众号图片。

![](/img/wechat.png)

