---
title: POJ 1005 I Think I Need a Houseboat
date: 2018-01-21
categories: [ACM, POJ]
comments: true
---

这一题也很简单，直接上代码：

C++ 代码：

```C++
#include <iostream>
#include <cmath>
using namespace std;

int main(){
    const double PI = 3.141592653589793;
    int n;
    double x;
    double y;
    double area;
    int year;
    cin >> n;
    int i=1;
    while(n--){
        cin >> x >> y;
        area = PI*(x*x+y*y)/2;
        year = area/50 + 1;
        cout << "Property " << i << ": This property will begin eroding in year " << year << "." << endl;
        i++;
    }
    cout << "END OF OUTPUT." << endl;
    return 0;
}
```
