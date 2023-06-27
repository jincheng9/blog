---
title: "ChatGPT API重大升级"
date: 2023-06-27T20:47:59+08:00
lastmod: 2023-06-27T20:47:59+08:00
draft: false
keywords: [chatgpt]
description: "ChatGPT API重大升级"
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

## 背景

OpenAI官方在2023.06.13发布了API层面的重磅升级，主要变化如下：

* 在Chat Completions这个API里支持了开发者自定义的函数调用。
* 更新了`gpt-4`和`gpt-3.5-turbo`模型。
* `gpt-3.5-turbo`支持的上下文长度扩容到16K，之前只支持4K个token。
* embedding model的使用成本降低75%。
* `gpt-3.5-turbo`模型的input token的成本降低25%，从原来的0.002美金 / 1K token降低为0.0015美金 / 1K token。
* 2023.09.13会下线`gpt-3.5-turbo-0301`、`gpt-4-0314`和`gpt-4-32k-0314` 模型，过了这个时间点调用这些模型会请求失败。

上面提到的这些模型都严格遵循2023.03.01发布的隐私和安全规定，用户通过API发送的数据和API返回的数据不会用于OpenAI大模型的训练。

## 函数调用示例

场景：我们希望ChatGPT告诉现在Boston的天气状况。

如果只靠ChatGPT是无法实现这个功能的，因为ChatGPT的训练数据只截止到2021年9月，无法知道现在的天气。这也是GPT目前最大的一个问题，不能很好地支持信息的及时更新。

那应该怎么使用ChatGPT来实现这个功能呢？

我们可以自己定义一个函数来获取当天某个城市的天气状况，ChatGPT只需要根据用户的提问生成我们自定义的函数的参数值(也叫实参)，那我们就可以调用自定义函数拿到我们想要的结果，然后把自定义函数生成的结果和对话记录作为Prompt送给ChatGPT，由ChatGPT做一个汇总，最后把汇总的结论返回给用户即可。

>  用户提问 -> ChatGPT生成函数的实参 -> 开发者调用自定义函数 -> 把函数执行结果+上下文对话记录发送给ChatGPT做汇总 -> 返回汇总结论给用户



下面我们来看一个具体的实现案例：

* 第一步：提问`What’s the weather like in Boston right now?`

```bash
curl https://api.openai.com/v1/chat/completions -u :$OPENAI_API_KEY -H 'Content-Type: application/json' -d '{
  "model": "gpt-3.5-turbo-0613",
  "messages": [
    {"role": "user", "content": "What is the weather like in Boston?"}
  ],
  "functions": [
    {
      "name": "get_current_weather",
      "description": "Get the current weather in a given location",
      "parameters": {
        "type": "object",
        "properties": {
          "location": {
            "type": "string",
            "description": "The city and state, e.g. San Francisco, CA"
          },
          "unit": {
            "type": "string",
            "enum": ["celsius", "fahrenheit"]
          }
        },
        "required": ["location"]
      }
    }
  ]
}'
```

拿到ChatGPT返回的结果，返回结果里content为null，function_call有值，表示需要调用自定义函数`get_current_weather`，并且返回了自定义函数的参数值。

```bash
{
  "id": "chatcmpl-123",
  ...
  "choices": [{
    "index": 0,
    "message": {
      "role": "assistant",
      "content": null,
      "function_call": {
        "name": "get_current_weather",
        "arguments": "{ \"location\": \"Boston, MA\"}"
      }
    },
    "finish_reason": "function_call"
  }]
}
```



* 第二步：调用自定义函数。

```plaintext
curl https://weatherapi.com/...
```

拿到自定义函数返回结果

```bash
{ "temperature": 22, "unit": "celsius", "description": "Sunny" }
```

* 第三步：把自定义函数执行结果和上下文对话记录发送给ChatGPT做汇总。

