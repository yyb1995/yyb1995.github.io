---
title: LSTM时序预测滞后现象
categories: LSTM
tags: 深度学习
icon: note
---

在使用LSTM进行时间序列预测时，有时会遇到预测滞后问题，也就是LSTM的预测值滞后于真实值的变化。出现滞后现象的一些文章和可能解释收集如下
1. [用 LSTM 做时间序列预测的一个小例子](https://blog.csdn.net/aliceyangxi1987/article/details/73420583)
2. [时间序列预测----预测的结果跟实际的时间序列值存在滞后](http://www.ilovematlab.cn/thread-165451-1-1.html)
3. [代码干货 | 基于Keras的LSTM多变量时间序列预测](https://blog.csdn.net/CS13522431352/article/details/77369300?locationNum=7)
4. [但如果只用time series数据，你很有可能得到的就是滞后一天的趋势](https://www.jianshu.com/p/5d6d5aac4dbd)
5. [一个带输出图像的LSTM预测](https://github.com/owoshch/time_series/blob/master/airline_prediction_one_lstm_layer_with_time_steps.ipynb)
6. [用LSTM预测时间序列存在延迟现象？](https://www.douban.com/group/topic/102741080/)
7. [Stackexchange problem](https://stats.stackexchange.com/questions/307340/role-of-delays-in-lstm-networks#comment587967_307340)
8. [Stackoverflow-Delay issue in time series prediction](https://stackoverflow.com/questions/35563758/delay-issue-in-time-series-prediction)
9. [LSTM for time series prediction](https://github.com/keras-team/keras/issues/2856)
10. [LSTM Neural Network for Time Series Prediction](http://www.jakob-aungiers.com/articles/a/LSTM-Neural-Network-for-Time-Series-Prediction)
11. [知乎问题-Pyhong的答案](https://www.zhihu.com/question/21229371)
12. [https://dashee87.github.io/deep%20learning/python/predicting-cryptocurrency-prices-with-deep-learning/](https://dashee87.github.io/deep%20learning/python/predicting-cryptocurrency-prices-with-deep-learning/)
13. [https://medium.com/@siavash_37715/how-to-predict-bitcoin-and-ethereum-price-with-rnn-lstm-in-keras-a6d8ee8a5109](https://medium.com/@siavash_37715/how-to-predict-bitcoin-and-ethereum-price-with-rnn-lstm-in-keras-a6d8ee8a5109)
14. [最全 LSTM 模型在量化交易中的应用汇总（代码+论文）](https://zhuanlan.zhihu.com/p/31783805)
15. [https://www.kaggle.com/pablocastilla/predict-stock-prices-with-lstm](https://www.kaggle.com/pablocastilla/predict-stock-prices-with-lstm)
16. [useful discussion](https://stackoverflow.com/questions/48034625/keras-lstm-predicted-timeseries-squashed-and-shifted/48050810#48050810)
17. [another discussion](https://stackoverflow.com/questions/49697457/lstm-nn-produces-shifted-forecast-low-quality-result/49700184#49700184)
18. [another another discussion](https://stackoverflow.com/questions/39139446/keras-lstm-rnn-forecast-shifting-fitted-forecast-backward)
19. [https://jiasuhui.com/article/3855](https://jiasuhui.com/article/3855)
20. [知乎问题](https://www.zhihu.com/question/275040228)
21. [延时现象解决](http://www.cnblogs.com/xuruilong100/p/8451790.html)
22. [github上的讨论](https://github.com/keras-team/keras/issues/2856)
23. [ResearchGate上的讨论](https://www.researchgate.net/post/How_can_I_decrease_the_ANN_forecasting_delay)
24. **possible solution**
	1. randomize training samples in each batch, make sure they are not followed one by one
	2. choose or design a better loss function other than MSE
	3. extract some features from the input time series
	4. manually limit the weight of x_{t-1}, x_{t-2}


ght of x_{t-1}, x_{t-2}


