Tree-based methods are common machine learning methods that can be used for both classification and regression. The main idea is segment the predictor space into multiple smaller subspaces based on rules. Given this separation, we then provide a predict from the model using the mode or average of the observations in the subspace the new sample falls into. The models are 'tree-based' since the splitting rules are actually applied in a sequential order. They are also called *decision trees*.

They might small and easily interpretable. Main con is that they might not be competitive with the best supervised learning.
###### Practical example to understand stratification

By observing the data, we might see that we can also divide the predictor space based on values of the predictor themselves.


![[Screenshot 2026-07-31 alle 15.46.44.png|478]]

*How can we compute these limits*? Tree-based algorithms are used for that! The main point is that these values are actually well-interpretable.

##### Foundations of tree-based models

*As a useful terminology*, we have **terminal nodes** being the actual regions in which we divide the predictor space into. The **internal nodes** are the non-terminal nodes.

*How do these work*? We divide the predictor space into $J$ distinct and non-overlapping regions, being $R_1, R_2 \dots, R_j$. We might also consider those in terms of *boxes*, meaning that the predictor region can be also divided into high-dimensional rectangles. The goal is to find the boxes that minimise the RSS, given by the formula $$\sum_{j=1}^{J}\sum_{i \in R_j}\left(y_i -\hat{y}_{R_j}\right)^{2},$$ where the second term is the mean response for the training observation with the $j$th box. Unfortunately, this calculation is computationally intractable, hence, we usually take a *top-down, greedy* approach that is known to be recursive binary splitting.

Being top down, it starts from the root of the tree and then successively splits the predictor space, being greedy it always select the best option for the current state, not the future one.

*On a graphical side,* it is mandatory to split the full region into a partition of boxes that cannot overlap. 
##### Pruning a tree to avoid overfitting

It is trivial to understand that a very large tree (meaning, a tree with an high number of terminal nodes) might easily overfit the data, as we can reach a situation in which each data point has its own terminal node. In order to avoid this, the process is to create such large tree and then prune it back to state in which we have subtree that can generalize enough without overfitting. 

The employed technique is called *Cost complexity pruning* and its use regards the minimization of the below function:

$$
\sum_{m=1}^{|T|}\sum_{i:x_i \in R_m} \left(y_i - \hat{y}_i \right)^{2} + \alpha|T|,
$$
where have $T$ as the set of terminal nodes, and $\alpha$ the penalising parameter (similar to the *Lasso* regularization term, which tries to minimize the number of non-zero terminal nodes). Furthermore, $R_m$ is the $m$-th region in which we divide the data. Again, in order to select the optimal $\alpha$, we use cross-validation.
##### Classification trees

Just like regression tasks, we use recursive binary splitting to grow a classification tree. Still, in these kind of tasks we cannot use RSS, and hence we rely on the *classification error rate* criteria.

The classification error rate computes the fraction of the training observation in a region that do not belong to the most common class, meaning 

$$
E = 1 - max_{k}(\hat{p}_{mk})
$$
where $p_{mk}$ represents the proportion of training observation in. the $m$-th region that belong in the $k$-th class. Unfortunately though classification error is not sufficiently sensitive for tree-growing, and in practice two other metrics are preferred.

We usually use the Gini index and the cross-entropy values. 

**Gini index** $$G = \sum_{k=1}^{K} \hat{p}_{mk}(1 - \hat{p}_{mk})$$ The Gini index is a measure of total variance across the $K$ classes: it takes on small values when all $\hat{p}_{mk}$ are close to 0 or 1 (i.e. the node is pure, dominated by a single class), and it is largest when classes are evenly mixed. For this reason it's often referred to as a measure of node **purity**. 

**Cross-entropy** $$D = -\sum_{k=1}^{K} \hat{p}_{mk} \log(\hat{p}_{mk})$$ Cross-entropy behaves numerically very similarly to the Gini index: since $0 \le \hat{p}_{mk} \le 1$, the term $-\hat{p}_{mk}\log(\hat{p}_{mk})$ is non-negative and approaches 0 as $\hat{p}_{mk}$ approaches 0 or 1. So $D$ takes small values for pure nodes, just like $G$. 

