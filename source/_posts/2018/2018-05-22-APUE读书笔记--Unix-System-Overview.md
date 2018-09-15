---
title: APUE读书笔记--Unix System Overview
date: 2018-05-22
categories: [Unix]
comments: true
---

## 1.1    Introduction

All operating systems provide services for programs they run. Typical services include executing a new program, opening a file, reading a file, allocating a region of memory, getting the current time of day, and so on. The focus of this text is to describe the services provided by various versions of the UNIX operating system. 所有的操作系统都提供一些服务让程序能够在其上运行，典型的服务包括：执行一个新程序，打开一个文件，读取一个文件，分配一块内存，获取当前时间，等等。 这本书所关注的重点就是众多种类的unix操作系统能提供服务。

Describing the UNIX System in a strictly linear fashion, without any forward references to terms that haven’t been described yet, is nearly impossible (and would probably be boring). 这里道出了一个众所周知的难题，想要线性的给读者讲授一个新课程，在讲授一个知识点时却又不引进任何其他的未知概念，这几乎是不可能的。

>但我认为能不能做到，不用去管，做教育的就是要尽可能的降低学习者的难度，将新知识点尽可能的用学习者已有的知识结构描述清楚。所以我们可以看到我们的课程都是有选修课的。

## 1.2    Unix Architecture

<img src="https://i.loli.net/2018/05/22/5b03742b0a818.png" width="50%">

- 内核（kernel）：In a strict sense, an operating system can be defined as the software that **controls the hardware resources of the computer** and **provides an environment under which programs can run**. Generally, we call this software the kernel, since it is **relatively small and resides at the core of the environment**. 内核控制硬件资源，并给其上的程序提供运行环境，内核相对（相对是指相对于上面这张图，整个软件环境：包括内核、系统调用、公用函数库、shell(命令解释器)、应用程序）来说比较小，并处于整个环境的中心。
- 系统调用（system calls）：**The interface to the kernel** is a layer of software called the system calls . 系统调用是内核对外的接口。
- Libraries of common functions are built on top of the system call interface, but applications are free to use both. The shell is a special application that provides an interface for running other applications. 公共库是建立在系统调用之上的，但应用程序既可以使用公共库也可以使用系统调用。shell是一种特殊的应用程序，给执行命令(运行其他程序)提供接口。

## 1.3    Logging In

### Login Name

**口令文件（password file）** ：当我们使用用户名和密码登陆unix的时候，系统会在`/etc/passwd`文件(password file，又叫：口令文件)中查找我们的用户名，口令文件中每个条目占一行，格式是：

```
登录名:加密过的密码:user ID:group ID:注解:home目录:shell
```

>不过加密过的密码现在也不显示在这个文件里了，而是用一个`*`号或者`x`号之类的取代，home目录又称为起始目录，新开一个shell，`pwd`一下，就是home目录。

例子：

```
sar:x:205:105:Stephen Rago:/home/sar:/bin/ksh
```

登录名是sar，加密过的密码是`x`（不显示在这里），user ID是205，group ID是105，注解是Stephen Rago，home目录是`/home/sar`，使用的shell是`/bin/ksh`

### Shells

A shell is a command-line interpreter that reads user input and executes commands. The user input to a shell is normally from the terminal (an interactive shell) or sometimes from a file (called a shell script). shell是一种命令解释器，可以读取用户的输入，并执行命令，用户可以交互式的输入命令，也可以把命令预先全部写在一个文本文件（shell脚本）中让shell执行。

常见的shell有：

<img src="https://i.loli.net/2018/05/23/5b04b995bf5e1.png" width="70%">

The system knows which shell to execute for us based on the final field in our entry in the password file. 系统是通过口令文件的最后一个字段知道我们登陆时使用哪个shell。

`bash`的全称是`Bourne-again shell`

The Bourne-again shell is the GNU shell provided with all Linux systems. It was designed to be POSIX conformant, while still remaining compatible with the Bourne shell. It supports features from both the C shell and the Korn shell.

## 1.4     Files and Directories

### File System

- **根目录（root）**：The UNIX file system is a hierarchical arrangement of directories and files. Everything starts in the directory called root, whose name is the single character /. unix文件系统是目录和文件的层级安排，所有东西都从一个叫root的目录开始，root的名字是一个单字符：`/`。
- **目录（directory）**：A directory is a file that contains directory entries. Logically, we can think of each directory entry as containing a filename along with a structure of information describing the attributes of the file. The attributes of a file are such things as the type of file (regular file, directory), the size of the file, the owner of the file, permissions for the file (whether other users may access this file), and when the file was last modified. 目录是一个包含目录条目的文件。逻辑上，我们可以认为每一个目录条目包含一个文件名和一个描述文件属性的结构信息。文件属性包括：文件类型（普通文件还是目录），文件大小，文件所属者，文件权限（其他用户是否能访问），文件最后被修改的时间。

>The stat and fstat functions return a structure of information containing all the attributes of a file. `stat`和`fstat`函数可以返回一个结构信息，包含文件的所有属性。

>We make a distinction between the logical view of a directory entry and the way it is actually stored on disk. Most implementations of UNIX file systems don’t store attributes in the directory entries themselves, because of the difficulty of keeping them in synch when a file has multiple hard links. 也就是说上面目录条目只是逻辑上的，实际上在硬盘存储上并不是直接将目录条目中的那些属性存储在目录文件中的，因为如果文件有硬链接的话，很难让这些属性信息保持同步。

### Filename

**文件名**：The names in a directory are called filenames. The only two characters that cannot appear in a filename are the slash character (/) and the null character. The slash separates the filenames that form a pathname (described next) and the null character terminates a pathname. Nevertheless, it’s good practice to restrict the characters in a filename to a subset of the normal printing characters. (If we use some of the shell’s special characters in the filename, we have to use the shell’s quoting mechanism to reference the filename, and this can get complicated.) Indeed, for portability, POSIX.1 recommends restricting filenames to consist of the following characters: letters (a-z, A-Z), numbers (0-9), period (.), dash (-), and underscore (_).
一个目录中的诸多名字（包括文件和目录）称为文件名，只有两个字符不能出现在文件名中：`斜杠/(slash)`和`空字符(null character)`，斜杠用来分割`路径名(pathname)`中的文件名，空字符用来结束一个路径名（实际上编程语言中字符串就是由空字符来结束的）。然而，我们命名文件的时候最好不要使用一些乱七八糟的字符，如果我们使用了shell的特殊字符，我们就必须使用shell的引用机制去引用文件名。实际上，为了可移植性，POSIX.1标准推荐严格使用以下字符来命名文件：字母(a-z, A-Z)，数字(0-9)，点(.)，中杠(-)，下划线(_)。

