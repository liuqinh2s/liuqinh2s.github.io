---
title: POJ 1001 Exponentiation
date: 2018-01-20
categories: [ACM, POJ]
comments: true
---

这是我第一次做 ACM，也是 POJ 上的第一题，本来以为这一题应该属于很简单的题，但没想到却花了不少时间。

本来我是想着用C++标准库的 pow 函数，但这一题显然是个大实数乘法，double 可能就不够用了，所以只能自己手动实现乘法。Java 有个 BigInteger 和 BigDecimal 分别用来处理大整数和大实数。

我的代码（已经 AC）：

C++代码：

```C++
#include <iostream>  
#include <string>  
#include <vector>
using namespace std;

 // 两数相乘，使用vector保存每一位数字，从低位到高位
vector<int> Multi(vector<int> v1,vector<int> v2)
{
    vector<int> result;
    for (int i = 0; i < v1.size() + v2.size() + 1; i++)
    {
        result.push_back(0);
    }
    for (int i = 0; i < v1.size(); i++)
    {
        for (int j = 0; j < v2.size(); j++)
        {
            result[i + j] += v1[i] * v2[j];
            result[i + j + 1] += result[i + j] / 10;
            result[i + j] %= 10;
        }
    }
    return result;
}

int main()  
{
    string str;
    int n;
    while(cin >> str >> n){
        // cout << str << " " << n << " ";
        vector<int> numArray;
        vector<int> resultArray;
        int dotPos = 0, begin = 0, end = str.length() - 1;

        //如果全是0和. 就直接输出0
        bool isAllZero = true;
        bool hasDot = false;
        for(int i=0;i<str.length();i++){
            if(str[i]!='0' && str[i]!='.'){
                isAllZero = false;
            }
            if(str[i]=='.'){
                hasDot = true;
            }
        }
        if(isAllZero){
            cout << 0;
            cout << endl;
            continue;
        }

        //如果没有小数点就不用删除后序零
        if(hasDot){
            //删除后序零（只删除小数点后的后序零，小数点之前的零保留，比如 10.0，变成10，而不是1）
            for (int i = str.length() - 1; i >= 0; i--)
            {
                if(str[i] != '0')
                {
                    end = i;
                    break;
                }
            }
        }

        // 测出小数位数
        for (int i = end; i >= 0; i--)
        {
            if (str[i] == '.')
            {
                dotPos = end - i;
                break;
            }
        }
        //删除前序零（如果小数点被0包裹，那么也将小数点删除，比如：00.001，变成1）
        for (int i = 0; i < str.length(); i++)
        {
            if(str[i] != '0' && str[i]!='.')
            {
                begin = i;
                break;
            }
        }
        // 将浮点数按位保存为整型
        for (int i = end; i >= begin; i--)
        {
            if(str[i]!='.'){
                numArray.push_back(str[i] - '0');
            }
        }
        // 复制数组
        for (int i = 0; i < numArray.size(); i++)
        {
            resultArray.push_back(numArray[i]);
        }
        // 计算数组的n次方
        for (int j = 0; j < n-1; j++)
        {
            resultArray = Multi(resultArray, numArray);
            int len = resultArray.size();
            // 移除前导零
            while (resultArray[len-1] == 0)
            {
                resultArray.pop_back();
                len--;
            }
        }
        // 输出结果，结果小于零时
        if(n * dotPos >= resultArray.size())
        {
            cout << ".";
            for(int k = 0; k < n * dotPos - resultArray.size(); k++)
            {
                cout << "0";
            }
            for(int k=resultArray.size()-1;k>=0;k--){
                cout << resultArray[k];
            }
        }else{
            for (int k = resultArray.size() - 1; k >= 0; k--)
            {
                if (k == n * dotPos - 1)
                {
                    cout << ".";
                }
                cout << resultArray[k];
            }
        }

        cout << endl;
    }
    return 0;
}
```

我自己写了一个测试用例的脚本：

input.cpp:

```C++
#include <iostream>
#include <vector>
#include <string>
#include <fstream>

using namespace std;

int main(){
    string buffer;
    ifstream in("hello.txt");
    if (! in.is_open())  
       { cout << "Error opening file"; exit (1); }
       while (!in.eof() )  
       {  
        getline(in,buffer);
        cout << buffer << endl;

       }
    return 0;
}
```

hello.txt 内容如下：

```
95.123 12
0.4321 20
5.1234 15
6.7592  9
98.999 10
1.0100 12
.00001  1
.12345  1
0001.1  1
1.1000  1
10.000  1
000.10  1
000000  1
000.00  1
.00000  0
000010  1
000.10  1
0000.1  1
00.111  1

0.0001  1
0.0001  3
0.0010  1
0.0010  3
0.0100  1
0.0100  3
0.1000  1
0.1000  3
1.0000  1
1.0000  3
1.0001  1
1.0001  3
1.0010  1
1.0010  3
1.0100  1
1.0100  3
1.1000  1
1.1000  3
10.000  1
10.000  3
10.001  1
10.001  3
10.010  1
10.010  3
10.100  1
10.100  3
99.000  1
99.000  3
99.001  1
99.001  3
99.010  1
99.010  3
99.100  1
99.100  3
99.998  1
99.998  3
```

这些测试用例都是前辈们留下来的资料。链接在这里：[测试用例](http://poj.org/showmessage?message_id=76017)

顺带讲一下怎么自动测试：

```
//打开命令行，先编译两个 C++ 文件：
g++ -std=c++11 -o input input.cpp
g++ -std=c++11 -o main main.cpp
//然后用管道把输入输出串起来，把最终解输入到文件 output.txt 里
./input | ./main > output.txt
```
