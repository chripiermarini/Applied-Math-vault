Classification tasks arise when we want to build a model to compute/predict a response that can come from a finite, discrete set (e.g. "red", "blue", "green" - which can be mapped to 1,2 or 3 for example). 

In this instance, as seen in the [[Introduction to the course]], we would want to build a model that, given a out-of-sample value, provides the probabilities of that new sample to fall into each of the class. Rationally, we would choose the class with the highest probability associated. 
 ![[Screenshot 2026-07-22 alle 16.28.21.png]]

In this instance, linear regression could be not useful, as the linear model can provide a negative value as response. Also, linear regression does not work properly even for multi-class classification (i.e. more than two possible responses). For these reasons, we actually use the so-called *Logistic regression*.
#### Logistic regression

The logistic regression formula comes from a simple transformation of the linear regression model called *logit transformation*:

$$
\log\left(\frac{p(x)}{1-p(x)}\right) = \beta_0 + \beta_1x
$$
The interpretation of the response is simple in a binary response scenario: the logit transformation makes sure that, given an input value x, the model provides the probability of having $Y = 1$ (i.e. x belongs to class 1). 

The closed form of the logistic regression function is the following:

$$
p(x) = \frac{e^{(\beta_0 + \beta_1x)}}{1 + e^{(\beta_0 + \beta_1x)}} 
$$
From a numerical standpoint, $p(x)$ is actually a probability (i.e. $0 < p(x) > 1$). 

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

(Quickly check case-control sampling)
##### Logistic regression with more than two classes

If we had more than two classes, we can also build a linear function for each class. This means that, for K classes, the following holds:

$$p(Y = k \mid X) = \frac{e^{\beta_{0,k} + \beta_{1,k} X_1 + \beta_{2,k} X_2 + \dots + \beta_{p,k} X_p}}{\sum_{l=1}^{K} e^{\beta_{0,l} + \beta_{1,l} X_1 + \beta_{2,l} X_2 + \dots + \beta_{p,l} X_p}}$$

This function is also called the *softmax* function, and the corresponding model is called multinomial logistic regression.

Note that each class k now has its own set of coefficients, beta_{0,k}, beta_{1,k}, up to beta_{p,k}, rather than a single shared set of coefficients as in the two-class case. The denominator sums the exponentiated linear function over all K classes, which ensures that the probabilities across all classes sum to 1:

$$\sum_{k=1}^{K} p(Y = k \mid X) = 1$$

For K = 2 classes, this formula reduces exactly to the standard logistic regression formula, since one class's coefficients can be fixed at zero as a reference category without loss of generality.

(Start again from lecture 4.5)