---
title: 深度信念网络(Deep Belief Network,DBN)
categories: DBN
tags: 深度学习
icon: note
---

## 1 初识深度信念网络
深度信念网络是一个概率生成模型，与传统的判别模型的神经网络相对，生成模型是建立一个观察数据和标签之间的联合分布，对$P(Observation|Label)$和 $P(Label|Observation)$都做了评估，而判别模型仅仅而已评估了后者，也就是$P(Label|Observation)$。  
DBNs由多个限制玻尔兹曼机（Restricted Boltzmann Machines）层组成，一个典型的网络结构如图1所示。这些网络被“限制”为一个可视层和一个隐层，层间存在连接，但层内的单元间不存在连接。隐层单元被训练去捕捉在可视层表现出来的高阶数据的相关性。  
![](http://img.blog.csdn.net/20161213120114382?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTgxOTgyNTI5NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
## 2 需要面对的问题
对于在深度神经网络应用传统的BP算法的时候，DBN遇到了以下问题：  
1. 需要为训练提供一个有标签的样本集；
2. 学习过程较慢；
3. 不适当的参数选择会导致学习收敛于局部最优解。

**Solution：**  
首先，先不考虑最顶构成一个联想记忆（associative memory）的两层，一个DBN的连接是通过自顶向下的生成权值来指导确定的，RBMs就像一个建筑块一样，相比传统和深度分层的sigmoid信念网络，它能易于连接权值的学习。  
最开始的时候，通过一个非监督贪婪逐层方法去预训练获得生成模型的权值，非监督贪婪逐层方法被Hinton证明是有效的，并被其称为对比分歧（contrastive divergence）。  
![](http://img.blog.csdn.net/20161213123012466?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTgxOTgyNTI5NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
在这个训练阶段，在可视层会产生一个向量v，通过它将值传递到隐层。反过来，可视层的输入会被随机的选择，以尝试去重构原始的输入信号。最后，这些新的可视的神经元激活单元将前向传递重构隐层激活单元，获得h（在训练过程中，首先将可视向量值映射给隐单元；然后可视单元由隐层单元重建；这些新可视单元再次映射给隐单元，这样就获取新的隐单元。执行这种反复步骤叫做吉布斯采样）。这些后退和前进的步骤就是我们熟悉的Gibbs采样，而隐层激活单元和可视层输入之间的相关性差别就作为权值更新的主要依据。  
训练时间会显著的减少，因为只需要单个步骤就可以接近最大似然学习。增加进网络的每一层都会改进训练数据的对数概率，我们可以理解为越来越接近能量的真实表达。这个有意义的拓展，和无标签数据的使用，是任何一个深度学习应用的决定性的因素。
在最高两层，权值被连接到一起，这样更低层的输出将会提供一个参考的线索或者关联给顶层，这样顶层就会将其联系到它的记忆内容。而我们最关心的，最后想得到的就是判别性能，例如分类任务里面。  
在预训练后，DBN可以通过利用带标签数据用BP算法去对判别性能做调整。在这里，一个标签集将被附加到顶层（推广联想记忆），通过一个自下向上的，学习到的识别权值获得一个网络的分类面。这个性能会比单纯的BP算法训练的网络好。这可以很直观的解释，DBNs的BP算法只需要对权值参数空间进行一个局部的搜索，这相比前向神经网络来说，训练是要快的，而且收敛的时间也少。
## 3 详细训练算法流程
![](http://img.blog.csdn.net/20161213122524412?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTgxOTgyNTI5NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
在训练时,Hinton采用了逐层无监督的方法来学习参数。如图3所示，首先把数据向量x和第一层隐藏层作为一个RBM, 训练出这个RBM的参数(连接x和h1的权重, x和h1各个节点的偏置等等), 然后固定这个RBM的参数, 把h1视作可见向量, 把h2视作隐藏向量, 训练第二个RBM, 得到其参数, 然后固定这些参数, 训练h2和h3构成的RBM, 具体的训练算法如下:
![](http://img.blog.csdn.net/20161213123230571?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTgxOTgyNTI5NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
  CD的训练过程中用到了Gibbs 采样，即在训练过程中，首先将可视向量值映射给隐单元，然后用隐层单元重建可视向量，接着再将可视向量值映射给隐单元……反复执行这种步骤。
　k-Gibbs的过程如下：
![](http://img.blog.csdn.net/20161213123405362?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTgxOTgyNTI5NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
其中，P是model distribution，$\hat{P}$是training set distribution.
DBN训练算法：
![](http://img.blog.csdn.net/20161213123735570?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTgxOTgyNTI5NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
DBN运用CD算法逐层进行训练，得到每一层的参数Wi和ci用于初始化DBN，之后再用监督学习算法对参数进行微调。
![](http://img.blog.csdn.net/20161213123934323?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTgxOTgyNTI5NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
## 4 经典网络结构
经典的DBN网络结构是由若干层RBM和一层BP组成的一种深层神经网络, 结构如下图4所示。
![](http://img.blog.csdn.net/20161213124220124?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTgxOTgyNTI5NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
DBN 在训练模型的过程中主要分为两步:
1. 分别单独无监督地训练每一层 RBM 网络,确保特征向量映射到不同特征空间时,都尽可能多地保留特征信息;
2. 在 DBN 的最后一层设置BP网络,接收RBM的输出特征向量作为它的输入特征向量,有监督地训练实体关系分类器.而且每一层 RBM 网络只能确保自身层内的 权值对该层特征向量映射达到最优,并不是对整个 DBN 的特征向量映射达到最优,所以反向传播网络还将错误信息自顶向下传播至每一层 RBM,微调整个 DBN 网络.RBM 网络训练模型的过程可以看作对一个深层 BP 网络权值参数的初始化,使DBN 克服了 BP 网络因随机初始化权值参数而容易陷入局部最优和训练时间长的缺点。
上述训练模型中第一步在深度学习的术语叫做预训练，第二步叫做微调。最上面有监督学习的那一层，根据具体的应用领域可以换成任何分类器模型，而不必是BP网络。

## 5 拓展
DBN的灵活性使得它的拓展比较容易。一个拓展就是卷积DBNs(Convolutional Deep Belief Networks(CDBN))。DBN并没有考虑到图像的2维结构信息，因为输入是简单的从一个图像矩阵一维向量化的。而CDBN就是考虑到了这个问题，它利用邻域像素的空域关系，通过一个称为卷积RBM的模型区达到生成模型的变换不变性，而且可以容易得变换到高维图像。DBN并没有明确地处理对观察变量的时间联系的学习上，虽然目前已经有这方面的研究，例如堆叠时间RBMs，以此为推广，有序列学习的dubbed temporal convolutionmachines，这种序列学习的应用，给语音信号处理问题带来了一个让人激动的未来研究方向。  
目前，和DBN有关的研究包括堆叠自动编码器，它是通过用堆叠自动编码器来替换传统DBN里面的RBM。这就使得可以通过同样的规则来训练产生深度多层神经网络架构，但它缺少层的参数化的严格要求。与DBN不同，自动编码器使用判别模型，这样这个结构就很难采样输入采样空间，这就使得网络更难捕捉它的内部表达。但是，降噪自动编码器却能很好的避免这个问题，并且比传统的DBN更优。它通过在训练过程添加随机的污染并堆叠产生场泛化性能。训练单一的降噪自动编码器的过程和RBM训练生成模型的过程一样。


## 6 更多资料
1. [深度学习方法：受限玻尔兹曼机RBM（一）基本概念](http://blog.csdn.net/xbinworld/article/details/44901865)
2. [深度学习方法：受限玻尔兹曼机RBM（二）网络模型](http://blog.csdn.net/xbinworld/article/details/45013825)
3. [深度学习方法：受限玻尔兹曼机RBM（三）模型求解，Gibbs sampling](http://blog.csdn.net/xbinworld/article/details/45128733)
4. [深度学习方法：受限玻尔兹曼机RBM（四）对比散度contrastive divergence，C](http://blog.csdn.net/xbinworld/article/details/45274289)
