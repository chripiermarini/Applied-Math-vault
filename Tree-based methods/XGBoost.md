
XGBoost is one of the most widely used tree-based models for tabular data, achieving state-of-the-art results across many benchmarks. In a nutshell, it improves on classical Gradient Boosting by fitting each tree via a second-order optimization step (recalling a classic Newton step, using both gradient and Hessian of the loss) instead of a first-order gradient-only approximation.

This is applied to a regularized objective, $\Omega(f_t) = \gamma T + \frac{1}{2}\lambda\sum_j w_j^2$, which penalizes both the number of leaves and their weights, yielding a closed-form optimal weight per leaf and a regularized gain criterion (replacing RSS/Gini) that prunes unproductive splits automatically. On a technical note, it is built and optimized for large tabular data, employing parallel distributed computing, built-in regularization and handling missing values.

##### Mathematical properties of XGBoost

The model benefits from multiple mathematical properties tied to the learning paradigm and tree-splitting:

1) *Regularized Learning objective* where the Gradient Tree Boosting algorithm benefits from the use of second order Taylor-expansion of the loss function. The use of the hessian function might sound intractable in an ML environment due to the extremely high amount of data points usually used for training. This problem is solved by computing the first and second derivative only with respect to the scalar prediction of the single observation, not w.r.t the models parameters. In this case, the derivatives are trivial (in the regression case with $L_2$ norm loss, the first derivative is the residual itself $y - \hat{y}$ and the hessian is the single integer value one). Of course, the regularization must be also included in the loss function.
2) *Split finding algorithms*: (to be completed)

##### Computational properties of XGBoost

1) *Column block for parallel learning*:
2) *Cache-aware access*:
3) *Blocks for Out-of-core*:

Reference: Chen & Guestrin (2016), *"XGBoost: A Scalable Tree Boosting System"*, arXiv:1603.02754. [[XGBoost-paper.pdf]] 

IBM explanation on XGBoost [here](https://www.ibm.com/think/topics/xgboost). 