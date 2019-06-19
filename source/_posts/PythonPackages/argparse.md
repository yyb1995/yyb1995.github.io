---
title: argparse
categories: Python Packages
tags: argparse Python
icon: note
---

argparse模块用于在运行python程序时加入命令行参数。其基本用法是：
1. 创建一个parser

   `parser = argparse.ArgumentParser()`
2. 添加一个命令行参数
   `parser.add_argument('-data', help='func_help' required=True, type=int, choices=[1, 2, 3], nargs='?)`
其中，nargs表示需要参数的个数，'?'表示0或1，'*'表示任意，'+'表示1或多
3. 获取传入的参数变量
```python
opt = parser.parse_args()
data = opt.data
```

[Reference](https://blog.csdn.net/lis_12/article/details/54618868#%E6%B7%BB%E5%8A%A0%E5%8F%82%E6%95%B0%E9%80%89%E9%A1%B9-addargument)


