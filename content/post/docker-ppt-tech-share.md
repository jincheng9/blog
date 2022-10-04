---
title: "Docker技术PPT分享给大家"
date: 2022-10-02T10:19:36+08:00
lastmod: 2022-10-02T10:19:36+08:00
draft: false
keywords: [docker, container, go]
description: "docker技术PPT"
tags: [docker, container, go]
categories: [docker]
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

本周给公司同事分享了Docker容器化技术诞生的背景，技术架构，底层原理和使用场景。

演示用的源代码开源在[GitHub分布式系统笔记](https://github.com/jincheng9/disributed-system-notes/tree/main/docker/02/python-docker-demo "GitHub分布式系统笔记")。

本文把PPT分享出来供大家参考。

更多Docker的精华解析是以口述形式分享给大家的，PPT大纲可以看做学习Docker的一个内容纲要。

PPT下载地址：[docker-101-tutorial.pdf](https://github.com/jincheng9/disributed-system-notes/blob/main/docker/02/docker-101-tutorial.pdf)

![1](https://files.mdnice.com/user/25936/33c28c11-6c41-48a1-bf76-aee172fd5138.png)
![](https://files.mdnice.com/user/25936/8f9f4e93-9eb8-467b-876e-1484586a811c.png)
![](https://files.mdnice.com/user/25936/5dac1680-cfbc-439a-befb-a870fed1db6d.png)
![](https://files.mdnice.com/user/25936/efddd424-2478-4bba-acae-6be838b30d69.png)
![](https://files.mdnice.com/user/25936/234f55f1-fcf9-4f82-bab2-814df3340320.png)
![](https://files.mdnice.com/user/25936/80fab6a4-1019-4fb4-9ecc-ee44d75512fd.png)
![](https://files.mdnice.com/user/25936/cd29107e-7d26-42b8-82a1-709e2d1802ee.png)
![](https://files.mdnice.com/user/25936/b109f73e-9355-4382-bad9-1c112a8cf8ae.png)
![](https://files.mdnice.com/user/25936/643fe129-9219-4076-ad52-27738284548a.png)
![](https://files.mdnice.com/user/25936/f37a538a-141a-4d53-adaf-5a5ec0ab48f6.png)
![](https://files.mdnice.com/user/25936/7c00b0e1-dbe2-475b-bdd6-4cd7e5d3ed95.png)
![](https://files.mdnice.com/user/25936/1430ba67-53ca-4a82-9da7-3121e19e965a.png)
![](https://files.mdnice.com/user/25936/95ba998f-2f6b-445f-888e-643ac9fef8d5.png)
![](https://files.mdnice.com/user/25936/f9f95b56-90f3-4bac-9622-277ad9d9d62b.png)
![](https://files.mdnice.com/user/25936/eeb67ef4-ef61-4c62-bb38-1d47ad2a97f8.png)
![](https://files.mdnice.com/user/25936/a7d7c68c-8f77-4ea4-b4d5-5180742411d9.png)
![](https://files.mdnice.com/user/25936/86ca6c94-272f-43d7-b439-6f46dd0dabc9.png)
![](https://files.mdnice.com/user/25936/ba0477e9-fc60-406d-b347-88f0816ffb0b.png)
![](https://files.mdnice.com/user/25936/d1eb0cc9-6da1-49ad-8cad-2be038c526d5.png)
![](https://files.mdnice.com/user/25936/d39eb6ee-4704-47d7-ba23-c37820761104.png)
![](https://files.mdnice.com/user/25936/17fa0fbc-3a68-4b51-8bdf-d6f9c82eadb3.png)
![](https://files.mdnice.com/user/25936/126a2047-0b39-4537-8639-89bd00e00bb2.png)
![](https://files.mdnice.com/user/25936/37f211e3-1840-4241-9b39-506d718c8e23.png)
![](https://files.mdnice.com/user/25936/1aef94ba-9a77-4004-b383-741211140c43.png)
![](https://files.mdnice.com/user/25936/4b8284c3-8507-4c44-9d82-6d9d66de7ab7.png)
![](https://files.mdnice.com/user/25936/43b5fcd2-22f9-41e9-803e-020a687a8d95.png)
![](https://files.mdnice.com/user/25936/c0c42f32-3bb4-45d5-8a18-b68d88cf3446.png)
![](https://files.mdnice.com/user/25936/adf5c756-5277-470c-90fb-190393d0832e.png)
![](https://files.mdnice.com/user/25936/fc8da697-6984-4dac-8adb-31190c832949.png)
![](https://files.mdnice.com/user/25936/8844b02b-9ef5-4f55-a9a2-c772a5d9c028.png)
![](https://files.mdnice.com/user/25936/2b09909a-0aec-4044-90b2-20a6b0683467.png)

想要探讨技术细节的可以入群交流。
想要获取PPT原稿的可以给公众号发送消息PPT。

## 推荐阅读
* [最通俗易懂的Docker入门文字版介绍](https://mp.weixin.qq.com/s/OTULjZIOWgxlAuauUHkHVg)
* [5分钟教会你如何用Docker部署Go应用](https://mp.weixin.qq.com/s/uFc68fRFg4pGJRGr3dh3QQ)

## 开源地址

文章和示例代码开源在GitHub: [Go语言初级、中级和高级教程](https://github.com/jincheng9/go-tutorial "Go语言初级、中级和高级教程")。

公众号：coding进阶。关注公众号可以获取最新Go面试题和技术栈。

个人网站：[Jincheng's Blog](https://jincheng9.github.io/ "Jincheng's Blog")。

知乎：[无忌](https://www.zhihu.com/people/thucuhkwuji "无忌")。



## 福利

我为大家整理了一份后端开发学习资料礼包，包含编程语言入门到进阶知识(Go、C++、Python)、后端开发技术栈、面试题等。

关注公众号「coding进阶」，发送消息 **backend** 领取资料礼包，这份资料会不定期更新，加入我觉得有价值的资料。还可以发送消息「**进群**」，和同行一起交流学习，答疑解惑。

![](/img/wechat.png)

