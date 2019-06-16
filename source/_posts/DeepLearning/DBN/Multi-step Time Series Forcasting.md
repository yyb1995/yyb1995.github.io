---
style: ocean
---
# Multi-step Time Series Forcasting
---
There are at least four commonly used strategies for making multi-step forecasts.
They are:
## 1. Direct Multi-step Forecast Strategy
Create seperate prediction model for each ppint. Add computational add maintenance burden and there is **no dependencies** between points.
```
prediction(t+1) = model1(obs(t-1), obs(t-2), ..., obs(t-n))
prediction(t+2) = model2(obs(t-1), obs(t-3), ..., obs(t-n))
```
**Attention:** This kind of method just changes the network parameters. The input data remains the same for the whole prediction process.
## 2. Recursive Multi-step Forecast
The recursive strategy involves using a one-step model multiple times where the prediction for the prior time step is used as an input for making a prediction on the following time step.
Because predictions are used in place of observations, the recursive strategy **allows prediction errors to accumulate** such that performance can quickly degrade as the prediction time horizon increases. 
**Attention:** This kind of method just changes the input data. The network parameters remain the same for the whole prediction process.
```
prediction(t+1) = model(obs(t-1), obs(t-2), ..., obs(t-n))
prediction(t+2) = model(prediction(t+1), obs(t-1), ..., obs(t-n))
```
## 3. Direct-Recursive Hybrid Multi-step Forecast Strategies
The direct and recursive strategies can be combined to offer the benefits of both methods.
For example, a separate model can be constructed for each time step to be predicted, but each model may use the predictions made by models at prior time steps as input values.
This model learn a different model for each point using known data as well as prediction data.
```
prediction(t+1) = model1(obs(t-1), obs(t-2), ..., obs(t-n))
prediction(t+2) = model2(prediction(t+1), obs(t-1), ..., obs(t-n))
```
## 4. Multiple Output Forecast Strategy
The multiple output strategy involves developing one model that is capable of predicting the entire forecast sequence in a one-shot manner.
In the case of predicting the temperature for the next two days, we would develop one model and use it to predict the next two days as one operation.
Multiple output models are more complex as they can learn the dependence structure between inputs and outputs as well as between outputs.
Being more complex may mean that they are slower to train and require more data to avoid overfitting the problem.
```
prediction(t+1), prediction(t+2) = model(obs(t-1), obs(t-2), ..., obs(t-n))
```

## Personal View
Among the four methods, actually the first three methods are one-step ahead method. They realizes multi-step prediction by changing eather training datasets or network parameters. The fourth method is a multi-step network from the network structure aspect.


## Reference
[https://machinelearningmastery.com/multi-step-time-series-forecasting/](https://machinelearningmastery.com/multi-step-time-series-forecasting/)

