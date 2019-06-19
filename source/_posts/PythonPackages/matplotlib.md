---
title: Matplotlib
categories: Python Packages
tags: matplotlib Python
icon: note
---

实用网站：
- [https://matplotlib.org/gallery/index.html](https://matplotlib.org/gallery/index.html)
- [https://matplotlib.org/index.html](https://matplotlib.org/index.html)


## 1 画图
### 1.1 直线
```python
plt.subplots(1, 1)
x= range(100)
y= [i**2 for i in x]

plt.plot(x, y, linewidth = '1', label = "test", color=' coral ', linestyle=':', marker='|')
plt.legend(loc='upper left')
plt.show()
```

### 1.2 平行于坐标轴的直线
```python
plt.axvline(x, ymin, ymax)
plt.axhline(y, xmin, xmax)
```
### 1.3 柱状图
```
plt.figure(4, figsize=(10, 8), dpi=100
mean = [35.886, 20.512, 22.029, 31.130, 42.842]
std = [82.987, 65.015, 66.816, 71.533, 74.364]
width = 0.2
axislabel = ['10', '25', '50', '75', '100']
x = list(range(len(mean)))
 plt.bar(x, mean, width=width, label='均值', edgecolor='black', facecolor='white', hatch='')
for i in range(len(x)):
    x[i] = x[i] + width
plt.bar(x, std, width=width, label='标准差', edgecolor='black', facecolor='white', hatch='/')
plt.xticks([(i + width / 2) for i in range(len(axislabel))], axislabel)
plt.xlabel('输入历史窗长')
plt.legend(loc='upper right')
plt.show()
```


### 1.4 画热图（Heatmap）
[Creating annotated heatmaps](https://matplotlib.org/gallery/images_contours_and_fields/image_annotated_heatmap.html#sphx-glr-gallery-images-contours-and-fields-image-annotated-heatmap-py)

- [cmap参数查询](https://matplotlib.org/users/colormaps.html)

## 2 图像参数及属性
### 2.1 linestyle参数
```
'-'       solid line style
'--'      dashed line style
'-.'      dash-dot line style
':'       dotted line style
```
### 2.2 marker参数
```
'.'       point marker
','       pixel marker
'o'       circle marker
'v'       triangle_down marker
'^'       triangle_up marker
'<'       triangle_left marker
'>'       triangle_right marker
'1'       tri_down marker
'2'       tri_up marker
'3'       tri_left marker
'4'       tri_right marker
's'       square marker
'p'       pentagon marker
'*'       star marker
'h'       hexagon1 marker
'H'       hexagon2 marker
'+'       plus marker
'x'       x marker
'D'       diamond marker
'd'       thin_diamond marker
'|'       vline marker
'_'       hline marker
```
### 2.3 color参数
```
'aliceblue':            '#F0F8FF',
'antiquewhite':         '#FAEBD7',
'aqua':                 '#00FFFF',
'aquamarine':           '#7FFFD4',
'azure':                '#F0FFFF',
'beige':                '#F5F5DC',
'bisque':               '#FFE4C4',
'black':                '#000000',
'blanchedalmond':       '#FFEBCD',
'blue':                 '#0000FF',
'blueviolet':           '#8A2BE2',
'brown':                '#A52A2A',
'burlywood':            '#DEB887',
'cadetblue':            '#5F9EA0',
'chartreuse':           '#7FFF00',
'chocolate':            '#D2691E',
'coral':                '#FF7F50',
'cornflowerblue':       '#6495ED',
'cornsilk':             '#FFF8DC',
'crimson':              '#DC143C',
'cyan':                 '#00FFFF',
'darkblue':             '#00008B',
'darkcyan':             '#008B8B',
'darkgoldenrod':        '#B8860B',
'darkgray':             '#A9A9A9',
'darkgreen':            '#006400',
'darkkhaki':            '#BDB76B',
'darkmagenta':          '#8B008B',
'darkolivegreen':       '#556B2F',
'darkorange':           '#FF8C00',
'darkorchid':           '#9932CC',
'darkred':              '#8B0000',
'darksalmon':           '#E9967A',
'darkseagreen':         '#8FBC8F',
'darkslateblue':        '#483D8B',
'darkslategray':        '#2F4F4F',
'darkturquoise':        '#00CED1',
'darkviolet':           '#9400D3',
'deeppink':             '#FF1493',
'deepskyblue':          '#00BFFF',
'dimgray':              '#696969',
'dodgerblue':           '#1E90FF',
'firebrick':            '#B22222',
'floralwhite':          '#FFFAF0',
'forestgreen':          '#228B22',
'fuchsia':              '#FF00FF',
'gainsboro':            '#DCDCDC',
'ghostwhite':           '#F8F8FF',
'gold':                 '#FFD700',
'goldenrod':            '#DAA520',
'gray':                 '#808080',
'green':                '#008000',
'greenyellow':          '#ADFF2F',
'honeydew':             '#F0FFF0',
'hotpink':              '#FF69B4',
'indianred':            '#CD5C5C',
'indigo':               '#4B0082',
'ivory':                '#FFFFF0',
'khaki':                '#F0E68C',
'lavender':             '#E6E6FA',
'lavenderblush':        '#FFF0F5',
'lawngreen':            '#7CFC00',
'lemonchiffon':         '#FFFACD',
'lightblue':            '#ADD8E6',
'lightcoral':           '#F08080',
'lightcyan':            '#E0FFFF',
'lightgoldenrodyellow': '#FAFAD2',
'lightgreen':           '#90EE90',
'lightgray':            '#D3D3D3',
'lightpink':            '#FFB6C1',
'lightsalmon':          '#FFA07A',
'lightseagreen':        '#20B2AA',
'lightskyblue':         '#87CEFA',
'lightslategray':       '#778899',
'lightsteelblue':       '#B0C4DE',
'lightyellow':          '#FFFFE0',
'lime':                 '#00FF00',
'limegreen':            '#32CD32',
'linen':                '#FAF0E6',
'magenta':              '#FF00FF',
'maroon':               '#800000',
'mediumaquamarine':     '#66CDAA',
'mediumblue':           '#0000CD',
'mediumorchid':         '#BA55D3',
'mediumpurple':         '#9370DB',
'mediumseagreen':       '#3CB371',
'mediumslateblue':      '#7B68EE',
'mediumspringgreen':    '#00FA9A',
'mediumturquoise':      '#48D1CC',
'mediumvioletred':      '#C71585',
'midnightblue':         '#191970',
'mintcream':            '#F5FFFA',
'mistyrose':            '#FFE4E1',
'moccasin':             '#FFE4B5',
'navajowhite':          '#FFDEAD',
'navy':                 '#000080',
'oldlace':              '#FDF5E6',
'olive':                '#808000',
'olivedrab':            '#6B8E23',
'orange':               '#FFA500',
'orangered':            '#FF4500',
'orchid':               '#DA70D6',
'palegoldenrod':        '#EEE8AA',
'palegreen':            '#98FB98',
'paleturquoise':        '#AFEEEE',
'palevioletred':        '#DB7093',
'papayawhip':           '#FFEFD5',
'peachpuff':            '#FFDAB9',
'peru':                 '#CD853F',
'pink':                 '#FFC0CB',
'plum':                 '#DDA0DD',
'powderblue':           '#B0E0E6',
'purple':               '#800080',
'red':                  '#FF0000',
'rosybrown':            '#BC8F8F',
'royalblue':            '#4169E1',
'saddlebrown':          '#8B4513',
'salmon':               '#FA8072',
'sandybrown':           '#FAA460',
'seagreen':             '#2E8B57',
'seashell':             '#FFF5EE',
'sienna':               '#A0522D',
'silver':               '#C0C0C0',
'skyblue':              '#87CEEB',
'slateblue':            '#6A5ACD',
'slategray':            '#708090',
'snow':                 '#FFFAFA',
'springgreen':          '#00FF7F',
'steelblue':            '#4682B4',
'tan':                  '#D2B48C',
'teal':                 '#008080',
'thistle':              '#D8BFD8',
'tomato':               '#FF6347',
'turquoise':            '#40E0D0',
'violet':               '#EE82EE',
'wheat':                '#F5DEB3',
'white':                '#FFFFFF',
'whitesmoke':           '#F5F5F5',
'yellow':               '#FFFF00',
'yellowgreen':          '#9ACD32'
```

## 3 图像处理

### 3.1 最大化图像
```python
figManager = plt.get_current_fig_manager()
figManager.window.showMaximized()
```
### 3.2 将图像紧密排布
`plt.tight_layout()`

### 3.3 在线上标注某个点
`plt.scatter(x, y, size)`

### 3.4 给线加标签
```
plt.plot([1,2,3], label='abc')
plt.legend()
```

### 3.5 坐标轴
```python
# 设置X轴和Y轴的说明
plt.xlabel()  
plt.ylabel()

# 设置X轴和Y轴的刻度
plt.xticks()
plt.yticks()

# 设置范围
plt.xlim(0, 1)
plt.ylim(ymin=0)
```

## 4 字体修改
```python
## 查看可用系统字体
import matplotlib.font_manager
print ([f.name for f in matplotlib.font_manager.fontManager.ttflist])

# 修改默认字体为Times New Roman
plt.rcParams['font.sans-serif'] = ['Times New Roman']
plt.rcParams['axes.unicode_minus'] = False

# 单独修改某些部分的字体
    chinese_font = 'Simsun'
    ax = plt.gca()
    # 8 这四个分别为图例、x轴标签、y轴标签和标题
    modified_font_list = [ax.legend().texts, ax.xaxis.label, ax.title]
    plt.setp(modified_font_list, family=chinese_font)
```

## 5 图例(legend)
`plt.legend()`可传入的一些参数：
1. fontsize: 字号
2. labelspacing: label之间的间距
3. loc:图例的位置

## 6 figsize和dpi的关系
在plt.figure()中，figsize只改变图像的绝对大小不改变图像内元素的大小。而dpi会改变图像中元素的大小，同时改变图像的大小。



## 7 对ax对象的处理
### 7.1 画子图
```python
figure, ax = plt.subplots(2, 1, figsize=(12, 8))
ax[0].plot()
ax[1].plot()
```

### 7.2 设置axis
```python
# 设置label
ax.set_xlabel()
ax.set_ylabel()

# 设置label到坐标轴的距离
ax.xaxis.set_label_coords(-0.0075, 0.5)
ax.yaxis.set_label_coords(-0.0075, 0.5)

```

## 8 一些常用代码
### 8.1 Gaussian分布拟合
```python
# Gaussian distribution simulation
    xt = plt.xticks()[0]
    xmin, xmax = min(xt), max(xt)
    lnspc = np.linspace(xmin, xmax, len(mape_attn))

    # 12 lets try the normal distribution first
    m, s = stats.norm.fit(mape_attn)  # get mean and standard deviation
    pdf_g = stats.norm.pdf(lnspc, m, s)  # now get theoretical values in our interval
    plt.plot(lnspc, pdf_g, label="高斯分布拟合", linestyle='--')  # plot it
```

### 8.2 PDF & CDF
```python
plt.figure(7, figsize=(18, 9))
mape_attn = (attn_predict - attn_real) / attn_real * 100
mape_lstm = (lstm_predict - lstm_real) / lstm_real * 100
bins = np.arange(-15, 16, 2)
bins_axis = [(bins[i] + bins[i + 1]) / 2 for i in range(len(bins) - 1)]

plt.subplot(121)
hist_attn, _ = np.histogram(mape_attn, bins=bins, density=True)
cdf_attn = np.cumsum(hist_attn / sum(hist_attn))
hist_lstm, _ = np.histogram(mape_lstm, bins=bins, density=True)
cdf_lstm = np.cumsum(hist_lstm / sum(hist_lstm))

plt.plot(bins_axis, hist_attn, label='注意力网络', marker='^', markersize=markersize)
plt.plot(bins_axis, hist_lstm, label='GRU网络', marker='o', markersize=markersize)

plt.xlabel('相对百分比误差（%）')
plt.xlim(-15, 15)
plt.ylim(ymin=0)
plt.title('PDF')

# Gaussian distribution simulation
xt = plt.xticks()[0]
xmin, xmax = min(xt), max(xt)
lnspc = np.linspace(xmin, xmax, len(mape_attn))

# lets try the normal distribution first
m, s = stats.norm.fit(mape_attn)  # get mean and standard deviation
pdf_g = stats.norm.pdf(lnspc, m, s)  # now get theoretical values in our interval
plt.plot(lnspc, pdf_g, label="高斯分布拟合", linestyle='--')  # plot it

ax = plt.gca()
modified_font_list = [ax.xaxis.label, ax.yaxis.label]
plt.setp(ax.legend(loc='upper left').texts, family=chinese_font, fontsize=legendsize)
plt.setp(modified_font_list, family=chinese_font)

# Draw cdf figure
plt.subplot(122)
plt.plot(bins_axis, cdf_attn, label='注意力网络', marker='^', markersize=markersize)
plt.plot(bins_axis, cdf_lstm, label='GRU网络', marker='o', markersize=markersize)
plt.xlabel('相对百分比误差（%）')
plt.xlim(-15, 15)
plt.ylim(ymin=0)
plt.title('CDF')

ax = plt.gca()
modified_font_list = [ax.xaxis.label, ax.yaxis.label]
plt.setp(ax.legend(loc='upper left').texts, family=chinese_font, fontsize=legendsize)
plt.setp(modified_font_list, family=chinese_font)

plt.tight_layout()

plt.show()
```

## 9 一些实用步骤
### 9.1 matplotlib加入中文字体
1. 下载相应的中文ttf字体放入matplotlib的font文件夹
2. 在代码中加入`plt.rcParams['font.sans-serif'] = ['SimSun']`
3. 删除`C://Username//.matplotlib//文件夹下的字体缓存`

### 9.2 matplotlib画图名称不重复
`plt.figure(len(plt.get_fignums()) + 1)`

## 10 matplotlib面向对象编程
1. 绘制基本图片
```python
fig = plt.figure(1)
axes = fig.add_subplot(111)
axes1.plot(range(10))
plt.show()
```

2. 添加子图
`fig, ax = plt.subplots(3, 2)`
3. 添加标题
`ax.set_title('title1')`
4. 修改x/y轴刻度
`ax.set_xticks(range(len(data)))`
`ax.set_xticklabels(ticklist)`
`ax.set_yticklabels(ticklist)`


### 10.1 放大图像的一部分
```python
plt.figure(1, figsize=(16, 8), dpi=100)
# Add subplot16
ax1 = plt.subplot(211)
ts1 = attn_predict[0: 120 * displayday] / 1e9
ts2 = attn_real[0: 120 * displayday] / 1e9
ax1.plot(ts1, label='注意力模型预测值', color='k')
ax1.plot(ts2, label='真实值', linestyle='--', color='k')
plt.xlabel('时间(30秒)')ax1.yaxis.set_label_coords(-0.05, 0.5)
plt.ylabel('流量（GByte）')
plt.xlim(0, len(attn_predict[0: 120 * displayday + 100]))
# plt.ylim(0,17 )
plt.legend()

x1 = 560
x2 = 600

# select y-range for zoomed region
y1 = 28
y2 = 35

# Make the zoom-in plot:
axins1 = zoomed_inset_axes(ax1, zoom_scale, loc=4)  # zoom = 2
axins1.plot(ts1, color='k')
axins1.plot(ts2, linestyle='--', color='k')
axins1.set_xlim(x1, x2)
axins1.set_ylim(y1, y2)
plt.xticks(visible=False)
plt.yticks(visible=False)
mark_inset(ax1, axins1, loc1=2, loc2=4, fc="none", ec="0.5")
```
