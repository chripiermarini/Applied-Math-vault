
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