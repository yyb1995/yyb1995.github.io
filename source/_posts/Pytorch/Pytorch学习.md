---
title: Pytorch知识点学习
categories: Pytorch
tags: Pytorch
icon: note
---

## 1 torch.nn和torch.nn.functional中神经网络层的区别
torch.nn中的层是类，torch.nn.functional中的层是函数。torch.nn中的forward()方法是调用torch.nn.functional实现。因此两者从原理上等价。torch.nn一般用于较复杂层的实现，torch.nn.functional一般用于简单层的实现

## 2 Pytorch的层建立方式
1. nn.Sequential().add_module(layer)
```python
net1 = nn.Sequential()
net1.add_module('conv', nn.Conv2d(3, 3, 3))
net1.add_module('batchnorm', nn.BatchNorm2d(3))
net1.add_module('activation_layer', nn.ReLU())
```

2. nn.Sequential(layer)
```python
net2 = nn.Sequential(
    nn.Conv2d(3, 3, 3),
    nn.BatchNorm2d(3),
    nn.ReLU()
    )
```

3. nn.Sequential(OrderedDict([(multi layername, layer)]))
```python
from collections import OrderedDict
net3 = nn.Sequential(OrderedDict([
    ('conv', nn.Conv2d(3, 3, 3)),
    ('batchnorm', nn.BatchNorm2d(3)),
    ('activation_layer', nn.ReLU())
]))

```
4. nn.ModuleList([layers])
```python
model1 = nn.ModuleList(
    nn.Linear(10, 1) for _ in range(3)
)
```

## 3 Pytorch自定义层的编写
下面是一个典型的Pytorch自定义层的实现方法
```python
class ScaledDotProductAttention(nn.Module):
    """ Scaled Dot-Product Attention """

    def __init__(self, temperature, attn_dropout=0.1):
        """
        :param temperature: scale parameter in the equation
            out = Q * K.T / temperature * V. Default is \sqrt d_k
        :param attn_dropout: dropout rate in the self-attention
            layer
        """
        super(ScaledDotProductAttention, self).__init__()
        self.temperature = temperature
        self.dropout = nn.Dropout(attn_dropout)
        self.softmax = nn.Softmax(dim=2)

    def forward(self, q, k, v, mask=None):
        """
        Calculate self-attention output
        :param q: size: (batch_size, max_seq_len or input_len, d_k)
        :param k: same as q
        :param v: size: (batch_size, max_seq_len or input_len, d_k)
        :param mask:
        :return:
        """
        attn = torch.bmm(q, k.transpose(1, 2))
        attn = attn / self.temperature

        if mask is not None:
            attn = attn.masked_fill(mask, -np.inf)

        attn = self.softmax(attn)
        attn = self.dropout(attn)
        output = torch.bmm(attn, v)

        return output, attn
```

在`self.__init__()`函数中，完成对层中要使用的Tensor和调用的层的产生和初始化。初始化可以直接写在`__init__()`方法中，也可以单独创建一个`self.reset_parameters()`方法，在`__init__()`方法中调用。初始化可以自定义，也可以使用`torch.nn.init`中提供的初始化方法。在该方法中调用的层只是对层的声明，并不是调用。

在`self.forward()`方法中，完成对调用该层时完成的功能的编写。层的输出写在return行。

## 4 Pytorch中常见的层
1. LayerNorm
LayerNorm层是对数据的最后一维进行归一化。多用在深层RNN中。

2. Conv1d
```python
Conv1d
class torch.nn.Conv1d(in_channels, out_channels, kernel_size, stride=1, padding=0, dilation=1, groups=1, bias=True)
```
in_channels是输入数据第二维，out_channels是输出数据第二维，kernel_size是卷积占的列数。
输入：shape为(a, b, c)的Tensor。Conv1d在最后一维做卷积，卷积核的维数是(in_channels, out_channels, kernel_size)。输出的第二维是out_channels，说明用out_channels个不同的卷积做运算，输出第三维是c - kernel_size + 1,表示共卷积的次数。
```python
# 一个Conv1d的例子
m = nn.Conv1d(16, 33, 3, stride=2)
input = torch.randn(20, 16, 50)
output = m(input)
print(output.shape)
print(m.weight.shape)
```
输出为torch.Size([20, 33, 48])和torch.Size([33, 16, 3])。原理为：Conv1d只在最后一维做卷积。第二维由in_channels变为out_channels，说明共有out_channels组卷积核，1组in_channels个，卷积核宽度为3，每个卷积核分别与输入数据第二维中的一行做卷积。

