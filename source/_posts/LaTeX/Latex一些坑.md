---
title: $\LaTeX$一些坑
categories: Latex
tags: Latex
icon: note
---

1. `\indent`后要跟空格
2. `\gather`不要留空行
3. `\gather`内嵌`\aligned`不能加`*`
4. 在使用`\mathbb`前需要导入包`\usepackage{amsfonts}`
5. 插入小、中、大括号的语句分别为：
```latex
\left\{ ... \right\}
\left[ ... \right]
\left( ... \right)
```
6. 图片、算法的插入一般使用`!htb`控制位置。如果强制指定图片位置，可使用`H`
7. **在编译$\LaTeX$前一定要先关闭用acrobat打开的pdf文档！**
8. **插入图片时图片名称不要带空格和点`.`！**否则可能报`Cannot determine size of graphic in fig.png (no BoundingBox).`
9. 一个`gather`环境中加入多个`align`环境且需要纵向对齐时在每一个`align`后加上`\\`

