
One last difference also between the classical LDA technique is assuming that the predictors within each class are independent from each other. This means that the covariance matrix is actually diagonal (all zeros apart from the diagonal/variance values). In this case, the probability density is the following:

$$
f_{k}(x) = \prod_{j=1}^{p} f_{jk}(x_{j})
$$
This classifier is then called *Naive Bayes*.

*Careful*: Naive Bayes assumes that all the predictors are independent one another. By predictors we mean that the features of each observation are actually independent, not the single observations. 