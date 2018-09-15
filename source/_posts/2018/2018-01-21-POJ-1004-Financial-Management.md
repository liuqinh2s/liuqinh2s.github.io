---
title: POJ 1003 Hangover
date: 2018-01-21
categories: [ACM, POJ]
comments: true
---

这一题简单的有点过分了，一度让我有点怀疑，但当我直接提交 AC 的那一刻，才发现还真是就这么简单。

C++ 代码：

```C++
#include <iostream>  
using namespace std;

int main(){
    double x;
    double sum;
    while(cin >> x){
        sum += x;
    }
    cout << "$" << sum/12 << endl;
    return 0;
}
```
