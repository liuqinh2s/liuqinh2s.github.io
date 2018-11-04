---
title: mac osx 与 linux 的一些区别
categories: [Unix]
comments: true
tags: [Unix]
---

## mac osx上`.bashrc`和`.bash_profile`的区别

`.bash_profile` is executed for login shells, while `.bashrc` is executed for interactive non-login shells.

When you login (type username and password) via console, either sitting at the machine, or remotely via ssh: `.bash_profile` is executed to configure your shell before the initial command prompt.

But, if you’ve already logged into your machine and open a new terminal window (xterm) then .bashrc is executed before the window command prompt. `.bashrc` is also run when you start a new bash instance by typing `/bin/bash` in a terminal.

On OS X, Terminal by default runs a login shell every time, so this is a little different to most other systems, but you can configure that in the preferences.

## Mac OSX系统的环境变量加载顺序

1. /etc/profile
2. /etc/paths
3. ~/.bash_profile
4. ~/.bash_login
5. ~/.profile
6. ~/.bashrc

/etc/profile和/etc/paths是系统级别的，系统启动就会加载，后面几个是当前用户级的环境变量，后面几个按从前往后的顺序读取，如果`~/.bash_profile`文件存在，则后面的几个文件就会被忽略不读了，如果`~/.bash_profile`文件不存在，才会以此类推读取后面的文件。

## 设置PATH的语法

```
// 中间用冒号隔开
export PATH=$PATH:<PATH 1>:<PATH 2>:---:<PATH N>
```

>export的意思是使子进程拥有父进程的环境变量

