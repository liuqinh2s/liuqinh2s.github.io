---
title: Hexo+Markdown+MathJax搭建个人博客
tags: [DIY]
comments: true
---

## 为什么搭建个人博客

其实有想过用简书写博客，但简书不支持 `mathjax`，而我写作的时候要用到不少数学公式。

以前也用过 leanote，也就是现在的 蚂蚁笔记，但蚂蚁笔记的博客中，[TOC] 是有 mathjax 渲染的，但右上角的目录导航却是没有加 mathjax 渲染的，这样看着会相当别扭。另外还有几个原因：

1. leanote收费
2. 无法彻底的自定义
3. 博客不像使用静态网站引擎那样直观的以文件的形式展示在我面前

<!-- more -->

## 为什么选 hexo

为什么选择 `hexo`，而不是 `jekyll`，或者 `hugo`。

其实我以前的博客是用的 jekyll，弃用 jekyll 是因为这东西实在是太慢了，我更新文章之后无法立即看到结果，要刷新很多次，或者说要等很久，git pages 上才会显示新的东西。听说 hexo 和 hugo 的速度都比较快，所以就换了 hexo，hexo 的主题比 hugo 多，另外 hugo 的官网和主题网站访问实在太慢了，成功恶心到了我。所以我最后选了 hexo，用上了经典主题 next。现在来说，主要是next主题吸引我，而hugo的next主题太简陋了。

>hexo 是用 nodejs 写的，jekyll 是用 ruby 写的，hugo 是用 go 语言写的，wordpress 是用 php 实现的。

那为什么不用 wordpress 呢，因为我想用 git pages 这个平台，而这个平台只支持静态博客。

## 搭建过程

### 环境配置

首先你要安装 `git` 和 `npm`，git 是一种版本控制工具，npm 则是 nodejs 的包管理工具。

mac 上，使用 brew 和 brew cask 可以像许多 Linux 系统一样直接通过命令行安装软件。

```
brew install git
brew install node
```

另外很不幸的是 git 和 npm 在国内都是无法愉快的使用的，虽然没有被墙，但是速度奇慢无比。于是我们需要做些工作：

给 git 挂代理：

```
git config --global http.proxy https://127.0.0.1:1087
git config --global https.proxy https://127.0.0.1:1087
```

但为了实现上面的功能，首先你得有个翻墙代理。关于翻墙都可以额外写篇文章了。

想看详细的解决办法：

- https://www.zhihu.com/question/27159393
- https://www.zhihu.com/question/27159393/answer/141047266

然后给 npm 换源：

```
npm config set registry https://registry.npm.taobao.org
npm info underscore （这个只是为了检验上面的设置命令是否成功，若成功，会返回[指定包]的信息）
```

想看更详细的解决办法：

- https://segmentfault.com/a/1190000007829080

好了，之后就是

- hexo 安装
- 初始化 blog 目录
- 然后 hexo server 开启本地服务器，一个 demo 就出现啦。

命令如下：

```
npm install hexo-cli -g
hexo init blog
hexo server
```

### 基本的建站过程

#### 从 jekyll 迁移到 hexo

我是从 jekyll 迁移过来的，所以先把文章全都拷贝进 `source/_posts` 目录下面，然后修改 `_config.yml`，把：

```
new_post_name: :title.md
```

变成：

```
new_post_name: :year-:month-:day-:title.md
```

官网迁移教程：https://hexo.io/zh-cn/docs/migration.html

#### 下载 next 主题并添加 mathjax

然后下载一个 `next` 主题：

```
git clone https://github.com/iissnan/hexo-theme-next themes/next
```

然后修改 `_config.yml`，把：

```
theme: landscape
```

变成：

```
theme: next
```

然后修改 `next` 的 `_config.yml`，把：

```
mathjax:
  enable: false
  per_page: false
  cdn: //cdn.bootcss.com/mathjax/2.7.1/latest.js?config#TeX-AMS-MML_HTMLorMML
```

变成：


```
mathjax:
  enable: true
  per_page: false
  cdn: //cdn.bootcss.com/mathjax/2.7.1/latest.js?config#TeX-AMS-MML_HTMLorMML
```

>注意`per_page`不能是true，一定要是false。

#### 解决 markdown 与 mathjax 的冲突

为了解决 `markdown` 下划线转义成 `<em>` 标签（HTML标签），从而导致 `mathjax` 的下标无法使用，这个问题，我们修改 `marked.js` 文件，如果你使用的是 `sublime text` 或者 `Atom` 编辑器，`cmd+o`打开你的博客目录，然后 `cmd+p` 输入你要在此目录下找的文件名：`marked.js` 就可以找到这个文件。这个文件的是：`node_modules/marked/lib/marked.js`。

