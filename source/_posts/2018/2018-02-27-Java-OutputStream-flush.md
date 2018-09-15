---
layout: post
title: Java OutputStream flush
date: 2018-02-27
categories: [Java]
comments: true
---

FileOutPutStream继承OutputStream，并不提供flush()方法的重写，所以无论内容多少，write都会将二进制流直接传递给底层操作系统的I/O，flush无效果。而Buffered系列的输入输出流函数单从Buffered这个单词就可以看出他们是使用缓冲区的。

应用程序每次IO都要和设备进行通信，效率很低，因此缓冲区为了提高效率，当写入设备时，先写入缓冲区，每次等到缓冲区满了时，就将数据一次性整体写入设备，避免了每一个数据都和IO进行一次交互，IO交互消耗太大。

## 使用flush()和不使用flush()效果对比

### 不使用flush()

```Java
String s = "Hello World";
try {
    // create a new stream at specified file
    PrintWriter pw = new PrintWriter(System.out);
    // write the string in the file
    pw.write(s);
//            // flush the writer
//            pw.flush();
} catch (Exception ex) {
    ex.printStackTrace();
}
```

buffer没有满，输出为空。

### 使用flush()

```Java
String s = "Hello World";
try {
    // create a new stream at specified file
    PrintWriter pw = new PrintWriter(System.out);
    // write the string in the file
    pw.write(s);
    // flush the writer
    pw.flush();
} catch (Exception ex) {
    ex.printStackTrace();
}
```

得到期望的输出结果。

## close()和flush()作用有交集！

```Java
public static void main(String[] args) {
    BufferedWriter fw =null;
    try {
        fw =  new BufferedWriter(new FileWriter("e:\\test.txt"));
        fw.write("wo shi lucky girl.");
        //fw.flush();
        fw.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

因为close的时候，会把你没flush掉的一起flush掉。
缓冲区中的数据保存直到缓冲区满后才写出，也可以使用flush方法将缓冲区中的数据强制写出或使用close()方法关闭流，关闭流之前，缓冲输出流将缓冲区数据一次性写出。在这个例子中，flush()和close()都使数据强制写出，所以两种结果是一样的，如果都不写的话，会发现不能成功写出。

## Java默认缓冲区大小是多少？

默认缓冲去大小8192字节，也就是8k。

### 实验

```Java
char[] array  = new char[8192];
Arrays.fill(array,'s');
PrintWriter pw = new PrintWriter(System.out);
pw.write(array);
```

```Java
char[] array  = new char[8193];
Arrays.fill(array,'s');
PrintWriter pw = new PrintWriter(System.out);
pw.write(array);
```

当设置数组长度为8192时没有输出，设置8193时有输出。