**Practical notes** - Both Gini and cross-entropy are more sensitive to node purity than the classification error rate, which makes them preferable when *growing* the tree (choosing splits). - When *pruning* the tree, all three criteria (error rate, Gini, cross-entropy) can be used, but classification error rate is typically preferred if prediction accuracy of the final tree is the goal. 

##### Pros and cons of decision trees

1) Easily interpretable by humans.
2) Decision making process extremely similar to actual human one. 
3) It can easily handle qualitative predictors without dummy variables.
4) Unfortunately, their performance might be suboptimal with respect to linear models.

##### Bagging

This technique uses both the bootstrap method to generate simulated data, and decision trees to provide the response prediction.  

The statistical principle involved is that the variance of the average of a specific statistic is smaller than the variance of the component of statistic themselves. As an example, if we had multiple observations $Z_i$ with variance $\sigma^{2}$ , the average of the observations would have variance equal to $\sigma^{2}/n$. The important assumption that is violated in bagging is that the observations should always be independent, which is not the case for bootstrapped datasets. In fact, knowing that they are generated from the same dataset, the bootstrapped datasets are inevitably correlated. The variance reduction exists then, even though it is smaller than expected.

The bagging methods works as follows: given a training set, we generate $B$ different bootstrap datasets, and we build a tree for each of such dataset. Then we average of the predictions of each tree, and provide the value.

From a mathematical perspective, we will obtain the following 
$$ 
\hat{f}_{bag}(x) = \frac{1}{B}\sum_{b=1}^{B}\hat{f}^{*b}(x),
$$
where each single component is the prediction of the tree trained on the $b$-th bootstrapped dataset.

The cleverness of the idea comes form the fact that we don't need to prune the trees when using this technique: by pruning, we might introduce a lot of bias in the prediction as we are trying to reduce the variance. But, using bagging, one can have the 'bushy'/deep trees, and we can reduce the variance by combining the predictions.

For classification, we usually take the *majority vote* approach: we take the most common class among all the predictions.

**Out-of-Bag Error estimate**: a special type of error estimate that comes essentially for free when bagging. For a bootstrap sample of size $n$, the probability that a given observation is *not* included is $(1-1/n)^n \to e^{-1} \approx 0.368$ as $n \to \infty$. So on average about 63% of observations appear in each bootstrap sample, and the remaining ~37% are "out-of-bag" (OOB) for that tree. To estimate the test error for the $i$-th observation, we predict its response using only the trees for which it was OOB (roughly $B/3$ of them), and average (or majority-vote) those predictions. Averaging this across all $n$ observations gives the OOB error estimate — a valid approximation of test error that requires no separate validation set or cross-validation, which is particularly convenient for large $B$.
##### Random Forest model

This model tries to solve the major problem that Bagging models are affected by, which is the existing correlation between trees trained on bootstrapped datasets.

In Random Forest models, one tree is trained for each bootstrapped dataset. At each split, one subset of randomly chosen predictors (typically of size $m \approx \sqrt{p}$ for classification, $m \approx p/3$ for regression, where $p$ is the total number of predictors) is selected as candidate predictors for that split. This means that, given two distinct trees trained on different bootstrapped datasets, the splits are different not only because the datasets differ, but because the subset of candidate predictors is re-drawn randomly at *every single split*, independently of what happened at previous splits. Usually, the number of predictors which are taken from the candidates is equal to $\sqrt{p}$. 

*Why does this impact uncorrelation?* If we happen to have a dominant predictor (i.e. a predictor which has a strong relationship with the response), most of the trees in a plain Bagging ensemble will choose to split on this predictor near the top, regardless of which bootstrapped dataset they were trained on — making the trees highly similar (correlated) to one another. By randomly restricting the set of candidate predictors at each split, there is a chance that the dominant predictor is simply not available in the candidate subset, forcing the tree to split on a different predictor instead.

*Why does this matter?* Recall that for $B$ identically distributed but correlated predictors with pairwise correlation $\rho$ and variance $\sigma^2$, the variance of their average is

$$\text{Var}\left(\frac{1}{B}\sum_{b=1}^{B}\hat{f}^{*b}(x)\right) = \rho\sigma^2 + \frac{1-\rho}{B}\sigma^2$$

