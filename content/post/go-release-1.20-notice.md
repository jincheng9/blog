---
title: "Go 1.20正式发布！"
date: 2023-02-04T12:03:15+08:00
lastmod: 2023-02-04T12:03:15+08:00
draft: false
keywords: [go1.20]
description: "Go 1.20正式发布！"
tags: [go1.20]
categories: [go1.20]
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

Go官方团队在2023.02.01发布了Go 1.20的正式release版本。

安装方法：

```bash
$ go install golang.org/dl/go1.20@latest
$ go1.20 download
```

去年2022.12.08 Go官方团队就已经发布了Go 1.20 rc1(release candidate)版本，此前我已经对Go 1.20的版本升级内容作了详细的讲解，详情如下。

## Go 1.20发布清单

和Go 1.19相比，改动内容适中，主要涉及语言(Language)、可移植性(Ports)、工具链(Go Tools)、运行时(Runtime)、编译器(Compiler)、汇编器(Assembler)、链接器(Linker)和核心库(Core library)等方面的优化。

第1篇主要涉及Go 1.20在语言、可移植性方面的优化，原文链接：[Go 1.20版本升级内容第1篇](https://mp.weixin.qq.com/s?__biz=Mzg2MTcwNjc1Mg==&mid=2247484629&idx=1&sn=60a01d3cc85ef2462156f0565c30738d&chksm=ce124bbaf965c2ac351cd9c602e8b67d5119b2a89a7f2de0289bdeb7608ae589c329eb8f7275&token=1619842941&lang=zh_CN#rd)。

第2篇主要涉及Go命令和工具链方面的优化，原文链接：[Go 1.20版本升级内容第2篇](https://mp.weixin.qq.com/s?__biz=Mzg2MTcwNjc1Mg==&mid=2247484638&idx=1&sn=459a22d4a9bf5d9715e70d3c25b05b93&chksm=ce124bb1f965c2a76bacc1135799ab268be66a861e99391b354a9f2dfd8c22a60853cc1d689d&token=1342188569&lang=zh_CN#rd)。

第3篇主要涉及Go在运行时、编译器、汇编器、链接器等方面的优化，原文链接：[Go 1.20版本升级内容第3篇](https://mp.weixin.qq.com/s?__biz=Mzg2MTcwNjc1Mg==&mid=2247484644&idx=1&sn=3c1c4d852b220595ef633f30084f3a11&chksm=ce124b8bf965c29d38c8f17702003c3531b58be15470f7b5c13f67784806a532850b79f464cc&token=1794942092&lang=zh_CN#rd)。

第4篇主要涉及Go 1.20在核心库方面的优化，原文链接：[Go 1.20版本升级内容终结篇](https://mp.weixin.qq.com/s?__biz=Mzg2MTcwNjc1Mg==&mid=2247484653&idx=1&sn=b776c4213a7bbf03400a1abe8ccca43e&chksm=ce124b82f965c29468536acda4334a8b14ede16840c2593e8a29f9b984ce2a1bd9fb026bc317&token=1226295991&lang=zh_CN#rd)。

## 注意事项

如果打算对Go 1.20的源码做编译，要求编译环境之前已经安装过Go 1.17.13或更高的版本才可以。

Go官方计划后续每年新的Go版本，如果想从源码开始编译，那编译环境会要求更高的Go版本才行。

比如Go 1.20源码编译需要依赖的最低Go版本是Go 1.17.13，到了Go 1.22，可能依赖的编译环境最低Go版本是Go 1.18。

Go 1.20增加了对RISC-V架构上FreeBSD操作系统的实验性支持。

此外，从Go 1.21开始，一些旧操作系统将会不再被支持，这包括Windows 7, 8, Server 2008 和 Server 2012, macOS 10.13 High Sierra和10.14 Mojave，届时大家又要升级操作系统啦。

## 推荐阅读

* [Go面试题系列，看看你会几题](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzg2MTcwNjc1Mg==&action=getalbum&album_id=2199553588283179010#wechat_redirect)

* [Go常见错误和最佳实践系列](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzg2MTcwNjc1Mg==&action=getalbum&album_id=2549657749539028992#wechat_redirect)

* [Go语言进阶知识](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzg2MTcwNjc1Mg==&action=getalbum&album_id=2549661543605764097#wechat_redirect)

## 开源地址

文章和示例代码开源在GitHub: [Go语言初级、中级和高级教程](https://github.com/jincheng9/go-tutorial)。

公众号：coding进阶。关注公众号可以获取最新Go面试题和技术栈。

个人网站：[Jincheng's Blog](https://jincheng9.github.io/)。

知乎：[无忌](https://www.zhihu.com/people/thucuhkwuji)。



## 福利

我为大家整理了一份后端开发学习资料礼包，包含编程语言入门到进阶知识(Go、C++、Python)、后端开发技术栈、面试题等。

关注公众号「coding进阶」，发送消息 **backend** 领取资料礼包，这份资料会不定期更新，加入我觉得有价值的资料。还可以发送消息「**进群**」，和同行一起交流学习，答疑解惑。



## References

* https://tip.golang.org/doc/go1.20
* https://go.dev/blog/go1.20

![](/img/wechat.png)

