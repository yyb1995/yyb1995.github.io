---
title: 数据可视化Seaborn从零开始学习教程（二） 颜色调控篇
categories: Python Packages
tags: seaborn
icon: note
---

>本文转载自[数据可视化Seaborn从零开始学习教程（二） 颜色调控篇](https://segmentfault.com/a/1190000014966210)

## Seaborn 学习大纲

`seaborn`的学习内容主要包含以下几个部分：

1.  **风格管理**

    *   绘图风格设置
    *   `颜色风格设置`
2.  **绘图方法**

    *   数据集的分布可视化
    *   分类数据可视化
    *   线性关系可视化
3.  **结构网格**

    *   数据识别网格绘图

本次将主要介绍`颜色调控`的使用。

## 颜色风格设置

在`Seaborn`的使用中，是可以针对数据类型而选择合适的颜色，并且使用选择的颜色进行可视化，节省了大量的可视化的颜色调整工作。

还是一样，在介绍如何使用颜色外观设置之前，我们引入所需要的模块。

```
%matplotlib inline
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
sns.set(rc={"figure.figsize": (6, 6)})
np.random.seed(sum(map(ord, "palettes")))
```

下面所有操作均在 Jupyter notebook 中执行，如果对这个软件还不熟悉的朋友可以参考：[Jupyter notebook 快速入门教程](https://zhuanlan.zhihu.com/p/36764170)

## 建立调色板

对于不连续的外观颜色设置而言，最重要的函数恐怕要属`color_palette`了。这个函数拥有许多方法，让你可以随心所欲的可以生成各种颜色。并且，它可以被任何有`palette`参数的函数在内部进行使用（`palette`的中文意思是 "调色板"）。

关于这个函数有几个点需要知道一下：

*   `color_palette`函数可以接受任何`seaborn`或者`matplotlib`颜色表中颜色名称（除了`jet`），也可以接受任何有效的`matplotlib`形式的颜色列表（比如`RGB`元组，`hex`颜色代码，或者`HTML`颜色名称）。
*   这个函数的返回值总是一个由 RGB 元组组成的列表，无参数调用`color_palette`函数则会返回当前默认的色环的列表。

![](https://segmentfault.com/img/bVbaXrK?w=1055&h=148)

*   还有一个相应的函数，是`set_palette`，它接受与`color_palette`一样的参数，并会对所有的绘图的默认色环进行设置。当然，你也可以在`with`语句中使用`color_palette`来临时的改变默认颜色。

通常，在不知道数据特点的情况下，要找出并知道哪组颜色对一组数据是最好的有点不太现实。因此，我们将分为多种方式来使用`color_palette`函数和其它的 `seaborn paletee` 函数。

有三种通用的`color palette`可以使用，它们分别是：**qualitative**，**sequential**，**diverging**。

#### 1\. 分类色板（quanlitative）

`Qualitative`调色板，也可以说成是 **类型** 调色板，因为它对于分类数据的显示很有帮助。当你想要区别 _不连续的且内在没有顺序关系的_ 数据时，这个方式是最好的。

当导入`seaborn`时，默认的色环就被改变成一组包含 6 种颜色的调色板，它使用了标准的`matplolib`色环，为了让绘图变得更好看一些。

```
current_palette = sns.color_palette()
sns.palplot(current_palette)
```

![](https://segmentfault.com/img/bVbaXr5?w=665&h=94)

有 6 种不同的默认主题，它们分别是：**deep**，**muted**，**pastel**，**birght**，**dark**，**colorblind**。

```
themes = ['deep', 'muted', 'pastel', 'bright', 'dark', 'colorblind']
for theme in themes:
    current_palette = sns.color_palette(theme)
    sns.palplot(current_palette)
```

![](https://segmentfault.com/img/bVbaXse?w=560&h=340)

#### 使用色圈系统

默认的 6 种颜色看上去真不错，但是如果我们想要超过 6 种颜色呢？

当你有超过 6 种类型的数据要区分时，最简单的方法就是 **在一个色圈空间内使用均匀分布的颜色**。这也是当需要使用更多颜色时大多数`seaborn`函数的默认方式。

最常用的方法就是使用 `hls` 色空间，它是一种简单的`RGB`值的转换。

```
sns.palplot(sns.color_palette("hls", 8))
```

![](https://segmentfault.com/img/bVbaXss?w=848&h=93)

除此之外，还有一个 `hls_palette` 函数，它可以让你控制 `hls` 颜色的亮度和饱和度。

```
sns.palplot(sns.hls_palette(8, l=.3, s=.8))
```

![](https://segmentfault.com/img/bVbaXsw?w=853&h=90)

然而，由于人类视觉系统工作的原因，根据 RGB 颜色产生的平均视觉强度的颜色，从视觉上看起来并不是相同的强度。如果你观察仔细，就会察觉到，黄色和绿色会更亮一些，而蓝色则相对暗一些。因此，如果你想用`hls`系统达到一致性的效果，就会出现上面的问题。

为了修补这个问题，`seaborn`给`hls`系统提供了一个接口，可以让操作者简单容易的选择均匀分布，且亮度和饱和度看上去明显一致的色调。

```
sns.palplot(sns.color_palette("husl", 8))
```

![](https://segmentfault.com/img/bVbaXsx?w=849&h=93)

同样与之对应的，也有个`husl_palette`函数提供更灵活的操作。

#### 使用分类 Color Brewer 调色板

另外一种对分类数据比较友好的调色板来自`Color Brewer`工具。在`matplotlib`中也存在这些颜色表，但是它们并没有被合适的处理。在`seaborn`中，当你想要分类的 `Color Brewer` 调色板的时候，你总是可以得到不连续颜色，但是这也意味着在某一点上，这些颜色将会开始循环。

`Color Brewer` 网站中的一个很好的特点就是它提供了一个色盲安全指导。色盲颜色有很多种 [http://en.wikipedia.org/wiki/Color_blindness](http://en.wikipedia.org/wiki/Color_blindness)，但是最常见的当属辨别绿色和红色。如果可以避免使用红色和绿色来对绘图元素上色，那么对于一些色盲人群将会是一个很好的消息。

下面两组颜色就是使用红色和绿色组合，这可能并不是最好的选择。

```
sns.palplot(sns.color_palette("Paired"))
```

![](https://segmentfault.com/img/bVbaXs0?w=763&h=90)

```
sns.palplot(sns.color_palette("Set2", 10))
```

![](https://segmentfault.com/img/bVbaXs2?w=1041&h=86)

为了避免这些组合，我们需要从`Color Brewer`库中进行选择调色，有一个专门的 `choose_colorbrewer_palette` 函数可以实现这个功能。这个函数需要在 IPython notebook 中使用，因为 notebook 是一个交互式的工具，可以让你浏览各种选择并且调节参数。

```
sns_tpye = ["qualitative", "sequential", "diverging"]
for elem in sns_type:
    sns.choose_colorbrewer_palette(elem)
```

![](https://segmentfault.com/img/bVbaXs9?w=1054&h=413)

![](https://segmentfault.com/img/bVbaXta?w=1034&h=372)

![](https://segmentfault.com/img/bVbaXtd?w=1034&h=404)

*   n：调节颜色的个数；
*   desat：调节明暗和饱和度；

当然，您可能只想使用一组您特别喜欢的颜色。因为 color_palette() 接受一个颜色列表，这很容易做到。

```
flatui = ["#9b59b6", "#3498db", "#95a5a6", "#e74c3c", "#34495e", "#2ecc71"]
sns.palplot(sns.color_palette(flatui))
```

![](https://segmentfault.com/img/bVbaXtm?w=776&h=99)

#### 使用 xkcd 颜色来命名颜色

在众多的努力帮助下，`xkcd` 完成了随机的 `RGB` 颜色的命名。一共生成了 954 个颜色 [http://xkcd.com/color/rgb/](http://xkcd.com/color/rgb/)，并可可以随时通过`xkcd_rgb`字典调用。

```
plt.plot([0, 1], [0, 1], sns.xkcd_rgb["pale red"], lw=3)
plt.plot([0, 1], [0, 2], sns.xkcd_rgb["medium green"], lw=3)
plt.plot([0, 1], [0, 3], sns.xkcd_rgb["denim blue"], lw=3);
```

![](https://segmentfault.com/img/bVbaXtr?w=628&h=472)

如果想要从 `xkcd_rgb` 字典中单独的抽取出一些颜色，你也可以将一组选择好的颜色放到 `xkcd_palette` 函数中。

```
colors = ["windows blue", "amber", "greyish", "faded green", "dusty purple"]
sns.palplot(sns.xkcd_palette(colors))
```

![](https://segmentfault.com/img/bVbaXtF?w=657&h=103)

#### 2\. 连续色板（sequential）

调色板的第二大类被成为 "顺序"，这种调色板对于有从低（无意义）到高（有意义）范围过度的数据非常适合。尽管有些时候你可能想要在连续色板中使用不连续颜色，但是更通用的情况下是连续色板会作为颜色表在 `kdeplot()` 或者 `corrplot()` 或是一些 matplotlib 的函数中使用。

对于连续的数据，最好是使用那些在色调上有相对细微变化的调色板，同时在亮度和饱和度上有很大的变化。这种方法将自然地将数据中相对重要的部分成为关注点。

`Color Brewer` 的字典中就有一组很好的调色板。它们是以在调色板中的主导颜色 (或颜色) 命名的。

```
sns.palplot(sns.color_palette("Blues"))
```

![](https://segmentfault.com/img/bVbaXul?w=849&h=94)

就像在 matplotlib 中一样，如果您想要翻转渐变，您可以在面板名称中添加一个`_r`后缀。

```
sns.palplot(sns.color_palette("BuGn_r"))
```

![](https://segmentfault.com/img/bVbaXum?w=800&h=92)

seaborn 还增加了一个允许创建没有动态范围的 "dark" 面板。如果你想按顺序画线或点，这可能是有用的，因为颜色鲜艳的线可能很难区分。

类似的，这种暗处理的颜色，需要在面板名称中添加一个`_d`后缀。

```
sns.palplot(sns.color_palette("GnBu_d"))
```

![](https://segmentfault.com/img/bVbaXuq?w=748&h=96)

注意，你可能想使用 `choose_colorbrewer_palette()` 函数取绘制各种不同的选项。如果你想返回一个变量当做颜色映射传入 seaborn 或 matplotlib 的函数中，可以设置 `as_cmap` 参数为`True`。

#### “cubehelix” 连续调色板

`cubehelix`调色板系统在亮度和色变变化上具有线性上升或下降的特点。这意味着，当颜色表中的信息被转化为黑色和白色或者被一个色盲者看到的时候，它将会被保存下来。

matplotlib 有内建的默认 cubehelix 版本：

```
sns.palplot(sns.color_palette("cubehelix", 8))
```

![](https://segmentfault.com/img/bVbaXuw?w=913&h=96)

seaborn 为 cubehelix 系统添加一个接口使得其可以在各种变化中都保持良好的亮度线性梯度。

通过 seaborn 的 cubehelix_palette() 函数返回的调色板与 matplotlib 默认值稍有所不同，它不会在色轮周围旋转或覆盖更广的强度范围。seaborn 还改变了排序使得更重要的值显得更暗：

```
sns.palplot(sns.cubehelix_palette(8))
```

![](https://segmentfault.com/img/bVbaXuH?w=930&h=92)

其他`cubehelix_palette()`的参数主要调整色板的视觉。两个重要的选择是：`start`(值的范围为 03）和`rot`，或者旋转的次数（-1 和 1 之间）

```
sns.palplot(sns.cubehelix_palette(8, start=.5, rot=-.75))
```

![](https://segmentfault.com/img/bVbaXuI?w=934&h=91)

你也可以控制断点的亮度和甚至对调结果顺序：

```
sns.palplot(sns.cubehelix_palette(8, start=2, rot=0, dark=0, light=.95, reverse=True))
```

![](https://segmentfault.com/img/bVbaXuZ?w=930&h=89)

默认情况下你只会得到一些像 seaborn 其它调色板一样的颜色列表，但你也可以通过使用`as_cmap=True`让调色板返回一个可以被传入 seaborn 或 matplotlib 函数的颜色映射对象。

```
x, y = np.random.multivariate_normal([0, 0], [[1, -.5], [-.5, 1]], size=300).T
cmap = sns.cubehelix_palette(light=1, as_cmap=True)
sns.kdeplot(x, y, cmap=cmap, shade=True);
```

![](https://segmentfault.com/img/bVbaXu5?w=490&h=469)

与前面一样，也可以在 notebook 中使用`choose_cubehelix_palette()`来调节参数帮助选择更适合的调色板或颜色映射。如果想让函数返回一个类似`hexbin`的颜色映射而非一个列表则需要传入`as_cmap=True`。

#### 定制的连续调色板

对于一个更简单的接口定制连续色板，你可以使用 light_palette() 或者 dark_palette() 函数。它们都是单一颜色，并且能产生从亮值或者暗去饱和的值到这个颜色的调色板。伴随着这些函数，也同样有 `choose_light_palette` 和 `choose_dark_palette` 两个函数来交互式的调节创建调色板。

```
sns.palplot(sns.light_palette("green"))
```

![](https://segmentfault.com/img/bVbaXvj?w=735&h=87)

```
sns.palplot(sns.dark_palette("purple"))
```

![](https://segmentfault.com/img/bVbaXvs?w=807&h=93)

```
sns.palplot(sns.light_palette("navy", reverse=True))
```

![](https://segmentfault.com/img/bVbaXvv?w=790&h=93)

它们也可以创建一个颜色映射对象，而不仅仅是颜色列表。

```
pal = sns.dark_palette("palegreen", as_cmap=True)
sns.kdeplot(x, y, cmap=pal);
```

![](https://segmentfault.com/img/bVbaXvQ?w=489&h=475)

默认情况下，任何有效的 matplotlib 颜色可以作为输入。另外辅助的解释可以由`input`参数来控制。目前你可以在 hls 或 husl 空间中提供默认的 rgb 元组，您还可以使用任何有效的 xkcd 颜色的种子。

```
sns.palplot(sns.light_palette((210, 90, 60), input="husl"))
```

![](https://segmentfault.com/img/bVbaXvS?w=778&h=84)

```
sns.palplot(sns.dark_palette("muted purple", input="xkcd"))
```

![](https://segmentfault.com/img/bVbaXvY?w=791&h=90)

需要注意的是，`husl`是提供交互的组件的默认 input 空间，这与函数自身默认的并不同，但这在背景下却是更有用的。

#### 3\. 离散色板

调色板中的第三类被称为 **“离散”**。这类色板适用于数据特征含有大的低值和大的高值。数据中通常有一个意义明确的中点。例如，如果你想要从某个基线时间点绘制温度变化，最好使用离散的颜色表显示相对降低和相对增加面积的地区。

除了你想满足一个低强度颜色的中点以及用不同起始颜色的两个相对微妙的变化，其实选择离散色板的规则类似于顺序色板。同样重要的是，起始值的亮度和饱和度是相同的。

同样重要的是要强调，应该避免使用红色和绿色，因为大量的潜在观众将无法分辨它们。

同样，Color Brewer 颜色字典里也同时拥有一套精心挑选的离散颜色映射:

```
sns.palplot(sns.color_palette("BrBG", 7))
```

![](https://segmentfault.com/img/bVbaXv6?w=842&h=88)

```
sns.palplot(sns.color_palette("RdBu_r", 7))
```

![](https://segmentfault.com/img/bVbaXv8?w=833&h=93)

```
sns.palplot(sns.color_palette("coolwarm", 7))
```

![](https://segmentfault.com/img/bVbaXv9?w=820&h=92)

#### 定制的离散色板

你也可以使用 seaborn 函数 `diverging_palette()` 为离散的数据创建一个定制的颜色映射。（当然也有一个类似配套的互动工具：`choose_diverging_palette()`）。该函数使用 husl 颜色系统的离散色板。你需要传递两种色调，并可选择性的设定明度和饱和度的端点。函数将使用 husl 的端点值及由此产生的中间值进行均衡。

```
sns.palplot(sns.diverging_palette(220, 20, n=7))
```

![](https://segmentfault.com/img/bVbaXwh?w=804&h=86)

```
sns.palplot(sns.diverging_palette(145, 280, s=85, l=25, n=7))
```

![](https://segmentfault.com/img/bVbaXwA?w=821&h=88)
sep 参数控制面板中间区域的两个渐变的宽度。

```
sns.palplot(sns.diverging_palette(10, 220, sep=80, n=7))
```

![](https://segmentfault.com/img/bVbaXwF?w=865&h=93)

也可以用中间的色调来选择调色，而不是用亮度。

```
sns.palplot(sns.diverging_palette(255, 133, l=60, n=7, center="dark"))
```

![](https://segmentfault.com/img/bVbaXxH?w=832&h=90)

#### 设置默认的调色板

`color_palette()` 函数有一个名为`set_palette()`的配套使用函数。 set_palette()。set_palette() 接受与 color_palette() 相同的参数，但是它会更改默认的 matplotlib 参数，以便成为所有的调色板配置。

```
def sinplot(flip=1):
    x = np.linspace(0, 14, 100)
    for i in range(1, 7):
        plt.plot(x, np.sin(x + i * .5) * (7 - i) * flip)
sns.set_palette("husl")
sinplot()
```

![](https://segmentfault.com/img/bVbaXxK?w=622&h=471)

color_palette() 函数也可以在一个 with 块中使用，以达到临时更改调色板的目的。

```
with sns.color_palette("PuBuGn_d"):
    sinplot()
```

![](https://segmentfault.com/img/bVbaXxS?w=652&h=472)

#### 总结

*   本篇介绍了 seaborn 中的颜色调控方法，提到的一个重要函数是：color_palette()。针对不同的数据类型有三种调色方式：

    *   分类色板（qualitative）
    *   连续色板（sequential）
    *   离散色板（diverging）
*   其中还提到了 Color Brewer 工具，它可以很好的应用在以上三种调色方式上。
*   记住还有一个很有用的函数 choose_xxx_paletee()，用于交互式的调试颜色。
*   还可以通过 set_palette() 函数设置绘图默认参数。

> 参考：[http://seaborn.pydata.org/tut...](http://seaborn.pydata.org/tutorial.html)

* * *

关注微信公众号 **Python 数据科学**，获取 `120G` 人工智能 学习资料。

![](https://segmentfault.com/img/bV7oO9?w=344&h=344)
![](https://segmentfault.com/img/bV93KE?w=572&h=367)