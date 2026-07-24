Linear regression is a simple approach to supervised learning that assumes that the relationship between X and Y is linear.

There might be multiple questions to answer to check if the model works fine:

1) Is there an actual relationship?
2) Is it linear?
3) What is the accuracy?

##### Linear regression structure

The usual structure is the same:

$$ 
\hat{y} = \hat{\beta_{0}} + \hat{\beta_{1}}x
$$

The estimation of the optimal parameters is done by the means of the minimisation of the *Residual Sum of Errors*. 

The formulas for the coefficients are closed (**please look at the book**).

In order to assess the accuracy of the coefficient estimates, we use the concepts of *SE* (Standard error), which formulas are closed and already defined, for both slope and intercept.

Looking at the values of the Standard Error formulas, one can easily recognise that the higher the variance of the points, the lower the error of the parameter estimation.

Furthermore, in order not have the accuracy defined by a single point estimation, one can compute the *confidence* *intervals*. Such intervals are the defined as follows:

$$
\left[ \hat{\beta_0} - 2 \cdot \text{SE}(\hat{\beta_0}), \ \hat{\beta_0} + 2 \cdot \text{SE}(\hat{\beta_0}) \right]
$$
This is supposed to have the true value of the parameter, with a given confidence of $95\%$ .

*Disclaimer*: the term **2** in front of the standard error values is taken by assuming a generic number of degrees of freedom, and a confidence of 95%. If one want to be extremely precise, please recall the theory of confidence intervals: the values must be taken from the table of values from the t-Student distribution, taking $n-2$ degrees of freedom and the associated $t_{\alpha/2}$ score. We are using $\alpha/2$ because the test is bilateral.
##### Hypothesis testing

Standard errors can be used in order to perform formal *hypothesis tests* on the coefficents. One of the most common hypothesis testing that can be carried out is the *null hypothesis*:

$H_{0}:$ There is *not* a relationship between X and Y against the alternative hypothesis that there is.

Mathematically, that corresponds to testing whether the true value of the slope coefficent of the linear regression is actually zero or non-zero.

Here comes one of the most crucial concepts of hypothesis testing. In order to test the null hypothesis *(hence, to understand if the null hypothesis is correct)*, we compute what is called **t-statistic**. 

In this test, the t-statistic is the following:

$$
\frac{\hat{\beta_{1}} - 0}{SE(\hat{\beta_{1}})} 
$$

Then we compare this value against the values of table of the t-distribution, considering the degrees of freedom and the confidence. 

Using that, we are able to compute the p-value. The p-value has multiple different possible definitions, but in simple terms is the probability of seeing, through any data, a value of $|t|$ or larger, given the null hypothesis being true. 

If the probability is extremely low, then that means that the null hypothesis cannot be true (we reject it). Hence, we know that the coefficient is extremely significant.

There is a 1:1 relationship between confidence interval and hypothesis testing: if the in the test we reject the *null hypothesis*, we should not be able to compute a confidence interval in which 0 lies in. It is then beneficial to test both. 

**Remember**: for a two-sided test at α = 0.05:

$$P(|Z| > 1.960) = 0.05$$

This is because the 0.05 is split symmetrically between the two tails:

$$P(Z > 1.960) = 0.025 \quad \text{and} \quad P(Z < -1.960) = 0.025$$

$$0.025 + 0.025 = 0.05 = \alpha$$

So 1.960 is the number you compare your test statistic to directly, and it aslo the number you will find in the table. It is also called *z-score*. 
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
##### Multiple regression

The general Multiple Linear Regression extends the simple model in order to take more than one predictor:

$$ 
\hat{y} = \hat{\beta_{0}} + \hat{\beta_{1}}x_1 + \hat{\beta_{2}}x_2 + \dots + \hat{\beta_{n}}x_n
$$

Now the line of the simple linear regression model is replaced by a hyperplane of multiple dimensions.

![[Screenshot 2026-07-21 alle 16.29.04.png]]

To **interpret** the model coefficients, one can imagine that the *average* effect of a specific predictor when the others are fixed. 

The ideal scenario in this kind of analysis is to have different predictors which are uncorrelated, having a balanced design. Correlation amongst predictors cause problems, since variance of all coefficient tends to increase, sometimes dramatically. In such cases, the fixed predictors assumptions does not hold anymore. 

