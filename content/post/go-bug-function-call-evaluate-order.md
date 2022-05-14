---
title: "Go 1.13版本引入的bug，你遇到过这个坑么？"
date: 2022-05-14T11:12:51+08:00
lastmod: 2022-05-14T11:12:51+08:00
draft: false
keywords: [go, bug]
description: "Go编译器解析函数调用顺序的bug"
tags: [go, bug]
categories: [go, bug]
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

## Bug

大家看看下面这段程序，思考下输出结果应该是什么？

```go
func main() {
  n := 0

  f := func() func(int, int) {
    n = 1
    return func(int, int) {}
  }
  g := func() (int, int) {
    println(n)
    return 0, 0
  }

  f()(g())
}
```

先思考几秒钟。。。



------

在Go 1.13版本之前，上述程序打印的结果是

```bash
1
```

在Go 1.13版本开始，上述程序打印的结果是

```bash
0
```

从习惯认知来看，编译器应该按照如下顺序执行

* step 1: evaluate `f()`，此时变量`n`的值为1
* step 2: evaluate `g()`，此时打印`n`时，因为step 1已经把`n`修改为1了，所以应该打印1
* step3:  根据step 1和step 2的结果，执行最终的函数调用

Go编译器其实也是遵循这个设计的，官方文档的描述如下：

> At package level, [initialization dependencies](https://go.dev/ref/spec#Package_initialization) determine the evaluation order of individual initialization expressions in [variable declarations](https://go.dev/ref/spec#Variable_declarations). 
>
> Otherwise, when evaluating the [operands](https://go.dev/ref/spec#Operands) of an expression, assignment, or [return statement](https://go.dev/ref/spec#Return_statements), all function calls, method calls, and communication operations are evaluated in lexical left-to-right order.

最后一句的表述可以知道，在执行函数调用时，对于函数的操作数，语法上是按照从左到右的顺序进行解析的。

这个bug其实是Google Go团队成员[Matthew Dempsky](https://github.com/mdempsky)提出来的，Go官方[认领了这个bug](https://github.com/golang/go/issues/50672)，这个bug产生的原因也很明显，就是先执行了`g()`，然后执行的`f()`。

> This is because we rewrite `f()(g())` into `t1, t2 := g(); f()(t1, t2)` during type checking.
>
> gccgo compiles it correctly.
>
> [@cuonglm](https://github.com/cuonglm) Yeah, rewriting as `t0 := f(); t1, t2 := g(); t0(t1, t2)` instead seems like a reasonable quick fix to me. I think direct function calls and method calls are probably the most common case though, so we should make sure those are still handled efficiently.
>
> Longer term, I think we should probably incorporate order.go directly into unified IR.

目前这个bug的影响范围如下：

* Go 1.13版本开始引入的这个bug，目前已经被修复，预计在1.19版本发布。
* 只有官方的编译器`gc`有这个bug，如果你使用的是`gccgo`编译器，也不会有这个问题。
* 只对多个参数的函数调用才会有这个bug，比如下面这个例子`f()`的结果是一个函数，该函数只有1个参数，就不会有本文提到的这个bug。

```go
package main

func main() {
	n := 0

	f := func() func(int) {
		n = 1
		return func(int) {}
	}
	g := func() int {
		println(n)
		return 0
	}

	f()(g())
}
```

上面程序执行结果是`1`。



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

* https://twitter.com/go100and1/status/1524410425680404485
* https://github.com/golang/go/issues/50672
* https://go.dev/ref/spec#Order_of_evaluation

![](/img/wechat.png)
