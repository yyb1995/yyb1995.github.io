---
title: 数据可视化Seaborn从零开始学习教程（四） 分类数据可视化篇
categories: Python Packages
tags: seaborn
icon: note
---

>本文转载自[数据可视化Seaborn从零开始学习教程（四） 分类数据可视化篇](https://segmentfault.com/a/1190000015310299)

## Seaborn 学习大纲

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

本次将主要介绍分类数据可视化的使用。

## 分类数据可视化

数据集中的数据类型有很多种，除了连续的特征变量之外，最常见的就是类目型的数据类型了，常见的比如人的性别，学历，爱好等。这些数据类型都不能用连续的变量来表示，而是用分类的数据来表示。

`seaborn`针对分类型的数据有专门的可视化函数，这些函数可大致分为三种：

*   **分类数据散点图：** swarmplot(), stripplot()
*   **分类数据的分布图：** boxplot(), violinplot()
*   **分类数据的统计估算图 ：** barplot(), pointplot()

这三类函数可有特点，可以从各个方面展示分类数据的可视化效果，下面我们一一介绍。
首先的首先还是先导入需要的模块和数据集。

```
%matplotlib inline
import numpy as np
import pandas as pd
import matplotlib as mpl
import matplotlib.pyplot as plt
import seaborn as sns
sns.set(style="whitegrid", color_codes=True)
np.random.seed(sum(map(ord, "categorical")))
titanic = sns.load_dataset("titanic")
tips = sns.load_dataset("tips")
iris = sns.load_dataset("iris")
```

#### 分类数据散点图

在分类数据的基础上展示定量数据的最简单函数就是 stripplot()。

```
sns.stripplot(x="day", y="total_bill", data=tips);
```

![](https://segmentfault.com/img/bVbco27?w=571&h=342)

这看上去类似散点图，但不同的是，横坐标是分类的数据，只不过一些数据点上会互相重叠，不便于观察。所以一个简单的解决办法是加入`jitter`参数，调整横坐标位置。

```
sns.stripplot(x="day", y="total_bill", data=tips, jitter=True);
```

![](https://segmentfault.com/img/bVbco3a?w=563&h=346)

当然，还有一个不同的方法就是使用 swarmplot() 函数，这个函数的好处就是所有的点都不会重叠，这样可以很清晰的观察到数据的分布。

```
sns.swarmplot(x="day", y="total_bill", data=tips);
```

![](https://segmentfault.com/img/bVbco3b?w=559&h=346)

在这基础上，也可以通过 hue 参数加入另一个嵌套的分类变量，而且嵌套的分类变量可以以不同的颜色区别，十分方便。

```
sns.swarmplot(x="day", y="total_bill", hue="sex", data=tips);
```

![](https://segmentfault.com/img/bVbco3d?w=563&h=344)

通常情况下，seaborn 还会尝试推断出分类变量的顺序。如果你的数据是 pandas 的分类数据类型，那么就是使用默认的分类数据顺序，如果是其他的数据类型，字符串类型的类别将按照它们在 DataFrame 中显示的顺序进行绘制，但是数组类别将被排序：

```
sns.swarmplot(x="size", y="total_bill", data=tips);
```

![](https://segmentfault.com/img/bVbco3f?w=563&h=351)

有时候将分类变量放在垂直轴上是非常有用的（当类别名称相对较长或有很多类别时，这一点特别有用）。 可以使用 orient 关键字强制定向，但通常可以互换 x 和 y 的变量的数据类型来完成：

```
sns.swarmplot(x="total_bill", y="day", hue="time", data=tips);
```

![](https://segmentfault.com/img/bVbco3g?w=589&h=351)

#### 分类数据分布图

虽然分类的散点图很有用，但有时候想要快速查看各分类下的数据分布就不是很直观了。为此，第二种函数解决了这个问题。

**箱型图**
通过箱型图可以很直观的观察到数据的四分位分布（1/4 分位，中位数，3/4 分位，以及四分位距），这种可视化对于在机器学习的预处理阶段（尤其是发现数据异常离散值）十分有效。

```
sns.boxplot(x="day", y="total_bill", hue="time", data=tips);
```

![](https://segmentfault.com/img/bVbco3j?w=561&h=348)

对于箱型图来说，使用 hue 参数的假设是这个变量嵌套在 x 或者 y 轴内。所以默认的情况下，hue 变量的不同类型值会保持偏置状态（两类或几类数据共同在 x 轴数据类型的一个类中），就像上面那个图所示。但是如果 hue 所使用的变量不是嵌套的，那么你可以使用 dodge 参数来禁止这个默认的偏置状态。

```
tips["weekend"] = tips["day"].isin(["Sat", "Sun"])
sns.boxplot(x="day", y="total_bill", hue="weekend", data=tips, dodge=False);
```

![](https://segmentfault.com/img/bVbco3k?w=563&h=350)

**提琴图**
另一种不同的方法是 violinplot() 函数，它结合了箱体图和分布教程中描述的核心密度估计过程：

```
sns.violinplot(x="total_bill", y="day", hue="time", data=tips);
```

![](https://segmentfault.com/img/bVbco3p?w=559&h=344)

这种方法使用核密度估计来更好地描述值的分布。此外，小提琴内还显示了箱体四分位数和四分位距。由于小提琴使用 KDE，还有一些其他可以调整的参数，相对于简单的 boxplot 增加了一些复杂性：

```
sns.violinplot(x="total_bill", y="day", hue="time", data=tips,
               bw=.1, scale="count", scale_hue=False);
```

![](https://segmentfault.com/img/bVbco3q?w=552&h=348)

当 hue 的嵌套类型只有两类的时候，也可以使用 split 参数将小提琴分割：

```
sns.violinplot(x="day", y="total_bill", hue="sex", data=tips, split=True);
```

![](https://segmentfault.com/img/bVbco3r?w=543&h=349)

在提琴图内，也可以使用 inner 参数以横线的形式来展示每个观察点的分布，来代替箱型的整体分布：

```
sns.violinplot(x="day", y="total_bill", hue="sex", data=tips,
               split=True, inner="stick", palette="Set3");
```

![](https://segmentfault.com/img/bVbco3s?w=543&h=350)

还有一点比较好的是，可以将 swarmplot()，violinplot()，或 boxplot() 混合使用，这样可以结合多种绘图的特点展示更完美的效果。

```
sns.violinplot(x="day", y="total_bill", data=tips, inner=None)
sns.swarmplot(x="day", y="total_bill", data=tips, color="w", alpha=.5);
```

![](https://segmentfault.com/img/bVbco3u?w=548&h=354)

#### 分类数据统计估计图

有时候，我们不想展示分类数据下的分布，而是想展示每一类的集中趋势。seaborn 有两个主要的方法来展示这个，并且这些函数 api 与上面函数的用法是一样的。
**条形图**
我们最熟悉的方式就是使用一个条形图。 在 Seaborn 中 barplot() 函数会在整个数据集上显示估计，默认情况下使用均值进行估计。 当在每个类别中有多个类别时（使用了 hue），它可以使用引导来计算估计的置信区间，并使用误差条来表示置信区间：

```
sns.barplot(x="sex", y="survived", hue="class", data=titanic);
```

![](https://segmentfault.com/img/bVbco3w?w=569&h=345)

条形图的特殊情况是当您想要显示每个类别的数量，而不是计算统计量。这有点类似于一个分类而不是定量变量的直方图。在 Seaborn 中，使用 countplot() 函数很轻易的完成：

```
sns.countplot(x="deck", data=titanic, palette="Greens_d");
```

![](https://segmentfault.com/img/bVbco3x?w=546&h=345)

如果将要计数的变量移动到 y 轴上，那么条形就会横过来显示：

```
sns.countplot(y="deck", hue="class", data=titanic, palette="Greens_d");
```

![](https://segmentfault.com/img/bVbco3B?w=552&h=355)

**点图**

pointplot() 函数提供了估计可视化的另一种风格。该函数会用高度估计值对数据进行描述，而不是显示一个完整的条形，它只绘制点估计和置信区间。另外，点图连接相同 hue 类别的点，比如 male 中的蓝色会连接 female 中的蓝色。这使得很容易看出主要关系如何随着第二个变量的变化而变化，因为你的眼睛可以很好地辨别斜率的差异：

```
sns.pointplot(x="sex", y="survived", hue="class", data=titanic);
```

![](https://segmentfault.com/img/bVbco3G?w=553&h=350)

为了使能够更好的显示，可以使用不同的标记和线条样式来展示不同 hue 类别的层次:

```
sns.pointplot(x="class", y="survived", hue="sex", data=titanic,
              palette={"male": "g", "female": "m"},
              markers=["^", "o"], linestyles=["-", "--"]);
```

![](https://segmentfault.com/img/bVbco3H?w=546&h=352)

#### 绘制 “宽格式” 数据

虽然使用 “长格式” 或“整洁”数据是优选的，但是这些函数也可以应用于各种格式的 “宽格式” 数据，包括 pandas DataFrame 或二维 numpy 数组阵列。这些对象应该直接传递给数据参数：

```
sns.boxplot(data=iris, orient="h");
```

![](https://segmentfault.com/img/bVbco3O?w=606&h=334)

此外，这些函数也接受 Pandas 或 numpy 对象的向量，而不仅仅是 DataFrame 中的变量：

```
sns.violinplot(x=iris.species, y=iris.sepal_length);
```

![](https://segmentfault.com/img/bVbco3Q?w=557&h=344)

为了控制由上述函数制作的图形的大小和形状，你必须使用 matplotlib 命令自己设置图形。 当然，这也意味着这些图块可以和其他种类的图块一起在一个多面板的绘制中共存：

```
f, ax = plt.subplots(figsize=(7, 3))
sns.countplot(y="deck", data=titanic, color="c");
```

![](https://segmentfault.com/img/bVbco3S?w=610&h=281)

#### 绘制多层面板分类图

正如我们上面提到的，有两种方法可以在 Seaborn 中绘制分类图。与回归图中的二元性相似，您可以使用上面介绍的函数，也可以使用更高级别的函数 factorplot()，将这些函数与 FacetGrid() 相结合，通过这个图形的更大的结构来增加展示其他类别的能力。 默认情况下，factorplot() 产生一个 pairplot()：

```
sns.factorplot(x="day", y="total_bill", hue="smoker", data=tips);
```

![](https://segmentfault.com/img/bVbco3T?w=502&h=365)

但是，kind 参数可以让你选择以上讨论的任何种类的图：

```
sns.factorplot(x="day", y="total_bill", hue="smoker", data=tips, kind="bar");
```

![](https://segmentfault.com/img/bVbco3U?w=501&h=367)

使用 factorplot() 的主要优点是可以很容易使用 "facet" 绘制多面图，展示更多其他分类变量：

```
sns.factorplot(x="day", y="total_bill", hue="smoker",
               col="time", data=tips, kind="swarm");
```

![](https://segmentfault.com/img/bVbco3V?w=850&h=370)

任何一种图形都可以画出来。由于 FacetGrid 的工作原理，要更改图形的大小和形状，需要指定适用于每个图的 size 和 aspect 参数：

```
sns.factorplot(x="time", y="total_bill", hue="smoker",
               col="day", data=tips, kind="box", size=4, aspect=.5);
```

![](https://segmentfault.com/img/bVbco31?w=863&h=365)

你也可以直接使用 boxplot() 和 FacetGrid 来制作这个图。但是，必须特别注意确保每个图的分类变量的顺序需要被强制，或者是使用具有 Categorical 数据类型的数据或通过命令和 hue_order。

```
sns.factorplot(x="time", y="total_bill", hue="smoker",hue_order=["No","Yes"]
               ,col="day", data=tips, kind="box", size=4, aspect=.5,
              palette="Set3");
```

![](https://segmentfault.com/img/bVbco33?w=581&h=259)

由于广义 API 函数的存在，分类数据也可以很容易应用于其他更复杂的上下文。 例如，它们可以轻松地与 PairGrid 结合，以显示多个不同变量之间的分类关系：

```
g = sns.PairGrid(tips,
                 x_vars=["smoker", "time", "sex"],
                 y_vars=["total_bill", "tip"],
                 aspect=.75, size=3.5)
g.map(sns.violinplot, palette="pastel");
```

![](https://segmentfault.com/img/bVbco34?w=540&h=455)

> 参考：[http://seaborn.pydata.org/tut...](http://seaborn.pydata.org/tutorial.html)

* * *

关注微信公众号 **Python 数据科学**，获取 `120G` 人工智能 学习资料。

![](https://segmentfault.com/img/bV7oO9?w=344&h=344)

![](https://segmentfault.com/img/bV93KE?w=572&h=367)