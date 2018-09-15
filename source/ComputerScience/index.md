---
title: 信息科学
date: 2018-04-17 10:40:39
---

信息科学是一门有趣的科学，我们的宇宙充满着各种各样的信息，而且信息量及其庞大，我觉得信息是宇宙、意识等复杂事物的本质（另一个本质是：量变与质变），虚拟世界与真实世界的唯一区别便是：

1. 信息量的大小（目前来看完全不在一个数量级）
2. 寄生关系（真实世界的真实这个属性并非绝对，而是相对，虚拟世界与所寄生的母世界相比，母世界的真实属性就更高）

好了哲思就写到这里，让我们回到脚踏实地的现在，信息科学发展到现在，最突出的部分便是计算机的发展。一个码农和计算机的纪元正火热开启，条条大路通 Computer Science。现如今编程自然不用纸带+打孔，裸机跑程序。我们的工作建立在前人的工作之上。让我们看看我们现在要学习的是什么：

## 基础理论课

1. 数据结构和算法
2. 操作系统
3. 计算机组成原理与汇编
4. 计算机网络

我列出这四门课，是因为你国内本科结束之后，考研初试要考的就是这四门课。复试的话会涉及到更深入的一点东西，比如：编译原理、软件工程、人工智能、多媒体技术、信号处理。

这里推荐一本书：

