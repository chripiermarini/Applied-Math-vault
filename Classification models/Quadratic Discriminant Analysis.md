#### Quadratic Discriminant Analysis

The form used for the discriminant analysis might be generalised instead of just using Gaussian densities.  Furthermore, it might be possible that when using multivariate Gaussian distribution, the covariance matrix is not equal to all the classes. We might have a situation in which the covariance $\Sigma_{k}$ is actually class dependant. In those instance, we have the called *quadratic discriminant analysis*, where the name comes from the fact that in the discriminant score function the quadratic term does not cancel out.

Unfortunately, if the number of variable is too high, LDA/QDA can break down.  That is why is almost always used the Naive Bayes classifier. 

QDA relaxes the shared-covariance assumption of LDA, estimating a separate covariance matrix per class. This adds flexibility (a quadratic decision boundary instead of a linear one), but also adds more parameters to estimate — which can lead to overfitting/noisier estimates when there isn't much real signal in the data, potentially resulting in worse generalization (e.g., lower test-set AUC) despite a better fit on training data.