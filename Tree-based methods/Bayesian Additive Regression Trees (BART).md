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