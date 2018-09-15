---
title: 投奔vim系列之--NerdTree插件的使用
date: 2018-05-14
categories: [vim]
comments: true
---

## NERDTree的使用

### 切换工作台和目录

```
ctrl+w+h    光标focus左侧树形目录
ctrl+w+l    光标focus右侧文件显示窗口
ctrl+w+w    光标自动在左右侧窗口切换
ctrl+w+r    移动当前窗口的布局位置
```

```
o   在已有窗口中打开文件、目录或书签，并跳到该窗口
go  在已有窗口中打开文件、目录或书签，但不跳到该窗口
t   在新Tab中打开选中文件/书签，并跳到新Tab
T   在新Tab中打开选中文件/书签，但不跳到新Tab
i   split一个新窗口打开选中文件，并跳到该窗口
gi  split一个新窗口打开选中文件，但不跳到该窗口
s   vsplit一个新窗口打开选中文件，并跳到该窗口
gs  vsplit一个新窗口打开选中文件，但不跳到该窗口
!   执行当前文件
O   递归打开选中结点下的所有目录
x   合拢选中结点的父目录
X   递归合拢选中结点下的所有目录
e   Edit the current dif

D   删除当前书签

P   跳到根节点
p   跳到父节点
K   跳到当前目录下同级的第一个结点
J   跳到当前目录下同级的最后一个结点
k   跳到当前目录下同级的前一个结点
j   跳到当前目录下同级的后一个结点

C   将选中目录或选中文件的父目录设为根节点
u   将当前根节点的父目录设为根目录，并变成合拢原根节点
U   将当前根节点的父目录设为根目录，但保持展开原根节点
r   递归刷新选中目录
R   递归刷新根节点
m   显示文件系统菜单
cd  将CWD设为选中目录

I   切换是否显示隐藏文件
f   切换是否使用文件过滤器
F   切换是否显示文件
B   切换是否显示书签

q   关闭NerdTree窗口
?  切换是否显示Quick help
```

### 切换标签页

```
:tabnew [++opt选项] [+cmd]  文件    建立对指定文件新的tab
:tabc   关闭当前的tab
:tabo   关闭所有其他的tab
:tabs   查看所有打开的tab
:tabp   前一个tab
:tabn   后一个tab

标准模式下：
gT  前一个tab
gt  后一个tab

MacVim还可以借助快捷键来完成tab的关闭、切换
cmd+w   关闭当前的tab
cmd+{   前一个tab
cmd+}   后一个tab
```

### NerdTree在.vimrc中的常用配置

```
" 在 vim 启动的时候默认开启 NERDTree（autocmd 可以缩写为 au）
autocmd VimEnter * NERDTree

" 按下 F2 调出/隐藏 NERDTree
map  :silent! NERDTreeToggle

" 将 NERDTree 的窗口设置在 vim 窗口的右侧（默认为左侧）
let NERDTreeWinPos="right"

" 当打开 NERDTree 窗口时，自动显示 Bookmarks
let NERDTreeShowBookmarks=1
```

### 一些简单问题的解决方案

- 以上提到了怎么关闭tab，但没有提到怎么关闭窗口。关于怎么关闭一个窗口，其实很简单，只要使用vim的命令：`:wq`即可:
- 怎么在vim中创建一个文件：`:e %:h/filename`，这是在当前目录下新建一个文件
- 在mac osx中遇到一个问题，insert模式下delete键无法删除文字，发出恼人的duang duang duang声音，出现这个问题，基本是因为你的VIM使用了compatible模式，或者把`backspace`变量设置为空了。好奇的读者一定会问，这两个配置又代表了什么意思？其实compatible模式是VIM为了兼容vi而出现的配置，它的作用是使VIM的操作行为和规范和vi一致，而这种模式下backspace配置是空的。即意味着backspace无法删除`indent`，`end of line`，`start`这三种字符。如果你出现了和博主一样的情况，不妨在解决问题前先在VIM中用set backspace?命令查看下自己当前的删除模式。如果`backspace`为空，效果就相当于delete只能删除本次insert模式中输入的字符。那么为什么backspace=2又能解决问题呢？其实这个命令是set backspace=indent,eol,start的简化写法，也就相当于把delete键配置成增强模式。具体数值和对应增强模式的对应关系见 vim官方文档，简单摘录如下：

- 1 same as “:set backspace=” (Vi compatible)
- 2 same as “:set backspace=indent,eol”
- 3 same as “:set backspace=indent,eol,start”

还有个问题就是，在进入visual block模式，进行列编辑的时候，按I或者A发现没反应，最后发现要先输入你要输入的东西，然后再按I或者A，而不是先按I或者A再进行输入。
