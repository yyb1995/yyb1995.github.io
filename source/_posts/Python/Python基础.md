---
title: Python基础
categories: Python
icon: note
---

## 引用类型和值类型
在Python中，变量实际上都可以看成指向一个对象的指针。对象可以分为可变(Mutable)对象和不可变(Immutable)对象。如果变量指向的是一个不可变对象，那么这个对象的值在创建后不允许改变。如改变这个变量的值，实际上是把变量指向了一个新的不可变对象，也就是内存里面一个新的地址。如果变量指向的是一个可变对象，那么这个对象的值在创建后允许改变。改变这个对象的内容（如增删改等）不会改变这个对象在内存中的地址。Python中常见的可变和不可变类有

Class|Immutable
:-:|:-:
bool| True
int | True
float | True
list | False
tuple | True
str | True
set | False
frozenset | True
dict | False

