##### Model regularization

Shrinkage methods try to minimize the number of coefficients which value is non-zero, in order to reduce the model to only the relevant features for our task. In linear regression, this means that in a multiple linear regression we are trying to minimise the number of non-zero $\beta_i$ parameters.

The main good point about these techniques is that, by shrinking the number of parameters towards zero, they basically reduce the variance of the model, as its 'flexibility' is reduced. 

The two best known techniques for shrinkage methods are the *ridge* and *lasso* techniques.
##### Ridge regression

Recall the, in a linear regression model, the least squares approach aims at minimising the Residual Sum of Squares: 

$$
\text{RSS} = \sum_{i=1}^{n} \left( y_i - \beta_0 - \sum_{j=1}^{p}\beta_jx_j\right)^{2}
$$
The idea is to *penalise* choosing coefficients which value is non-zero. Given that we want to minimise, the *Ridge regression* approach adds to the RSS a *tuning parameter* times the sum of squared parameters:

$$
\text{Ridge\_RSS} = \text{RSS} + \lambda \sum_{j=1}^{p}\beta_j^{2}
$$
The second term is called *shrinkage penalty*. The larger the tuning parameter, the higher the cost of adding the parameters to our model. Hence, selecting a good $\lambda$ value is critical, and hence cross validation is applied on this.

An important caveat: standard linear regression is **scale equivariant** — if we multiply a predictor by some constant $c$, the corresponding coefficient simply gets divided by $c$, so the product $\hat\beta_j X_j$ (and hence the final prediction) stays unchanged. This does not hold for Ridge regression. Since Ridge adds a penalty term $\lambda \sum_j \beta_j^2$ to the RSS, and this penalty depends on the squared magnitude of each coefficient, rescaling a predictor changes how heavily its coefficient gets penalized — the same real-world effect would be penalized differently depending on the arbitrary unit of measurement used (e.g. income in dollars vs. thousands of dollars).

Ridge regression practically shrinks the coefficients by zero, but usually they are not actually set to zero, but still they get much closer.
##### Lasso regression

The mosto important change is the change of the penalty term. We go to the *Ridge* penalty from the *Lasso* penalty, where we switch the penalty term in the following way:

$$
\text{Lasso\_RSS} = \text{RSS} + \lambda \sum_{j=1}^{p}|\beta_j|
$$
The important feature of the Lasso regression is that it induces *sparsity*. In mathematics, we intend as sparsity in the case a single vector or matrix have an high amount of zero values (in the case of the linear regression, of course we are talking about the coefficient vector). 

In the below figure, we can see the difference between the Ridge and Lasso regression from a mathematical optimization point of view. Given that we have two different feasibility regions (the first one being a square - due to module operator, and the second one being a circle being the square operator), we see that in order to get a set of parameters which is feasible for our new regularization constraints we have to move away from the true minimum of the RSS until we touch the blue region.

![[Screenshot 2026-07-30 alle 11.55.14.png]]

***No golden rule to use one over the other!***