---
title: POJ 1008 Maya Calendar
date: 2018-02-24
categories: [ACM, POJ]
comments: true
---

这一题花了不少时间，首先是题目没看懂，然后搜了很久的资料，才看懂了。

边缘检测是图像处理里面的基础，这题用的边缘检测就是最简单的边缘检测，根据周边八个像素点和此像素点差值 来描绘图像中的物体的边缘。

比如第一个点15，它周围的三个点是100，100，15，得到的结果是85，再比如第4行第2列是175，它周边的点是100，100，100，175，25，175，175，25，得到的结果是150.

输入也是刚开始没看懂，其实也很简单，第一个数表示图的宽度，接下来是若干对数字，前面是像素的值，后面是个数。

最后贴上有注释的代码（C++）：

```C++
//Memory Time   
//332K   32MS   

#include<iostream>  
#include<cmath>  
#include<algorithm>  
using namespace std;  

const int size=1000;  //每幅图片的pair上限  
int width;  //Map的宽  
int total=0;  //像素点总个数  

typedef class OutMapPix  
{  
    public:  
        int pos;    //OutMap中每个像素点的顺序位置，pos从1开始  
        int code;   //OutMap中每个像素点对应InMap的编码  
}Pix;  

int InMapPair[size][2];  //InMapPair[][0]为像素值，InMapPair[][1]为InMapPair[][0]连续出现的个数  
Pix OutMap[size*9];    //每个pix都依赖其周围的8个点编码  

int cmp(const void* a,const void* b);  //快排比较规则  
int GetValue(int pos);  //返回第pos个像素点的像素值  
int GetCode(int pos);   //返回第pos个像素点的编码  

int main(int k)  
{  
    while(cin>>width && width)  
    {  
        int pairv,pairt;  
        k=total=0;  
        while(cin>>pairv>>pairt && pairt)  
        {  
            InMapPair[k][0]=pairv;  
            InMapPair[k++][1]=pairt;  
            total+=pairt;  
        }  
        int PairNum=k;  //pair的个数  

        cout<<width<<endl;  

        int pos=1;  //当前处理的像素点的位置  
        k=0; //OutMap[]指针  
        for(int p=0;p<=PairNum;p++)  
        {  
            int row=(pos-1)/width;  //得到pos在二维图对应的坐标  
            int col=(pos-1)%width;  

            for(int i=row-1;i<=row+1;i++)        //枚举(row,col)周围及其自身共9个点(x,y)  
                for(int j=col-1;j<=col+1;j++)  
                {  
                    int tpos=i*width+j;  //得到(x,y)的顺序位置  

                    if(i<0 || j<0 || j>=width || tpos>=total)  
                        continue;  

                    OutMap[k].pos=tpos+1;  
                    OutMap[k++].code=GetCode(tpos+1);  //对发生变化的像素点的附近8个点和这一点本身编码（总共是9个点）
                }  

            pos+=InMapPair[p][1];  //跳跃，确定下一个像素发生变化的点的位置  
        }  

        qsort(OutMap,k,sizeof(Pix),cmp);  //对OutMap根据顺序位置  

        /*OutPut*/  

        Pix temp=OutMap[0];  
        for(int i=0;i<k;i++)  
        {  
            if(temp.code==OutMap[i].code)  
                continue;  
            cout<<temp.code<<' '<<OutMap[i].pos-temp.pos<<endl;  
            temp=OutMap[i];  
        }  
        cout<<temp.code<<' '<<total-temp.pos+1<<endl;  
        cout<<"0 0"<<endl;  

    }  
    cout<<0<<endl;  

    return 0;  
}  


/*快排比较规则*/  
int cmp(const void* a,const void* b)  
{  
    Pix* x=(Pix*)a;  
    Pix* y=(Pix*)b;  
    return x->pos - y->pos;  
}  

/*返回第pos个像素点的像素值*/  
int GetValue(int pos)  
{  
    int i=0,p=0;  
    while(p<pos)  
        p+=InMapPair[i++][1];  

    return InMapPair[i-1][0];  
}  

/*返回第pos个像素点的编码*/  
int GetCode(int pos)  
{  
    int code=GetValue(pos);  
    int MaxAbs=0;  

    int row=(pos-1)/width;  
    int col=(pos-1)%width;  

    for(int i=row-1;i<=row+1;i++)  
        for(int j=col-1;j<=col+1;j++)  
        {  
            int tpos=i*width+j;  

            if(i<0 || j<0 || j>=width || tpos>=total || tpos==pos-1)  //tpos==pos-1为中心的像素点，即当前待编码的点  
                continue;  

            int tcode=GetValue(tpos+1);  

            if(MaxAbs<abs(tcode-code))   //注意取绝对值  
                MaxAbs=abs(tcode-code);  
        }  

    return MaxAbs;  
}
```

要注意的点：

1. 主要就是自加，这里面用了很多的右自加，右自加是先使用值，然后再自加。
2. GetValue函数 是一个典型的寻区间代码。
3. 算法：只计算 RLE 起始点和其周围的点。这里不做证明。
