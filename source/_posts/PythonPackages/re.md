---
title: python中的正则表达式——re模块
categories: Python
icon: note
---

参考教程：[https://www.runoob.com/python/python-reg-expressions.html](https://www.runoob.com/python/python-reg-expressions.html)

一些有用的技巧：
1. `(?P<tag\d>)`。这种表示方式可以为搜索结果分组。在获取结果时可以使用`result.group('tag')`。通常用于`re.search()`和`re.match(
2. `re.S`表示把`.`认为是`(.|\n)`，也就是`.`包含换行符。`re.I`表示忽略大小写。
3. `re.findall()`返回一个列表，列表中每个元素表示一个匹配。
