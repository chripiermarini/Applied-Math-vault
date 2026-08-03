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