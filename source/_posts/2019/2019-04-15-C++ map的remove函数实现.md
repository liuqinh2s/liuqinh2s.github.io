---
title: C++ map的remove函数实现
categories: [fixed issues]
comments: true
---

今天同学群里面讨论了这样一段代码，说是产品出了bug，现场急着修复。

<!--more-->

```c++
#include <iostream>
#include <map>
#include <string>

int main()
{
    std::cout << "hello world" << std::endl;
    std::map<int, std::string> mapPeople;
    mapPeople[1] = "hexu1";
    mapPeople[2] = "hexu2";
    // mapPeople[3] = "hexu3";
    // mapPeople[4] = "hexu4";

    std::map<int, std::string>::iterator iter = mapPeople.begin();
    for (; iter != mapPeople.end(); iter++)
    {
        if (iter->first == 2)
        {
            std::cout << "id : " << iter->first << ", name : " << iter->second << std::endl;
            mapPeople.erase(iter++);
            std::cout << (iter != mapPeople.end()) << std::endl;
        }
    }

    
    //for (; iter != mapPeople.end();)
    //{
    //	if (iter->first == 2)
    //	{
    //		std::cout << "id : " << iter->first << ", name : " << iter->second << std::endl;
    //		mapPeople.erase(iter++);
    //		if (iter == mapPeople.end())
    //			break;
    //	}
    //	iter++;
    //}

//    getchar();
    return 0;
}
```

说是第一个for循环会崩溃，第二个for循环可以通过。经过分析，主要问题其实是出在对迭代器和erase的不熟悉上，导致错误的使用，除此之外这个程序还有一个明显的问题，就是在erase这条分支中，iter++了两次。那么如何正确的写一个remove函数呢？代码如下：

```c++
#include <iostream>
#include <map>
#include <string>

int main()
{
    std::cout << "hello world" << std::endl;
    std::map<int, std::string> mapPeople;
    mapPeople[1] = "hexu1";
    mapPeople[2] = "hexu2";
    mapPeople[3] = "hexu3";
    mapPeople[4] = "hexu4";

    std::map<int, std::string>::iterator iter = mapPeople.begin();
    while (iter != mapPeople.end())
    {
        if (iter->first == 2)
        {
            std::cout << "id : " << iter->first << ", name : " << iter->second << std::endl;
            iter = mapPeople.erase(iter);
            continue;
        }
        iter++;
    }

    return 0;
}
```

碰到删除的时候，要小心，可以这么写：`iter = mapPeople.erase(iter);`，也可以这么写：`mapPeople.erase(iter++);`（必须在删除前给迭代器，否则迭代器会失效）。

另外注意迭代器不要加两次。