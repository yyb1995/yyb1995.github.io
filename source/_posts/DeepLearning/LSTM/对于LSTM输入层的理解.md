# 对于LSTM输入层、隐含层及输出层参数的理解
---

LSTM输入层要求的维度是三维的，其中包含三个参数:batch_size, input_dim和time_step。隐含层有一个参数：n_hidden。输出层有两个参数：n_hidden和output_dim。下面举两个例子：[利用LSTM识别MNIST手写数字集](https://blog.csdn.net/mebiuw/article/details/52705731)和[LSTM时间序列分析](https://blog.csdn.net/a819825294/article/details/54376781)，谈谈个人对这些参数含义的理解。

## 1.利用LSTM识别MNIST手写数字集

```python
n_input = 28  # 输入层的n 
n_steps = 28  # 28长度 
n_hidden = 128  # 隐含层的特征数 
n_classes = 10  # 输出的数量，因为是分类问题，0~9个数字，这里一共有10个
batch_size = 128  
```
**输入层**：首先说下batch_size。这个参数其实和其他神经网络的batch_size意义相同，都指一次性输入到神经网络中训练的个数。这里batch_size=128，含义是一次性将128个图像输入到LSTM中进行训练，完成一次参数计算和更新。再说说n_steps。n_steps实际上指的是构造的LSTM总共有多少个时间上的输入。在这里取n_step = 28，指的是按时间顺序依次输入28次，在同一时刻输入的个数为batch_size * n_input。在MNIST数据集中，一幅图片表示为28\*28的矩阵，因此如果一次输入1行，那么要先后依次输入28行才能将一个图片的信息完全输入。那么同时input_dim（在此处为n_input）的含义也很清楚了，就是一次输入的数据维数，在这里就是1行的数据个数。因此，输入端的操作是，在t时刻输入128幅图片的第1行矩阵，t+1时刻输入128幅图片的第2行矩阵。以此类推直到输入完毕。
**隐含层**：隐含层只有一个新的参数：n_hidden。这个参数表示的是用于记忆和储存过去状态的节点个数。
**输出层**：输出层也只有一个新的参数：output_dim（在此处为n_classes）。这个参数的含义是输出结果维数。在MNIST数据集中，由于做的是0~9的分类，所以输出维度自然是10，类似于softmax分类。

## 2.LSTM时间序列分析
**输入层**：在这个例子中，使用了Keras作为搭建LSTM工具。查看Keras的文档，得知其对输入数据的要求是
>形如（samples，timesteps，input_dim）的3D张量

而第二个例子中对于输入数据做的处理为
`x_train = np.reshape(x_train, (x_train.shape[0], x_train.shape[1], 1))`
因此不难比较得到：
```python
batch_size = x_train.shape[0]
time_steps = x_train.shape[1]
input_dim = 1
```
由于这个例子是给定一个已知序列，对该序列接下来的走势进行预测，因此自然而然想到把一个序列切成训练集和测试集，训练集再根据合适的时间长度分成t~(t+n)的训练集和t+n+1的测试集。那么batch_size的含义是一次性输入训练的序列数。time_step为取的一个时间序列的长度，也就是上一句话的n。在这个例子中，input_dim为1，说明在一个时间点，一个序列只输入1个点。隐含层和输出层类似，不再重复。