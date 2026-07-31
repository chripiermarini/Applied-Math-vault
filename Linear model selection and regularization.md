In the following section, we are going back to regression models, and we are going to address some potential issues that might arise when fitting the model into *badly-posed datasets* (i.e. datasets where the set of predictors/features is much higher than the number of dataset points). 

The aim is always to balance the bias-variance trade-off (introduced in [[Foundations of Statistical learning]], which arises when trying to fit a model to a dataset.

The following techniques might be applied to optimally select and tune models in order minimize the bias-variance tradeoff as much as possible. These will lead to better prediction accuracy first, and also better model interpretability later (as we can reduce the number of predictor used, and hence the assess the effect of each of the remaining ones more precisely).
##### Best subset selection

The best subset selection technique refers to a structured way to select the best optimal subset of predictors in: 

1) We start from the *null model* which has zero predictors. This model has a given performance, that might be computed through $MSE/RSS$, adjusted $R^{2}$, or possibly AIC or BIC. We point such model as the 'best model' with zero predictors ($\mathbb{M}_0$) 
2) For each $k = 1, \dots, p$: fit **all** $\binom{p}{k}$ possible models containing exactly $k$ predictors. Among these $\binom{p}{k}$ models, select the one with the lowest RSS (or highest $R^2$) as $\mathbb{M}_k$ — comparing purely on training error is valid here, since all candidate models have the same number of predictors. 
3) Having obtained $\mathbb{M}_0, \mathbb{M}_1, \dots, \mathbb{M}_p$ (one best model per subset size), select a single final model among them using a metric that accounts for model complexity — cross-validation error, AIC, BIC, or Adjusted $R^2$ — never raw RSS/$R^2$, since these always favor larger models regardless of true predictive value. 

The total number of models fit across the entire procedure is $2^p$ (every possible subset of predictors), which becomes computationally infeasible for large p. Furthermore, looking at too many models could also lead to *overfitting*. This is the key motivation for the cheaper, approximate alternative: forward (and backward) stepwise selection, which builds models incrementally rather than exhaustively.

##### Forward Stepwise Selection

Similar to best subset selection, we start from the null model $\mathbb{M}_0$. At each step, we consider adding **one** predictor to the current model — trying each of the remaining candidates — and select the one that yields the best improvement in training performance (RSS/$R^2$, valid here since all candidates being compared have the same number of predictors). This gives $\mathbb{M}_1$. We then repeat, always adding one predictor to the current best model, until all p predictors have been included, giving us $\mathbb{M}_0, \mathbb{M}_1, \dots, \mathbb{M}_p$. As with best subset selection, we then select the single best model among these using cross-validation, AIC, BIC, or Adjusted $R^2$ — not raw RSS/$R^2$.

##### Backward Stepwise Selection

This works in the opposite direction: it starts from the full model  containing all p predictors, and at each step removes the single  predictor whose removal least hurts (or most improves) the model's training performance, progressively producing models with fewer and fewer predictors. Backward selection requires $n > p$, since it needs to be able to fit the full model with all predictors first — unlike forward selection, which can be used even when $p > n$.


Best subset selection **exhaustively** tries every possible combination of predictors ($2^p$ models total), guaranteeing that the best model of each size is found. Stepwise methods (forward or backward), by contrast, apply a **greedy** algorithm: at each step, they commit to the best local choice given the model built so far, without reconsidering it later. This drastically reduces the number of models fit (only $1 + p(p+1)/2$ in total), but comes at a cost: the final model at each step depends on the specific path taken (which predictors were added/removed first), and stepwise methods are **not guaranteed** to find the true best subset of a given size — unlike best subset selection.

##### Estimating test error using two specific approaches

The reason we cannot use the training error as an estimate of the test error is that model parameters are estimated specifically to minimize the error on the training data itself — this makes the training error systematically optimistic, regardless of the model's flexibility, since fitting well on the data used for estimation does not guarantee the model has learned the true underlying pattern of the phenomenon, rather than the noise specific to that sample. 

This problem is compounded by the bias-variance trade-off: as we increase model flexibility to reduce bias, variance increases in turn, and the model increasingly overfits to the training data's noise. This widens the gap between training error (which keeps decreasing with more flexibility) and true test error (which eventually increases past some optimal flexibility level). 

To obtain a reliable estimate of test error, we have two broad options: 

1. **Adjust the training error mathematically**, using metrics like $C_p$, AIC, BIC, or Adjusted $R^2$, which add a penalty term accounting for model complexity — computationally cheap, since no refitting is required, but based on theoretical (often asymptotic) approximations. 
2. **Estimate the test error directly**, using the validation set approach or cross-validation — more computationally expensive, but makes fewer theoretical assumptions.

Aggiungi dettagli su metriche di calcolo, piú la principale limitazione che risiede nel calcolo di alcuni parametri (tipo $d$, $\sigma$ etc.).

##### Shrinkage methods

Shrinkage methods try to minimize the number of coefficents which value is non-zero, in order to reduce the model to only the relevant features for our task. In linear regression, this means that in a multiple linear regression we are trying to minimise the number of non-zero $\beta_i$ parameters.

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
##### Selecting the tuning parameter 

As usual, we rely on cross validation to optimally select the tuning parameter among a set of discrete values. Basically, what we do is define a discrete set of tuning parameters, and for each one we apply a cross validation procedure and compute the CV error (as the average of the errors for each K-fold). We then select the tuning parameter with the smallest CV error.

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