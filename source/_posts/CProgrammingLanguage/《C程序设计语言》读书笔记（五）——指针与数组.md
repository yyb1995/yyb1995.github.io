---
style: ocean
---
Tags:《C语言程序设计》读书笔记
# 《C程序设计语言》读书笔记（五）——指针与数组
## book
### 5.1 指针的概念
  1. 声明：`int *ip;  \\一个指向int类型的指针`
  2. 基本操作：`ip = &x;  \\将ip指向x，ip中存放x的地址`  `b = *ip; \\将ip指向地址的变量中的值取出赋给b`
取地址符号的优先级高于+-\*/和+=等，但与++和--优先级相同，运算顺序从右向左，因此在表示取ip指向的地址里的值并自加时应表示为：`(*ip)++`
### 5.3 指针与数组
1. 当一个指针指向数组的某一个元素时，指针数值加一必然指向数组的下一个元素，无论数组中元素的类型数组的长度是多少。
2. 数组名与指向该数组第一个元素的指针含义相同，即`int a[10]; a = &a[0]; *(a+i) = a[i];`。但是注意指针是变量，值可更改，数组名是常量，值不可更改。
3. 当数组作为参数传入一个函数中时，实际传入的是指向该数组的指针，因此之前在传入数组的函数定义中使用的a[]等价于*a。
### 5.4 指针用于地址分配
1. 在进行存储空间的分配和释放时，对于外部来说存放数据的数组名字和容量并不需要知道，只需设置一个静态数组和一个指向数组栈顶的指针，返回指向存储起点的指针即可。
### 5.5
1. 字符指针
`char *s`为指向一个字符串常量的指针，其实际指向为该字符串首个字符的地址址，可以通过`*(s + 1)`来访问字符串中的每一个字符，但**试图通过s来修改字符串是没有意义的**，因为指针指向的字符串是作为常量保存在静态存储区的。如果想要通过类似于`*(s + 1) = 'A';`的方法来修改字符串，只能通过前面介绍的字符串数组实现。
ex:
```c
void Strcpy(char *ori,char *des)
{
    while(*des++ = *ori++);  //先计算++，再赋值并与'\0'比较，最后des和des自加
}
```
### 5.6 指针数组和指向指针的指针
1. 形如`char *a[]`格式的称为指针数组，数组中的每一个元素为一个char类型的指针，可指向一个字符串或一个字符串数组。以`char *a[]`为例，下面给出一些指针数组的用法：
``` c
a[0] = "abcd";  //将指针数组中的第一个元素——一个char类型的指针指向一个字符串"abcd"
putchar(*(a[0]+1));  //打印a[0]指向的字符串中的第二个字符
```
2. 指向指针的指针
下面是一个简单的指向指针的指针的例子：
```
int a = 1,b = 2;
int *p1 = &a;
int *p2 = &b;
int *pp1 = &p1;
printf("%d %d %d",a,*p1,**pp1);  //利用三种方法打印变量a的值，结果相同
```
由指针的知识我们知道变量p1中存放的是变量a的地址，变量p2中存放的是变量b的地址。如果我们想用一个变量存放p1或p2的地址，就需要对p1或p2取地址，再放到一个变量里，这种变量就称为指向指针的指针，在上例中为pp1。那么，自然有`*pp1 = p1`和`pp1 = &p1`。
如果令`*pp1 = p2;`，完成的操作是将p2中存放的地址拷贝给p1，这时候p1和p2均指向b，自然有`*p1 == *p2`，且该值为2。**注意**：此时pp1仍然指向p1，也就是仍有`pp1 = &p1`成立，改变的是存储在p1中的值。如果想改变pp1的指向，应该用`pp1 = &p2;`来表示。在使用这些指向指针的指针时，记住一句话：***要想改变一个指针指向的变量，等号左边必须是指针名字；要想改变指针指向变量的值而不是改变指向关系，等号左边必须是对指针取值，即\*号。***
### 5.7 多维数组
1. 个人感觉多维数组和指针数组以及指向指针的指针有异曲同工之处。还是拿一个简单的例子来说说。设有一个二维数组`char a[3][10]`。容易联想到，这个数组实际上包含3个元素，而每一个元素又是一个长度为10的字符串数组。我们分两个层次分析这个数组：
 1. 联想到刚才的指针数组以及前面所学的***数组名实际上是一个指向数组首地址的指针***，我们发现a[3]实际上就是一个指针数组，a[0]~a[2]是分别指向三个字符串的首地址的指针。这样一来，想要打印出第i个字符串中的第j个字符的代码就显而易见了，有以下三种方法：
