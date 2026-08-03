Tree-based methods are common machine learning methods that can be used for both classification and regression. The main idea is segment the predictor space into multiple smaller subspaces based on rules. Given this separation, we then provide a predict from the model using the mode or average of the observations in the subspace the new sample falls into. The models are 'tree-based' since the splitting rules are actually applied in a sequential order. They are also called *decision trees*.

They might small and easily interpretable. Main con is that they might not be competitive with the best supervised learning.

*Important caveat*: standard tree-based models might be easily interpretable, but usually their performance might lower than many other standard-level models. It is though to *merge* these type of models together, creating the so-called ***Ensemble models***. That is why usually the model merging techniques like [[Bagging]] or [[Boosting]] are introduced after tree-based models. 
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

###### Related topics

- [[Bagging]]
- [[Random Forest model]]
- [[Boosting]]
- [[Bayesian Additive Regression Trees (BART)]]