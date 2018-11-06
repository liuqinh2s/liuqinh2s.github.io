---
title: Linux Shell 配色
date: 2018-05-08
categories: [Unix, Linux]
comments: true
---

早就想自己给bash配个色，但是一直没空学习这方面的知识，今天抽个空，搞了一点点。

## 切换shell

切换shell，分临时和永久。

临时：输入命令：`bash`或者`zsh`都行，shell调用shell，一层套一层，按`ctrl+d`或者输入`exit`可以退出。
永久：输入命令：`chsh -s /bin/bash`

>chsh意思就是change shell，使用`man chsh`进行查看

## 修改prompt

prompt也就是提示符，首先就是要修改这个，什么样的提示符才是一个好的提示符呢？我觉得首要的是要短，太长的提示符占了命令的空间。其次提示符还必须显示用户名和机器名，因为只有这样才能在你远程登录的时候与远程shell区分开。机器名最好是缩写。然后我就得出了我自己的提示符的前缀：`liuqinh2s@mbp`，然后我觉得得加个短路径，不要求绝对路径，这样太占空间，只需要加个当前文件夹即可。这样进一步修改之后，我的提示符变成了：`liuqinh2s@mbp:~`，最后需要添加一个普通用户和超级用户的区分，普通用户用：`$`，超级用户用`#`，所以我的提示符最终的样子是：`liuqinh2s@mbp:~$ `，注意`$`后面我加了一个空格，这样看起来比较舒服，提示符和命令的界限一目了然。

但其实还是不清晰，我就给prompt上了个颜色。

最后就变成这样啦：

<img src="https://i.loli.net/2018/05/08/5af13dcea3fa6.png" alt="屏幕快照 2018-05-08 下午2.03.34.png" title="屏幕快照 2018-05-08 下午2.03.34.png" width="30%" height="30%"/>

配置代码：`PS1="\[\e[0;32m\]\u@\h:\[\e[0;34m\]\W\$\[\e[0m\] "`

>使用`export PS1="\[\e[0;32m\]\u@\h:\[\e[0;34m\]\W\$\[\e[0m\] "`，这样的话，你输入`bash`的时候，也能用到你的配置

>export in sh and related shells (such as bash), marks an environment variable to be exported to child-processes, so that the child inherits them.

export的作用就是让子进程也继承环境变量

如果你用的是mac的话，在home目录的`.bash_profile`中修改就可以了

下面是配置代码的讲解：

### Bash转义序列

```
\a     an ASCII bell character (07)
\d     the date in "Weekday Month Date" format (e.g., "Tue May 26")
\D{format}
       the format is passed to strftime(3) and the result is inserted into the prompt string; an empty format results in a locale-
       specific time representation.  The braces are required
\e     an ASCII escape character (033)
\h     the hostname up to the first `.'
\H     the hostname
\j     the number of jobs currently managed by the shell
\l     the basename of the shell's terminal device name
\n     newline
\r     carriage return
\s     the name of the shell, the basename of $0 (the portion following the final slash)
\t     the current time in 24-hour HH:MM:SS format
\T     the current time in 12-hour HH:MM:SS format
\@     the current time in 12-hour am/pm format
\A     the current time in 24-hour HH:MM format
\u     the username of the current user
\v     the version of bash (e.g., 2.00)
\V     the release of bash, version + patch level (e.g., 2.00.0)
\w     the current working directory, with $HOME abbreviated with a tilde (uses the value of the PROMPT_DIRTRIM variable)
\W     the basename of the current working directory, with $HOME abbreviated with a tilde
\!     the history number of this command
\#     the command number of this command
\$     if the effective UID is 0, a #, otherwise a $
\nnn   the character corresponding to the octal number nnn
\\     a backslash
\[     begin a sequence of non-printing characters, which could be used to embed a terminal control sequence into the prompt
\]     end a sequence of non-printing characters
```

### 变更prompt颜色

首先，大家必须以[与]作为色彩代码的描述范围。对于Bash，这代表两个括号间的字符为非输出字符。

Bash需要在此基础上估算字符数量，以备后续输出。如果不将色彩代码纳入[与]之间，那么Bash会将全部字符都计为文本字符并在下一行中进行打包。

另外，在括号内的非输出序列中，我们需要输入\e[或者\033[指定彩色prompt的起点。二者的作用相同，都负责指定该反义序列的起始位置。

在]之前，我们还需要使用“m”来表示即将提供一条色彩序列。

基本上，每次进行色彩修改时，我们都需要输入下面这种命令格式：

```
\[\e[color_informationm\]
```

下面来看用于变更前景文本颜色的基本代码：

- 30: Black
- 31: Red
- 32: Green
- 33: Yellow
- 34: Blue
- 35: Purple
- 36: Cyan
- 37: White

大家也可以通过在以上设定前设置“属性”修改这些基础值，各值之间以分号分隔。

根据实际终端的不同，操作效果也有所区别。部分常见属性包括：

- 0: 普通文本
- 1: 在不同终端中可能代表粗体或者浅色显示
- 4: 下划线文本

因此如果大家希望使用下划线绿色文本，则：`\[\e[4;32m\]`

接下来继续正常使用即可。另外，我们也可以随时将色彩重置为初始值。

重置命令如下：

```
\[\e[0m\]
```

我们也可以指定背景颜色。背景颜色无法获取属性，具体包括：

- 40: Black background
- 41: Red background
- 42: Green background
- 43: Yellow background
- 44: Blue background
- 45: Purple background
- 46: Cyan background
- 47: White background

不过大家可以一次性指定背景颜色、属性与文本颜色：

```
\[\e[42;1;36m\]
```

当然，这里建议各位将背景信息与其它信息分隔开来：

```
\[\e[42m\]\[\e[1;36m\]
```

在使用普通文本属性（0）时，终端中可能出现一些乱码。如果遇到这种问题，大家最好避免使用0值指定普通属性——由于属于默认值，我们无需额外指定。

## 配置ls和grep的颜色

```
# Tell ls to be colourful
 export CLICOLOR=1
 export LSCOLORS=Exfxcxdxbxegedabagacad

 # Tell grep to highlight matches
 export GREP_OPTIONS='--color=auto’
```

- CLICOLOR是用来设置是否进行颜色的显示。CLI是Command Line Interface的缩写。

- LSCOLORS是用来设置当CLICOLOR被启用后，各种文件类型的颜色。LSCOLORS的值中每两个字母为一组，分别设置某个文件类型的文字颜色和背景颜色。LSCOLORS中一共11组颜色设置，按照先后顺序，分别对以下的文件类型进行设置：

![文件类型 - 文件颜色](https://upload-images.jianshu.io/upload_images/1233651-e586f58eae66c4ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

所以，如果我们想把目录显示成红色，就可以把LSCOLORS设置为fxfxaxdxcxegedabagacad就可以了
