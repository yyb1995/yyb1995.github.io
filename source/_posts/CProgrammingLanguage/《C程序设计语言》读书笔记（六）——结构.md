Tags:《C语言程序设计》读书笔记
# 《C程序设计语言》读书笔记（六）——结构
## book
### 6.1
结构的初始化只能紧跟在结构类型的变量声明后面，必须对所有结构成员进行初始化，且初始化的值必须为常量。比如
``` c
	struct abc
	{
		int a;
		int b;
	} x = {100,200};
```
或者
``` c
struct abc y = {200,390};
```
或者
``` c
struct abc z;
z.a = 200;
z.b = 300;
```
而不允许
``` c
struct abc y;
y = {200,300};
```
### 6.2 结构与函数、指针
#### 1.
在函数中可以调用结构，也可以在一个函数中返回一个结构。这里要注意的是，在函数的声明中如果出现结构的形式，其类型是struct + 结构标记，如`struct abc`。下面给出课本上两个使用结构的函数：
``` c
struct point makepoint (int x,int y)  //返回类型为struct point
{
	struct point temp;
	temp.x = x;
	temp.y = y;
	return temp;  //返回结构时只写结构名即可
}

struct point addpoint(struct point p1,struct point p2)  //形参为两个struct point类型，返回为struct point类型
{
	p1.x += p2.x;
	p1.y += p2.y;
	return p1;
}
```
#### 2. 结构指针
在结构很大的条件下，使用指针方式的结构比普通的结构效率更高。结构指针的定义方式和普通指针类似：`struct point *pp;`。调用结构成员时可使用两种方法：`(*pp).a`或`pp->a`。个人感觉第二种方法不容易出错，因为运算符`.`的优先级高于`*`，所以第一种方法中圆括号不可少，而第二种方法简单直观。实际上，通过查找2.12的运算符优先级表可以看到，`->`和`.`的优先级是最高的，所以在类似于`++`、`*`等操作时一定注意括号的使用。
### 6.5 自引用结构
在结构的声明中不能包括结构本身，但是可以包括指向本结构的指针。在下面这个课本例子对于结构的声明中我们可以看到这一点。
``` c
/*例6.5 统计单词出现的次数完整版代码*/
#include <stdio.h>
#include <string.h>
#include <malloc.h>
#define MAXWORDNUM 100  //输入字符串最大数目
#define MAXWORDLEN 10  //输入每个字符串最大长度

struct tnode
{
	char *tword;
	int num;
	struct tnode *left;
	struct tnode *right;
};

struct tnode * Addtree(struct tnode *p, char *word)  //判断一个单词是否在二叉树中并添加其到合适位置，返回根节点
{
	if (p == NULL)
	{
		p = (struct tnode *)malloc(sizeof(struct tnode));  //分配struct tnode大小的空间
		p->tword = word;
		p->num = 1;
		p->left = NULL;
		p->right = NULL;
	}
	else if ((strcmp(p->tword, word)) == 0)
		p->num++;
	else if ((strcmp(p->tword, word)) > 0)
		p->left = Addtree(p->left, word);  //递归调用
	else p->right = Addtree(p->right, word);
	return p;
}
int Findtree(struct tnode *p,char *word)  //查找一个单词是否在二叉树中，找到返回1，否则返回0
{
	int result = 0;
	if(p != NULL)
	{
		result |= Findtree(p->left,word);  //如果在左子树中找到，左子树的Findtree返回1，和总结果做或运算即可
		if(strcmp(p->tword,word) == 0)
			result = 1;
		result |= Findtree(p->right,word);
	}
	return result;
}


void Printtree(struct tnode *p)  //打印整个二叉树
{
	if (p != NULL)  //这里不能使用while，否则会一直循环
	{
		Printtree(p->left);  //递归调用
		printf("word:%10s\t\tnumber:%2d\n", p->tword, p->num);
		Printtree(p->right);
	}
}


int main(int argc, char const *argv[])
{
	char *wordarray[MAXWORDNUM];
	int i = 0, j,result;
	char *temp,*tempword;
	printf("Please enter all words:\n");
	//将所有单词输入进一个字符串指针数组中
	do
	{
		wordarray[i] = (char*)malloc(MAXWORDLEN * sizeof(char));  //使用gets()输入字符串
		gets(wordarray[i]);
		i++;
	} while (*wordarray[i - 1] != '\0' && i < MAXWORDNUM);
	struct tnode *root;
	root = NULL;
	for (j = 0; j < ((*wordarray[i - 1] == '\0' ? i - 1 : i); j++)
		root = Addtree(root, wordarray[j]);  //创建整棵二叉树。注意每一个Addtree的调用返回的是它本身，所以root并不会因此
	printf("Press 1 to find a specific word,press 2 to print all words existed:\n");
	temp = (char *)malloc(sizeof(char));
	gets(temp);
	switch(*temp)
	{
		case '1':  //选择1：查找某个单词是否在二叉树中
		printf("Please enter the word you want to find:\n");
		tempword = (char*)malloc(MAXWORDLEN*sizeof(char));
		gets(tempword);
		result = Findtree(root,tempword);
		result > 0 ? puts("Found!"):puts("Not found!");
		break;
		case '2':  //选择2：打印整棵二叉树
		Printtree(root);
		printf("\nPrint complete!\n");
		break;
		default:
		printf("Input error!\n");
		break;
	}
return 0;
}
```