``` c
putchar(a[i - 1][j - 1]);  //直接取数组元素，最容易想到的方法
putchar(*(a[i - 1] + j - 1))  //利用指针来取元素，利用数组来取指针
putchar(*(*(a + i - 1) + j - 1)) //这里用指针来取指针数组的元素，感觉是最容易把人绕晕的方法_(:з」∠)_
```

 2. 再来看a[3]这个指针数组，重复使用刚才提到的***数组名实际上是一个指向数组首地址的指针***，可以看出a实际上是一个指向指针数组首个元素a[0]的指针，那么我们要想访问这个指针数组中的第i个元素，可以使用*(a + i - 1)完成，也就有了上面的第三种最容易把人绕晕的方法。
接下来提供两个有关多维数组和指向指针的指针的例子（写的不完善，仅供参考）
ex1：
``` c
//一个简单的利用指针进行字符串排序的程序
#include<stdio.h>
#include<string.h>
#define MAXSTRNUM 4   //最大输入数组数
#define MAXSTRLEN 10  //每个数组最大长度
void StringComp(char *[]);
int main()
{
	char s[MAXSTRNUM][MAXSTRLEN];
	char *q[MAXSTRNUM];
	int i;
	printf("Please enter %d strings:\n", MAXSTRNUM);
	for (i = 0;i < MAXSTRNUM;i++)
	{
		scanf("%s",*(s + i));
		*(q + i) = *(s + i);   /*指针数组q的每一个元素指向二维数组的每一个字符串，这里不能直接用s[i]当做指向字符串的指针，因为后面涉及到指针指向地址的交换，s[i]不具有交换功能*/
	}
	StringComp(q);
	for(i = 0;i < MAXSTRNUM;i++)
		printf("%s\n",*(q + i));
	return 0;
}

void StringComp(char *l[])
{
	int j,k;
	char *temp;
	for(j = 0;j < MAXSTRNUM - 1;j++)  //冒泡排序法
	{
		for(k = 0;k < MAXSTRNUM - 1 - j;k++)
		if(strcmp(*(l + k),*(l + k + 1)) > 0 )
		{
			temp = *(l + k + 1);  // 交换指针指向的对象
			*(l + k + 1) = *(l + k);
			*(l + k) = temp;
		}

	}
}
```
ex2：
``` c
//一个可以得到某年第几天的具体日期或某天是当年第几天的程序
#include <stdio.h>
int day_of_year(int,int *,int *);
void month_day(int,int,int *,int *);
int main(int argc, char const *argv[])
{
	int year,yearday,i,c,monthtemp,daytemp;
	int temp[4];
	int *month,*day;
	month = &monthtemp;
	day = &daytemp;
	printf("Please enter the year and yearday or the year,month and day:\n");
	scanf("%d,%d,%d,%d",temp,temp + 1,temp +2,temp + 3);
	if(*(temp + 3) == 1)
	{
		year = temp[0];
		yearday = temp[1];
		month_day(year,yearday,month,day);
		printf("year:%d month:%d day:%d\n",year,*month,*day );
	}
	else if(*(temp + 3) == 2)
	{
		year = temp[0];
		month = &temp[1];
		day = &temp[2];
		printf("year:%d yearday:%d\n",year,day_of_year(year,month,day));

	}
	else printf("Enter wrong~\n");


	return 0;
}


static char daytab[2][13] =
{
	{0,29,28,31,30,31,30,31,31,30,31,30,31},
	{0,31,29,31,30,31,30,31,31,30,31,30,31}
};  //daytab[][0]被设为0是为了方便计算月份的时候从1开始，符合习惯
int day_of_year(int year,int *month,int *day)
{
	int i,leap,yearday;
	yearday = *day;
	leap = year % 4 == 0 && year % 100 != 0 || year % 400 == 0;  //选择是否为闰年，闰年leap = 1
	for(i = 1;i < *month;i++)
		yearday += daytab[leap][i]; //总天数等于当月天数day加上该月份之前的天数之和
	return yearday;
}

void month_day(int year,int yearday,int *month,int *day)
{
	int i,leap;
	leap = year % 4 == 0 && year % 100 != 0 || year % 400 == 0;  //选择是否为闰年，闰年leap = 1
	for(i = 1;yearday > daytab[leap][i];i++)  //判断天数是否还大于一个月，若不大于，则返回结果
		yearday -= daytab[leap][i];
	*month = i;
	*day = yearday;
}
```
### 5.10 命令行参数
1. 在c程序开始执行时，可以向程序传递命令行参数，我们经常见到的`int main(int argc, char const *argv[])`就是命令行参数的表示。可以看到，命令行参数包括两个参数：int 类型的argc和char \*[]类型的argv。argc用于存放命令行参数的数目，argv是一个指向一系列字符串的数组，用于存放参数。由c语言规定，\*argv指向的是启动该程序的程序名，因此argc的值最小为1，自定义参数的存放也从\*(argv + 1)开始。同时，argv[argc]的值规定为null，也就是空指针，因此能够存放命令行参数的就只有argv[1]到argv[argc - 1]。
在编译时，可以通过IDE选择传递到程序的参数，在Dev-C++中设置方法为菜单栏Execute->Parameters。相邻参数使用空格分隔。比如，传入a b c，则有argc = 4,\*(\*argv + 1) = "a",\*(\*argv + 2) = "b",\*(\*argv + 3) = "c"。
2. 课本中的改写后的find函数实际上就是将一些指定的参数（如-n，-x）传进程序，从而查找并输出相应的行。`while(--argc > 0 && (*++argv)[0] == '-'`的含义是：遍历每一个输入参数的第一个字符，判断是否等于'-'。`(*++argv)[0]`以二维数组的形式改写就是argv\[i + 1\]\[0\]。下一行`c = *++argv[0]`的意思是取下一个字符，由此判断要查找的类型。由于'\*'和'++'是同级运算符，结合顺序是由右向左，而argv[0]是指向一个字符串（第一个字符）的指针，因此自加后该指针指向第二个字符，再用*取出该字符。
### 5.11 指向函数的指针
1. 这一节出现了一个看上去很奇怪的函数声明：
 `void  qsort(void *lineptr[],int left,int right,int (*comp)(void    *,void *)`.
