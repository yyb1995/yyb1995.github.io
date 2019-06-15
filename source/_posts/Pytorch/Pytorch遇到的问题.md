# Pytorch使用过程中遇到的问题

## `tensor.to(device)`和`module.to(device)`
`tensor.to(device)`不是inplace operation。因此在给一个tensor指定device时需要用：`tensor1 = tensor1.to(device)`。而`Module.to(device)`是inplace operation，因此直接使用`Module.to(device)`即可。

## Numpy和Tensor的数据转换
在将Numpy的数据转化为Tensor时，需要注意一个问题。Numpy中默认浮点类型是`np.float64`，Pytorch中默认浮点类型是`torch.float32`。直接使用`torch.tensor(array)`得到的会是`torch.double`类型的数据。如果想得到`torch.float`类型的数据，需要对Numpy数据进行类型转换：`array = array.astype(np.float32)`

## 'bool value of Tensor with more than one value is ambiguous'
这种情况可能是损失函数声明时没有加括号。即应该是`loss_function=nn.MSELoss()`而不是`loss_function=nn.MSELoss`

## Dataloader的数据格式
在使用Dataloader导入训练数据时，会使用`for batch in dataloader`，`batch`实际上是一个包含所有data的列表。即使只有一组数据，也必须使用`batch[0]`对数据进行提取。

## 在训练过程中loss不下降
一种可能原因是在计算`loss = torch.nn.functional.mseloss(output, real)`时，output和real的维数不完全一致。

## matplotlib和pytorch同时导入报错
在同时导入matplotlib和pytorch.optim时报错：`Process finished with exit code -1073740791 (0xC0000409)`。解决方法：先导入pytorch再导入matplotlib，如下：
```python
import torch.optim as opt
import matplotlib.pyplot as plt
```

## Pytorch显存不足
在使用Pytorch时，有时可能遇到显存不足的问题。即`RuntimeError: CUDA out of memory. Tried to allocate 11.88 MiB (GPU 0; 6.00 GiB total capacity; 4.52 GiB already allocated; 11.60 MiB free; 758.50 KiB cached)`。出现这种问题的原因可能是在保存结果时将过大的张量保存在GPU中，导致GPU存储空间不足。解决方法：检查所有存储在GPU中的张量，将其中一些较大的用`tensor.to('cpu')`转到内存中储存。