Two filenames are automatically created whenever a new directory is created: . (called dot) and .. (called dot-dot). Dot refers to the current directory, and dot-dot refers to the parent directory. In the root directory, dot-dot is the same as dot. 有两个文件名在目录被创建的时候自动创建：`.`和`..`，`.`指向当前目录，`..`指向父目录，在根目录中，`..`和`.`一样（都指向当前目录）。

>The Research UNIX System and some older UNIX System V file systems restricted a filename to 14 characters. BSD versions extended this limit to 255 characters. Today, almost all commercial UNIX file systems support at least 255-character filenames. 现如今的unix文件系统都支持至少255字符的文件名。

### Pathname

A sequence of one or more filenames, separated by slashes and optionally starting with a slash, forms a pathname. A pathname that begins with a slash is called an absolute pathname; otherwise, it’s called a relative pathname. Relative pathnames refer to files relative to the current directory. 一系列的由斜杠分割而开的文件名组成一个路径名，一个路径名可以由一个斜杠开始，这叫做：`绝对路径`，反之就是相对路径，相对路径是相对于当前路径的。

### Example

Listing the names of all the files in a directory is not difficult. There is a bare-bones implementation of the ls(1) command:

> Figure 1.3 List all the files in a directory 

```C
#include "apue.h"
#include <dirent.h>

int main(int argc, char *argv[])
{
   DIR *dp;
   struct dirent *dirp;
   if (argc != 2)
       err_quit("usage: ls directory_name");
   if ((dp = opendir(argv[1])) == NULL)
       err_sys("can’t open %s", argv[1]);
   while ((dirp = readdir(dp)) != NULL)
       printf("%s\n", dirp->d_name);
    closedir(dp);
    exit(0);
}
```

The notation ls(1) is the normal way to reference a particular entry in the UNIX system manuals. It refers to the entry for ls in Section 1. The sections are normally numbered 1 through 8, and all the entries within each section are arranged alphabetically. Throughout this text, we assume that you have a copy of the manuals for your UNIX system.  熟悉unix的人应该都知道，unix有个man命令，可以查看其它命令的说明书，当然也可以`man man`查看自己的说明书。 man命令有8个section，每个section里的条目按照字幕顺序排列。

> Historically, UNIX systems lumped all eight sections together into what was called the UNIX Programmer’s Manual. As the page count increased, the trend changed to distributing the sections among separate manuals: **one for users, one for programmers, and one for system administrators**, for example. Some UNIX systems further divide the manual pages within a given section, using an uppercase letter. For example, all the standard input/output (I/O) functions in AT&T [1990e] are indicated as being in Section 3S, as in fopen(3S). Other systems have replaced the numeric sections with alphabetic ones, such as C for commands. 

Today, most manuals are distributed in electronic form. If your manuals are online, the way to see the manual pages for the ls command would be something like 

```shell
man 1 ls
```

or

```shell
man -s1 ls
```

Figure 1.3 is a program that just prints the name of every file in a directory, and nothing else. If the source file is named myls.c, we compile it into the default a.out executable file by running 

```shell
cc myls.c
```

> Historically, cc(1) is the C compiler. On systems with the GNU C compilation system, the C 
>
> compiler is gcc(1). Here, cc is usually linked to gcc. 

但在实际的操作过程中，遇到了如下错误：

```
Undefined symbols for architecture x86_64:
  "_err_quit", referenced from:
      _main in fig1-457251.o
  "_err_sys", referenced from:
      _main in fig1-457251.o
ld: symbol(s) not found for architecture x86_64
clang: error: linker command failed with exit code 1 (use -v to see invocation)
```

