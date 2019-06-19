---
title: 数据可视化Seaborn从零开始学习教程（一） 风格选择
categories: Python Packages
tags: seaborn
icon: note
---

>本文转载自[数据可视化Seaborn从零开始学习教程（一） 风格选择](https://segmentfault.com/a/1190000014915873)

> 最近在做几个项目的数据分析，每次用到`seaborn`进行可视化绘图的时候总是忘记具体操作。虽然`seaborn`的官方网站已经详细的介绍了使用方法，但是毕竟是英文，而且查找不是很方便。因此博主想从零开始将`seaborn`学习一遍，做一个总结，也希望供大家使用参考。

## Seaborn 简介

`seaborn`同`matplotlib`一样，也是 Python 进行数据可视化分析的重要第三方包。但`seaborn`是在 `matplotlib`的基础上进行了更高级的 API 封装，使得作图更加容易，图形更加漂亮。

博主并不认为`seaborn`可以替代`matplotlib`。虽然`seaborn`可以满足大部分情况下的数据分析需求，但是针对一些特殊情况，还是需要用到`matplotlib`的。换句话说，`matplotlib`更加灵活，可定制化，而`seaborn`像是更高级的封装，使用方便快捷。

应该把`seaborn`视为`matplotlib`的补充，而不是替代物。

## Seaborn 学习内容

`seaborn`的学习内容主要包含以下几个部分：

1.  **风格管理**

    *   绘图风格设置
    *   颜色风格设置
2.  **绘图方法**

    *   数据集的分布可视化
    *   分类数据可视化
    *   线性关系可视化
3.  **结构网格**

    *   数据识别网格绘图

本次将主要介绍风格管理的使用。

## 风格管理 - 绘图风格设置

除了各种绘图方式外，图形的美观程度可能是我们最关心的了。将它放到第一部分，因为风格设置是一些通用性的操作，对于各种绘图方法都适用。

让我们先看一个例子。

```
%matplotlib inline
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
import seaborn as sns
np.random.seed(sum(map(ord, "aesthetics")))
```

我们定义了一个简单的方程来绘制一些偏置的正弦波，用来帮助我们查看不同的图画风格是什么样子的。

```
def sinplot(flip=1):
    x = np.linspace(0, 14, 100)
    for i in range(1, 7):
        plt.plot(x, np.sin(x + i * .5) * (7 - i) * flip)
```

`matplotlib`默认参数下绘制结果是这样的：

```
sinplot()
```

![](https://segmentfault.com/img/bVbaKnO?w=493&h=338)

转换为`seaborn`默认绘图，可以简单的用`set()`方法。

```
sns.set()
sinplot()
```

![](https://segmentfault.com/img/bVbaKnT?w=486&h=330)

`Seaborn` 将 `matplotlib` 的参数划分为两个独立的组合。第一组是设置绘图的外观风格的，第二组主要将绘图的各种元素按比例缩放的，以至可以嵌入到不同的背景环境中。

操控这些参数的接口主要有两对方法：

*   控制风格：`axes_style()`, `set_style()`
*   缩放绘图：`plotting_context()`, `set_context()`

每对方法中的第一个方法（`axes_style()`, `plotting_context()`）会返回一组字典参数，而第二个方法（`set_style()`, `set_context()`）会设置 matplotlib 的默认参数。

### Seaborn 的五种绘图风格

有五种`seaborn`的风格，它们分别是：**darkgrid**, **whitegrid**, **dark**, **white**, **ticks**。它们各自适合不同的应用和个人喜好。默认的主题是 **darkgrid**。

```
sns.set_style("whitegrid")
data = np.random.normal(size=(20, 6)) + np.arange(6) / 2
sns.boxplot(data=data);
```

![](https://segmentfault.com/img/bVbaKpk?w=486&h=321)

```
sns.set_style("dark")
sinplot()
```

![](https://segmentfault.com/img/bVbaKpw?w=484&h=327)

```
sns.set_style("white")
sinplot()
```

![](https://segmentfault.com/img/bVbaKpA?w=481&h=321)

```
sns.set_style("ticks")
sinplot()
```

![](https://segmentfault.com/img/bVbaKpG?w=491&h=341)

### 移除轴脊柱

**white** 和 **ticks** 两个风格都能够移除顶部和右侧的不必要的轴脊柱。通过`matplotlib`参数是做不到这一点的，但是你可以使用`seaborn`的`despine()`方法来移除它们：

```
sinplot()
sns.despine()
```

![](https://segmentfault.com/img/bVbaKpO?w=495&h=347)

一些绘图也可以针对数据将轴脊柱进行偏置，当然也是通过调用`despine()`方法来完成。而当刻度没有完全覆盖整个轴的范围时，`trim`参数可以用来限制已有脊柱的范围。

```
f, ax = plt.subplots()
sns.violinplot(data=data)
sns.despine(offset=10, trim=True);
```

![](https://segmentfault.com/img/bVbaKpS?w=493&h=354)

你也可以通过`despine()`控制哪个脊柱将被移除。

```
sns.set_style("whitegrid")
sns.boxplot(data=data, palette="deep")
sns.despine(left=True)
```

![](https://segmentfault.com/img/bVbaKqi?w=491&h=333)

### 临时设置绘图风格

虽然来回切换风格很容易，但是你也可以在一个`with`语句中使用`axes_style()`方法来临时的设置绘图参数。这也允许你用不同风格的轴来绘图：

```
with sns.axes_style("darkgrid"):
    plt.subplot(211)
    sinplot()
plt.subplot(212)
sinplot(-1)
```

![](https://segmentfault.com/img/bVbaKqB?w=482&h=329)

### 覆盖 seaborn 风格元素

如果你想定制化`seaborn`风格，你可以将一个字典参数传递给`axes_style()`和`set_style()`的参数`rc`。而且你只能通过这个方法来覆盖风格定义中的部分参数。

如果你想要看看这些参数都是些什么，可以调用这个方法，且无参数，这将会返回下面的设置：

```
sns.axes_style()
{'axes.axisbelow': True,
 'axes.edgecolor': '.8',
 'axes.facecolor': 'white',
 'axes.grid': True,
 'axes.labelcolor': '.15',
 'axes.linewidth': 1.0,
 'figure.facecolor': 'white',
 'font.family': [u'sans-serif'],
 'font.sans-serif': [u'Arial',
  u'DejaVu Sans',
  u'Liberation Sans',
  u'Bitstream Vera Sans',
  u'sans-serif'],
 'grid.color': '.8',
 'grid.linestyle': u'-',
 'image.cmap': u'rocket',
 'legend.frameon': False,
 'legend.numpoints': 1,
 'legend.scatterpoints': 1,
 'lines.solid_capstyle': u'round',
 'text.color': '.15',
 'xtick.color': '.15',
 'xtick.direction': u'out',
 'xtick.major.size': 0.0,
 'xtick.minor.size': 0.0,
 'ytick.color': '.15',
 'ytick.direction': u'out',
 'ytick.major.size': 0.0,
 'ytick.minor.size': 0.0}
```

然后，你可以设置这些参数的不同版本了。

```
sns.set_style("darkgrid", {"axes.facecolor": ".9"})
sinplot()
```

![](https://segmentfault.com/img/bVbaKqJ?w=489&h=332)

### 绘图元素比例

有一套的参数可以控制绘图元素的比例。
首先，让我们通过`set()`重置默认的参数：

```
sns.set()
```

有四个预置的环境，按大小从小到大排列分别为：**paper**, **notebook**, **talk**, **poster**。其中，**notebook** 是默认的。

```
sns.set_context("paper")
sinplot()
```

![](https://segmentfault.com/img/bVbaKq8?w=476&h=319)

```
sns.set_context("talk")
sinplot()
```

![](https://segmentfault.com/img/bVbaKrb?w=491&h=326)

```
sns.set_context("poster")
sinplot()
```

![](https://segmentfault.com/img/bVbaKrt?w=529&h=339)

你可以通过使用这些名字中的一个调用`set_context()`来设置参数，并且你可以通过提供一个字典参数值来覆盖参数。当改变环境时，你也可以独立的去缩放字体元素的大小。

```
sns.set_context("notebook", font_scale=1.5, rc={"lines.linewidth": 2.5})
sinplot()
```

![](https://segmentfault.com/img/bVbaKrz?w=515&h=334)

同样的，你可以通过嵌入`with`语句临时的控制绘图的比例。

## 总结

介绍了 Seaborn 的 5 中绘图风格

*   移除轴脊柱
*   临时设置绘图风格
*   覆盖 Seaborn 风格元素
*   绘图元素比例缩放

下一节将会介绍颜色风格的使用。

* * *

关注微信公众号 **Python 数据科学**，获取 `120G` 人工智能 学习资料。

![](https://segmentfault.com/img/bV7oO9?w=344&h=344)

![](https://segmentfault.com/img/bV93KE?w=572&h=367)