总共发现 mathjax 中的三处冲突：

1. `_`变成了`<em>`，造成数学公式下标无法显示
2. `\\`变成了单个`\`，数学公式`\begin{case}...\end{case}`之间换行需要用到`\\`
3. `< xxx >`大于号小于号之间会新增一个 `#""`

将

```
escape: /^\\([\\`*{}\[\]()# +\-.!_>])/,
```

改为

```
escape: /^\\([`*{}\[\]()# +\-.!_>])/,
```

这样就去掉了，双斜杠转义。

把

```
em: /^\b_((?:[^_]|__)+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
```

改为

```
em:/^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
```

这样就禁掉了 `_` 变 `<em>`（斜体标记）。

最后，为了解决第三个冲突，我把 `>` 写成了 HTML 实体形式：`&gt;`，这样就无法组成一对尖括号了，终于不会冲突了。

我为什么不装个 `hexo-renderer-pandoc` + `pandoc` ，说出来都是泪啊，装了啊，一执行就报错，google 了一圈，没有有用的解答，遂作罢。

**2019-01-11更新**：这种修改源码的方式其实是及其不推荐的，因为如果重新npm install的话，修改就作废了。现在的`hexo-renderer-marked`已经修复了`_`和`>`的问题，但是依然存在`\\`的问题。有一个渲染器`hexo-renderer-kramed`，以上三个问题都完美解决。

#### 解决语言不正确的问题

我发现有些地方居然默认的是德语还是什么其他语言，反正不是英语，所以我们需要改：

- 根目录下的 `_config.yml`
- next 主题的 `_config.yml`

两个都改成：

```
language: en
```

#### 生成 public 静态网站目录 和 部署到 github

生成静态网站目录：

```
hexo g
```

下载 hexo-deployer-git 插件：

```
npm install hexo-deployer-git --save
```

再修改 `_config.yml`，把：

```
deploy:
  type:
```

变成：

```
deploy:
  type: git
  repo: https://github.com/liuqinh2s/liuqinh2s.github.io
  branch: master
```

然后，用命令 `hexo d` 部署就行了，不过首先你得有个 github 账号，然后还得有个叫 `liuqinh2s.github.io` 的项目，然后你还得配置好 github 环境：

```
git config --global user.name "Your Name Here"
git config --global user.email "your_email@example.com"
```

然后把公钥的内容传给 github 就行了。这里只说原理，具体的操作懒得贴了。

>只有多懂原理（哪怕只是基本的原理），你才能顺利解决遇到的诸多问题。

官网的部署教程：https://hexo.io/zh-cn/docs/deployment.html

基本的建站就结束了，然后就是慢慢把博客进行个性化吧。

### hexo 个性化配置

- hexo-reference，用来支持 markdown 脚注的
- hexo-generator-seo-friendly-sitemap，sitemap用来喂给搜索引擎的，更好的爬取网站
- hexo-generator-search，博客内部搜索
- hexo-wordcount，统计字数用的

然后就是调 next 主题，把自己喜欢的特性用上。

然后就是加上 `不蒜子`，百度统计这类统计工具，和 disqus 评论等等。

## 遇到的问题以及解决方案

### 如何使用HTML锚点

如果不了解HTML锚点，可以参考这个：

- [w3school -- HTML 链接](http://www.w3school.com.cn/html/html_links.asp)
- [百度百科 -- 锚点](https://baike.baidu.com/item/%E9%94%9A%E7%82%B9)。HTML可以在页面内跳转，只需要定义一个锚点，访问的时候 **将 `#` 符号和锚名称添加到 URL 的末端**。

markdown本身是没有这个功能的，所以我们直接把标题用HTML写出来就行了。标题的对应是 `#` 到 `######` 总共6级，分别对应 `<h1>` 到 `<h6>`。

举个例子：`<h4 id="3.2.3">解决 markdown 与 mathjax 的冲突</h4>`

这里要注意的是：**不要使用`name`属性，而必须使用`id`属性，否则会不起作用**

实际上可以使用一个markdown插件来实现：上标、下标、锚点、脚注。

```
npm un hexo-renderer-marked --save
npm i hexo-renderer-markdown-it --save
```

锚点的用法，其实可以先`hexo g`一下，然后看看生成的HTML长什么样，就知道改怎么引用锚点了，经我观察，空格会被渲染成`-`，比如一个四级标题：

```
#### 解决 markdown 与 mathjax 的冲突
```

会被渲染成：

```html
<h4 id="解决-markdown-与-mathjax-的冲突"><a class="header-anchor" href="#解决-markdown-与-mathjax-的冲突">¶</a>解决 markdown 与 mathjax 的冲突</h4>
```


