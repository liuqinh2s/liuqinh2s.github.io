---
title: 札记
date: 2018-11-03 14:59:45
---

## 2018-11-03

No programming language is perfect. There is not even a single best language; there are only languages well suited or perhaps poorly suited for particular purposes.

Shell scripting hearkens back to the classic UNIX philosophy of breaking complex projects into simpler subtasks, of chaining together components and utilities.

>Unix的哲学就是将复杂的任务打破成多个简单的小任务，然后把工作流拼接起来。甚至在设计软件和工具型的程序时也是遵循这种思想，这种做法对程序员来说是一种直觉，这种思想就蕴含在编程中，但普通用户只喜欢傻瓜式操作。而Windows则倾向于迎合普通用户，打造集成环境，尽管许多集成环境都由不少重复的小轮子组成，但我们不缺硬盘空间和内存空间。

运用抽象是走向代码优化的第一步。面向接口编程。面向抽象编程。

消除重复部分。 优秀设计的根本是：消除重复部分！（DRY = Don’t repeat yourself）

回调和多态都是为了解耦，实现更好的模块化，升级代码时候改动更少的代码

我发现我取消Home、catagories、about、tags等原生目录之后，打开博客网站的首页，有个点会停留在第一个目录上，也就是目前的“信息科学”这个目录上。所以我想恢复首页这个目录（之前取消这个目录是因为点击我的博客名，也就是我的网名：liuqinh2s的时候，会跳转到首页，也就是网站根目录，我不想搞两个有相同作用的按钮，但其实我错了，用户并不知道那个可以点击，就像安卓的侧滑出菜单一样，用户并不知道侧滑可以出菜单，所以才需要一个菜单按钮），恢复这个首页目录是设计上的必要。

>设计思维就是要面向用户，不要以为用户知道那些隐含的功能点。

1.色彩的概念：色彩是物体发出或反射的光在视觉系统中的形成的反应，这种反应使人们得以从视觉上区分物体的大小、形状、结构和属性等外部特征。
2.色彩的三特性：亮度Luminance，色调Hue，饱和度Saturation（色调的深浅程度）各种单色光饱和度最高，单色光中掺入白光越多，饱和度越低，白光占绝大部分时，饱和度接近于零，白光的饱和度等于零。注意区分亮度和饱和度。
3.色彩模型：RGB，CMYK（颜料的三原色：C青，M品，Y黄，+K黑，应用于印刷工业）。RGB是加色模型，CMYK是减色模型。HSV（H是色相即色调，S是饱和度（取值0~100%），V是亮度值Value（取值0~100%））。YUV。

和分治算法比较类似，但不同的是分治算法把原问题划归为几个相互独立的子问题，从而一一解决，而动态规划则是针对子问题有重叠的情况的一种解决方案。