这是个链接错误，具体的解决办法请看这篇博客：[OS X下UNIX环境高级编程（第三版）学习日志－第一章ChapterI，编译apue包与第一个例程](https://my.oschina.net/alextuan/blog/530425)

- When the program is done, it calls the function exit with an argument of 0. The function exit terminates a program. By convention, an argument of 0 means OK, and an argument between 1 and 255 means that an error occurred.  **0代表OK，1到255代表各种类型的错误**。

### Working Directory

**工作目录（working directory）**：Every process has a working directory, sometimes called the **current working directory**. This is the directory from which all relative pathnames are interpreted. A process can change its working directory with the `chdir` function.  每个进程都有一个工作目录，又叫做：当前工作目录，相对路径就是相对于当前工作目录的来解释的，可以调用`chdir`函数来改变工作目录。

For example, the relative pathname doc/memo/joe refers to the file or directory joe, in the directory memo, in the directory doc, which must be a directory within the working directory. From looking just at this pathname, we know that both doc and memo have to be directories, but we can’t tell whether joe is a file or a directory. The pathname /usr/lib/lint is an absolute pathname that refers to the file or directory lint in the directory lib, in the directory usr, which is in the root directory.  这里有趣的一点是，我们无法判断joe是文件还是目录。

### Home Directory

When we log in, the working directory is set to our **home directory**. Our home directory is obtained from our entry in the **password file** (Section 1.3).  当我们登陆的时候，工作目录会设定为home目录，而我们的home目录设置在口令文件中。

## 1.5    Input and Output

### File Descriptors

**文件描述符（file descriptor）**：File descriptors are normally small non-negative integers that the kernel uses to identify the files accessed by a process. Whenever it opens an existing file or creates a new file, the kernel returns a file descriptor that we use when we want to read or write the file.  文件描述符是一个小的非负整数，内核用它来标识正在被进程访问的文件。打开或者创建文件的时候内核会返回一个文件描述符，我们可以使用这个文件描述符来对文件进行读写。

### Standard Input, Standard Output, and Standard Error

**标准输入，标准输出，标准错误**：By convention, all shells open three descriptors whenever a new program is run: standard input, standard output, and standard error. If nothing special is done, as in the simple command 

```
ls
```

then **all three are connected to the terminal**. Most shells provide a way to redirect any or all of these three descriptors to any file. For example, 

```
ls > file.list
```

executes the ls command with its standard output redirected to the file named 

file.list. 

**按照惯例，当新程序运行的时候，shell会打开三个文件描述符：标准输入，标准输出，标准错误。如果没有进行指明，那么三个文件描述符都会连接到终端。**

### Unbuffered I/O

Unbuffered I/O is provided by the functions `open`, `read`,` write`, `lseek`, and `close`. These functions all work with file descriptors. **unbuffered I/O**意思是系统不提供buffer管理，要你自己申请buffer，并传递给系统函数。

### Example

If we’re willing to read from the standard input and write to the standard output, then the program in Figure 1.4 copies any regular file on a UNIX system. 

> Figure 1.4 Copy standard input to standard output 

```C
#include "apue.h"
#define BUFFSIZE 4096
int main(void)
{
    int n;
    char buf[BUFFSIZE];
    while ((n = read(STDIN_FILENO, buf, BUFFSIZE)) > 0)
        if (write(STDOUT_FILENO, buf, n) != n)
            err_sys("write error");
    if (n < 0)
        err_sys("read error");
	exit(0); 
}
```

下面是对程序的解释：

The `<unistd.h>` header, included by `apue.h`, and the two constants `STDIN_FILENO` and `STDOUT_FILENO` are part of the POSIX standard (about which we’ll have a lot more to say in the next chapter). This header contains function prototypes for many of the UNIX system services, such as the `read` and `write` functions that we call. 

The constants STDIN_FILENO and STDOUT_FILENO are defined in <unistd.h> and specify the file descriptors for standard input and standard output. **These values are 0 and 1, respectively, as required by POSIX.1**, but we’ll use the names for readability. POSIX.1标准：**标准输入是0，标准输出是1，标准错误是2**。

The `read` function returns the number of bytes that are read, and this value is used as the number of bytes to `write`. When the end of the input file is encountered, read returns 0 and the program stops. If a read error occurs, read returns −1. Most of the system functions return −1 when an error occurs.  `read`函数返回的是读入字节的个数，把这个返回值传给`write`函数，就可以读多少写多少了，当遇到输入文件结束时，read函数返回0，当遇到错误时，read函数返回-1。**许多系统函数返回-1，当它们遇到错误时**。

If we compile the program into the standard name (a.out) and execute it as 

```shell
./a.out > data
```

standard input is the terminal, standard output is redirected to the file data, and standard error is also the terminal. **If this output file doesn’t exist, the shell creates it by default**. The program copies lines that we type to the standard output **until we type the end-of-file character (usually Control-D)**. 

If we run 

```shell
./a.out < infile > outfile
```

then the file named infile will be copied to the file named outfile. 

如果文件不存在，shell默认给我们创建一个。标准输入和标准错误都是终端，我们可以一直输入，直到输入一个文件结束符，也就是`ctrl+d`。

### Standard I/O

The standard I/O functions provide a buffered interface to the unbuffered I/O functions. **Using standard I/O relieves us from having to choose optimal buffer sizes**, such as the BUFFSIZE constant in Figure 1.4. The standard I/O functions also simplify dealing with lines of input (a common occurrence in UNIX applications). **The `fgets` function, for example, reads an entire line. The read function, in contrast, reads a specified number of bytes**. As we shall see in Section 5.4, the standard I/O library provides functions that let us control the style of buffering used by the library. 标准I/O函数给unbuffered I/O 函数提供了缓冲接口，使用标准I/O函数可以让我们从优化buffer大小中解脱出来，举个例子，`fgets`函数直接读取一整行，而`read`函数读取固定个数的字节。

The most common standard I/O function is `printf`. In programs that call printf, we’ll always include <stdio.h>—normally by including apue.h—as this header contains the function prototypes for all the standard I/O functions. 

### Example

The program in Figure 1.5, which we’ll examine in more detail in Section 5.8, is like the previous program that called read and write. This program copies standard input to standard output and can copy any regular file. 

> Figure 1.5 Copy standard input to standard output, using standard I/O 

```C
#include "apue.h"
int main(void)
{
	int c;
    while ((c = getc(stdin)) != EOF)
    	if (putc(c, stdout) == EOF)
    		err_sys("output error");
    if (ferror(stdin))
    	err_sys("input error");
	exit(0);
}
```

**The function getc reads one character at a time, and this character is written by putc**. After the last byte of input has been read, getc returns the constant EOF (defined in <stdio.h>). The standard I/O constants stdin and stdout are also defined in the <stdio.h> header and refer to the standard input and standard output. 

## 1.6    Programs and Processes

### Program

A program is an executable file residing on disk in a directory. A program is read into memory and is executed by the kernel as a result of one of the **seven exec functions**. 

### Processes and Process ID 

An executing instance of a program is called a **process**, a term used on almost every page of this text. Some operating systems use the term **task** to refer to a program that is being executed.  程序运行的一个实例叫做：进程，也有些操作系统使用：task这个术语来描述被执行的程序。

The UNIX System guarantees that every process has a unique numeric identifier called the process ID. The process ID is always a non-negative integer. unix系统保证每个进程都有独一无二的进程ID，这个进程ID是一个非负整数。

### Example

The program in Figure 1.6 prints its process ID. 

> Figure 1.6 Print the process ID  

```C
#include "apue.h"
int main(void)
{
    printf("hello world from process ID %ld\n", (long)getpid());
    exit(0); 
}
```

输出结果：

```
➜  apue.3e ./fig1.6
hello world from process ID 8080
➜  apue.3e ./fig1.6
hello world from process ID 8086
```

### Process Control

There are three primary functions for process control: `fork`, `exec`, and `waitpid`. (The exec function has seven variants, but we often refer to them collectively as simply the exec function.)  有三个进程控制的基本函数：`fork`，`exec`和`waitpid`，虽然exec函数有7种变体，但我们往往简单的用一个exec来表示它们。

### Example

The process control features of the UNIX System are demonstrated using a simple program (Figure 1.7) that reads commands from standard input and executes the commands. This is a bare-bones implementation of a shell-like program. 

> Figure 1.7 Read commands from standard input and execute them 

```C
#include "include/apue.h"
#include <sys/wait.h>

int main(void)
{
    char    buf[MAXLINE];   /* from apue.h */
    pid_t   pid;
    int     status;

    printf("%% ");  /* print prompt (printf requires %% to print %) */
    while (fgets(buf, MAXLINE, stdin) != NULL) {
        if (buf[strlen(buf) - 1] == '\n')
            buf[strlen(buf) - 1] = 0; /* replace newline with null */
        if ((pid = fork()) < 0) {
            err_sys("fork error");
        } else if (pid == 0) {      /* child */
            execlp(buf, buf, (char *)0);
            // or
            // execlp(buf, buf, (char *)NULL);
            err_ret("couldn’t execute: %s", buf);
            exit(127);
		}
        /* parent */
        if ((pid = waitpid(pid, &status, 0)) < 0)
            err_sys("waitpid error");
        printf("%% ");
	}
	exit(0); 
}
```

There are several features to consider in this 30-line program. 

- We use the standard I/O function fgets to read one line at a time from the standard input. When we type the end-of-file character (which is often Control-D) as the first character of a line, fgets returns a null pointer, the loop stops, and the process terminates. In Chapter 18, we describe all the special terminal characters—end of file, backspace one character, erase entire line, and so on—and how to change them.  我们使用了标准I/O函数`fgets`来一次读取标准输入的一行。当我们直接输入一个EOF时，fgets返回一个空指针，循环停止，进程终止。在第18章，我们将讲述特殊终止符，比如：end of file，backspace one character, erase entire line, 等等，以及如何改变它们。
- Because each line returned by fgets is terminated with a newline character, followed by a null byte, we use the standard C function strlen to calculate the length of the string, and then replace the newline with a null byte. We do this because the execlp function wants a null-terminated argument, not a newline-terminated argument.  因为fgets返回的每一行都被一个换行符终止，换行符之后是一个空字符，我们使用标准C函数`strlen`来计算string的长度，然后将换行符替换成空字符（这样末尾就两个空字符了）。我们这样做是因为`execlp`函数希望有一个空字符来作为结尾参数，而不是一个换行符。
- We call fork to create a new process, which is a copy of the caller. We say that the caller is the parent and that the newly created process is the child. Then fork returns the non-negative process ID of the new child process to the parent, and returns 0 to the child. Because fork creates a new process, we say that it is called once—by the parent—but returns twice—in the parent and in the child.  我们调用`fork`创建一个新进程，这个新进程是调用进程的一个复制。我们说，调用者是父进程，新创建出来的进程是子进程。**然后fork返回子进程的非负进程ID给父进程，并返回0给子进程**。因为fork创建了一个新进程，**我们说它调用了一次（被父进程），但是返回了两次**，在父进程和子进程里面。
- In the child, we call execlp to execute the command that was read from the standard input. This replaces the child process with the new program file. The combination of fork followed by exec is called spawning a new process on some operating systems. In the UNIX System, the two parts are separated into individual functions. We’ll say a lot more about these functions in Chapter 8.  在子进程中，我们调用`execlp`来执行从标注输入中读取来的命令。这就把子进程替换成了新执行的程序。fork后面跟个exec这种结合方式被叫做 **spawning a new process** 在某些操作系统中。在unix系统中，这两部分被分别放到了两个单独的函数中。我们将在第8章中讨论更多的这类函数。
- Because the child calls execlp to execute the new program file, the parent wants to wait for the child to terminate. This is done by calling waitpid, specifying which process to wait for: the pid argument, which is the process ID of the child. The waitpid function also returns the termination status of the child—the status variable—but in this simple program, we don’t do anything with this value. We could examine it to determine how the child terminated.  因为子进程调用了`execlp`来执行新程序文件，父进程想要等子进程结束。通过调用`waitpid`可以完成这个任务，用`pid`（子进程的进程ID）参数来明确需要等待哪个进程。`waitpid`函数同样也返回子进程的终止状态（记录在status这个参数），但在这个简单的程序里，我们没有用到这个值。我们通过这个值得知子进程是如何结束的。
- The most fundamental limitation of this program is that we can’t pass arguments to the command we execute. We can’t, for example, specify the name of a directory to list. We can execute ls only on the working directory. To allow arguments would require that we parse the input line, separating the arguments by some convention, probably spaces or tabs, and then pass each argument as a separate parameter to the execlp function. Nevertheless, this program is still a useful demonstration of the UNIX System’s process control functions.  这个程序最大的限制就是，我们不能传递参数给我们要执行的命令。比如，我们不能给定一个目录给list程序（展示目录下的所有目录和文件的程序）。我们只能在当前目录下执行ls。如果要允许传递参数，就需要我们分析输入行，按照惯例，比如空格或者制表符，把参数分割开来，然后把参数传给execlp函数。不管怎么说，这个程序已经很好的展示了unix系统是如何控制函数的。

If we run this program, we get the following result. Note that our program has a different prompt—the percent sign—to distinguish it from the shell’s prompt. 

```
➜  apue.3e ./fig1.7
% pwd
/Users/liuqinh2s/Downloads/apue.3e
% who
liuqinh2s console  May 21 12:09
liuqinh2s ttys000  May 21 12:10
% date
2018年 5月24日 星期四 15时26分59秒 CST
% %                                                                                                                                                                                                         ➜  apue.3e
```

> The notation ˆD is used to indicate a control character. Control characters are special characters formed by holding down the control key—often labeled Control or Ctrl—on your keyboard and then pressing another key at the same time. **Control-D, or ˆD, is the default end-of-file character**. We’ll see many more control characters when we discuss terminal I/O in Chapter 18.  `^D`这种记法用于表示控制字符，控制字符是一类特殊的字符，由`ctrl`键加一个其他键组成，`Control-D`或者说`^D`是默认的文件终止符。我们将在第18章讨论输入输出终止符的时候看到更多的控制字符。

### Threads and Thread IDs

Usually, a process has only one thread of control—one set of machine instructions executing at a time. Some problems are easier to solve when more than one thread of control can operate on different parts of the problem. Additionally, multiple threads of control can exploit the parallelism possible on multiprocessor systems. 

All threads within a process share the same address space, file descriptors, stacks, and process-related attributes. Each thread executes on its own stack, although any thread can access the stacks of other threads in the same process. Because they can access the same memory, the threads need to synchronize access to shared data among themselves to avoid inconsistencies.  属于同一个进程的多个线程共享同一块内存空间，文件描述符，栈，以及和进程相关的属性。每个线程都在自己的栈里面执行，但每个线程又能访问其他线程的栈（同属于一个进程的多个线程）。因为它们能访问同一块内存，所以为了避免不一致性，需要保护好临界资源。

Like processes, threads are identified by IDs. **Thread IDs, however, are local to a process**. A thread ID from one process has no meaning in another process. We use thread IDs to refer to specific threads as we manipulate the threads within a process.  就像进程一样，线程也用ID标识。**然而线程ID是局部的，只在某个进程内有效，出了这个进程，对其他进程来说这个线程ID就没有任何意义了。**

> threads were added to the UNIX System long after the process model was established 

## 1.7    Error Handling

When an error occurs in one of the UNIX System functions, a negative value is often returned, and the integer **errno** is usually set to a value that tells why. For example, the open function returns either a non-negative file descriptor if all is OK or −1 if an error occurs. An error from open has about 15 possible errno values, such as file doesn’t exist, permission problem, and so on. Some functions use a convention other than returning a negative value. For example, most functions that return a pointer to an object return a null pointer to indicate an error.  当unix系统函数出错时，会返回一个负数，整形变量**errno**会设置为一个值，这个值告诉我们为什么出错。例如，open函数返回一个非负的文件描述符，如果成功的话，如果出现错误则返回一个-1。open函数返回的错误有15个可能的errno值，比如：文件不存在，权限问题，等等。有些函数使用另一个传统而非返回一个负数。例如，很多函数返回一个对象指针，或者一个空指针如果出现错误。

The file <errno.h> defines the symbol errno and constants for each value that errno can assume. Each of these constants begins with the character E. Also, the first page of Section 2 of the UNIX system manuals, named intro(2), usually lists all these error constants. For example, if errno is equal to the constant EACCES, this indicates a permission problem, such as insufficient permission to open the requested file.  `<errno.h>`文件定义了变量errno和一系列常量（errno可能的值）。每个常量都以字符`E`开头。unix系统手册`intro(2)`展示了这些常量。例如，如果errno等于常量EACCES，就表示是权限问题，没有足够的权限去打开这个文件。

> On Linux, the error constants are listed in the errno(3) manual page. 

POSIX and ISO C define errno as a symbol expanding into a modifiable lvalue of type integer. This can be either an integer that contains the error number or a function that returns a pointer to the error number. The historical definition is 

```C
extern int errno;
```

But in an environment that supports threads, the process address space is shared among multiple threads, and each thread needs its own local copy of errno to prevent one thread from interfering with another. Linux, for example, supports multithreaded access to errno by defining it as 

```C
extern int *__errno_location(void); 
#define errno (*__errno_location())
```

POSIX和ISO C把errno定义为一个可以修改的左值。可以定义为一个整形值，也可以定义为一个指针，指针指向错误码（String类型）。如果是多线程环境下，每个线程都有自己的一个errno拷贝。通过宏定义把errno给替换成函数：`int *__errno_location(void); `了。

There are two rules to be aware of with respect to errno. First, its value is never cleared by a routine if an error does not occur. Therefore, we should examine its value only when the return value from a function indicates that an error occurred. Second, the value of errno is never set to 0 by any of the functions, and none of the constants defined in <errno.h> has a value of 0.  第一，如果没有出错，errno的值不会被重置，因此，我们只有在函数返回出错的时候才检查errno；第二，errno不会等于0。

Two functions are defined by the C standard to help with printing error messages. 

```C
#include <string.h>

char *strerror(int errnum);	//Returns: pointer to message string
```

This function maps errnum, which is typically the errno value, into an error message string and returns a pointer to the string. 

The perror function produces an error message on the standard error, based on the current value of errno, and returns. 

```C
#include <stdio.h>

void perror(const char* msg);
```

It outputs the string pointed to by msg, followed by a colon and a space, followed by the error message corresponding to the value of errno, followed by a newline. 

### Example

Figure 1.8 shows the use of these two error functions. 

> Figure 1.8 Demonstrate strerror and perror 

```C
#include "apue.h"
#include <errno.h>
int main(int argc, char *argv[])
{
    fprintf(stderr, "EACCES: %s\n", strerror(EACCES));
    errno = ENOENT;
    perror(argv[0]);
    exit(0);
}
```

输出结果：

```
➜  apue.3e ./fig1.8
EACCES: Permission denied
./fig1.8: No such file or directory
➜  apue.3e
```

> argv[0] 表示输入的第一个参数，也就是命令名

### Error Recovery

The errors defined in <errno.h> can be divided into two categories: fatal and nonfatal. A fatal error has no recovery action. The best we can do is print an error message on the user’s screen or to a log file, and then exit. Nonfatal errors, on the other hand, can sometimes be dealt with more robustly. Most nonfatal errors are temporary, such as a resource shortage, and might not occur when there is less activity on the system.  定义在`<errno.h>`中的错误可以分为两类：fatal和nonfatal，致命和非致命。致命错误没有恢复动作，我们最多能做的就是把错误信息在用户显示屏上打印出来，或者写到log文件里，然后退出。非致命错误，可以更妥善的处理，许多非致命错误都是暂时的，比如：资源短缺，当系统活动较少时这类错误可能不会发生。

Resource-related nonfatal errors include **EAGAIN, ENFILE, ENOBUFS, ENOLCK, ENOSPC, EWOULDBLOCK**, and sometimes **ENOMEM**. **EBUSY** can be treated as nonfatal when it indicates that a shared resource is in use. Sometimes, **EINTR** can be treated as a nonfatal error when it interrupts a slow system call (more on this in Section 10.5).  

The typical recovery action for a resource-related nonfatal error is to delay and retry later. This technique can be applied in other circumstances. For example, if an error indicates that a network connection is no longer functioning, it might be possible for the application to delay a short time and then reestablish the connection. Some applications use an exponential backoff algorithm, waiting a longer period of time in each subsequent iteration.  **典型的资源相关性非致命错误的处理办法是先等一下，之后再重试。**

**Ultimately, it is up to the application developer to determine the cases where an application can recover from an error. If a reasonable recovery strategy can be used, we can improve the robustness of our application by avoiding an abnormal exit. **

## 1.8    User Identification

### User ID

The user ID from our entry in the password file is a numeric value that identifies us to the system. This user ID is assigned by the system administrator when our login name is assigned, and we cannot change it. The user ID is normally assigned to be unique for every user. We’ll see how the kernel uses the user ID to check whether we have the appropriate permissions to perform certain operations.  用户ID来自口令文件中对应的条目，它是以数字的形式帮助系统对我们进行标识。用户ID是系统管理员给我们分配的（当分配登录名时，同时也必须分配用户ID），我们自己无法改。每个人的用户ID应该是唯一的，内核使用用户ID来检查我们是否有合适的权限来进行一个操作。

We call the user whose user ID is 0 either root or the superuser. The entry in the password file normally has a login name of root, and we refer to the special privileges of this user as superuser privileges. As we’ll see in Chapter 4, if a process has superuser privileges, most file permission checks are bypassed. Some operating system functions are restricted to the superuser. The superuser has free rein over the system.  我们把用户ID为0的用户称为：**root**或者**superuser**。口令文件中有一个条目的登录名是root，root用户拥有特殊权限。拥有superuser特权的进程可以自由的使用任意文件，而且有些操作系统函数是只对superuser开放的。superuser拥有对系统的绝对的权限（可以把系统弄残）。

> Client versions of Mac OS X ship with the superuser account disabled; server versions ship with the account already enabled. Instructions are available on Apple’s Web site describing how to enable it. See http://support.apple.com/kb/HT1528. 

### Group ID

Our entry in the password file also specifies our numeric group ID. This, too, is assigned by the system administrator when our login name is assigned. Typically, the password file contains multiple entries that specify the same group ID. Groups are normally used to collect users together into projects or departments. This allows the sharing of resources, such as files, among members of the same group. We’ll see in Section 4.5 that we can set the permissions on a file so that all members of a group can access the file, whereas others outside the group cannot.  Group ID的作用就是让相同组的人共享资源。

There is also a group file that maps group names into numeric group IDs. The group file is usually `/etc/group`. 

The use of numeric user IDs and numeric group IDs for permissions is historical. With every file on disk, the file system stores both the user ID and the group ID of a file’s owner. Storing both of these values requires only four bytes, assuming that each is stored as a two-byte integer. If the full ASCII login name and group name were used instead, additional disk space would be required. In addition, comparing strings during permission checks is more expensive than comparing integers.  使用数字的用户ID和组ID是有历史原因的。对于每个存放在磁盘上的文件，文件系统都存储了该文件的拥有者的用户ID和组ID。存储这两个数字需要4字节（每个2字节），如果使用ASCII编码的登录名和组名，需要多用掉很多额外的磁盘空间。另外在检查权限是否合格时，整形数字比较要比字符串比较更快。

Users, however, work better with names than with numbers, so the password file maintains the mapping between login names and user IDs, and the group file provides the mapping between group names and group IDs. The ls -l command, for example, prints the login name of the owner of a file, using the password file to map the numeric user ID into the corresponding login name.  然而对于用户来说名字比数字更好记，所以password file和group file分别记录了登录名和用户ID的映射，组名和组ID的映射。使用`ls -l`命令，可以看到打印出了文件所属者和所属的组，其原理就是查找了password file和group file，把相应的数字ID换成名字。

> Early UNIX systems used 16-bit integers to represent user and group IDs. Contemporary UNIX systems use 32-bit integers. 

### Example

The program in Figure 1.9 prints the user ID and the group ID. 

> Figure 1.9 Print user ID and group ID  

```C
#include "apue.h"

int main(void)
{
	printf("uid = %d, gid = %d\n", getuid(), getgid());
	exit(0); 
}
```

### Supplementary Group IDs 

**附加组（supplementary group）**：In addition to the group ID specified in the password file for a login name, most versions of the UNIX System allow a user to belong to other groups. This practice started with 4.2BSD, which allowed a user to belong to up to 16 additional groups. These supplementary group IDs are obtained at login time by reading the file /etc/group and finding the first 16 entries that list the user as a member. As we shall see in the next chapter, POSIX requires that a system support at least 8 supplementary groups per process, but most systems support at least 16.  许多unix系统允许用户属于多个组，最多16个。主组，也就是登陆时的默认组记录在`/etc/passwd`中。

`/etc/group`格式如下：

```shell
_analyticsusers:*:250:_analyticsd,_networkd,_timed
_analyticsd:*:263:_analyticsd
```

解释：

```
组名:口令:组ID:组内用户列表
```

## 1.9    Signal

Signals are a technique used to notify a process that some condition has occurred. For example, if a process divides by zero, the signal whose name is SIGFPE (floating-point exception) is sent to the process. The process has three choices for dealing with the signal.  信号是一种用来通知进程发生了某些事的技术。举个例子：当进程除以0时，就会有一个SIGFPE (floating-point exception)发送到这个进程。进程处理信号有三种选择：

1. Ignore the signal. This option isn’t recommended for signals that denote a hardware exception, such as dividing by zero or referencing memory outside the address space of the process, as the results are undefined.  忽视信号。如果是硬件异常不推荐这个选择，例如：被0除，引用进程外的内存，因为这些结果都是不确定的。
2. Let the default action occur. For a divide-by-zero condition, the default is to terminate the process.  让默认动作出现，比如被0除的情况下，默认是终止该进程。
3. Provide a function that is called when the signal occurs (this is called ‘‘catching’’ the signal). By providing a function of our own, we’ll know when the signal occurs and we can handle it as we wish.  我们自己提供一个函数捕获信号，这样我们就能让程序以我们的意愿处理异常。

Many conditions generate signals. Two terminal keys, called the **interrupt key**— often the **DELETE** key or **Control-C**—and the **quit key**—often **Control-backslash**—are used to interrupt the currently running process. Another way to generate a signal is by calling the `kill` function. **We can call this function from a process to send a signal to another process. Naturally, there are limitations: we have to be the owner of the other process (or the superuser) to be able to send it a signal**.    很多条件下可以生成信号，终端键有两种，interrupt key（delete键或者ctrl+c）和 quit key（ctrl+\）。另一个生成信号的方法是调用`kill`函数，我们可以在一个进程里调用kill函数来结束另一个线程，但我们需要有权限（如果我们是另一个进程的拥有者，或者是超级用户，就可以）。

### Example

Recall the bare-bones shell example (Figure 1.7). If we invoke this program and press the interrupt key, the process terminates because the default action for this signal, named SIGINT, is to terminate the process. The process hasn’t told the kernel to do anything other than the default with this signal, so the process terminates.  如果直接执行Figure 1.7的代码，我们按下中断键，程序就会终止，因为这个SIGINT信号的默认动作就是终止进程。

To catch this signal, the program needs to call the signal function, specifying the name of the function to call when the SIGINT signal is generated. The function is named sig_int; when it’s called, it just prints a message and a new prompt. Adding 11 lines to the program in Figure 1.7 gives us the version in Figure 1.10. (The 11 new lines are indicated with a plus sign at the beginning of the line.)  为了捕获这个信号，程序需要调用一个信号函数。我们给它命名为：`sig_int`函数，在捕获到**SIGINT**信号之后，打印信息并打印一个新的提示符。下面的程序相比Figure 1.7多了11行，用`+`号标识了。

> Figure 1.10 Read commands from standard input and execute them 

```C
  #include "include/apue.h"
  #include <sys/wait.h>
  
+ static void sig_int(int);	/* our signal-catching function */
+
  int main(void)
  {
      char    buf[MAXLINE];   /* from apue.h */
      pid_t   pid;
      int     status;
  
  +	  if(signal(SIGINT, sig_int) == SIG_ERR)
          err_sys("signal error");
    
      printf("%% ");  /* print prompt (printf requires %% to print %) */
      while (fgets(buf, MAXLINE, stdin) != NULL) {
          if (buf[strlen(buf) - 1] == '\n')
              buf[strlen(buf) - 1] = 0; /* replace newline with null */
          if ((pid = fork()) < 0) {
              err_sys("fork error");
          } else if (pid == 0) {      /* child */
              execlp(buf, buf, (char *)0);
              err_ret("couldn’t execute: %s", buf);
              exit(127);
		  }
          /* parent */
          if ((pid = waitpid(pid, &status, 0)) < 0)
              err_sys("waitpid error");
          printf("%% ");
	  }
	  exit(0); 
  }
+
+ void sig_int(int signo){
+     printf("interrupt\n%% ");
+ }
```

## 1.10    Time Values

Historically, UNIX systems have maintained two different time values: 

1. **Calendar time**. This value counts the number of seconds since the **Epoch: 00:00:00 January 1, 1970,Coordinated Universal Time (UTC)**. (Older manuals refer to UTC as Greenwich Mean Time.) These time values are used to record the time when a file was last modified, for example. 

   The primitive system data type `time_t` holds these time values. 

2. **Process time**. This is also called CPU time and measures the central processor resources used by a process. Process time is measured in clock ticks, which have historically been 50, 60, or 100 ticks per second. 

   The primitive system data type `clock_t` holds these time values. (We’ll show how to obtain the number of clock ticks per second with the `sysconf` function in Section 2.5.4.) 

有两种类型的时间：**日历时间**和**进程时间**，日历时间也就是UTC。

When we measure the execution time of a process, as in Section 3.9, we’ll see that the UNIX System maintains three values for a process: 

- Clock time 
- User CPU time 
- System CPU time 

The clock time, sometimes called **wall clock time**, is the amount of time the process takes to run, and its value depends on the number of other processes being run on the system. Whenever we report the clock time, the measurements are made with no other activities on the system. 

The user CPU time is the CPU time attributed to user instructions. The system CPU time is the CPU time attributed to the kernel when it executes on behalf of the process. For example, whenever a process executes a system service, such as read or write, the time spent within the kernel performing that system service is charged to the process. The sum of user CPU time and system CPU time is often called the CPU time. 

度量进程执行时间，有三种：

1. 墙上时钟，也就是进程执行花费的总时间。
2. 用户CPU时间，是用户模式（非内核）下的CPU使用时间
3. 系统CPU时间，是进程进入内核执行的CPU使用时间

It is easy to measure the clock time, user time, and system time of any process: simply execute the time(1) command, with the argument to the time command being the command we want to measure. For example: 

```shell
$ cd /usr/include
$ time -p grep _POSIX_SOURCE */*.h > /dev/null
```

结果：

```
real    0m0.81s
user    0m0.11s
sys     0m0.07s
```

The output format from the time command depends on the shell being used, because some shells don’t run /usr/bin/time, but instead have a separate built-in function to measure the time it takes commands to run.  time命令的输出格式取决于使用什么shell，因为有些shell并不运行：`/usr/bin/time`，而是运行自己内置的一个time函数。

## 1.11    System Calls and Library Functions

All operating systems provide service points through which programs request services from the kernel. All implementations of the UNIX System provide a well-defined, limited number of entry points directly into the kernel called system calls (recall Figure 1.1). Version 7 of the Research UNIX System provided about 50 system calls, 4.4BSD provided about 110, and SVR4 had around 120. The exact number of system calls varies depending on the operating system version. More recent systems have seen incredible growth in the number of supported system calls. Linux 3.2.0 has 380 system calls and FreeBSD 8.0 has over 450.  随着时间的推移，系统调用越来越多，可见系统是越来越完善的。

The system call interface has always been documented in Section 2 of the UNIX Programmer’s Manual. Its definition is in the C language, no matter which implementation technique is actually used on any given system to invoke a system call. This differs from many older operating systems, which traditionally defined the kernel entry points in the assembly language of the machine.  **系统调用的文档总是在unix编程手册的第二个章节里。它是用C语言定义的**，不管系统具体是如何实现系统调用的。这一点与很多老操作系统不同（老操作系统使用汇编语言定义内核接口）

The technique used on UNIX systems is for each system call to have a function of the same name in the standard C library. The user process calls this function, using the standard C calling sequence. This function then invokes the appropriate kernel service, using whatever technique is required on the system. For example, the function may put one or more of the C arguments into general registers and then execute some machine instruction that generates a software interrupt in the kernel. For our purposes, we can consider the system calls to be C functions.  每个系统调用都对应一个相同名字的函数在标准C库里。用户进程调用这个函数，然后这个函数调用相应的内核服务。举个例子，这个函数可能会把一个或多个C参数放到通用寄存器，并执行机器指令在内核中产生一个软件中断。从我们的角度看，我们可以直接认为系统调用就是C函数。

Section 3 of the UNIX Programmer’s Manual defines the general-purpose library functions available to programmers. These functions aren’t entry points into the kernel, although they may invoke one or more of the kernel’s system calls. For example, the printf function may use the write system call to output a string, but the strcpy (copy a string) and atoi (convert ASCII to integer) functions don’t involve the kernel at all.  在**unix编程手册第三章定义了通用库函数给程序员。这些函数不是内核入口，虽然它们可能会调用一个或几个内核的系统调用**。举个例子，`printf`函数可能会使用`write`系统调用来输出一个字符串，但是`strcpy`（拷贝一个字符串）和`atoi`（吧ASCII字符转成整形）函数根本没有调用内核。

From an implementor’s point of view, the distinction between a system call and a library function is fundamental. From a user’s perspective, however, the difference is not as critical. From our perspective in this text, both system calls and library functions appear as normal C functions. Both exist to provide services for application programs. We should realize, however, that we can replace the library functions, if desired, whereas the system calls usually cannot be replaced.  从实现者的角度来看，系统调用和库函数的区别是很大的。然而从使用者的角度来看，这个区别并不重要。在这本书中，在我们看来，系统调用和库函数都以C函数的形式出现。两者的存在都是为了给应用开发者提供服务。然而我们应该意识到，**虽然我们能替换库函数（如果我们想这样做），但系统调用不能被替换**。

Consider the memory allocation function malloc as an example. There are many ways to do memory allocation and its associated garbage collection (best fit, first fit, and so on). No single technique is optimal for all programs. The UNIX system call that handles memory allocation, sbrk(2), is not a general-purpose memory manager. It increases or decreases the address space of the process by a specified number of bytes. How that space is managed is up to the process. The memory allocation function, malloc(3), implements one particular type of allocation. If we don’t like its operation, we can define our own malloc function, which will probably use the sbrk system call. In fact, numerous software packages implement their own memory allocation algorithms with the sbrk system call. Figure 1.11 shows the relationship between the application, the malloc function, and the sbrk system call.  让我们来看看内存分配函数`malloc`这个例子。有很多内存分配和相关的垃圾回收方法（最好适应算法，最先适应算法，等等）。没有哪个技术是对所有程序优化的。**unix系统调用`sbrk(2)`不是一个通用的存储管理器。它给进程增加和减少内存空间都是固定的字节数。怎么管理空间其实还要取决于进程自己。内存分配函数`malloc(3)`，实现了特定类型的分配。如果我们不喜欢它的做法，我们可以定义自己的malloc函数，但也是要用到sbrk系统调用的。实际上大量的软件包都通过直接使用sbrk系统调用实现了自己的内存管理算法**。图1.11展示了应用，malloc函数，和sbrk系统调用之间的关系。

<img src="https://i.loli.net/2018/05/25/5b07647fc7597.png", width="70%">

Here we have a clean separation of duties: the system call in the kernel allocates an additional chunk of space on behalf of the process. The malloc library function manages this space from user level.  这里职责是分明的：系统调用代表进程在内核里分配了额外的一块空间。malloc库函数在用户层级上管理这块空间。

Another example to illustrate the difference between a system call and a library function is the interface the UNIX System provides to determine the current time and date. Some operating systems provide one system call to return the time and another to return the date. Any special handling, such as the switch to or from daylight saving time, is handled by the kernel or requires human intervention. The UNIX System, in contrast, provides a single system call that returns the number of seconds since the Epoch: midnight, January 1, 1970, Coordinated Universal Time. Any interpretation of this value, such as converting it to a human-readable time and date using the local time zone, is left to the user process. The standard C library provides routines to handle most cases. These library routines handle such details as the various algorithms for daylight saving time.  另一个描述系统调用和库函数不同的例子是当前时间和日期。某些操作系统提供一个系统调用返回时间，另一个系统调用返回日期。任何特殊的处理，比如正常时制与夏令时的切换，需要内核的处理或者认为干预。Unix系统则相反，只提供一个系统调用，返回UTC（UTC是指从1970年的第一秒开始算起到现在经过的总时间）。任何对UTC这个值的解释，例如把它转成人类可读的时间日期使用当地时间，就留给了用户进程。标准C库提供了诸多例程来处理大多数情况。这些库例程处理这些细节，就像大多数算法处理夏令时切换一样。

An application can either make a system call or call a library routine. Also realize that many library routines invoke a system call. This is shown in Figure 1.12.  一个应用可以使用系统调用或者调用库例程，同样要意识到许多库例程调用了系统调用。

Another difference between system calls and library functions is that system calls usually **provide a minimal interface**, whereas library functions often **provide more elaborate functionality**. We’ve seen this already in the difference between the sbrk system call and the malloc library function. We’ll see this difference again later, when we compare the unbuffered I/O functions (Chapter 3) and the standard I/O functions (Chapter 5).  另一个系统调用和库函数的区别就是，系统调用往往只提供一个很小的接口，然而库函数经常提供更多精细的功能。

<img src="https://i.loli.net/2018/05/25/5b07d2d3861c0.png" width="70%">

The process control system calls (**fork, exec, and waitpid**) are usually invoked by the user’s application code directly. (Recall the bare-bones shell in Figure 1.7.) But some library routines exist to simplify certain common cases: the **system** and **popen** library routines, for example. In Section 8.13, we’ll show an implementation of the system function that invokes the basic process control system calls. We’ll enhance this example in Section 10.18 to handle signals correctly. 

To define the interface to the UNIX System that most programmers use, we have to describe both the system calls and some of the library functions. If we described only the sbrk system call, for example, we would skip the more programmer-friendly malloc library function that many applications use. **In this text, we’ll use the term function to refer to both system calls and library functions, except when the distinction is necessary.** 

