---
title: Leetcode Algorithm 274. H-Index
date: 2018-04-30
categories: [ACM, Leetcode]
comments: true
---

Given an array of citations (each citation is a non-negative integer) of a researcher, write a function to compute the researcher's h-index.

According to the [definition of h-index on Wikipedia](https://en.wikipedia.org/wiki/H-index): "A scientist has index h if h of his/her N papers have **at least** h citations each, and the other N − h papers have **no more than** h citations each."

For example, given `citations = [3, 0, 6, 1, 5]`, which means the researcher has `5` papers in total and each of them had received `3, 0, 6, 1, 5` citations respectively. Since the researcher has `3` papers with **at least** `3` citations each and the remaining two with **no more than** `3` citations each, his h-index is `3`.

**Note:** If there are several possible values for `h`, the maximum one is taken as the h-index.

**Credits:**
Special thanks to @jianchao.li.fighter for adding this problem and creating all test cases.

看了维基百科的解说之后，我发现这题还是挺简单的，思路就是排序，如果用C++解这题，我就直接用std::sort了。

C++代码如下：

```C++
class Solution {
public:
    int hIndex(vector<int>& citations) {
        if(citations.size()<=0){
            return 0;
        }
        std::sort(citations.begin(), citations.end(), compare);
        int result = citations[0];
        for(int i=0;i<citations.size();i++){
            if(i<citations[i]){
                result = i+1;
            }
        }
        return result;
    }
    
    static bool compare(int i, int j){
        return i>j;
    }
};
```

