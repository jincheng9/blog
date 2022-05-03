---
title: "Go 1.18版本iota的bug"
date: 2022-05-03T11:45:33+08:00
lastmod: 2022-05-03T11:45:33+08:00
draft: false
keywords: [go1.18, iota, bug]
description: "Go 1.18版本iota的bug"
tags: [go1.18, iota, bug]
categories: [go1.18, iota, bug]
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

## Iota

`iota`是Go语言的[预声明标识符](https://go.dev/ref/spec#Predeclared_identifiers)，用于常量的声明。

`iota`的值是`const`语句块里的行索引，值从0开始，每次递增加1。通过下面的代码示例我们先回顾下`iota`的特性。

```go
const (
	c0 = iota  // c0 == 0
	c1 = iota  // c1 == 1
	c2 = iota  // c2 == 2
)

const (
	a = 1 << iota  // a == 1  (iota == 0)
	b = 1 << iota  // b == 2  (iota == 1)
	c = 3          // c == 3  (iota == 2, unused)
	d = 1 << iota  // d == 8  (iota == 3)
)

const (
	u         = iota * 42  // u == 0     (untyped integer constant)
	v float64 = iota * 42  // v == 42.0  (float64 constant)
	w         = iota * 42  // w == 84    (untyped integer constant)
)

const x = iota  // x == 0
const y = iota  // y == 0
const (
	class1 = 0
	class2 // class2 = 0
	class3 = iota  //iota is 2, so class3 = 2
	class4 // class4 = 3
	class5 = "abc" 
	class6 // class6 = "abc"
	class7 = iota // class7 is 6
)
```



## Bug

2022年3月15日，Go官方团队正式发布了Go 1.18版本。Go 1.18是Go语言诞生以来变化最大的版本，引入了泛型、Fuzzing、工作区模式等众多新功能和性能优化。

天下没有无bug的系统，Go当然也不例外。Go 1.18引入了一个和`iota`相关的bug。

大家看看下面这段程序，思考下输出结果应该是什么？

```go
package main

import "fmt"

const C1 = iota
const C2 = iota

func test1() {
	fmt.Println("C1=", C1, " C2=", C2)
}

func main() {
	test1()
}
```

先思考几秒钟。。。



------

在Go 1.18版本之前，上述程序打印的结果是

```bash
C1= 0  C2= 0
```

在Go 1.18版本，上述程序打印的结果是

```bash
C1= 0  C2= 1
```

很显然，这是一个bug，因为`const C1 = iota`和`const C2 = iota`是互相独立的`const`语句块，因此这2个`const`声明里的`iota`的值都是0。

Go官方也[认领了这个bug](https://github.com/golang/go/issues/52438)，Go语言的主要设计者Robert Griesemer解释了这个bug产生的原因：

> No need to bisect. This is due to a completely new type checker, so it won't be useful to pin-point to a single change. I've identified the bug and will have a fix in a little bit.
>
> This is clearly a bad bug; but only manifests itself when using iota outside a grouped constant declaration, twice.
>
> As a temporary work-around, you can change your code to:
>
> ```
> // OpOr is a logical or (precedence 0)
> const (OpOr Op = 0 + iota<<8)
> 
> // OpAnd is a logical and (precedence 1)
> const (OpAnd Op = 1 + iota<<8)
> ```
>
> (put parentheses around the const declarations).

产生这个bug是由于Go引入了全新的类型检查器导致的。

这个bug只有在**全局**、**未分组**的常量声明才会出现。

我们用括号`()`将声明包起来，也就是使用分组的常量声明，就不会有这个bug了。

```go
package main

import "fmt"

const (
	C1 = iota
)
const (
	C2 = iota
)

func test1() {
	fmt.Println("C1=", C1, " C2=", C2)
}

func main() {
	test1()
}
```

上面程序的执行结果是：

```bash
C1= 0  C2= 0
```

而且，对于局部常量声明也是不会有这个bug。

```go
package main

import "fmt"

func test1() {
	const C1 = iota
	const C2 = iota
	fmt.Println("C1=", C1, " C2=", C2)
}

func main() {
	test1()
}

```

上面程序的执行结果是：

```bash
C1= 0  C2= 0
```



## 推荐阅读

* 泛型
  * [泛型：Go泛型入门官方教程](https://github.com/jincheng9/go-tutorial/blob/main/workspace/senior/p6)
  * [泛型：一文读懂Go泛型设计和使用场景](https://github.com/jincheng9/go-tutorial/blob/main/workspace/senior/p7)
  * [泛型：Go 1.18正式版本将从标准库中移除constraints包](https://github.com/jincheng9/go-tutorial/blob/main/workspace/senior/p17)
  * [泛型：什么场景应该使用泛型](https://github.com/jincheng9/go-tutorial/blob/main/workspace/official-blog/when-to-use-generics.md)
* Fuzzing
  * [Fuzzing: Go Fuzzing入门官方教程](https://github.com/jincheng9/go-tutorial/blob/main/workspace/senior/p22)
  * [Fuzzing: 一文读懂Go Fuzzing使用和原理](https://github.com/jincheng9/go-tutorial/blob/main/workspace/senior/p23)
* 工作区模式
  * [Go 1.18：工作区模式workspace mode简介](https://github.com/jincheng9/go-tutorial/blob/main/workspace/senior/p25)
  * [Go 1.18：工作区模式最佳实践](https://github.com/jincheng9/go-tutorial/blob/main/workspace/official-blog/go1.18-workspace-best-practice.md)



## 开源地址

文章和示例代码开源在GitHub: [Go语言初级、中级和高级教程](https://github.com/jincheng9/go-tutorial)。

公众号：coding进阶。关注公众号可以获取最新Go面试题和技术栈。

个人网站：[Jincheng’s Blog](https://jincheng9.github.io/)。

知乎：[无忌](https://www.zhihu.com/people/thucuhkwuji)



## References

* https://twitter.com/go100and1/status/1517066479316836355
* https://github.com/golang/go/issues/52438
* https://go.dev/ref/spec#Iota

![](/img/wechat.png)

