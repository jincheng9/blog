---
title: "GitHub Pages+ Hugo + even + Utterances搭建个人博客"
date: 2021-12-21T17:02:58+08:00
lastmod: 2021-12-21T17:02:58+08:00
draft: false
keywords: [hugo github pages even utterances]
description: "GitHub Pages+ Hugo + even + Utterances搭建个人博客"
tags: [hugo, even, github pages, utterances]
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

多年前基于Jekyll和GitHub Pages搭建的博客样式总是出问题，调研了下目前最流行的静态博客主要有3个：

* hexo：基于Node.js，开源地址：[https://github.com/hexojs/hexo](https://github.com/hexojs/hexo)
* Hugo：基于Go实现，开源地址：[https://github.com/gohugoio/hugo](https://github.com/gohugoio/hugo)
* gridea：国产，个人维护，开源地址：[https://github.com/getgridea/gridea](https://github.com/getgridea/gridea)

Hugo目前GitHub上star最多，我关注的几个技术博主基本都选用的Hugo，再加上Hugo基于Go实现，有bug我也可以考虑自己解决，于是果断选择了Hugo。整体技术栈如下：

* Hugo: 静态博客网站框架

* [even](https://github.com/olOwOlo/hugo-theme-even): Hugo的配套主题 
* GitHub Pages: 网站托管
* utterances: 文章评论

## 步骤

### Hugo

1. 安装Hugo(以Mac为例)

   ```bash
   $ brew install hugo
   ```

2. 创建本地文件夹，用于存储博客内容，假设文件夹叫blog

   ```bash
   $ hugo new site blog
   ```

3. blog目录下的文件内容如下：

   ```markdown
   drwxr-xr-x   9 xxx  staff  288 12 21 14:46 .
   drwxr-xr-x  17 xxx  staff  544 12 21 14:46 ..
   drwxr-xr-x   3 xxx  staff   96 12 21 14:46 archetypes
   -rw-r--r--   1 xxx  staff   82 12 21 14:46 config.toml
   drwxr-xr-x   2 xxx  staff   64 12 21 14:46 content
   drwxr-xr-x   2 xxx  staff   64 12 21 14:46 data
   drwxr-xr-x   2 xxx  staff   64 12 21 14:46 layouts
   drwxr-xr-x   2 xxx  staff   64 12 21 14:46 static
   drwxr-xr-x   2 xxx  staff   64 12 21 14:46 themes
   ```

### even主题

给Hugo配置even主题：

1. 下载主题到blog下的themes目录，在blog所在目录执行如下命令：

   ```bash
   $ git clone https://github.com/olOwOlo/hugo-theme-even themes/even
   ```

2. 使用even主题自带的全局配置config.toml覆盖Hugo初始安装的config.toml

   ```bash
   $ cp themes/even/exampleSite/config.toml ./
   ```

3. 根据个人需要，修改blog/config.toml里的配置项，比如网站首页title，右上角菜单名称，底部链接名称等，可以参考[我的配置](https://github.com/jincheng9/blog)。

   **注意**：highlightInClient配置项和pygments开头的配置项不能都启用，都启用会导致markdown里插入代码块的时候样式错误。highlightInClient使用默认的false即可，不用修改。

4. 使用even主题自带的博客文章默认配置`themes/even/archetypes/default.md`覆盖Hugo初始安装的`archetypes/default.md`，在blog根目录执行如下命令：

   ```bash
   cp themes/even/archetypes/default.md ./archetypes/
   ```

   修改`archetypes/default.md`里的默认配置项，把comment和toc都设置为true，用于开启文章评论功能和文章自动生成目录功能，可以参考[我的配置](https://github.com/jincheng9/blog)。

5. 启动本地Hugo server，查看博客效果，本地地址：http://localhost:1313/

   ```bash
   $ hugo
   $ hugo server
   ```



### 写文章和发布

1. 在blog目录，使用`hugo new post/some-content.md`创建markdown文件

   ```bash
   $ hugo new post/test.md
   ```

2. 发布：在blog根目录执行`hugo`命令或者`hugo -t even`命令，自动根据md文件生成文章的静态页面，静态页面发布在根目录的public文件夹下面

   ```bash
   $ hugo
   ```

3. 启动Hugo server，查看网站最新效果，在blog根目录执行如下命令：

   ```bash
   $ hugo server
   ```

   

### GitHub Pages

以上流程走下来，我们就在自己本地电脑搭建好了一个基于Hugo + even主题的博客网站，但是不能被互联网访问。

我们可以借助GitHub Pages把本地博客的public文件夹内容发布到GitHub上，就可以通过互联网访问博客了。

1. 在自己GitHub上创建一个新的repo，命名为username.github.io，其中username是你的GitHub的用户名。比如我的GitHub个人主页是：[https://github.com/jincheng9](https://github.com/jincheng9)，那我的username就是jincheng9。

2. 把本地博客public目录下的内容push到GitHub的 username.github.io工程里，注意，要把git repo地址替换为你自己的。

   ```bash
   $ cd public
   $ git init
   $ git remote add origin git@github.com:jincheng9/jincheng9.github.io.git 
   $ git status
   $ git add .
   $ git commit -m 'add blog'
   $ git push origin master
   ```

3. 步骤2执行完之后，public文件夹下的内容就被推送到了你的GitHub上的username.github.io这个repo里了。

4. 等待几分钟，就可以通过https://username.github.io/ 访问你的博客了。

   

### Utterances

我们还希望给自己的博客文章添加评论功能，用来和读者互动，[utterances](https://utteranc.es/) 是一款基于 github issues 的评论系统，非常清爽，还可以避免了 disqus 的广告以及加载问题。

1. 安装utterances：点击[https://github.com/apps/utterances](https://github.com/apps/utterances)，按照提示操作，把上面存放博客的repo的权限给utterances。操作完成后，在username.github.io这个repo的Settings->Integrations里可以看到utterances。

2. 修改博客的全局配置config.toml里已有的`baseURL`和`params.utterances`这2个配置项。

   `baseURL`为博客网站的域名，`params.utterances` 为GitHub的信息。

   ```markdown
   baseURL = "http://jincheng9.github.io/"
   ```

   ```markdown
   [params.utterances]       # https://utteranc.es/
   	owner = "jincheng9"              # Your GitHub ID
   	repo = "jincheng9.github.io"     # The repo to store comments
   ```

3. 重新部署博客，就可以在文章下面看到评论区了，正如本篇文章最下面的评论区一样。

   ```bash
   $ hugo
   $ hugo server
   ```

4. **注意**：每次自己本地的修改，需要先执行`hugo`命令来更新public下的内容，再进入public目录把修改push到GitHub上，才能保证互联网访问的内容是更新后的，否则修改只会停留在本地。

   

## References

* https://github.com/olOwOlo/hugo-theme-even
* https://jdhao.github.io/2021/08/15/hugo_even_add_utterance/

![](/img/wechat.png)

