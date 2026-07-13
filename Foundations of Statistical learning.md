Let's explore a business problem in which we want to analyze the relationship between the Sales and advertising medium:

$$y \approx f(medium) $$
Given our input, we can model the phenomenon as

$$ Y = f(X) + \epsilon $$
where $\epsilon$ is the **irreducible error**/deviation made in the measurement.

Our goal is to define a practical function $\hat{f}$ which approximates with a good level of precision the original $f$. Of course, two questions arise in this setting:

1) Is there an **ideal** $f$? 
2) If we are trying to perform **prediction** (i.e. being able to predict the most likely value of the realization $y$ given an input data point $x$), what is a good value of $f(x)$?
3) If we are trying to perform **inference** (i.e. understanding the true relationship between a realization $Y$ and an input set $X_{1}, \dots, X_{p}$), what is a good function $f$ which properly describe such relationship? Which are the predictors that are actually associated with the response? Can the relationship between $Y$ and each predictor be adequately summarized using a linear equation?
##### Regression functions 

In a supervised learning framework, we might want to compute a good value of our realization $f(x)$ given any value of $X$. Still, we might have multiple values of X associated with the same Y, or even no value at all. In order to avoid this paradox, we might want to interpret the regression function using of the **conditional expected value**:

$$f(4) = \mathbb{E}(Y|X=4)$$
By this interpretation, **a regression function** is any function that provides the conditional expectation of the response variable, given a set of predictors.

How to compute an **optimal/ideal** regression function? We would compute the estimate function $\hat{f}$ which minimizes the mean-squared prediction error. Specifically, among all functions $g$ and given an input variable $x$, the ideal estimate function is the one that minimizes 

$$\mathbb{E}[(Y - g(X))^2 | X= x] $$
among all points $X=x$. 

Of course, knowing the definition of $f(x)$, we might also derive the **reducible** and **irreducibile** parts of the error. Specifically, we know that 

$$\epsilon = y - f(x)$$
is in eliminable error in the prediction, since at each $X = x$ there is typically a distribution of possible $Y$ realizations. Hence, we know that

$$\mathbb{E}[(Y - \hat{f}(X))^2 | X= x] = [f(x) - \hat{f}(x)]^2 + Var(\epsilon)$$
where the first component is reducible and second one is irreducible.

Finally, one can generalise the concept of conditional expectation using the **Nearest neighbourhood average estimate**.
##### Curse of dimensionality

Nearest neighbor techniques can be extremely ineffective when the dimension of the variables is extremely high. Practically speaking, the neighbours are actually pretty far in higher dimensions. 

Generally, the **curse of dimensionality** describes the phenomena that occur when analyzing and organising data in high-dimensional spaces. As the number of features or dimensions increases, the volume of the space increases exponentially. This causes data points to become extremely sparse, making distance metrics less meaningful and causing algorithms to require significantly more data and computational power to find patterns

In order to deal with this, we introduced *structure* to our models.
##### Linear models
A linear model, as the name suggests, models the phenomena realization as a linear combination of the features of an input data:
$$y = \beta_{0} + \beta_{1}x_{1} + \dots + \beta_{p}x_{p}$$Although this is usually never correct, linear models are simple to use and highly interpretable. 

These models can be easily improved, but we need to consider some **tradoffs**:
1)  Prediction accuracy (complex models) versus interpretability (simpler models);
2) *Good fit versus over-fitting or under-fitting*.
3) Parsimony over black-box models.
##### Assessing model accuracy

Given a fitted model $\hat{f}(x)$ to some training data, we would like to assess how well it performs.
There exists a plethora of different metrics, still one of the most relevant metrics is the **Mean Squared Error**:

$$ \text{MSE} = \frac{1}{n} \sum_{i=1}^{n} (y_{i} - \hat{f}(x))^2$$
This metric works fine as penalises predictions which are far from true realizations. Still, mistakes are extremely penalised. 
##### Bias-Variance trade-offs

It is possible (and extremely important to notice that) the MSE of a model can be decomposed into three main components:

$$\mathbb{E}(y_{0} - \hat{f}(x_{0}))^2 = \text{Var}(\hat{f}(x_{0})) + [\text{Bias}(\hat{f}(x_{0}))]^2 + \text{Var}(\epsilon)$$
The first is the **variance** of the estimating model $\hat{f}$ . This measures how much the models would change if we chose a different training dataset. A model is considered *flexible* if its variance is high.

The second component is the **bias**, which is the error that is introduced when approximating a real life phenomenon. Generally speaking, the higher variance, the lower the bias, and viceversa. This depends on the flexibility of the model, to being able to approximate well the underlying process. 

Third component is the **variance of the irreducible error**.

For an in-depth explanation of the Bias-Variance error please check the below screenshot from *Elements of Statistical learning*. 

![[Screenshot 2026-07-13 alle 16.47.17.png]]