### 6.6 表查找
给出一个课本例题的稍微改动和添加版本，其中包含了hash表的初始化、添加数据、删除和查找功能，也包括使用malloc()和get_s()为字符串指针申请内存以及一些编程中容易出错的地方（花了我快一天啊啊啊效率太低了(╯°口°)╯(┴—┴）
``` c
//hash表的初始化、添加、删除和查找练习


// ConsoleApplication2.cpp : 定义控制台应用程序的入口点。
//

#include "stdafx.h"
#include <stdio.h>
#include <string.h>
#include <malloc.h>
#define MAXHASHSIZE 20
#define MAXCHARSIZE 20
#define SUCCESS 1
#define FAIL -1
typedef struct tnode
{
	char *tword;
	int count;
}Hashtable;

int InitHashtable(Hashtable *[]);
int AddToHashtable(Hashtable *[], char*);
int DeleteHashtable(Hashtable *[], char *);
int FindInHashtable(Hashtable *[], char *);
int InitHashtable(Hashtable *root[]);
unsigned int CalcHashValue(char *);
int InitHashtable(Hashtable *root[])  //初始化hash表，所有字符串指针指向NULL，计数为0
{
	int i;

	for (i = 0; i < MAXHASHSIZE; i++)
	{
		root[i] = (Hashtable*)malloc(sizeof(Hashtable));  //为字符串指针分配地址
		if ((*(root + i))->tword == NULL)  //存储空间满
			return FAIL;
		(*(root + i))->tword = NULL;
		(*(root + i))->count = 0;
	}
	return SUCCESS;
}

int AddToHashtable(Hashtable *root[], char *data)  //将一个字符串加入hash表
{
	unsigned int hashvalue, i = 0;
	hashvalue = CalcHashValue(data);
	while ((*(root + hashvalue))->tword != NULL && i < MAXHASHSIZE)  //欲放入的位置不为空
	{
		if (strcmp((*(root + hashvalue))->tword, data) == 0)
		{
			(*(root + hashvalue))->count++;
			break;
		}
		else
			hashvalue = (hashvalue + i) % MAXHASHSIZE;  //存放位置加1
		i++;
	}
	if (i == MAXHASHSIZE)
	{
		printf("Stack full!");
		return FAIL;
	}
	else
	{
		(*(root + hashvalue))->tword = data;  //放入字符串
		(*(root + hashvalue))->count = 1;  //计数
		return SUCCESS;
	}

}

int DeleteHashtable(Hashtable *root[], char *word)  //从hash表中删除一个字符串数据
{
	unsigned int hashvalue;
	int temp;
	temp = FindInHashtable(root, word);
	if (temp == FAIL)
	{
		printf("Cannot delete!\n");
		return FAIL;
	}
	else
	{
		(*(root + temp))->tword = NULL;  //字符串指针指向NULL
		(*(root + temp))->count = 0;
		return SUCCESS;
	}
}

int FindInHashtable(Hashtable *root[], char *word)  //在hash表中查找某个字符串
{
	unsigned int hashvalue;
	int i;
	hashvalue = CalcHashValue(word);
	for (i = 0;(*(root + hashvalue))->tword != NULL && strcmp((*(root + hashvalue))->tword, word) != 0 && i < MAXHASHSIZE; i++)
		hashvalue = (hashvalue + i) % MAXHASHSIZE;
	if (i < MAXHASHSIZE && (*(root + hashvalue))->tword != NULL)  //找到的时候没有循环一次而且不为空指针，说明已经找到
		return (hashvalue + i) % MAXHASHSIZE;  //返回hash表中位置
	else return FAIL;
}

unsigned int CalcHashValue(char *data)  //计算一个字符串hash的值
{
	int i = 0;
	unsigned int result = 0;
	while (*data != '\0')
		result = result * 31 + *data++;
	return result % MAXHASHSIZE;
}

int main(int argc, char const *argv[])
{
	char *temp[MAXHASHSIZE];
	char *wordtemp;
	int i = 0, j = 0, k, l;
	char m;
	Hashtable *root[MAXHASHSIZE];

	//初始化过程
	printf("Please enter all words:\n");
	do
	{
		*(temp + i) = (char *)malloc(MAXCHARSIZE * sizeof(char));  //为字符串指针分配存储空间
		gets_s(*(temp + i),MAXCHARSIZE);
		i++;
	} while (**(temp + i - 1) != '\0' && i < MAXHASHSIZE);
	if (InitHashtable(root) == SUCCESS)
		printf("Initial complete!\n");
	else
	{
		printf("Initial fail!\n");
		return FAIL;
	}


	//添加字符串数据过程
	for (j = 0; j < (**(temp + i - 1) == '\0' ? i - 1 : i); j++)
	{
		if (l = AddToHashtable(root, *(temp + j)) != SUCCESS)
		{
			printf("Add fail!\n");
			return FAIL;
		}
	}
	printf("Add complete!\n");


	//查找，删除或退出
	while (1)
	{
		printf("Press 1 to delete a word,press 2 to find a word,press 3 to break:\n");
		m = getchar();
		getchar();  /*注意注意注意在getchar()获取一个字符后一定要加一个getchar()清楚缓存区，
		否则下面的gets_s()会直接从缓存区读取回车号，造成的后果是读取的直接为空字符_(:3」∠)_*/
		switch (m)
		{
			case '1':  //删除
			printf("Please enter the word you want to delete:\n");
			wordtemp = (char*)malloc(MAXCHARSIZE * sizeof(char));
			gets_s(wordtemp, MAXCHARSIZE);
			k = DeleteHashtable(root, wordtemp);
			if (k == SUCCESS)
				printf("Delete successful!\n");
			else
				printf("Delete fail!\n");
			break;

			case '2':  //查找
			printf("Please enter the word you want to find:\n");
			wordtemp = (char*)malloc(MAXCHARSIZE * sizeof(char));
			gets_s(wordtemp, MAXCHARSIZE);
			k = FindInHashtable(root, wordtemp);
			if (k >= 0 && k < MAXHASHSIZE)
				printf("Found!%s is in the %d place.\n", (*(root + k))->tword, k);
			else if (k == FAIL)
				printf("Not found!\n");
			else
				printf("Error!\n");
			break;
			case '3':
			return 0;
		}
	}
	return 0;
}
```

###6.7 类型定义
格式：typedef 原类型名 新类型名
例子：
1. `typedef int length;`
2. `typedef char *string;` 定义string为字符串指针类型
3. `typedef struct tnode{} Tree;`  定义Tree为struct tnode类型，因此Tree tnode1等价于struct tnode tnode1;
4. `typedef struct tnode *pTree;`  定义pTree为指向struct tnode类型的指针，因此可用：`pTree pTree1; pTree1->member`访问成员变量
5. `typedef int (*Func1)(char*,char*);`  定义一个指向函数的指针，这个函数返回char类型，因此可用：`Func1 func1; a = (*func1)(str1,str2);`来调用这个函数

###6.8 共用体
[共用体（Union）简介](http://blog.csdn.net/yyb19951015/article/details/71036444)
### 6.9 位字段
位字段的作用是将多个对象保存在同一个机器字中，机器字是指计算机一次运算能够同时处理的最大位数，通常所说的32/64位电脑就是指机器字的长度。位字段在一个结构中声明，方式为：
``` c
//代码引用自：http://blog.csdn.net/lovecodeless/article/details/23270911
struct  
{
    unsigned int a  : 1;     //冒号“:”后的数字为该位字段所占的bit位数
    unsigned int b  : 3;      
    unsigned int c  : 5;
}flags;
```
因此，在对每个字段进行赋值的时候，要注意不能超过定义的最大范围，在上例中范围为0~2^(n-1)
更详细的介绍请参考：[http://blog.csdn.net/lovecodeless/article/details/23270911](http://blog.csdn.net/lovecodeless/article/details/23270911)
