---
title: C语言中数组名作为参数进行函数传递
categories: C语言
tags: 编程语言
icon: note
---


在普通变量或下标变量作函数参数时，形参变量和实参变量是由编译系统分配的两个不同的内存单元。在函数调用时发生的值传送是把实参变量的值赋予形参变量。在用数组名作函数参数时，不是进行值的传送，即不是把实参数组的每一个元素的值都赋予形参数组的各个元素。因为实际上形参数组并不存在，编译系统不为形参数组分配内存。那么，数据的传送是如何实现的呢?在我们曾介绍过，数组名就是数组的首地址。***因此在数组名作函数参数时所进行的传送只是地址的传送，也就是说把实参数组的首地址赋予形参数组名。***形参数组名取得该首地址之后，也就等于有了实在的数组。实际上是形参数组和实参数组为同一数组，共同拥有一段内存空间。***因此当形参数组发生变化时，实参数组也随之变化。***
![图片1](http://c.biancheng.net/cpp/uploads/allimg/120129/343dfgdgd645ccvnbm.gif?_=5778318)
上图说明了这种情形。图中设a为实参数组，类型为整型。a占有以2000为首地址的一块内存区。b为形参数组名。当发生函数调用时，进行地址传送，把实参数组a的首地址传送给形参数组名b，于是b也取得该地址2000。于是a，b两数组共同占有以2000为首地址的一段连续内存单元。从图中还可以看出a和b下标相同的元素实际上也占相同的两个内存单元（整型数组每个元素占二字节）。例如a[0]和b[0]都占用2000和2001单元，当然a[0]等于b[0]。类推则有a[i]等于b[i]。
多维数组也可以作为函数的参数。在函数定义时对形参数组可以指定每一维的长度，也可省去第一维的长度。因此，以下写法都是合法的：
`int MA(int a[3][10]);`或`int MA(int a[][10]);`

下面给出两个将数组传递进函数的例子：
ex1:
``` c
//求5名学生的平均成绩
#include <stdio.h>

float aver(float a[5])  //此处函数的定义中数组的元素个数可以省略，由传入的数组决定
{
    int i;
    float av,s=a[0];
    for(i=1;i<5;i++)
        s=s+a[i];
    av=s/5;
    return av;
}

int main(void){
    float sco[5],av;
    int i;
    printf("\ninput 5 scores:\n");
    for(i=0;i<5;i++)
        scanf("%f",&sco[i]);
    av=aver(sco);  //此处将数组传递进函数时使用的是数组名称，没有括号
    printf("average score is %5.2f",av);
    return 0;
}
```
ex2:
``` c
//将数组中小于0的元素置成0
#include <stdio.h>
void nzp(int a[8])
{
    int i;
    printf("\nvalues of array are:\n");
    for(i=0;i<8;i++)
    {
        if(a[i]<0)
            a[i]=0;
        printf("%d ",a[i]);
    }
}

int main(void)
{
    int b[5],i;
    printf("\ninput 5 numbers:\n");
    for(i=0;i<5;i++)
        scanf("%d",&b[i]);
    printf("initial values of array b are:\n");
    for(i=0;i<5;i++)
        printf("%d ",b[i]);
    nzp(b);  //将有5个元素的数组传递进一个有8个元素的数组的函数，编译通过
    printf("\nlast values of array b are:\n");
    for(i=0;i<5;i++)
        printf("%d ",b[i]);
    return 0;
}
```