3. Conv2d
```python
CLASS torch.nn.Conv2d(in_channels, out_channels, kernel_size, stride=1, padding=0, dilation=1, groups=1, bias=True)
# 一个Conv2d的例子：
m = nn.Conv2d(16, 33, (3, 4))
input = torch.randn(20, 16, 50, 100)
output = m(input)
print(output.size)
print(m.weight.size)
```
输出为torch.Size([16, 33, 48, 97])和torch.Size([33, 16, 3, 4])。原理为：Conv2d的in_channels和out_channels与Conv1d保持一致，不同的是kernel_size可以是两维的，也就是同时对input的最后两维进行卷积。其他部分与Conv1d相同。因此由Conv1d不难推出Conv2d的维数变换规律。

## 5 Pytorch中的数据处理
reference:[Pytorch数据读取(Dataset, DataLoader, DataLoaderIter)](https://zhuanlan.zhihu.com/p/30934236)

### 5.1 `torch.utils.data.Dataset`
reference: [Pytorch cn doc](http://pytorch.apachecn.org/cn/docs/0.3.0/data.html?highlight=dataloader#torch.utils.data.Dataset)
Dataset是一个抽象类，用于将数据封装成Dataset类。它是一个抽象类。在具体使用时需要继承Dataset类并实现其中的2个方法：
- `__getitem__(self, index)`
   用于决定每次如何取数据。比如对于形如(batch_size, input_length)的数据，每次读取第index行数据
- `__len__()`
   用于获取数据的长度


```python
class DealDataset(Dataset):
    """
        下载数据、初始5. Pytorch中的数据处理数据，都可以在这里完成
    """
    def __init__(self):
        xy = np.loadtxt('../dataSet/diabetes.csv.gz', delimiter=',', dtype=np.float32) # 使用numpy读取数据
        self.x_data = torch.from_numpy(xy[:, 0:-1])
        self.y_data = torch.from_numpy(xy[:, [-1]])
        self.len = xy.shape[0]
    
    def __getitem__(self, index):
        return self.x_data[index], self.y_data[index]

    def __len__(self):
        return self.len
```

### 5.2 `torch.utils.data.DataLoader`

reference:[Pytorch cn doc](http://pytorch.apachecn.org/cn/docs/0.3.0/data.html?highlight=dataloader#torch.utils.data.DataLoader)

用于定义从Dataset中读取数据的方式，包括batch_size, shuffle等
```python税务总局发票
class torch.utils.data.DataLoader(dataset, batch_size=1, shuffle=False, sampler=None, batch_sampler=None, num_workers=0, collate_fn=<function default_collate at 0x4316c08>, pin_memory=False, drop_last=False)
```
主要参数：
- dataset: dataset对象
- batch_size: 每个 batch 加载多少个样本 (默认值: 1)
- shuffle: 设置为 True 时, 会在每个 epoch 重新打乱数据 (默认值: False).

```python
train_loader = DataLoader(dataset=dealDataset, batch_size=32, shuffle=True)
```
### 5.3 训练过程
```python
for epoch in epochs:
    for i, batch in enumerate(train_loader):
    print('the {}th batch: {}'.format(i, batch))
```
**注意:**如果在dataloader中定义了多个返回值，那么在训练过程中每个batch都是一个list，使用batch[i]或者(train, test)来调用每个batch中的参数

## 6 Pytorch中的数据类型及相互转换
Pytorch中dtype是tensor的一个属性，使用`tensor.dtype`获取一个张量的数据类型。主要包括以下几类：

| Data type | dtype | Tensor types |
| --- | --- | --- |
| 32-bit floating point | `torch.float32` or `torch.float` | `torch.*.FloatTensor` |
| 64-bit floating point | `torch.float64` or `torch.double` | `torch.*.DoubleTensor` |
| 16-bit floating point | `torch.float16` or `torch.half` | `torch.*.HalfTensor` |
| 8-bit integer (unsigned) | `torch.uint8` | `torch.*.ByteTensor` |
| 8-bit integer (signed) | `torch.int8` | `torch.*.CharTensor` |
| 16-bit integer (signed) | `torch.int16` or `torch.short` | `torch.*.ShortTensor` |
| 32-bit integer (signed) | `torch.int32` or `torch.int` | `torch.*.IntTensor` |
| 64-bit integer (signed) | `torch.int64` or `torch.long` | `torch.*.LongTensor` |

数据类型的查看：数据类型之间的转换使用`Tensor.long()`或`dtype=torch.long`实现。转换函数有：`long(), int(), double(), float(), byte()`

**注意：** 
1. Pytorch中的一些层对输入的tensor类型有要求。如Embedding层要求输入的tensor为`torch.long`类型。
2. 如果Pytorch的数据来源是numpy，要十分注意numpy和pytorch的数据类型匹配。在numpy中，默认数据类型是`float`，但`float`与`np.float64`等价；在pytorch中，整数默认数据类型是`torch.long`，小数默认数据类型是`torch.float`，但`float`与`torch.float32`等价。也就是说，如果不加转换地使用`torch.from_numpy`，numpy中的数组将会被转换成pytorch中的`torch.double`类型。数据类型的不匹配将造成网络无法正确搭建。解决方法：在numpy端将数据转换为`np.float32`类型，即`.astype(np.float32)`
```python
a = np.sin([i for i in range(10)]).astype(np.float)
print(a.dtype)
```



## 7 Pytorch中的数据运算位置及相互转换
Pytorch中device是tensor的一个属性，使用`tensor.device`获取一个张量的运算位置。
- 从cpu转换到gpu：`tensor.to('cuda')`或`tensor.cuda()`
- 从gpu转换到cpu：`tensor.to('cpu')`或`tensor.cpu()`

此外，还要注意tensor和numpy数组之间的转换只能在cpu上完成。即要先使用`tensor.to('cpu')`后才能使用`tensor.numpy()`

## 8 Pytorch展示模型结构
1. 展示模型所有层：`print(modelname)`
2. 展示模型所有参数：`print(list(model.named_parameters()))`

## 9 Pytorch学习率调整
使用的类：torch.optim.lr_scheduler。这个类的optimizer为常用的优化方法。如果使用scheduler，则在训练过程中只需写scheduler.step()而不需写optimizer.step()。

常用的学习率调整方法：
1. `torch.optim.lr_scheduler.StepLR(optimizer, step_size, gamma=0.1, last_epoch=-1)` 
   
   每过step_size将learning_rate调整为gamma * learning_rate.

2. `torch.optim.lr_scheduler.MultiStepLR(optimizer, milestones, gamma=0.1, last_epoch=-1)`
   
   每到milestones中包含的训练次数时把learning_rate调整为gamma * learning_rate.
   
   milestones:包含迭代次数的列表，必须递增。

3. `torch.optim.lr_scheduler.ReduceLROnPlateau(optimizer, mode='min', factor=0.1, patience=10, verbose=False, threshold=0.0001, threshold_mode='rel', cooldown=0, min_lr=0, eps=1e-08)`

   学习率递减函数。当模型性能不再提升时将学习率减少。

   mode: 'min'或'max'。在'min'模式下，衡量标准不再下降时学习率减小。
   
   factor: 学习率更新系数

   patience: 学习率更新前指标不再下降/上升的迭代次数

   verbose: 每次更新学习率时是否打印信息

## 10 Pytorch设置随机数种子

```python
torch.manual_seed(args.seed)
torch.cuda.manual_seed(args.seed)
```
