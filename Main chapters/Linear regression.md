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
##### Hypothesis testing of the linear regression parameters

Standard errors can be used in order to perform formal *hypothesis tests* on the coefficients. One of the most common hypothesis testing that can be carried out is the *null hypothesis*:

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

General regression topics:

1) [[Assessing the accuracy of a regression model]]
2) [[Extensions of the linear model]]
3) [[General regression-related concepts]]