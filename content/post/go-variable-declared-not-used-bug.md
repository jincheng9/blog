---
title: "轻松一刻：Go 1.18修复了一个经典bug"
date: 2022-02-02T14:49:38+08:00
lastmod: 2022-02-02T14:49:38+08:00
draft: false
keywords: [go, bug, go1.18, variable declared not used]
description: "轻松一刻：Go 1.18修复了一个经典bug"
tags: [go, bug, go1.18, variable declared not used]
categories: [go]
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

# 前言
大家在写Go的时候，初期应该都会遇到过下面的编译报错：

`xx declared but not used`

比如下面的代码示例：
```go
// example2.go
package main

func main() {
	a := 10
	a = 1
}
```

执行`go build example2.go`，会出现如下编译错误：
```bash
./example2.go:5:2: a declared but not used
```

# 原因
上面的报错，实际上是因为对于标准Go编译器，**局部变量**必须至少有一次被作为右值(r-value: right-hand-side-value)使用过。

上面的例子里局部变量a都是左值，并没有作为右值被使用过，因此编译报错。

我们来看一个新的例子，大家觉得下面的代码，执行`go build example2.go`的结果是什么？
```go
// example2.go
package main

func main() {
	a := 10
	func() {
		a = 1
	}()
}
```

# 结论
按理来说，上面的示例里头，局部变量`a`在闭包里没有被作为右值使用过，还是应该会编译报错`declared but not used`。

但是大家知道么，Go标准编译器对于`declared but not used`这样的检查，也会有bug。

上面的代码如果是Go1.18之前的版本，执行的话，结果是没有任何报错。

从Go1.18版本开始，解决了局部变量在闭包里没被使用但是编译器不报错的bug，Go1.18开始编译会报错”declared but not used“。

官方说明如下：

>The Go 1.18 compiler now correctly reports "declared but not used" errors
>for variables that are set inside a function literal but are never used. Before Go 1.18,
>the compiler did not report an error in such cases. This fixes long-outstanding compiler
>issue #8560. As a result of this change,
>(possibly incorrect) programs may not compile anymore. The necessary fix is
>straightforward: fix the program if it was in fact incorrect, or use the offending
>variable, for instance by assigning it to the blank identifier _.
>Since go vet always pointed out this error, the number of affected
>programs is likely very small.

关于这个bug修改的讨论，感兴趣的可以参考[cmd/compile: consistently report "declared but not used" errors· Issue #49214](https://github.com/golang/go/issues/49214)



# 开源地址

GitHub: [https://github.com/jincheng9/go-tutorial](https://github.com/jincheng9/go-tutorial)

公众号：coding进阶，获取最新Go面试题和技术栈

个人网站：[https://jincheng9.github.io/](https://jincheng9.github.io/)

知乎：[https://www.zhihu.com/people/thucuhkwuji](https://www.zhihu.com/people/thucuhkwuji)

![](/img/wechat.png)
