---
title: POJ 1008 Maya Calendar
date: 2018-02-22
categories: [ACM, POJ]
comments: true
---

这一题的话：

1. 要注意 C++ 的 switch 不能使用 string，所以只好写成 if 来判断了。
2. 另外一个值得注意的地方是，空格会中断标准输入，所以不能使用一个 string 来装下一行输入，而是分别用 string Day，string str，int Year，装下 day, month, year。
3. 注意 Day[i]-'0'
4. 可以使用分批输出，无需打包成一个 string。

C++代码：

```C++
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

#define Haab 365
#define Tzolkin 260

int main() {
    int n;
    cin>>n;
    string Day;
    int Year;
    string str;
    cout << n << endl;
    while(n--){
        int num=0;
        cin >> Day;
        for(int i=0;Day[i]!='.';++i){
            num = num*10+Day[i]-'0';
        }
        cin >> str;
        if(str=="no")num += 20;
        if(str=="zip")num+=40;
        if(str=="zotz")num+=60;
        if(str=="tzec")num+=80;
        if(str=="xul")num+=100;
        if(str=="yoxkin")num+=120;
        if(str=="mol")num+=140;
        if(str=="chen")num+=160;
        if(str=="yax")num+=180;
        if(str=="zac")num+=200;
        if(str=="ceh")num+=220;
        if(str=="mac")num+=240;
        if(str=="kankin")num+=260;
        if(str=="muan")num+=280;
        if(str=="pax")num+=300;
        if(str=="koyab")num+=320;
        if(str=="cumhu")num+=340;
        if(str=="uayet")num+=360;
        cin >> Year;
        num += Year*365;
        cout << num%13+1;
        int month = num%260;
        switch(month%20){
            case 0:
                str = "imix";
                break;
            case 1:
                str = "ik";
                break;
            case 2:
                str = "akbal";
                break;
            case 3:
                str = "kan";
                break;
            case 4:
                str = "chicchan";
                break;
            case 5:
                str = "cimi";
                break;
            case 6:
                str = "manik";
                break;
            case 7:
                str = "lamat";
                break;
            case 8:
                str = "muluk";
                break;
            case 9:
                str = "ok";
                break;
            case 10:
                str = "chuen";
                break;
            case 11:
                str = "eb";
                break;
            case 12:
                str = "ben";
                break;
            case 13:
                str = "ix";
                break;
            case 14:
                str = "mem";
                break;
            case 15:
                str = "cib";
                break;
            case 16:
                str = "caban";
                break;
            case 17:
                str = "eznab";
                break;
            case 18:
                str = "canac";
                break;
            case 19:
                str = "ahau";
                break;
        }
        cout << " " +str +" ";
        cout << num/260 << endl;
    }
    return 0;
}
```