As $B \to \infty$, the second term vanishes, but the first term $\rho\sigma^2$ does not — it acts as a floor on how much variance reduction is achievable. By reducing $\rho$ (the average correlation between trees), predictor subsampling lowers this floor, yielding a further variance reduction beyond what Bagging alone (which corresponds to the special case $m = p$) can achieve. Trees remain low-bias (since they are grown deep and unpruned), while the ensemble now attains lower variance than plain Bagging thanks to the reduced correlation between trees.

##### Boosting

The boosting technique can be applied also to multiple other models, whether regression or classification, but we are going to discuss them in the tree-based models area.

*Boosting is a sequential iterative approach*: we first start from a null 'current' model and residuals equal to $y$ (the response). We first train a tree-based model on the full dataset, and compute the residuals on the prediction. We then shrink the obtained model by applying a multiplicative factor of $\lambda$ (very small, e.g. 0.01), and we add it to the 'current' model. We then repeat the process by training a new tree-based model on the residuals. We compute the new residuals, and then update the current model again with the new shrinked model. We then continue over and over.

The *main idea* is to solve the possible overfitting of the data which can occur when fitting a large tree, by using the boosting approach and instead learning *slowly*. All the contributions from the model updates might be quite small given the fact that we continuously fit on residuals, but we slowly improve the the current model $\hat{f}$ in areas where it does not perform well. Furthermore,  the multiplicative factor of $\lambda$ shrinks the process even further allowing more and different shaped trees to attack the residuals. 

In this setting, boosting for decision trees involve a large amount of hyperparameters to be chosen: the number of different trees trained, the shrinkage parameter and the number of splits for each tree.

##### Variable Importance measures

For Bagging and Random Forest models, since we no longer have a single tree to interpret, we need an aggregate measure to assess which predictors matter most across the whole ensemble.

- **For regression trees**: for each predictor, we record the total amount by which RSS is decreased due to splits over that predictor, averaged over all $B$ trees.
- **For classification trees**: analogously, we record the total amount by which the Gini index is decreased due to splits over that predictor, averaged over all $B$ trees.

A large value indicates an important predictor. Averaging over many trees (each built on a bootstrapped dataset and, for Random Forest, a random subset of candidate predictors at each split) yields a much more stable and reliable importance measure than what could be obtained from a single tree, and also helps distribute importance more sensibly among correlated predictors.

##### Bayesian Additive Regression Trees (BART)

BART is a decision-tree based model applicable to both regression and classification. Its structural principle — a sum of trees where each tree captures whatever the others do not yet explain — is conceptually closer to Boosting than to Bagging/Random Forest, but the fitting mechanism is entirely different: instead of greedily fitting each tree once and freezing it, BART treats the sum-of-trees as a Bayesian model and samples from its posterior via MCMC (Bayesian backfitting).

The model is composed of $K$ tree models, all initialized as trivial root nodes (single leaf), and updated over $B$ MCMC iterations. At iteration $b$, to update the $k$-th tree, we first compute the partial residual using the most recently available version of every other tree:

$$r_i = y_i - \sum_{k' < k} \hat{f}_{k'}^{b}(x_i) - \sum_{k' > k} \hat{f}_{k'}^{b-1}(x_i)$$

i.e. the already-updated trees in the current iteration ($k' < k$), and the not-yet-updated trees still holding their previous iteration's value ($k' > k$) — this is a Gibbs sampling step, using the latest available state of the system.

Given $r_i$, the tree is updated in two sub-steps:
1. **Structure update (Metropolis-Hastings)**: propose a random perturbation to the tree structure (grow/prune/change/swap), and accept or reject it with probability depending on how well the perturbed structure explains $r_i$ relative to the prior (which penalizes overly deep/complex trees).
2. **Leaf value update (Gibbs step)**: given the (possibly updated) tree structure, sample the leaf values $\mu = \hat{f}_k^{b}(x)$ from their closed-form posterior conditional on $r_i$.

After discarding an initial burn-in period ($B_0$ iterations), the final prediction is obtained by averaging the sum-of-trees prediction across the remaining post burn-in iterations:

$$\hat{f}(x) = \frac{1}{B - B_0}\sum_{b=B_0+1}^{B}\sum_{k=1}^{K} \hat{f}_k^{b}(x)$$

This averaging over posterior draws (rather than using a single final model) is also what naturally yields credible intervals for the prediction, not just a point estimate.