中文叫：[深入理解计算机系统](https://book.douban.com/subject/5333562/)
英文叫：[Computer Systems: A Programmer’s Perspective](https://book.douban.com/subject/26344642/)，简称 CSAPP。

>1. 有能力的话，尽量看英文版
>2. 看书要做笔记
>3. 习题不要略过，一定要做

这是 CMU（Carnegie Mellon University，卡内基梅隆大学）的计算机入门教材，这本教材对应的课程是 CS 213 ，是很多课程的先修课。

![先修课](https://i.loli.net/2018/03/25/5ab6fab31181c.jpg)

[如何阅读《深入理解计算机系统》这本书？](https://www.zhihu.com/question/20402534/answer/124950081)

看完知乎答主的回答，我们需要注意的是这门课也有一个先修课，是一门编程语言 C0（C 语言的子集），对应的课程是 CS 122，或者你学 Python 也行。CMU一般是给大二学生开这个课，学过C以后就可以上了。

这本书之所以成为圣经般的入门书，就是因为：

1. 突出原理
2. 知识点涵盖非常全
3. 讲的非常棒（可以说计算机组成原理部分比专门讲这个的书讲的更好，操作系统部分比专门讲操作系统的书讲得好，这完全就是可怕的存在）

## 理论之外

然而仅仅是学习上面的理论是完全不够的。

我这里要讲的是 ACM，`ACM国际大学生程序设计竞赛（ACM International Collegiate Programming Contest, ICPC）`的简称，由 ACM（Association for Computing Machinery，（美国）计算机协会）主办。

![ACM logo](https://i.loli.net/2018/03/25/5ab6fdd8398c7.png)

为什么 ACM 如此重要（以及 ACM 的大奖对本科生找工作的分量这么重），因为编程活动所要依赖的只有一个：抽象和逻辑能力，这就是为什么其实编程可以从小学开始（没有过高的知识门槛）。计算机科学的诸多课程，就是由先辈们靠着抽象和逻辑搭建起来的知识大厦，这些知识和经验是叶，而你抽象和逻辑能力（或者说依靠计算机解决问题的能力）是根。练习 ACM 就跟武术世界里面的李小龙每天练拳一样，是基本功。

练习 ACM 一般用 Online Judge 系统，这里我推荐几个：

- [北京大学的在线裁判系统，简称POJ](http://poj.org/)
- [杭州电子科技大学的在线裁判系统](http://acm.hdu.edu.cn/)
- [leetcode](https://leetcode.com/)

## ACM之外

ACM自然跟真正的应用程序的编写是不一样的，当你真正步入开发的时候，你会发现还有非常多的东西要学，一个看似简单的APP，其架构却是非常复杂的，代码量也远超你的想象。那么如何上手学习呢，你也不可能一步登天，一上来就开发一款应用啊。答案自然是：**一个功能，一个功能的实现，慢慢积累，有点像搭积木**。你不要觉得仅实现一个功能会拿不出手，初学者就是这样积累起来的，所以你大可以把你实现的那些小东西都整理好（这也就是我下面会写到的，做好编程札记），**文档和代码都好好保存，因为编程最忌讳的就是重复工作，从零做起**。

## 脱离 Windows 的襁褓

很多人喜欢争论 Linux 和 Windows，就像各种语言之间的争论一样，无休无止。但同样很多人喜欢说，Windows 和 Linux 无所谓，真正的开发者都是什么好用用什么，主要看需求。

而这里我要说的，跟这些都不一样，对于初学者我的建议一定是：**脱离 Windows 的襁褓**。否则你将很难对计算机的 CLI（Command Line Interface）产生直观性的体验。Linux 让你更贴近事物的原理，让你看的更加清楚，让你脱离不动脑子傻瓜式的鼠标点点点。而之所以那些高手喜欢说 Windows 很好用，没必要用 Linux，是因为他们都是脱离了低级层次，已经到达更高层次的人，他们当然无所谓，Windows 早已蒙蔽不了他们的双眼。这是一种看山还是山，看水还是水的境界。而你不经过历练，是不会到达这个境界的。

不要一见到命令行就恐惧，正如你不应该一见到英文网站和资料就恐惧一样，这种回避绝对是有害的。

对于初学者，Linux比mac os x好，因为mac也可以点点点。

>当然如果你已经脱离了低级层次，那么我绝对更推荐你用windows，其次是mac，最后是linux。别问我为什么，原因简单到我不想说：支持力度哪家强？（这里说的支持力度包括操作系统层面和应用开发层面）

## 编程心得

其实我早就想写一个系列，就叫：**编程心得**，这个里面放上很多编程中的细节问题，就跟网上的笔试面试经典一样，其实这些东西虽然吸收的快，但有些东西还是必须经过亲力亲为才能真正体会到，但我并不反对直接看这种东西，否则我就不必花心思去写了。直接看前人总结的经验非常有好处，这就像武侠世界里的传输内力，人类之所以发展起了文明，就是因为知识的传递非常迅速而有效，我们根本不需要做任何事之前都需要自己做过一遍才知道怎么做，我们往往都借助他人的经验。

说到这个话题，我觉得有必要强调一下，**编程札记** 这个东西，一直以来我都十分建议，不管是工作还是学习，都要做好记录，特别是重要的点，一定要记下来，比如实现一个东西的时候遇到的困难，以及如何解决的整个过程，有哪些关键点，等等。写这个东西是为了便于回顾，以及衡量自己的工作量。

>学而时习之，不亦说乎

目前免费的云笔记首推：有道云笔记，虽然用的没有为知笔记爽，对于穷学生来说还是比较友好的。另外人家后台是网易，而且跟网易云音乐、有道词典、网易公开课、网易云课堂、网易严选等等产品构成了一个网易的高口碑生态链，所以你一定程度上不用担心它关门倒闭。

## LaTex 和 Markdown

说到文档和笔记的编写，就得推这两个东西了：LaTex是学术界写论文的标准（现在微软的word软件似乎也有一席之地了，毕竟LaTex比word难用多了，特别是对非计算机类的学者来说），排版效果非常棒，我感觉，出版社出版，维基百科词条，也都用得上LaTex。Markdown是github上文档的标准，也是网络上各种野生博主的写作标准，最最关键的点是，mathjax这个组织，让Markdown的便捷和LaTex的强大结合起来了，你可以在Markdown中插入用LaTex书写的数学公式。**这也是我不用简书的原因，简书虽然有Markdown，却不支持mathjax，公式贴图可以啊，但你总不至于写个上下标都贴图吧。**

关于LaTax的手册，我还是希望大家自己去找，在这个时代一定要把自己的搜索能力练起来，特别是寻找 **最权威的资料，最一手的信息**，因为这样你才有可能在比特币刚诞生不久的时候就了解到它，当然很多人会说其实他们早就知道这个东西，只不过一直不想去深入了解。

当然我也不会吝啬于分享，我先讲讲我如何找这份资料：首先使用google搜索引擎，其次使用英文搜索，输入 `LaTex math symbols`，我得到的第一条就很不错：[LaTex Math Symbols](https://web.ift.uib.no/Teori/KURS/WRK/TeX/symALL.html)，第二条也不错：[LATEX Mathematical Symbols](https://reu.dimacs.rutgers.edu/Symbols.pdf)

至于要不要把它们加进你的书签，我的建议是这类东西没必要加，你可以仔细思考一下这个搜索有什么特别的，对，他加了 `math` 这个关键信息，如果你只是单纯的搜索 `LaTex` 那么你将得到大量无关的信息，但其实你平常用的时候经常就只是用到数学方面的东西。

## 访问全球网

众所周知，我们处于一个国域网当中，如果你想看看外面的世界，必须要学会翻墙。互联网时代，这是必备技能。目前我用的是 **搬瓦工+shadowsocks**。



## Java

- [Java OutputStream flush](../2018/02/27/Java-OutputStream-flush)

## C++

- [Inside the C++ Object Model 系列笔记 一 -- Object Lessons](../2017/10/19/Inside-the-C++-Object-Model-系列笔记--Object-Lessons)
- [Inside the C++ Object Model 系列笔记 二 -- The Semantics of constructors](../2017/10/19/Inside-the-C++-Object-Model-系列笔记--The-Semantics-of-constructors)
- [Inside the C++ Object Model 系列笔记 三 -- The Semantics of Data](../2017/10/21/Inside-the-C++-Object-Model-系列笔记--The-Semantics-of-Data)
- [Inside the C++ Object Model 系列笔记 四 -- The Semantics of Function](../2017/10/23/Inside-the-C++-Object-Model-系列笔记--The-Semantics-of-Function)

## PeKing University Online Judge

这个POJ相比于 leetcode 的不同就是：

出现的年代比 leetcode 这种更早，考察的范围比 leetcode 更广
需要自己处理输入输出，以及更宽泛的发挥空间（要编写一个完整的程序，包括头文件或者引用了哪些库），而 leetcode 则一般只给出一个函数，让你填写完整。

- [POJ 1001 Exponentiation](../2018/01/20/POJ-1001-Exponentiation)
- [POJ 1002 487 3279](../2018/01/21/POJ-1002-487-3279)
- [POJ 1003 Hangover](../2018/01/21/POJ-1003-Hangover)
- [POJ 1004 Financial Management](../2018/01/21/POJ-1004-Financial-Management)
- [POJ 1005 I Think I Need a Houseboat](../2018/01/21/POJ-1005-I-Think-I-Need-a-Houseboat)
- [POJ 1006 Biorhythms](../2018/02/15/POJ-1006-Biorhythms)
- [POJ 1007 DNA sorting](../2018/02/21/POJ-1007-DNA-sorting)
- [POJ 1008 Maya Calendar](../2018/02/22/POJ-1008-Maya-Calendar)
- [POJ 1009 Edge Detection](../2018/02/24/POJ-1009-Edge-Detection)

## leetcode 刷题

leetcode是一个 Online Judge 网站，在线练习编程，尤其是练习数据结构和算法相关的题。

刷OJ对于锻炼自己的计算机思维，算法能力，对常用编程语言的数据结构部分的熟练使用，都是非常有帮助的。

这个系列中，我将提供leetcode上 **所有免费题目的答案**，并提供 **详细的解答思路**，所使用的编程语言有：**Python、C++、Java、JavaScript**。

### 一些建议：

1. 按照AC（accepted）率从高到低刷题，这样就会从易到难，提供一个进步的缓冲空间，不至于一上来就被打击到。不过其实 AC 率并不真实，因为 leetcode 比较出名的缘故，所以很多题都可以在网上找到答案和解题思路。leetcode给每道题都配备了难度信息，有 Easy、Medium、Hard 三个难度等级。你也可以按照这个从易到难刷题。
2. 按照算法和数据结构体系，逐个模块的掌握，leetcode已经按照 **数据结构、公司、最受欢迎** 等等属性给题目做了划分，而且会提示哪些题目是类似的，另外网上也有不少按算法和数据结构总结的解答电子书可以参照。
3. 做不出来的时候可以看leetcode网站上的 Discuss 和 Solutions ，有不少大神的解题方法和精简代码。当然也可以在互联网上搜索，有很多讲解leetcode的博客。

我的这份教程将从 `String` 模块开始，按照 AC 率刷题。

### String

- [632. Smallest Range]()

### 未整理部分

- [2. Add Two Numbers]()
- [3. Longest Substring Without Repeating Characters]()
- [13. Roman to Integer]()
- [22. Generate Parentheses]()
- [34. Search for a Range]()
- [383. Ransom Note]()
- [541. Reverse String II]()
- [583. Delete Operation for Two Strings]()
- [606. Construct String from Binary Tree]()
- [657. Judge Route Circle]()
- [551. Student Attendance Record I]()