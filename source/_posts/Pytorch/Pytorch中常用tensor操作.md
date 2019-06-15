# 1 torch.cat
cat指的是对多个Tensor在**原有某一维度**进行拼接，拼接的结果是Tensor的总维数不变，其中用于拼接的那一维等于各分量维数之和。示例：
```python
x = torch.rand(2, 3)
y = torch.rand(4, 3)
z = torch.cat((x, y), 0)
```
即x和y沿着axis=0的维度进行拼接，得到的结果是一个(6*3)的Tensor
此外，还有如下用法：
1. `torch.cat((x, x), 0)`
2. `torch.cat([torch.rand(3, 4), torch.rand(5, 4)], dim=0)` 


# 2 torch.chunk
`torch.chunk(tensor, chunks, dim=0)`
chunk可以看成cat的逆操作，即将一个矩阵沿着某一维分割开。chunks为分割的份数， dim为分割的维度。例子：
```python
x = torch.zeros(2, 3, 4)
y = torch.chunk(x, 3, 1)
```
得到的y是一个tuple，每一个的维度为(2, 1, 4)
还可使用Tensor.chunks(chunks, dim=0)效果与上相同。


# 3 torch.stack
stack指的是在**新的维度上**进行拼接，这个操作会增加维度。示例：
```python
x = torch.ones(1, 3)
y = torch.ones(1, 3)
z1 = torch.stack((x, y), 0)
z2 = torch.stack((x, y), 1)
z3 = torch.stack((x, y), 2)
```
输出维数规律：除拼接的维数外别的维数保持不变，拼接的维数等于输入个数之和。则上述三个输出的维数分别为：
z1:(2, 1, 3), z2:(1, 2, 3), z3:(1, 3, 2)。
**注意：** torch.stack的输入tensor的维数必须一致，这样才能保证在能够在新的维度进行拼接操作。

# 4 torch.transpose
transpose指的是将Tensor的某两个维度进行交换。示例：
```python
x = torch.zeros(2, 3)
y = torch.transpose(x, 0, 1)
```


# 5 permute & reshape
permute是适合于多维度的维数交换。使用方法：输入希望产生的维度即可。例子：
```python
x = torch.zeros(2, 3, 4)
y = x.permute(2, 1, 0)
```
输出y的维度为:(4, 3, 2)

reshape和permute功能类似，不过reshape一般用于连续维度的改变，如
```python
x = torch.zeros(2, 3, 4)
y = x.reshape(2, 6, 2)
```
如果用于reshape的维度不是连续的，会出现数据改变的情况。

# 6 squeeze
squeeze是将某一个维度为1的维去除。使用方法：x.squeeze()。
例子：
```python
x = torch.zeros(3, 1, 2)
y = x.squeeze(1)
```
得到的y的维度为(3, 2)。如果选择的维数不为1，那么得到的结果的维数与原Tensor的维数一致

# 7 unsqueeze
unsqueeze是增加一个维度，维度位置为dim。使用方法：a.unsqueeze(dim)。
例子：
```python
x = torch.zeros(3, 2)
y = x.unsqueeze(2)
```
得到的y的维数为：(3, 2, 1)
use repeat – this will copy each vector 28 times.

X = torch.randn(100, 700)
X = X.unsqueeze(2).repeat(1, 1, 28)
# 8 torch.masked_fill
masked_fill将一个tensor中为1的元素用指定的值填充。例如：
```python
a = torch.ones(3, 3)
```

# 9 torch.view
view将一个tensor变换维度，但其中的数值保持不变

# 10 torch.bmm
bmm即batch_matmul，作用是不考虑batch维度将两个矩阵相乘。
```python
a = torch.ones(128, 4, 3)
b = torch.ones(128, 3, 10)
result = torch.bmm(a, b)
print(result.shape)
>>> (128, 4, 10)
```

# 11 expand&repeat
expand和repeat都用于扩展Tensor的维度。**使用前提：原矩阵的维度和扩展后矩阵后的维度一致。因此通常先进行squeeze(dim)或unsqueeze(dim)操作**。expand的输入参数是扩展后Tensor的维度，repeat的输入参数是扩展后Tensor相对于原Tensor扩展的倍数。**此外，注意expand仅限于维数为1的扩展，否则会报类型不匹配错误。** 例如：
```python
a = torch.Tensor([1, 2, 3])
# 最终维数为(3,12 )
b = a.unsqueeze(1).expand(3, 5)
# 13 在第一维扩展1次，在第二维扩展5次
c = a.unsqueeze(1).repeat(1, 5)

# 14 output:
tensor([[1., 1., 1., 1., 1.],
        [2., 2., 2., 2., 2.],
        [3., 3., 3., 3., 3.]])

# RuntimeError: The expanded size of the tensor (6) must match the existing size (3) at non-singleton dimension15 Target sizes: [1,15 ,15 ].  Tensor sizes: [1,15 ,15 ]
a.expand(1, 4, 6)

```

expand不会复制数组内存，节省空间。repeat会复制所有数据


