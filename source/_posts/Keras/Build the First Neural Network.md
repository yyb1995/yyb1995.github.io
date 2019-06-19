---
title: 用Keras搭建第一个神经网络
categories: Keras
tags: 深度学习
icon: note
---

## 1 Steps
1. Load Data
2. Define Model
3. Compile Model
4. Fit Model
5. Evaluate Model
6. Tie It All Together

## 2 Load Data
1. Define a certain random number seed. By doing so, you can get the same result to compare it with other model.
Method:
```python
import numpy as np
np.random.seed(7)
```
We use [pima-indians-diabetes.csv](https://yun.baidu.com/s/1c24tweW#list/path=%2F) as our dataset. To load the .csv data, we use `np.loadtxt` and split it into input and output section. 
## 3 Define Model
Models in Keras are defined as a sequence of layers.While defining the first layer, we can define the input number with the **input_dim** argument.
In this example, we will use a fully-connected network structure with three layers.
Fully connected layers are defined using the **Dense class**. We can specify:
1. **the number of neurons** in the layer as the first argument
2. the initialization method as the second argument as **init** 
3. specify the activation function using the **activation** argument.
## 4 Build the Model
define a model class. In this problem, we use a sequential model. So we use a `model = keras.sequential()` to instantiate the class. And then use the `.add` method to add layers in the model.
We want to add a layer into our model, so we use `keras.layers.Dense()` to be the parameter of `add`. The activation function must be in the string form like `activation='relu'`.

## 5 Compile the Model
When compiling, we must specify some additional properties required when training the network.We must specify the **loss function** to use to evaluate a set of weights, **the optimizer** used to search through different weights for the network and **any optional metrics we would like to collect and report** during training.
## 6 Fit the model
use `model.fit()` method to define fit parameters like epochs and batch_size.

## 7 Evaluate the model
use `model.evaluate` and a set of X and Y to calculate the model performance.

## 8 Prediction
use `model.fit` and a set of X to predict the predicted Y value.

## 9 Related material
1. [https://machinelearningmastery.com/](https://machinelearningmastery.com/)
2. [Keras中文文档](https://keras-cn.readthedocs.io/en/latest/)
3. [Keras Example](https://github.com/keras-team/keras/tree/master/examples)
s-team/keras/tree/master/examples)
