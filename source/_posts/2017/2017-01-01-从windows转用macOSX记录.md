---
title: 从 windows 转用 mac OS X 记录
date: 2017-01-01
categories: [数码]
tags: [工具]
comments: true
---

这篇文章我想讲讲我从用 Windows 到用 mac OS X 的过程中遇到的困难和解决方法。

由于我本人是学计算机的，电脑用的多，喜欢用一些设计的好的快捷键，所以我分为两部分吧，第一部分讲给普通用户，第二部分讲给程序员。

先告诉你们一个通用小技巧，众所周知，flash视频非常让电脑发热，而看HTML5视频则一点都不热。safari有个伪装成iPad或者iPhone的功能，可以把视频强制用HTML5播放。你的MacBook就再也不会在看视频的时候发烫了。

![safari强制使用HTML5播放视频](https://wx4.sinaimg.cn/mw690/006zFO3ggy1fcd4d2uzibj31kw0zkdug.jpg)

时至今日（2017.9.9），很多网站都可以使用 HTML5播放视频了，如：bilibili，中国大学MOOC，使用 HTML5播放视频是未来的方向。

## 普通用户使用mac指南

### 浏览器

找不到浏览器吗？点击像指南针图标的那个safari，那个应用就是浏览器。

### 输入法

和初次使用Linux一样，一上来我们肯定就要碰到输入法的难题，mac切换输入法的快捷键是command+space（空格），一般的系统或者软件都是支持自定义快捷键的，你可以修改快捷键。对于用惯了搜狗输入法的同学，这里我推荐使用搜狗输入法，它可以记住用户习惯。

![mac输入法截图](https://wx4.sinaimg.cn/mw690/006zFO3ggy1fcceny2wsuj31kw0zkdku.jpg)

搜狗默认的通用中英文切换键是 shift。

### 触摸板

mac的触摸板应该是所有笔记本电脑触摸板的宗师吧，而且也是最好用的触摸板。我用mac的时候，除了玩游戏，完全用不着鼠标。

没有鼠标，那么左右键怎么办？很简单：一指单击触摸板，就是左键，两指单击触摸板，就是右键。记得将轻触触摸板设置为点击，这样可以延长触摸板的寿命，而且点起来也舒服多了。

五指合拢，你可以看到你电脑上安装好的应用，这个界面叫做：launchpad。

![launchpad截图](https://wx2.sinaimg.cn/mw690/006zFO3ggy1fccexd55wpj31kw0zktgg.jpg)

五指弹开，如果你处在launchpad界面，五指弹开就会回到原先五指合拢之前的那个界面，回到这个界面之后，再用一次五指弹开，就是弹开所有应用，让你看到桌面（如果你当前窗口是全屏模式，则此法失效）。

![弹开应用看到桌面](https://wx3.sinaimg.cn/mw690/006zFO3ggy1fccf5tp4rpj31kw0zkjwj.jpg)

三指上推，可以看到所有打开的窗口的缩略图。

![窗口缩略图](https://wx4.sinaimg.cn/mw690/006zFO3ggy1fccf0mfcanj31kw0zkqcm.jpg)

三指横扫，可以在全屏之间切换，这是我最喜欢的设计之一。

### 安装应用

去官网下好应用，通常是.dmg后缀的安装包，打开，拖拽到Applications文件夹就完成了安装，比Windows爽多了吧。

### 文件管理器Finder

调整默认Finder窗口大小，可以按住option，然后进行拖动，窗口的大小就可以保留下来了。

command+shift+.可以显示隐藏文件，关闭显示隐藏文件也是这个快捷键。

右键之后，没有新建文件选项。

你只能先打开应用，然后在应用中新建文件。

记住在Windows上用的很爽的ctrl+a、ctrl+s、ctrl+c、ctrl+v、ctrl+f等等，在mac上统统把ctrl换成command就OK了。

command+x不能剪切文件和文件夹是吧。

解决办法是：先command+c，然后command+option+v，成功的完成文件的剪切和粘贴。

普通用户的指南到此结束，如果有什么问题，请私信我的知乎，我博客头像下面有个知乎的知的图标，点击就能到我的知乎个人主页。

## 程序员mac指南

先上几个零碎的小技巧：

触摸板双指双击，放大（我觉得其实没啥卵用）

在应用窗口顶栏双击，缩放窗口（这个Windows也有的）

### 文件管理器Finder

首先讲Finder，这是mac 系统，也就是OS X系统的文件管理器，和windows的文件管理器对比有点不同，最不方便的地方在于点击右键没有新建文件选项，如果你会Linux，可以用命令touch 文件名来新建文件，如果你不会命令，可以先打开文本编辑器，再新建文本文件，它的设计哲学是，你要新建什么文件，就先打开与这个文件相关的软件，再在这个软件里新建这个类型的文件，不过不方便就是不方便。再讲讲优秀的地方，Finder可以打开多级目录。这样你就能用上下方向键快速扫视本层路径下的下一层的内容。

![Finder多级目录](https://ww4.sinaimg.cn/mw690/005DrjN1gw1fbgxqax4waj316s0o8n0e.jpg)

另外可以 按空格键预览，这个预览功能现在 Windows 也有了。

Finder还有tag系统，你可以通过tag来访问属于同一个tag的文件

![Finder的tag功能](https://ww3.sinaimg.cn/mw690/005DrjN1gw1fbgxwxxpobj316s0o8ad8.jpg)

### 快捷键

>快捷键肯定是最重要的啦！

然后讲讲快捷键设计，我在windows下已经有了些使用快捷键的习惯，比如：

win+E是打开文件管理器

win+D是显示桌面（也就是将所有窗口最小化）

win+L锁屏

win+R打开运行程序输入框

ctrl+S保存

ctrl+A全选

ctrl+C复制

ctrl+X剪切

ctrl+V粘贴

ctrl+F查找

当我来到OS X下的时候，我同样先摸索了一番快捷键。设计快捷键的哲学就是通用，这样就可以节省记忆成本，mac上的cmd基本上承担了windows上ctrl的职能，常用的快捷键有：

cmd+S保存

cmd+A全选

cmd+C复制

cmd+X剪切

cmd+V粘贴

cmd+F查找

cmd+D制作副本

按住option+command，拖动文件，可以产生快捷方式，也就是链接。制作链接这个我很少用

`cmd+,` 打开preferance也就是打开设置界面，这个快捷键简直不要太爽，希望 Windows 上也弄一个

cmd+N 打开新窗口

cmd+W 关闭窗口

cmd+H 隐藏窗口

cmd+Q 退出程序

cmd+ctrl+F 全屏和退出全屏，播放视频时候退出全屏一般是 ESC 键，还有个不错是设计是双击切换全屏，大部分视频网站都支持（b站不支持这个，真的遗憾）。

cmd+Tab切换程序，windows上是alt+tab

你用了Finder之后可能会恼火没有剪切文件的功能，但其实是有的，只是快捷键不一样，剪切文件的快捷键是，先cmd+C复制，然后cmd+option+V粘贴。

这是我常用的几个快捷键。总之快捷键不用记很多，按你自己的需要，记住常用的就行。

Command+Option+D 控制Dock的显示与隐藏，我还是喜欢留着 Dock。

Command+Option+H 隐藏所有其他窗口

Shift+音量 会有声音，直接按音量默认是不出声

Command+i 显示简介（在Finder中），然后就 可以修改打开一个文件的默认APP

Command+Control+N 新建一个文件夹，并归类你选中的所有文件，这个极少用很容易忘记

focus到一个文件或文件夹(就是将焦点移到其上)，按一下enter就可以编辑文件名了。在Windows上这样会触发另一个事件：打开这个文件。

#### 编辑文本的快捷键

Command+Space 切换输入法

Command+left 让光标跳到最前面，相当于windows下的home键

Command+right 让光标跳到最后，相当于windows下的end键

Command+up 让光标跳到整个文本的最开头

Command+down 让光标跳到整个文本的最后

Command+delete 删除当前行，相当于windows下的home，shift+end（选中，从行头到行尾），backspace，这三个操作的组合。这个快捷键会让你相当爽的。

#### 截图快捷键

Command+Shift+3 截取整个桌面，并把截图作为一个文件存储在桌面上

Command+Shift+4 截取一个区域，并把截图作为一个文件存储在桌面上

Command+Shift+Ctrl+3 截取整个桌面，并复制到剪切板

Command+Shift+Ctrl+4 截取一个区域，并复制到剪切板

Command+Shift+4 然后按Space就会截取一个窗口。这样截图会自带阴影效果。如果不想自带阴影效果，可以按住option再点触摸板。

Command+Ctrl+A mac QQ 截图快捷键，由于屏幕分辨率太高，mac系统快捷键截出来的图都太大了，而macQQ截出来的图大小正合适。

#### Chrome浏览器快捷键

这个应该是与操作系统无关的（有些快捷键还是不一样的），但在Mac下养成了用快捷键的习惯，所以Chrome的快捷键都是在Mac上学的，在windows下注意用Ctrl替换Command键就行了。

Command+R 刷新，也可以用F5刷新

Command+L 将窗口焦点锁定到浏览器的地址栏，不用移动鼠标哦

Command+T 打开一个新Tab

Command+Shift+T 打开一个之前被关闭的Tab

Command+Shift+J 打开下载页面，Windows上是ctrl+j

Command+Shift+C 打开“检查(spectator)”，鼠标右键可以看到这个选项，一般是程序员使用的高级功能，但不能再次使用快捷键关闭。可以使用F12打开开发者界面。

Command+Shift+I 同样是打开spectator，使用开发人员工具，但可以再次使用快捷键关闭。

Command+Shift+B 打开或关闭书签栏

Command+Option+B 打开书签管理器

Command+Y 打开历史记录

Command+Option+左右方向键 切换标签页

Command+D 收藏此页为书签

Command+Shift+D 将所有标签页加书签

Command+上下方向键 跳到页面顶部或底部

Command+Option+J 打开javascript控制台

按住Command后点击链接，在新Tab（标签页）中打开这条链接。

Command+Shift再点击链接，在新标签页中打开并切换到新标签页

Command+Shift+N 用隐身模式打开新窗口

Command+Shift+W 关闭当前窗口

Command+[ 或者 ] 前进或者回退

Command+左右方向键 前进或者后退

Command+Option+U 查看网页源代码

#### iTerm2快捷键

iTerm2中的文本，选中即复制，这个其实没必要，我现在都习惯了选中后 cmd+c，而且当你选择替换目标时候由于这个选中即复制，会覆盖掉你的剪切板（不知情的人不小心选中了terminal中的文字，剪切板被覆盖，这回让他不知道为什么，不知所措，不过多试几次就会发现）

Command+D 水平分隔出一个终端

Command+Shift+D 垂直分割出一个终端

可以配置透明度，Command+U快速切换透明与否

可以配置全局唤出快捷键，我自己配置的是Command+U，覆盖了前面那个快捷键

可以配置快捷悬浮，Hotkey window

快捷键+快捷悬浮，真的很不错！

Command+Shift+H 查看复制历史

Command+Enter 快速切换全屏与否

搭配oh-my-zsh，体验更佳。

#### CLI(命令行)快捷键

Mac本身的特点就是GUI和CLI的完美结合（很多Linux的爱好者，又不想被Linux的桌面、各种驱动、不兼容等等杂七杂八的问题折腾的，大可选择Mac）。

首先教一个最重要的东西
记住按Tab补全，这是命令行用的爽的根源。

Ctrl+A 回到行首

Ctrl+E 到行末

Ctrl+U 删除一行

这是我写的：[$\color{red}{高效命令行}$](../../../../2018/11/03/高效命令行)

## 格式转换

有时候需要把 png 等格式的图片转成 jpg 的格式，于是有同学就去到处找格式转换软件了。
其实在 Mac 中直接更改图片的扩展名，即可自动转成相应地格式~试试吧，如果你改格式前看了文件修改日期，改完格式后你会发现修改日期依然没变，至于原因是什么，需要你有图片的文件格式的相关知识。

>在mac上改文件名字并不会触发文件修改日期的改变

## Launchpad与Dock

Launchpad是指，你在触摸板上用五指向中间收拢，出现的全是APP的页面，半透明的。

Dock是指，最下方的摆满APP的一栏，在Dock上你可以放上最常用的APP。

调整launchpad的图标大小：

```
//把行数重定义成6
delete write com.apple.dock springboard-rows -int 6
//把行数充定义成默认
defaults write com.apple.dock springboard-rows Default
//删除定义好的行数（恢复默认）
defaults delete com.apple.dock springboard-rows
//把列数定义成默认
defaults write com.apple.dock springboard-columns Default
//重置 Launchpad，效果就是 launchpad 的布局全给你打乱了，而第一页恢复默认，建议不要使用
defaults write com.apple.dock ResetLaunchPad -bool TRUE
//重启 Launchpad，并使你前面的设置生效
killall Dock
```

在appstore下载一个软件到一半，然后在Application删除了该软件，结果在launchpad中留下了一个垃圾残留空图标。
删除launchpad中的垃圾残留图标，只需拖拽到下载文件夹。其他方法都试过（无效），比如按住图标几秒，出现一个叉，点击叉，删除，没用。在Application找到相应项删除没用。

## Option的妙用

下面再讲些不常用但是有意思的：

除了cmd是常用的，option键也值得我们关注，下面讲几个option的妙用：

按住option+shift可以微调（1/4微调）音量、键盘背光亮度和屏幕亮度，按住option再按其他字母键等，会出现奇怪的字符¥©
©œ∑®†¥åß©≈ç等等。

按住option再把鼠标移到Dock上的APP上，你会发现退出变成了强制退出，怎么样option键还是挺强大的吧。

### 数学符号

约等于: Option + X = ≈
度数: Shift + Option + 8 = °
除号: Option + / = ÷
无穷: Option + 5 = ∞
大于等于和小于等于: Option + , 和 Option + . = ≤ 和 ≥
不等于: Option + = = ≠
圆周率: Option + P = π
加减: Shift + Option + = = ±
开方: Option + V = √
求和符号: Option + W = ∑

### 输入特殊符号

版权符号: Option + G = ©

人民币符号: Option + Y = ¥
