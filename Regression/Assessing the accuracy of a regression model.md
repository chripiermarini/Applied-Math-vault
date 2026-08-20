##### Assessing the accuracy of the model

We compute now the *Residual Standard Error* 

$$
\text{RSE} = \sqrt{\frac{1}{n -2} \sum_{i=1}^{n}{(y_i - \hat{y_i})^{2}}}
$$
Other two important metrics are the *R-squared* or fraction of variance explained, which, as the name suggests, computes how much of the variance is actually explained by the model itself.  It can be shown that, the in the simple regression settings, the R-squared is equal to the square of the *linear correlation* between data, which formula is the following:

$$
r = \frac{\sum_{i=1}^{n}(x_{i} - \bar{x})(y_{i} - \bar{y})}{\sqrt{\sum_{i=1}^{n}(x_{i} - \bar{x})^{2}\sum_{i=1}^{n}(y_{i} - \bar{x})^{2}}}
$$

Additional notes based on exercises:

1) R-squared is good, but in practice one has to take into consideration that polynomial models might be inevitably better than simple ones given that we have actually multiple predictors (based on the same input features, e.g. horsepower and horsepower squared). In these instances, it might be better to use the *adjusted* $R^{2}$ which penalises the model by the number of predictors. The formula is $$R^{2}_{adj} = 1 - (1- R^{2})\frac{n-1}{n-k-1},$$ where $n$ is the number of data points and k is the number of actual predictors. Point is the the adjusted R squared practically works when number of predictors is extremely high.
2) The standard de facto for accuracy metric in ML models are the *MSE* and *RMSE* metrics, mean squared error and its root.