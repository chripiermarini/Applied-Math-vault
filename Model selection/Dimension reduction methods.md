##### Dimension reduction methods

Dimension reduction techniques applied to linear models, where we have p predictors and want to reduce dimensionality to $m < p$, work by combining ALL the original p predictors into m new derived variables — rather than selecting a subset and discarding the rest (as subset selection methods do).

The principle of these techniques is straightforward: given the $x_i$ predictors of the original problem, we compute $Z_1, Z_2, \dots Z_m$ which are linear combinations of the original $p$ predictors. That is,

$$
Z_m = \sum_{j=1}^{p} \phi_{mj}X_{j}
$$

for som $\phi$ constants.

We can then fit the linear regression model on the new parameters, in the following way:

$$
y_i = \theta_0 + \sum_{m=1}^{M}\theta_mz_{im} + \epsilon_i \quad \forall i = 1, \dots, n
$$
using ordinary least squares. So the *main takeaway* of dimensionality reduction techniques is that we need a clever way to compute the $\phi$ parameters in order to compute the *new 'surrogate'* predictors. If those surrogates are well formed, the reduced form of the linear regression model could also outperform the standard OLS regression. 

##### Principal Components Regression

It involves a two step procedure. First, we compute the Principal Component through Principal Component Analysis. This technique will be explained in another chapter, but practically speaking it is about computing linear combinations of specific predictors, in order to have as much variance as possible. The idea is get PCs that summarize these components well.

Principal Component Analysis is based on the idea of computing new components which summarize well the actual existing predictors to a smaller number of variables that get the highest variance possible w.r.t. the original data. Those linear combinations are also called *directions*, and these directions are identified in a unsupervised way, since the response is not used to help determine the principal component directions. Hence, the major drawback of the PCR is that there is no guarantee that these directions are also the best ones for predicting the response, even though they are supposed to be the best ones that best explain the original predictors.

In order to take into account of the response, one can also use *Partial Least Squares*, in order to reduce the number of components of the regression. 