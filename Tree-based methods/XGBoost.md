
XGBoost is one of the most widely used tree-based models for tabular data, achieving state-of-the-art results across many benchmarks. In a nutshell, it improves on classical Gradient Boosting by fitting each tree via a second-order optimization step (recalling a classic Newton step, using both gradient and Hessian of the loss) instead of a first-order gradient-only approximation.

This is applied to a regularized objective, $\Omega(f_t) = \gamma T + \frac{1}{2}\lambda\sum_j w_j^2$, which penalizes both the number of leaves and their weights, yielding a closed-form optimal weight per leaf and a regularized gain criterion (replacing RSS/Gini) that prunes unproductive splits automatically.

Reference: Chen & Guestrin (2016), *"XGBoost: A Scalable Tree Boosting System"*, arXiv:1603.02754. [[XGBoost-paper.pdf]] 

IBM explanation on XGBoost [here](https://www.ibm.com/think/topics/xgboost). 