这里面的(*comp)其实就是一个指向函数的指针，这个函数传入两个通用指针类型void *,返回int类型。
1. 函数指针变量的声明、初始化和使用：
函数指针的声明和普通函数的声明几乎一样，与普通函数声明唯一区别之处在于函数名是(\*comp)而不是comp，这点应该很好理解，因为声明的是一个指向函数的指针。在声明完成后，要让这个函数指针指向一个具体的函数。很显然指向的函数必须与函数指针的输入和返回参数的类型完全一致，这样才能匹配。初始化方法为:`comp = &compare`，compare为函数名。而使用函数指针的方法为：`*comp(a,b)`原理很简单，相当于取出指针指向的函数。
下面给出一个简单的使用函数指针变量和函数指针类型的程序：
``` c
#include <stdio.h>
void Change(int,int);
int main(int argc, char const *argv[])
{
	int a = 1,b = 2;
	/*1.定义一个指向函数的指针变量*/
	void (*Func1)(int,int);
	printf("Exercise1:\n");
	Func1 = &Change;  //函数指针初始化
	(*Func1)(a,b);  //注意此处不可以写成*Func1(a,b)

	/*2.定义一个指向函数的指针变量类型*/
	typedef void(*Func2)(int,int);  //定义了一个名为Func2的函数指针类型
	Func2 func2;  //定义一个名为func2的函数指针
	func2 = &Change;
	printf("Exercise2:\n");
	(*func2)(a,b);

	/*3.将一个指向函数的指针变量传递进一个函数*/
	printf("Exercise3:\n");
	void Func3(void (*func3)(int,int),int c,int d)  //注意这里传入Func3的函数指针func3相当于声明，不用写形参
	{
		(*func3)(c,d);
	}
	Func3(Func1,a,b);

	/*4.在一个函数中定义一个指向函数的指针类型*/
	printf("Exercise4:\n");
	void Func4(Func2 func4,int a, int b)
	{
		(*func4)(a,b);
	}
	Func4(Func1,a,b);
	return 0;
}
void Change(int a,int b)  //交换a,b的值并打印
{
	int i;
	i = a;
	a = b;
	b = i;
	printf("a = %d, b = %d\n",a,b );
}

```
