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

(Quickly check case-control sampling)
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

###### Linear discriminant analysis

We now follow a different approach to perform these classification tasks. Logistic regression involves modeling the probability of class membership directly using the logistic function. The logistic function may suffer from some drawbacks in certain classification settings (e.g. very unstable parameter estimates when the classes are well separated).

Additionally, if the predictors X are approximately normally distributed within each class, another approach may be more accurate.

This approach, called Linear Discriminant Analysis, follows a generative approach. First, we assume that, within each class k, the distribution of the predictors is normal, with a class-specific mean $\mu_k$ and a variance $\sigma^2$ shared across all classes, giving an assigned density function $f_k(x)$. Furthermore, we also know the prior probabilities $\pi_k$ of a point belonging to class k a priori.

We can then use Bayes' theorem to infer the posterior probability of a specific point belonging to class k, and classify the point by assigning it to the class with the highest posterior probability (the Bayes classifier).

Please let's look at the Gaussian density form

$$f(x) = \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left( -\frac{(x - \mu)^2}{2\sigma^2} \right)$$

If we plug in the *Bayes formula*, we get a rather complex formula:

$$p_k(x) = \frac{\pi_k \dfrac{1}{\sqrt{2\pi}\sigma} e^{-\frac{1}{2}\left(\frac{x - \mu_k}{\sigma}\right)^2}}{\sum_{l=1}^{K} \pi_l \dfrac{1}{\sqrt{2\pi}\sigma} e^{-\frac{1}{2}\left(\frac{x - \mu_l}{\sigma}\right)^2}}$$

where:

- $\pi_k$ is the prior probability that an observation belongs to class $k$
- $\mu_k$ is the mean of the predictor $x$ for class $k$
- $\sigma$ is the common standard deviation, shared across all $K$ classes (this shared-variance assumption is the key simplification in LDA)
- the numerator is the density of class $k$ weighted by its prior
- the denominator sums this same quantity over all $K$ classes, ensuring the probabilities across classes sum to 1


Taking the log of $p_k(x)$ and dropping terms that are common across all classes (since they don't affect which class is largest), we obtain the *discriminant function*:

$$\delta_k(x) = x \cdot \frac{\mu_k}{\sigma^2} - \frac{\mu_k^2}{2\sigma^2} + \log(\pi_k)$$

The classifier assigns an observation $x$ to the class $k$ that maximizes $\delta_k(x)$:

$$\hat{y} = \arg\max_{k} \; \delta_k(x)$$

Notice that $\delta_k(x)$ is a linear function of $x$, which is exactly why the method is called linear discriminant analysis: the decision boundary between any two classes $k$ and $l$ is obtained by setting $\delta_k(x) = \delta_l(x)$, which results in a linear equation in $x$.

Since $\pi_k$, $\mu_k$, and $\sigma^2$ are unknown in practice, they must be estimated from the training data before $\delta_k(x)$ can be computed.

1) Estimating the prior, $\hat{\pi}_k$

$$\hat{\pi}_k = \frac{n_k}{n}$$

where $n_k$ is the number of training observations belonging to class $k$, and $n$ is the total number of observations. This is simply the proportion of observations in class $k$.

2) Estimating the class mean, $\hat{\mu}_k$

$$\hat{\mu}_k = \frac{1}{n_k} \sum_{i: y_i = k} x_i$$

This is the average of $x$ over only the observations belonging to class $k$.

3) Estimating the shared variance, $\hat{\sigma}^2$

$$\hat{\sigma}^2 = \frac{1}{n - K} \sum_{k=1}^{K} \sum_{i: y_i = k} \left( x_i - \hat{\mu}_k \right)^2$$

This is a pooled (weighted average) estimate of variance across all $K$ classes, reflecting the assumption that all classes share the same variance. Dividing by $n - K$ rather than $n$ corrects for the degrees of freedom lost from estimating $K$ separate class means.

4) Plugging the estimates into the discriminant function

$$\hat{\delta}_k(x) = x \cdot \frac{\hat{\mu}_k}{\hat{\sigma}^2} - \frac{\hat{\mu}_k^2}{2\hat{\sigma}^2} + \log(\hat{\pi}_k)$$

The classifier then assigns $x$ to the class $k$ that maximizes $\hat{\delta}_k(x)$.

*Please remember*: if we wanted to compare the main structure of the two models, remember that the function we will use for logistic is actually the logistic regression function, in the case of LDA, we are using the discriminant function actually!

