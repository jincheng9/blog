---
title: "手把手教会你如何通过ChatGPT API实现上下文对话"
date: 2023-04-01T13:44:54+08:00
lastmod: 2023-04-01T13:44:54+08:00
draft: false
keywords: [chatgpt, api]
description: "如何通过ChatGPT API实现上下文对话"
tags: [chatgpt, api]
categories: [chatgpt]
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

ChatGPT最近热度持续高涨，已经成为互联网和金融投资领域最热门的话题。

有的小伙伴可能需要在公司搭建一套ChatGPT系统，那使用ChatGPT的API显然是最好的选择。

不过ChatGPT的API都是无状态的，没有对话管理的功能。

你调用API发送一个问题(prompt)给ChatGPT，它就根据你发送的问题返回一个结果(completion)。

那如何通过ChatGPT的API实现带上下文功能的对话呢。

## ChatGPT API

ChatGPT的API实际上就是标准的HTTP接口

> https://api.openai.com/v1/chat/completions

官方封装了Python和Node.js库，可以直接使用。

我们来看一段Python代码示例：

```python
import os
import openai
# 设置API key
openai.api_key = os.getenv("OPENAI_API_KEY")

# 给ChatGPT发送请求
completion = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": "Hello!"}
  ]
)

# 打印请求结果
print(completion.choices[0].message)
```

这段代码很简单，但是有3个注意事项：

* 用到了官方的openai库，需要安装

  ```bash
  $ pip install openai
  ```

* 需要创建API key

  在下面这个链接创建你的API key

  >  https://platform.openai.com/account/api-keys

* 要使用`openai.ChatCompletion`

  openai这个库里封装了很多类



## 创建API Key

https://platform.openai.com/account/api-keys

## 



## 潜在问题



## 总结



## 开源地址

文章和示例代码开源在GitHub: [ChatGPT模型教程](https://github.com/jincheng9/gpt-tutorial)。

公众号：coding进阶。关注公众号可以获取最新Go面试题和技术栈。

个人网站：[Jincheng's Blog](https://jincheng9.github.io/)。

知乎：[无忌](https://www.zhihu.com/people/thucuhkwuji)。



## 福利

我为大家整理了一份后端开发学习资料礼包，包含编程语言入门到进阶知识(Go、C++、Python)、后端开发技术栈、面试题等。

关注公众号「coding进阶」，发送消息 **backend** 领取资料礼包，这份资料会不定期更新，加入我觉得有价值的资料。还可以发送消息「**进群**」，和同行一起交流学习，答疑解惑。



## References



![](/img/wechat.png)

