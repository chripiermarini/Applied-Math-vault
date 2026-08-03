##### Boosting

The boosting technique can be applied also to multiple other models, whether regression or classification, but we are going to discuss them in the tree-based models area.

*Boosting is a sequential iterative approach*: we first start from a null 'current' model and residuals equal to $y$ (the response). We first train a tree-based model on the full dataset, and compute the residuals on the prediction. We then shrink the obtained model by applying a multiplicative factor of $\lambda$ (very small, e.g. 0.01), and we add it to the 'current' model. We then repeat the process by training a new tree-based model on the residuals. We compute the new residuals, and then update the current model again with the new shrinked model. We then continue over and over.

The *main idea* is to solve the possible overfitting of the data which can occur when fitting a large tree, by using the boosting approach and instead learning *slowly*. All the contributions from the model updates might be quite small given the fact that we continuously fit on residuals, but we slowly improve the the current model $\hat{f}$ in areas where it does not perform well. Furthermore,  the multiplicative factor of $\lambda$ shrinks the process even further allowing more and different shaped trees to attack the residuals. 

*Be careful though*: when applying Gradient boosting techniques, the model output is a weighted sum of the outputs of all the trees, not just the final one. 

In this setting, boosting for decision trees involve a large amount of hyperparameters to be chosen: the number of different trees trained, the shrinkage parameter and the number of splits for each tree.
##### Variable Importance measures

For Bagging and Random Forest models, since we no longer have a single tree to interpret, we need an aggregate measure to assess which predictors matter most across the whole ensemble.

- **For regression trees**: for each predictor, we record the total amount by which RSS is decreased due to splits over that predictor, averaged over all $B$ trees.
- **For classification trees**: analogously, we record the total amount by which the Gini index is decreased due to splits over that predictor, averaged over all $B$ trees.

A large value indicates an important predictor. Averaging over many trees (each built on a bootstrapped dataset and, for Random Forest, a random subset of candidate predictors at each split) yields a much more stable and reliable importance measure than what could be obtained from a single tree, and also helps distribute importance more sensibly among correlated predictors.