##### Linear discriminant analysis with more than one variable ($p>1$)

The mathematics becomes slightly complicated in a sense, by the most important part is that variance is substituted by the covariance matrix.

Nonetheless, even with the introduction of the covariance matrix instead of the simple variance, the *discriminant scores* still remain linear w.r.t. the input data $x$. 

The important thing to make sure it is noticed in this instance is that when there are $K$ classes, the linear discriminant analysis plots the points in a $K-1$ dimensional space. Then, it classifies each point to the closes centroid. Hence, the LDA can be viewed exactly in a $K-1$ dimensional plot.
##### Confusion matrix and ROC curve

Whatever it is the algorithm that we apply for classification, we still need to assess the performances of the model. In order to do so, we might want to use some specific techniques.

1) *Accuracy*: straightforward proportion of the number of cases which have been correctly classified. 

2) *Confusion matrix*: a tabular display of 2x2 dimensions in the binary case, where we display the record counts by their predicted and actual classification status. In simpler terms, we state how many true positives and negatives the model properly exposed, as well as the false positives (predicted positives but actually negatives) and false negatives we have. From this table, other metrics can be then displayed:

	![[Screenshot 2026-07-23 alle 11.03.44.png|626]]

	In this table, other measurements are also displayed, which can be of variable importance. Specifically, it might be very important to correctly specify which data belongs to a 'rare class' (i.e. a class in which we a smaller amount of samples w.r.t the others).
	
3) *Precision, Recall/Sensitivity and Specificity*: those are three specific metrics, which are complementary to each other, yet providing different perspectives of the model performances. 
	
	Precision is computed as $$\frac{TP}{TP + FP}$$ and it measures the *accuracy* of predicting a positive (capacity of filtering out what is not positive).
	
	Recall is computed as $$\frac{TP}{TP + FN}$$ and it measures the strength of predicting a positive (capacity of correctly recognizing what is positive out of noise).
	
	Finally, *specificity* is computed as $$\frac{TN}{TN + FP}$$ and it measures the accuracy of predicting a *negative* (capacity of correctly recognizing what is negative out of noise).
	
3) *ROC curve*: *Receiving Operating Characteristics* curve, it plots in 2D graph the curve which represent of how much specificity of the model we sacrifice in order to have an improvement in the recall/sensitivity. We would like to have curve which is as high as possible. 
 
	![[Screenshot 2026-07-23 alle 12.11.57.png]]
	
	In order to build such curve, the procedure is as follows: 
	
	1) First, we fit the training data in the classifier, and then use our model on test data, which will provide us with the expected probability for each point to belong to class 1. 
	2) Then, we sort the data points based on such probabilities (from 0 to 1). 
	3) We select a threshold value, $t$, that will increase from 0 to 1. All the data points which probability is higher than $t$ are labeled positives, negative otherwise. 
	4) We compute the Recall and the Specificity for that particular t. This becomes a point in the graph.
	5) We finally repeat such procedure for all values from $t=0$ to $t=1$. 
	
	Finally, we can also summarize the visualization results into a practical statistic, which is the *AUC*, Area Under the Curve, as the integral of the curve between 0 and 1. 

#### Quadratic Discriminant Analysis

The form used for the discriminant analysis might be generalised instead of just using Gaussian densities.  Furthermore, it might be possible that when using multivariate Gaussian distribution, the covariance matrix is not equal to all the classes. We might have a situation in which the covariance $\Sigma_{k}$ is actually class dependant. In those instance, we have the called *quadratic discriminant analysis*, where the name comes from the fact that in the discriminant score function the quadratic term does not cancel out.

Unfortunately, if the number of variable is too high, LDA/QDA can break down.  That is why is almost always used the Naive Bayes classifier. 

QDA relaxes the shared-covariance assumption of LDA, estimating a separate covariance matrix per class. This adds flexibility (a quadratic decision boundary instead of a linear one), but also adds more parameters to estimate — which can lead to overfitting/noisier estimates when there isn't much real signal in the data, potentially resulting in worse generalization (e.g., lower test-set AUC) despite a better fit on training data.
#### Naive Bayes

One last difference also between the classical LDA technique is assuming that the predictors within each class are independent from each other. This means that the covariance matrix is actually diagonal (all zeros apart from the diagonal/variance values). In this case, the probability density is the following:

$$
f_{k}(x) = \prod_{j=1}^{p} f_{jk}(x_{j})
$$
This classifier is then called *Naive Bayes*.
