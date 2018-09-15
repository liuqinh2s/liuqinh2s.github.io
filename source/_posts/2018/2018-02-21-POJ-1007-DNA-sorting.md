---
title: POJ 1007 DNA sorting
date: 2018-02-21
categories: [ACM, POJ]
comments: true
---

这道题有点尴尬，刚开始一直没看懂，主要是先入为主，以为是对每一串字符串做排序，最后发现居然是根据每一串 DNA 的逆序数，对串之间进行排序。

题目本身是简单的，第一步统计逆序数，第二步排序，这里使用C++标准库的sort。

C++代码：

```C++
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

struct dna
{
    int unorder;
    string s;
}DNA[105];

int inversionNumber(string s){
    int result = 0;
    int A, C, G;
    A = C = G = 0;
    for(int i = s.length()-1;i>=0;--i){
        switch(s[i]){
            case 'A':
                A++;
                break;
            case 'C':
                C++;
                result += A;
                break;
            case 'G':
                G++;
                result += A;
                result += C;
                break;
            case 'T':
                result += A;
                result += C;
                result += G;
                break;
            default:
                break;
        }
    }
    return result;
}

bool compare(dna a, dna b){
    return a.unorder < b.unorder;
}

int main() {
    int n, m;
    int i=0;
    cin >> n >> m;
    while(i<m){
        cin >> DNA[i].s;
        DNA[i].unorder = inversionNumber(DNA[i].s);
        i++;
    }
    sort(DNA, DNA+m, compare);
    for(int i=0;i<m;++i){
        cout << DNA[i].s << endl;
    }
    return 0;
}
```