```bash
curl https://api.openai.com/v1/chat/completions -u :$OPENAI_API_KEY -H 'Content-Type: application/json' -d '{
  "model": "gpt-3.5-turbo-0613",
  "messages": [
    {"role": "user", "content": "What is the weather like in Boston?"},
    {"role": "assistant", "content": null, "function_call": {"name": "get_current_weather", "arguments": "{ \"location\": \"Boston, MA\"}"}},
    {"role": "function", "name": "get_current_weather", "content": "{\"temperature\": "22", \"unit\": \"celsius\", \"description\": \"Sunny\"}"}
  ],
  "functions": [
    {
      "name": "get_current_weather",
      "description": "Get the current weather in a given location",
      "parameters": {
        "type": "object",
        "properties": {
          "location": {
            "type": "string",
            "description": "The city and state, e.g. San Francisco, CA"
          },
          "unit": {
            "type": "string",
            "enum": ["celsius", "fahrenheit"]
          }
        },
        "required": ["location"]
      }
    }
  ]
}'
```

最后ChatGPT返回如下结果：

```bash
{
  "id": "chatcmpl-123",
  ...
  "choices": [{
    "index": 0,
    "message": {
      "role": "assistant",
      "content": "The weather in Boston is currently sunny with a temperature of 22 degrees Celsius.",
    },
    "finish_reason": "stop"
  }]
}
```

我们输出结果：

```bash
The weather in Boston is currently sunny with a temperature of 22 degrees Celsius.
```



以上功能实现的核心要素是ChatGPT可以智能地根据用户的输入来判断什么时候应该要调用开发者的自定义函数，并且把自定义函数的参数值给返回。开发者就可以直接自己去调用自定义函数拿到想要的结果，最后再把对话记录和自定义函数执行结果发送给大模型去做汇总。

目前这个功能可以在 `gpt-4-0613` 和 `gpt-3.5-turbo-0613`这2个模型里使用。

等OpenAI在2023.06.27完成模型升级后，`gpt-4`、`gpt-4-32k`和`gpt-3.5-turbo`模型也可以使用这个功能。

新API的使用详情可以参考：[developer documentation](https://platform.openai.com/docs/guides/gpt/function-calling)。

## 新模型

### GPT-4模型

`gpt-4-0613` 相对于`gpt-4`，新增了函数调用的支持。

`gpt-4-32k-0613` 相对于`gpt-4-32k`，同样是新增了函数调用的支持。

在接下来的几周里，OpenAI会把GPT-4 API waiting list上的申请都尽量审批通过，让开发者可以享用到GPT-4的强大能力。还没申请的赶紧去申请吧。

### GPT-3.5 Turbo模型

`gpt-3.5-turbo-0613` 相对于`gpt-3.5-turbo`，新增了函数调用的支持。

`gpt-3.5-turbo-16k` 支持的上下文长度扩容到了16K，是`gpt-3.5-turbo`的4倍，费用是`gpt-3.5-turbo`的2倍。具体费用是每1K input token需要0.003美金， 每1K output token需要0.004美金。

### 旧模型下线时间

从2023.06.13开始，OpenAI会开始升级生产环境的`gpt-4`、`gpt-4-32k`和`gpt-3.5-turbo`模型到最新版本，预计2023.06.27开始就可以使用到升级后的模型了。

如果开发者不想升级，可以继续使用旧版本的模型，不过需要在model参数里指定用
 `gpt-3.5-turbo-0301`，`gpt-4-0314` 或 `gpt-4-32k-0314` 。

这些旧版本的模型在2023.09.13会下线，后续继续调用会请求失败。

## 更低价格

### Embedding模型

`text-embedding-ada-002`目前是OpenAI所有embedding模型里最受欢迎的。

现在使用这个embedding模型的成本降低为0.0001美金/1K token，成本下降75%。

### GPT-3.5 Turbo模型

`gpt-3.5-turbo` 模型在收费的时候，既对用户发送的问题(input token)收费，也对API返回的结果(output token)收费。

现在该模型的input token成本降低25%，每1K input token的费用为0.0015美金。

output token的费用保持不变，还是0.002美金/1K token。

`gpt-3.5-turbo-16k` 模型的input token收费是0.003美金/1K token，output token收费是0.004美金/1K token。



## 总结

文章和示例代码开源在GitHub: [GPT实战教程](https://github.com/jincheng9/gpt-tutorial)，可以看到所有主流的开源LLM。

公众号：coding进阶。关注公众号可以获取最新GPT实战内容。

个人网站：[Jincheng's Blog](https://jincheng9.github.io/)。

知乎：[无忌](https://www.zhihu.com/people/thucuhkwuji)。



## References

* https://openai.com/blog/function-calling-and-other-api-updates

![](/img/wechat.png)

