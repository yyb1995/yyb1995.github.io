---
title: 数据可视化Seaborn从零开始学习教程（三） 数据分布可视化篇
categories: Python Packages
tags: seaborn
icon: note
---

>本文转载自[数据可视化Seaborn从零开始学习教程（三） 数据分布可视化篇](https://segmentfault.com/a/1190000015006667)

## Seaborn 学习大纲

`seaborn`的学习内容主要包含以下几个部分：

1.  **风格管理**

    *   绘图风格设置
    *   颜色风格设置
2.  **绘图方法**

    *   `数据集的分布可视化`
    *   分类数据可视化
    *   线性关系可视化
3.  **结构网格**

    *   数据识别网格绘图

本次将主要介绍`数据集的分布可视化`的使用。

## 数据集分布可视化

当处理一个数据集的时候，我们经常会想要先看看特征变量是如何分布的。这会让我们对数据特征有个很好的初始认识，同时也会影响后续数据分析以及特征工程的方法。本篇将会介绍如何使用 seaborn 的一些工具来检测单变量和双变量分布情况。

首先还是先导入需要的模块和数据集。

```
%matplotlib inline
import numpy as np
import pandas as pd
from scipy import stats, integrate
import matplotlib.pyplot as plt
import seaborn as sns
sns.set(color_codes=True)
np.random.seed(sum(map(ord, "distributions")))
```

> 注意：这里的数据集是随机产生的分布数据，由 numpy 生成，数据类型是 ndarray。当然，pandas 的 Series 数据类型也是可以使用的，比如我们经常需要从 DataFrame 表中提取某一特征（某一列）来查看分布情况。

## 绘制单变量分布

在 seaborn 中，快速观察单变量分布的最方便的方法就是使用 `distplot()` 函数。默认会使用柱状图 (histogram) 来绘制，并提供一个适配的核密度估计(KDE)。

```
x = np.random.normal(size=100)
sns.distplot(x);
```

![](https://segmentfault.com/img/bVba71r?w=599&h=337)

#### 直方图（histograms

直方图是比较常见的，并且在 matplotlib 中已经存在了 `hist` 函数。直方图在横坐标的数据值范围内均等分的形成一定数量的数据段（bins），并在每个数据段内用矩形条（bars）显示 y 轴观察数量的方式，完成了对的数据分布的可视化展示。

为了说明这个，我们可以移除 kde plot，然后添加 rug plot（在每个观察点上的垂直小标签）。当然，你也可以使用 rug plot 自带的 `rugplot()` 函数，但是也同样可以在 `distplot` 中实现：

```
sns.distplot(x, kde=False, rug=True);
```

![](https://segmentfault.com/img/bVba71A?w=620&h=334)

当绘制直方图时，你最需要确定的参数是矩形条的数目以及如何放置它们。`distplot()`使用了一个简单的规则推测出默认情况下最合适的数量，但是或多或少的对 bins 数量进行一些尝试也许能找出数据的其它特征：

```
sns.distplot(x, bins=20, kde=False, rug=True);
```

![](https://segmentfault.com/img/bVba71S?w=608&h=333)

#### 核密度估计（Kernel density estimation）

核密度估计可能不被大家所熟悉，但它对于绘制分布的形状是一个非常有用的工具。就像直方图那样，KDE plots 会在一个轴上通过高度沿着其它轴将观察的密度编码。

```
sns.distplot(x, hist=False, rug=True);
```

![](https://segmentfault.com/img/bVba72q?w=570&h=332)

绘制 KDE 比绘制直方图需要更多的计算。它的计算过程是这样的，每个观察点首先都被以这个点为中心的正态分布曲线所替代。

```
x = np.random.normal(0, 1, size=30)
bandwidth = 1.06 * x.std() * x.size ** (-1 / 5.)
support = np.linspace(-4, 4, 200)

kernels = []
for x_i in x:

    kernel = stats.norm(x_i, bandwidth).pdf(support)
    kernels.append(kernel)
    plt.plot(support, kernel, color="r")

sns.rugplot(x, color=".2", linewidth=3);
```

![](https://segmentfault.com/img/bVba72x?w=585&h=339)

然后，这些替代的曲线进行加和，并计算出在每个点的密度值。最终生成的曲线被归一化，以使得曲线下面包围的面积是 1。

```
density = np.sum(kernels, axis=0)
density /= integrate.trapz(density, support)
plt.plot(support, density);
```

![](https://segmentfault.com/img/bVba72y?w=577&h=330)

我们可以看到，如果我们使用 `kdeplot()` 函数，我们可以得到相同的曲线。这个函数实际上也被 `distplot()` 所使用，但是如果你就只想要密度估计，那么 `kdeplot()` 会提供一个直接的接口更简单的操作其它选项。

```
sns.kdeplot(x, shade=True);
```

![](https://segmentfault.com/img/bVba72B?w=584&h=327)

KDE 的带宽参数（bw）控制着密度估计曲线的宽窄形状，有点类似直方图中的 bins 参数的作用。它对应着我们上面绘制的 KDE 的宽度。默认情况下，函数会按照一个通用的参考规则来估算出一个合适的值，但是尝试更大或者更小也可能会有帮助：

```
sns.kdeplot(x)
sns.kdeplot(x, bw=.2, label="bw: 0.2")
sns.kdeplot(x, bw=2, label="bw: 2")
plt.legend();
```

![](https://segmentfault.com/img/bVba72E?w=588&h=337)

如上所述，高斯 KDE 过程的意味着估计延续了数据集中最大和最小的值。 可以通过`cut`参数来控制绘制曲线的极值值的距离; 然而，这只影响曲线的绘制方式，而不是曲线如何拟合：

```
sns.kdeplot(x, shade=True, cut=0)
sns.rugplot(x);
```

![](https://segmentfault.com/img/bVba72J?w=575&h=338)

#### 拟合参数分布

你也可以使用`distplot()`将参数分布拟合到数据集，并可视化地评估其与观察数据的对应程度：

```
x = np.random.gamma(6, size=200)
sns.distplot(x, kde=False, fit=stats.gamma);
```

![](https://segmentfault.com/img/bVba72Q?w=610&h=335)

## 绘制双变量分布

对于双变量分布的可视化也是非常有用的。在 seaborn 中最简单的方法就是使用 `joinplot()` 函数，它能够创建一个多面板图形来展示两个变量之间的联合关系，以及每个轴上单变量的分布情况。

```
mean, cov = [0, 1], [(1, .5), (.5, 1)]
data = np.random.multivariate_normal(mean, cov, 200)
df = pd.DataFrame(data, columns=["x", "y"])
```

#### Scatterplots

双变量分布最熟悉的可视化方法无疑是散点图了，在散点图中每个观察结果以 x 轴和 y 轴值所对应的点展示。你可以用 matplotlib 的 `plt.scatter` 函数来绘制一个散点图，它也是`jointplot()`函数显示的默认方式。

```
sns.jointplot(x="x", y="y", data=df)
```

![](https://segmentfault.com/img/bVba73e?w=614&h=553)

#### Hexbin plots

直方图 `histogram` 的双变量类似图被称为 “hexbin” 图，因为它展示了落在六角形箱内的观测量。这种绘图对于相对大的数据集效果最好。它可以通过 matplotlib 的 `plt.hexbin` 函数使用，也可以作为 `jointplot` 的一种类型参数使用。它使用白色背景的时候视觉效果最好。

```
x, y = np.random.multivariate_normal(mean, cov, 1000).T
with sns.axes_style("white"):
    sns.jointplot(x=x, y=y, kind="hex", color="k");
```

![](https://segmentfault.com/img/bVba73i?w=585&h=563)

#### Kernel density estimation

还使用上面描述的核密度估计过程来可视化双变量分布。在 seaborn 中，这种绘图以等高线图展示，并且可以作为 `jointplot()`的一种类型参数使用。

```
sns.jointplot(x="x", y="y", data=df, kind="kde");
```

![](https://segmentfault.com/img/bVba73o?w=578&h=557)

你也可以用 `kdeplot` 函数来绘制一个二维的核密度图形。这可以将这种绘图绘制到一个特定的（可能已经存在的）matplotlib 轴上，而`jointplot()`函数只能管理自己：

```
f, ax = plt.subplots(figsize=(6, 6))
sns.kdeplot(df.x, df.y, ax=ax)
sns.rugplot(df.x, color="g", ax=ax)
sns.rugplot(df.y, vertical=True, ax=ax);
```

![](https://segmentfault.com/img/bVba73u?w=515&h=503)

如果你希望让双变量密度看起来更连续，您可以简单地增加 n_levels 参数增加轮廓级数：

```
f, ax = plt.subplots(figsize=(6, 6))
cmap = sns.cubehelix_palette(as_cmap=True, dark=0, light=1, reverse=True)
sns.kdeplot(df.x, df.y, cmap=cmap, n_levels=60, shade=True);
```

![](https://segmentfault.com/img/bVba73B?w=537&h=495)

`jointplot()`函数使用`JointGrid`来管理图形。为了获得更多的灵活性，您可能需要直接使用`JointGrid`绘制图形。`jointplot()`在绘制后返回 JointGrid 对象，你可以用它来添加更多层或调整可视化的其他方面：

```
g = sns.jointplot(x="x", y="y", data=df, kind="kde", color="m")
g.plot_joint(plt.scatter, c="w", s=30, linewidth=1, marker="+")
g.ax_joint.collections[0].set_alpha(0)
g.set_axis_labels("$X$", "$Y$");
```

![](https://segmentfault.com/img/bVba73H?w=568&h=565)

## 可视化数据集成对关系

为了绘制数据集中多个成对的双变量，你可以使用 `pairplot()` 函数。这创建了一个轴矩阵，并展示了在一个 DataFrame 中每对列的关系。默认情况下，它也绘制每个变量在对角轴上的单变量。

```
iris = sns.load_dataset("iris")
sns.pairplot(iris);
```

![](https://segmentfault.com/img/bVba73I?w=976&h=934)

就像 `joinplot()` 和 `JoinGrid` 之间的关系，`pairplot()` 函数建立在 `PairGrid` 对象之上，直接使用可以更灵活。

```
g = sns.PairGrid(iris)
g.map_diag(sns.kdeplot)
g.map_offdiag(sns.kdeplot, cmap="Blues_d", n_levels=6);
/Users/mwaskom/anaconda/lib/python2.7/site-packages/matplotlib/axes/_axes.py:545: UserWarning: No labelled objects found. Use label='...' kwarg on individual plots.
  warnings.warn("No labelled objects found. "
```

![](https://segmentfault.com/img/bVba73P?w=961&h=932)

> 参考：[http://seaborn.pydata.org/tut...](http://seaborn.pydata.org/tutorial.html)

* * *

关注微信公众号 **Python 数据科学**，获取 `120G` 人工智能 学习资料。
![](https://segmentfault.com/img/bV7oO9?w=344&h=344)

![](https://segmentfault.com/img/bV93KE?w=572&h=367)