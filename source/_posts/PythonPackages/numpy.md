---
title: numpy
categories: Python Packages
tags: numpy
icon: note
---

## 1 导入numpy
常用代码：`import numpy as np`或`from numpy import *`。建议使用第一种。使用第一种时要在numpy函数前加上`np.xxx`，如·`np.array`

## 2 矩阵的创建
常用代码:
1. 创建数组
`np.array([[1,2,3]])`创建一个1*3的numpy数组
2. 创建全1矩阵
`np.ones([a,b])`创建一个a*b的矩阵，元素全为1
***注意：np.ones和np.zeros的参数是一维向量，需要使用[]，而np.random.rand不需要使用[]***
3. 创建全0矩阵
`np.zeros([a,b])`创建一个a*b的矩阵，元素全为0
4. 创建对角矩阵
`np.diag([a,b,c])`创建一个对角线元素为a,b,c的矩阵
`np.diag([[a,b,c],[d,e,f],[g,h,i]])`创建一个1*3向量，元素为矩阵对角线元素


## 3 矩阵的运算
1. 计算向量间的欧氏距离
``` python
a1 = np.array([[1,2],[2,3]])
a2 = np.array([[2,0],[2,1]])
dist = np.linalg.norm(a1 - a2)
print(dist)
```
2. 一维数组的转置
`np.transpose([a])`

3. 矩阵的排序
`np.sort(a, axis=-1, kind='quicksort', order=None)`
返回排序好的矩阵，a为原矩阵，axis为排序方向，axis = 0为纵轴排序，axis = 1位横轴排序
4. 垂直合并矩阵
`np.vstack((a,b))`
将a,b矩阵垂直合并
5. 水平合并矩阵
`np.hstack((a,b))`
将a,b矩阵水平合并
6.按矩阵某行排序
`data = data[:,data[2].argsort()]`
按第3行从小到大排序
6. 按矩阵某列排序
`data = data[data[:,2].argsort()]`
按第3列从小到大排序
7. 矩阵的乘法
`np.dot(a,b)`
8. 多矩阵乘法
`np.linalg.multi_dot([a,b,c])`
9. 矩阵的加法
`np.sum(a)`
`np.sum(a,axis = 0)  #每列相加`
`np.sum(a,axis = 1)  #每行相加`
axis代表相加后消失的维数
10. 矩阵的删除
`np.delete(X,0,axis=0)`
删除矩阵第一行
11. 行向量转列向量
`a.shape = (3,1)`
12. 矩阵的随机化
`np.random.shuffle(matrix)`
沿着第一维进行重新排列。如果是二维数据，那么就按行进行重新排列

13. 去除nan值
`x = x[~np.isnan(x)]`
`np.nanmean(np.array([1, 2, 3, np.nan])`

14. 统计矩阵不重复的行或列
`unq, cnt = np.unique(a, return_counts=True, axis=0)`
`return counts`表示返回不重复行/列的数目，`axis=0`表示从行的方向统计。

15. 矩阵的重叠
在numpy中，矩阵的重叠有两种： np.tile()和np.repeat()。np.tile()是将整个矩阵进行重叠，np.repeat()是将矩阵中的每个元素进行重叠。
```python
a = np.array([[1, 2, 3], [4, 5, 6]])
b = np.tile(a, (2, 1))
c = np.repeat(a, 2, axis=0)
>>> b
array([[1, 2, 3],
       [2, 3, 4],
       [1, 2, 3],
       [2, 3, 4]])
>>> c
array([[1, 2, 3],
       [1, 2, 3],
       [2, 3, 4],
       [2, 3, 4]])
```

16. 使用元组索引数组
可以使用元组对数组进行索引，即`a[(2, 3)]`。如果是其他类型的，可以使用`tuple()`进行转换。

## 4 数据的产生
1. 正态分布数据
`numpy.random.normal(loc, scale, size)`
loc：float
    此概率分布的均值（对应着整个分布的中心centre）
scale：float
    此概率分布的标准差（对应于分布的宽度，scale越大越矮胖，scale越小，越瘦高）
size：int or tuple of ints
    输出的shape，默认为None，只输出一个值
2. 随机分布数据
`numpy.random.rand(a,b)`创建一个维数为a*b，数值在[0,1)的随机矩阵
3. 标准正态分布数据中随机抽取
`numpy.random.randn(a,b)`创建一个维数为a*b，数值取值为标准正态分布中随机取样本点
4. 产生不重复随机数组
`random.sample(a,b)`在a数组的范围内随机产生b个不重复的随机数
