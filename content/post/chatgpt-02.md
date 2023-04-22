---
title: "一文汇总开源大语言模型，人人都可以拥有自己的ChatGPT"
date: 2023-04-16T14:58:42+08:00
lastmod: 2023-04-16T14:58:42+08:00
draft: false
keywords: [chatgpt, llm, open-source, 开源大模型]
description: "一文汇总开源大语言模型，人人都可以拥有自己的ChatGPT"
tags: [chatgpt]
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

OpenAI发布的ChatGPT火爆全球以来，全球互联网大厂陆续跟进，纷纷宣布了自家的Chat产品，如Google的Bard，百度的文心一言，阿里的通义千问等等。

这些Chat产品背后都是依赖的大语言模型(Large Language Model)。

如果是做一个垂直领域的Chat产品，有2种方案：

* 直接使用商业化产品，前提是商业化产品支持对模型做fine-tune(微调)。比如OpenAI就支持对它的基础模型做fine-tune来实现个性化的模型。
* 使用开源的大语言模型，对开源模型做fine-tune来实现垂直领域的Chat产品。

本文重点介绍有较大参考价值的开源大语言模型，方便大家快速找到适合自己应用场景的开源模型。

## 开源大语言模型

| Model      | 作者                                        | 参数量                                          | 训练数据量(tokens)                                      | 训练成本                                                     |
| :--------- | ------------------------------------------- | ----------------------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------ |
| LLaMA      | Meta                                        | 包括 70 亿、130 亿、330 亿、650 亿 4 种参数规模 | 1.4万亿                                                 | 2048个A100 GPU                                               |
| Alpaca     | Stanford                                    | 70亿                                            | 52k条问答指令数据，指令数据来源于OpenAI的API返回结果    | 500美元数据成本+100美元训练成本                              |
| Vicuna     | UC Berkeley, CMU, Stanford, UCSD and MBZUAI | 130亿                                           | 70k条问答指令数据，指令数据来源于用户分享出来的对话记录 | 300美元                                                      |
| Koala      | UC Berkeley                                 | 130亿                                           | 500k条问答直录功能数据，指令数据来源于网上公开数据集    | 在公共云计算平台上，预期训练成本不超过100美元。一台 Nvidia DGX 服务器与8个A100 GPU，需要6个小时训练完成2个epochs。 |
| Dolly 2.0  | Databricks                                  | 120亿                                           | 15k条问答指令数据，指令数据来源于Databricks员工         | 不到30美元                                                   |
| Bloom      | BigScience                                  | 1760亿                                          | 3660亿                                                  | 384 80GB A100 GPUs 训练3.5个月[数据来源](https://huggingface.co/blog/bloom-megatron-deepspeed) |
| StableLM   | Stability AI                                | 30亿、70亿、150亿和300亿                        | 1.5万亿                                                 | 未公布                                                       |
| ChatGLM    | 清华大学KEG 实验室和智谱AI                  | 60亿和1300亿共2种参数规模                       | 4000亿左右，中文和英文token各2000亿                     | 数百万人民币                                                 |
| 鹏程·盘古α | 鹏程实验室、华为                            | 26亿、130亿和2000亿共3种参数规模                | 2500亿                                                  | 2048 块昇腾处理器                                            |
| MOSS       | 复旦                                        | 160亿参数                                       | 约7000亿中英文                                          | 未公布。整体技术偏弱一些，暂时无法和ChatGLM相比。            |

注：以上汇总表格会持续在GitHub上更新，感兴趣的可以通过文末的开源地址进行关注。

开源模型有几个注意点：

* 第一，LLaMA由Meta开源，LLaMA目前仅用于学术、社会公益项目，不能用于商业化项目。

* 第二，Alpaca, Vicuna, Koala基于LLaMA衍生而来，是在LLaMA大语言模型基础上做了fine-tune得到的，因此训练成本极低，只需用比较少的指令数据做fine-tune即可。这也是为什么这几个模型的训练成本很低，因为站在了LLaMA这个巨人的肩膀上。另外，这几个模型由于本质上还是LLaMA，受限于LLaMA的license限制，同样不能用于商业化目的。
* Dolly 2.0是在EleutherAI pythia模型衍生而来，指令微调的数据集称为 databricks-dolly-15k，也已开源发布，包含来自数千名 Databricks 员工的 15,000 个高质量的人工生成的问答数据，专为指令调优大型语言模型而设计。且 databricks-dolly-15k 根据（Creative Commons Attribution-ShareAlike 3.0 Unported License）的许可条款，任何人都可以出于任何目的使用、修改或扩展此数据集，包括商业应用。
* 国内目前开源的主要就是清华主导的ChatGLM，以及华为和鹏程实验室主导的盘古alpha模型。



## 训练模型

如果拿大语言模型做训练，而不是简单的指令微调，那训练成本非常高昂，比如ChatGPT训练一次的成本在140万美元左右。

最近微软开源了DeepSpeed，可以加速大语言模型的训练，将ChatGPT 1750亿参数模型的训练成本降低到5120美元左右。

其本质是一个开源深度学习训练优化库，可以加速ChatGPT模型的训练，比目前最快的训练方法快大约15倍，如果想自己训练大语言模型的可以参考下。



## 总结

GPT模型现在真的是日新月异，很多是基于基础模型，结合问答的指令数据对模型做微调而得到的。

现在很多媒体报道的时候喜欢夸大，大家不要看到冒出一个新的开源模型就觉得多么厉害了，绝大部分都是站在巨人肩膀上做了微调而来的。

上面开源大语言模型的表格也会持续更新，欢迎大家关注下面的开源地址。



## 开源地址

持续更新的开源大语言模型开源地址： [ChatGPT模型教程](https://github.com/jincheng9/gpt-tutorial)。

公众号：coding进阶。

个人网站：[Jincheng's Blog](https://jincheng9.github.io/)。

知乎：[无忌](https://www.zhihu.com/people/thucuhkwuji)。



## 福利

我为大家整理了一份后端开发学习资料礼包，包含编程语言入门到进阶知识(Go、C++、Python)、后端开发技术栈、面试题等。

关注公众号「coding进阶」，发送消息 **backend** 领取资料礼包，这份资料会不定期更新，加入我觉得有价值的资料。还可以发送消息「**进群**」，和同行一起交流学习，答疑解惑。



## References

* https://mp.weixin.qq.com/s/7CW4p8RgAF3jYGmgefB_eg
* https://mp.weixin.qq.com/s/M-ToNk8SABoP2JG0xLUBxQ

![](/img/wechat.png)

