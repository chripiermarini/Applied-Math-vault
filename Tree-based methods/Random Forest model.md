##### Random Forest model

This model tries to solve the major problem that Bagging models are affected by, which is the existing correlation between trees trained on bootstrapped datasets.

In Random Forest models, one tree is trained for each bootstrapped dataset. At each split, one subset of randomly chosen predictors (typically of size $m \approx \sqrt{p}$ for classification, $m \approx p/3$ for regression, where $p$ is the total number of predictors) is selected as candidate predictors for that split. This means that, given two distinct trees trained on different bootstrapped datasets, the splits are different not only because the datasets differ, but because the subset of candidate predictors is re-drawn randomly at *every single split*, independently of what happened at previous splits. Usually, the number of predictors which are taken from the candidates is equal to $\sqrt{p}$. 

*Why does this impact uncorrelation?* If we happen to have a dominant predictor (i.e. a predictor which has a strong relationship with the response), most of the trees in a plain Bagging ensemble will choose to split on this predictor near the top, regardless of which bootstrapped dataset they were trained on — making the trees highly similar (correlated) to one another. By randomly restricting the set of candidate predictors at each split, there is a chance that the dominant predictor is simply not available in the candidate subset, forcing the tree to split on a different predictor instead.

*Why does this matter?* Recall that for $B$ identically distributed but correlated predictors with pairwise correlation $\rho$ and variance $\sigma^2$, the variance of their average is

$$\text{Var}\left(\frac{1}{B}\sum_{b=1}^{B}\hat{f}^{*b}(x)\right) = \rho\sigma^2 + \frac{1-\rho}{B}\sigma^2$$

As $B \to \infty$, the second term vanishes, but the first term $\rho\sigma^2$ does not — it acts as a floor on how much variance reduction is achievable. By reducing $\rho$ (the average correlation between trees), predictor subsampling lowers this floor, yielding a further variance reduction beyond what Bagging alone (which corresponds to the special case $m = p$) can achieve. Trees remain low-bias (since they are grown deep and unpruned), while the ensemble now attains lower variance than plain Bagging thanks to the reduced correlation between trees.