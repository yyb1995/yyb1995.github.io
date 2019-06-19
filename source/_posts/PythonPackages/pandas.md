---
title: pandas
categories: Python Packages
tags: pandas
icon: note
---

## 1 对于时间的处理
python中datetime模块非常好用，提供了日期格式和字符串格式相互转化的函数strftime/strptime

1、由日期格式转化为字符串格式的函数为: `datetime.datetime.strftime()`

![](http://img.blog.csdn.net/20170205194641724)

2、由字符串格式转化为日期格式的函数为: `datetime.datetime.strptime()`

![](http://img.blog.csdn.net/20170205194637147)

3、两个函数都涉及日期时间的格式化字符串，列举如下：
```
%a 星期几的简写;如 星期三为Web 
%A 星期几的全称;如 星期三为Wednesday 
%b 月份的简写; 如4月份为Apr 
%B 月份的全称; 如4月份为April 
%c 标准的日期的时间串;（如： 04/07/10 10:43:39） 
%C 年份的后两位数字 
%d 十进制表示的每月的第几天 
%D 月/天/年 
%e 在两字符域中，十进制表示的每月的第几天 
%F 年-月-日 
%g 年份的后两位数字，使用基于周的年 
%G 年分，使用基于周的年 
%h 简写的月份名 
%H 24小时制的小时 
%I 12小时制的小时 
%j 十进制表示的每年的第几天 
%m 十进制表示的月份 
%M 十时制表示的分钟数 
%n 新行符 
%p 本地的AM或PM的等价显示 
%r 12小时的时间 
%R 显示小时和分钟：hh:mm 
%S 十进制的秒数 
%t 水平制表符 
%T 显示时分秒：hh:mm:ss 
%u 每周的第几天，星期一为第一天 （值从0到6，星期一为0） 
%U 第年的第几周，把星期日做为第一天（值从0到53） 
%V 每年的第几周，使用基于周的年 
%w 十进制表示的星期几（值从0到6，星期天为0） 
%W 每年的第几周，把星期一做为第一天（值从0到53） 
%x 标准的日期串 
%X 标准的时间串 
%y 不带世纪的十进制年份（值从0到99） 
%Y 带世纪部分的十制年份 
%z，%Z 时区名称，如果不能得到时区名称则返回空字符。 
%% 百分号
```
## 2 入门及基本操作
1. 查看数据的头5行/后5行
```
df.head()`
df.tail()
```
2. 查看数据索引，数据的列名，数据的值
```
df.index
df.columns
df.values
```
3. 对坐标轴进行排序
`df.sort_index(axis=1, ascending=False)`
注意：axis=1是以列名作为索引，也就是columns；axis=0是以行名作为索引，也就是index
4. 对值进行排序
`df.sort_values(by='B')`

5. 数据切片操作
df.loc  按照行、列名选取
df.iloc 按照行、列号选取
df.ix iloc和loc的结合

```
df.loc['20010101']  #选取行
df.loc[:, ['A']]  #选取列
df.iloc[0]
df.iloc[:, [0]]
```
**注意**:使用loc进行切片时，括号左右两边的值都包含，如`df.loc['A':'B']`取出的行包括index为A和B的行。使用iloc进行切片时，括号右边的值不包含，如`df.iloc[:2]`取出的行为序号为0和1的行

6. 对残缺数据处理
```
df.dropna()
df.fillna()
```
inplace参数：在原dataframe中进行修改

7. 数据的读取：
读取txt文件：
`df = pd.read_table('./data/bili.txt', sep=',')`
查看各列类型：df.info()
调整列顺序：
`df = df[['a', 'c', 'b']]`
删除一列：
`df.drop(axis=1, inplace=True)`
查找某行某列的值
```
# df.loc[conditions, column name] = newvalue
df_full.loc[(df_full['date'] == date_temp) & (df_full['time'] == time_temp), 'reqnum'] = reqnum_temp`
```

```
