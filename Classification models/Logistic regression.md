
The logistic regression formula comes from a simple transformation of the linear regression model called *logit transformation*:

$$
\log\left(\frac{p(x)}{1-p(x)}\right) = \beta_0 + \beta_1x
$$
The interpretation of the response is simple in a binary response scenario: the logit transformation makes sure that, given an input value x, the model provides the probability of having $Y = 1$ (i.e. x belongs to class 1). 

The closed form of the logistic regression function is the following:

$$
p(x) = \frac{e^{(\beta_0 + \beta_1x)}}{1 + e^{(\beta_0 + \beta_1x)}} 
$$
From a numerical standpoint, $p(x)$ is actually a probability (i.e. $0 < p(x) < 1$). 

Contrary to a common simplification, logistic regression **does** have assumptions — they're just different (and generally weaker) than those of LDA:

- **Linearity in the log-odds**: the model assumes $\log\left(\frac{p}{1-p}\right) = \beta_0 + \beta_1 X$ is linear in the predictors, not that $Y$ is linear in $X$ directly.

- **Independent observations** — often overlooked, but important. In time-series-like data (e.g. daily financial returns), this assumption can be violated due to autocorrelation.

- **Limited multicollinearity** among predictors (checkable via VIF, Variance Inflation Factor).

- **No assumption of normality** on the predictors — this is the real point of contrast with LDA.


Once the model is fit, the focus shifts to correctly computing and interpreting the standard classification metrics: **confusion matrix, precision, recall, specificity, F1-score, ROC curve, and AUC**.

##### Estimating the regression coefficients in a classification task

In a similar fashion of the regression task, we compute the optimal coefficients by minimizing a predefined objective function, the *likelihood function*:

$$
p(x) = \prod_{i: y_i = 1}p(x_i)\prod_{i': y_i = 0}(1-p(x_{i'}))
$$

This function represent the likelihood that the model provides the highest probabilities to the correct class for each datapoint. Of course, we want to compute the coefficients that maximise this value, hence the name of the technique *maximum likelihood*.

Then, the rationale is identical to the regression task setting: each coefficient is then tested and then a *p-value* for each coefficient is computed. 

#### Multivariate logistic regression model

We have a general linear model, having basically an extended version of the linear model, with more than one predictor.

$$p(X) = \frac{e^{\beta_0 + \beta_1 X_1 + \beta_2 X_2 + \dots + \beta_p X_p}}{1 + e^{\beta_0 + \beta_1 X_1 + \beta_2 X_2 + \dots + \beta_p X_p}}$$

##### Diminishing returns in unbalanced binary data

Usually, sampling more controls than cases reduces the variance of the parameter estimates, but after a ratio of about 5 to 1, the variance reduction flattens out.

![[Screenshot 2026-07-22 alle 19.01.51.png]]

##### Logistic regression with more than two classes

If we had more than two classes, we can also build a linear function for each class. This means that, for K classes, the following holds:

$$p(Y = k \mid X) = \frac{e^{\beta_{0,k} + \beta_{1,k} X_1 + \beta_{2,k} X_2 + \dots + \beta_{p,k} X_p}}{\sum_{l=1}^{K} e^{\beta_{0,l} + \beta_{1,l} X_1 + \beta_{2,l} X_2 + \dots + \beta_{p,l} X_p}}$$

This function is also called the *softmax* function, and the corresponding model is called *multivariate logistic regression*.

Note that each class k now has its own set of coefficients, beta_{0,k}, beta_{1,k}, up to beta_{p,k}, rather than a single shared set of coefficients as in the two-class case. The denominator sums the exponentiated linear function over all K classes, which ensures that the probabilities across all classes sum to 1:

$$\sum_{k=1}^{K} p(Y = k \mid X) = 1$$

For K = 2 classes, this formula reduces exactly to the standard logistic regression formula, since one class's coefficients can be fixed at zero as a reference category without loss of generality.

If we wanted to compute the model a multiclass (i.e. $K > 2$) classification model, we might want to use another extension of the logistic model, which is the *multinomial logistic regression*. For this model, we select one class for baseline (e.g. $K$), and we define 

$$p(Y = k \mid X = x) = \frac{e^{\beta_{0,k} + \beta_{1,k} X_1 + \beta_{2,k} X_2 + \dots + \beta_{p,k} X_p}}{1+ \sum_{l=1}^{K-1} e^{\beta_{0,l} + \beta_{1,l} X_1 + \beta_{2,l} X_2 + \dots + \beta_{p,l} X_p}}$$
for $k = 1, \dots, K - 1$, and 

$$p(Y = K \mid X = x) = \frac{1}{1+ \sum_{l=1}^{K-1} e^{\beta_{0,l} + \beta_{1,l} X_1 + \beta_{2,l} X_2 + \dots + \beta_{p,l} X_p}}$$
By selecting one specific class as a baseline, we are able to prove that 

$$\log \left( \frac{\Pr(Y = k \mid X = x)}{\Pr(Y = K \mid X = x)} \right) = \beta_{k0} + \beta_{k1} x_1 + \dots + \beta_{kp} x_p$$

where class $K$ is treated as the baseline (reference) class, and the log-odds of belonging to class $k$ relative to class $K$ is modeled as a linear function of the predictors, for $k = 1, \dots, K-1$.

---

##### Personal notes

On *hyperparameter tuning*: