---
title: Pip换源
categories: Python Packages
tags: Pip
icon: note
---

## 临时
`pip install -i https://pypi.tuna.tsinghua.edu.cn/simple packagename`
[清华大学镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)

## 永久
在`./user/pip/`下新建`pip.ini`，内容为：
```[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
trusted-host = pypi.tuna.tsinghua.edu.cn
```du.cn
```