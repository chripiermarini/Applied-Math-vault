Let's explore a business problem in which we want to analyze the relationship between the Sales and advertising medium:

$$
Sales \approx f(medium)
$$

Given our input, we can model the phenomenon as

$$
Y = f(X) + \epsilon
$$

where $\epsilon$ is the **irreducible error**/deviation made in the measurement.

Our goal is to define a practical function $\hat{f}$ which approximates with a good level of precision the original $f$. Of course, two questions arie in this setting:

1) Is there an **ideal** $f$? 
2) If we are trying to perform **prediction** (i.e. being able to predict the most likely value of the realization $y$ given an input data point $x$), what is a good value of $f(x)$?
3) If we are trying to perform **inference** (i.e. understanding the true relationship between a realization $Y$ and an input set $X_{1}, \dots, X_{p}$), what is a good function $f$ which properly describe such relationship? Which are the predictors that are actually associated with the response? Can the relationship between $Y$ and each predictor be adequately summarized using a linear equation?

##### Regression functions 

In a supervised learning framework, we might want to compute a good value of our realization $f(x)$ given any value of $X$. Still, we might have multiple values of X associated with the same Y, or even no value at all. In order to avoid this paradox, we might want to interpret the regression function using of the **conditional expected value**:

$$
f(4) = \mathbb{E}(Y|X=4)
$$

By this interpretation, **a regression function** is any function that provides the conditional expectation of the response variable, given a set of predictors.

How to compute an **optimal/ideal** regression function? We would compute the estimate function $\hat{f}$ which minimizes the mean-squared prediction error. Specifically, among all functions $g$ and given an input variable $x$, the ideal estimate function is the one that minimizes 

$$
\mathbb{E}[(Y - g(X))^2 | X= x]
$$

among all points $X=x$. 

Of course, knowing the definition of $f(x)$, we might also derive the **reducible** and **irreducibile** parts of the error. Specifically, we know that 

$$
\epsilon = y - f(x)
$$

is in eliminable error in the prediction, since at each $X = x$ there is typically a distribution of possible $Y$ realizations. Hence, we know that

$$
\mathbb{E}[(Y - \hat{f}(X))^2 | X= x] = [f(x) - \hat{f}(x)]^2 + Var(\epsilon)
$$

where the first component is reducible and second one is irreducible.

Finally, one can generalise the concept of conditional expectation using the **Nearest neighbourhood average estimate**.

##### Curse of dimensionality

Nearest neighbor techniques can be ineffective when the dimension of the variables is extremely high. Practically speaking, the neighbours are actually farer in higher dimensions.

Generally, the **curse of dimensionality** describes the phenomena that occur when analyzing and organising data in high-dimensional spaces. As the number of features or dimensions increases, the volume of the space increases exponentially. This causes data points to become extremely sparse, making distance metrics less meaningful and causing algorithms to require significantly more data and computational power to find patterns

In order to deal with this, we introduced *structure* to our models.

##### Linear models

A linear model, as the name suggests, models the phenomena realization as a linear combination of the features of an input data:

$$
y = \beta_{0} + \beta_{1}x_{1} + \dots + \beta_{p}x_{p}
$$

Although this is usually never correct, linear models are simple to use and highly interpretable. 

These models can be easily improved, but we need to consider some **tradoffs**:
1)  Prediction accuracy (complex models) versus interpretability (simpler models);
2) *Good fit versus over-fitting or under-fitting*.
3) Parsimony over black-box models.

##### Assessing model accuracy
Given a fitted model $\hat{f}(x)$ to some training data, we would like to assess how well it performs.
There exists a plethora of different metrics, still one of the most relevant metrics is the **Mean Squared Error**:

$$
\text{MSE} = \frac{1}{n} \sum_{i=1}^{n} (y_{i} - \hat{f}(x))^2
$$

This metric works fine as penalises predictions which are far from true realizations. Still, mistakes are extremely penalised. 

##### Bias-Variance trade-offs
It is possible (and extremely important to notice that) the MSE of a model can be decomposed into three main components:

$$
\mathbb{E}(y_{0} - \hat{f}(x_{0}))^2 = \text{Var}(\hat{f}(x_{0})) + [\text{Bias}(\hat{f}(x_{0}))]^2 + \text{Var}(\epsilon)
$$

The first is the **variance** of the estimating model $\hat{f}$ . This measures how much the models would change if we chose a different training dataset. A model is considered *flexible* if its variance is high.

The second component is the **bias**, which is the error that is introduced when approximating a real life phenomenon. Generally speaking, the higher variance, the lower the bias, and viceversa. This depends on the flexibility of the model, to being able to approximate well the underlying process. 

Third component is the **variance of the irreducible error**.

For an in-depth explanation of the Bias-Variance error please check the below screenshot from *Elements of Statistical learning*. 

![[Screenshot 2026-07-13 alle 16.47.17.png]]

##### Classification task

In classification tasks, the response variable is **qualitative**, meaning it is a non-numerical attribute, usually belonging to a finite number of discrete realizations.

In these case of tasks, we need to define a ***Bayes optimal classifier function***, which computes, for each possible realization of the response variable, the probability that a specific realization occurs. 

We can write such function assigning each probability as the following

$$
p_{k}(x) = P(Y=k|X=x), \quad k = 1,2,\dots K
$$

Such probabilties are also deemed *conditional class probabilties*. We want to compute the Bayes optimal classifier as the following:

$$
C(x) = j \text{ if } p_{j}(x) = \text{max}\{p_{1}(x), p_{2}(x) \dots, p_{K}(x)\} 
$$

The $C(x)$ function assigns then the class of a given input associated with the highest probability. 

In classification task we can also use a ***Nearest neighbors averaging*** algorithm, but it suffers by the *curse of dimensionality* similarly as the regression task example. As soon as the dimension of input space grows larger and larger, the validity of the algorithm breaks down as the procedure is not *local* anymore.

We measure the performance of the classifier model using the **misclassification rate of error**, i.e. the number of classification mistakes given the input examples:

$$
\text{Err}_{\hat{C}} = Avg(\mathbb{I}[y_{i} \neq \hat{C}(x)])
$$

We now look at a graphical representation of the K-nearest neighbors procedure in two dimensions. The black curve represent the *contour*.

![[Screenshot 2026-07-13 alle 22.38.44.png|697]]

The choice of the number of K nearest neighbors is part of the tuning of the model.

![[Screenshot 2026-07-13 alle 22.42.56.png]]