Please also remember that *correlation is not necessarily causality*, hence no causality assumptions must be taken.

Important note: always remember that the results of the linear regression computation is always relative to the presence of the other coefficients. if *newspaper* has a low significance with TV and radio, it might still have it without the other two factors.
##### Practical use of the linear regression model

One should always question the capabilities of the importance of each predictors, the accuracy of the model, etc.

In order to check if the predictors are actually useful, one might use the *F-statistic*:

$$
F = \frac{(TSS - RSS)/ p}{RSS/(n-p-1)} \quad \approx F_{p, n-p-1}
$$
When fitting a regression model, it is important to decide which are the most important variables. The most direct approach is to use the *best subsets/all subsets* regression. The main problem is that the number of possible subsets might be extremely big.

There two possible (complementary) methods to select which predictors are the most relevant: the *forward selection* and the *backward selection*, which require starting from the null model (intercept only) and adding parameters until the p-value for he parameters becomes too high.

Of course there are multiple other model selection techniques which are more updated, hence we will skip to them later.
##### Other considerations in the Regression model

It might be possible that the model might take *qualitative* information, not *quantitative*. In that case, the number of different values that a variable might take is *finite* and *discrete*. They are also called *categorical* predictors or *factor variables*. 

An easy solution to this is the use of *dummy* variables:

![[Screenshot 2026-07-22 alle 10.17.40.png]]

With more than two levels, we can create multiple dummy variables, as an example:

![[Screenshot 2026-07-22 alle 10.20.49.png]]

##### Extensions of the linear model

It might be possible that two predictors might ***interact*** with each other, meaning that there might be effects on the variations of one predictor to another predictor. It is also called *synergy*.

How do we manage the two coefficients? We multiply them together:

$$
y = \beta_{0} + \beta_{1}x_1 + \beta_{2}x_2 + \beta_{3}(x_1 \text{ x } x_2)
$$

That means that multiple variables combined together have an interacting effect. As an example:

![[Screenshot 2026-07-22 alle 10.59.06.png]]

The interaction effect might be extremely powerful, and we also need to mention the *hierarchy* effect: if the p-value of an interactions is extremely low (hence, its effect is significant), we still keep the coefficients of the two values interacting, even if their p-value is high on paper.  

Another important extension of the linear regression are the models with ***non-linear effects***. It might be possible that the models might be polynomials to understand the different degrees of freedom of the model. To do that, we just introduce new dummy variables:

$$
y = \beta_{0} + \beta_{1}x_1 + \beta_{2}x_2^{2}
$$
In this case, the model itself has a polynomial structure, but still the model is called linear as it is linear in the coefficients. 

***Specific concepts:***

- *Outliers*:
		Outliers are data points for which $y_i$ is far from the value predicted by the model. These values may not be informative, in the sense that they can come from error in the data registration, recording, or single event that do not represent the usual behavior of the phenomenon.

- *Non-constant variance of error terms*:
		It might be possible that prediction errors (or *residuals*) are *heteroskedastic*, which means that the variance of the error is not constant. This might happen when the variance actually increases with the scale of the response itself. There are some refined techniques to apply when this happens (e.g. *weighted least squares*), but one easy trick would be to transform the response Y using the logarithmic or squared function (i.e. $\log(Y) \text{ or } \sqrt{Y}$) (of course, before fitting the data).

- *Difference between the Residual sum of Squared and the Least Squared*:
		Simple: the first one is the function that must be minimized in order to compute the optimal parameters (i.e. $\text{RSS} = \sum_{i=1}^{n} e_{i}^{2}$), while the second one is the actual criterion/method, which requires to minimize the RSS itself.

- *High leverage points*:
		High leverage points are observations whose predictor values (x_i) are unusual or extreme relative to the rest of the dataset, not necessarily unusual in y. This is not an actual problem for the model (as the observation is usually valid, it is just that its $x$ is far from the average), but you just need to be careful of the value as it might cause issues for the numerical side to the least squared method.

- *Collinearity*:
		A phenomenon that appears when multiple predictors have an high correlation one another. This can pose problems in the regression setting as it might be difficult to interpret the effects of each predictor, since they usually increase or decrease together as they are correlated. One simple technique to apply would be to just not use one predictor for the regression itself.