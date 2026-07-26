We want to validate the model against the sample set we already have, but in order to do that we have to apply some forms of resampling methods. The main methods are *cross-validation* and *bootstrap*.

Recall the distinction between test error and training error: test error is the average error that results from using a model against new observation, which were not in the training set. Training error is the error computed in the training set.

Let's see the below picture:

![[Screenshot 2026-07-26 alle 17.08.00.png]]

As we increase the model complexity, the fitting of the model on the training set is expected to decrease, as the variance of the model increases. Still, as we already discussed, there is a bias-variance tradeoff to take into account, hence as the variance of the model increases, the bias decreases (as the model has more degrees of freedom which can lead to a better fitting on the existing data).

The phenomenon of decreasing of the training error and increase of the test error is deemed *overfitting*. 
##### Validation-Set approach

The validation is performed as follows: we divide the entire dataset into two parts, which are *training set* and *validation set*. The validation set works as a proxy of the *test set*, meaning that the validation set error is used an estimate of the actual *test set error*.

The choice from the dataset is at random (the order is not important, and hence not respected).

The *validation set approach* (or validation against multiple sets) has a two-fold function. The first one is that allows to estimate the actual test error that we should receive (as already explained). The second one is that it might be used to allow the perfect choice of hyperparameters of the model.

![[Screenshot 2026-07-26 alle 17.22.14.png|697]]

The validation-set approach has two specific drawbacks, though:

1. Practically speaking, statistical models tend to perform worse when provided fewer data points. This means that by fitting the model over the training set only (rather than the whole dataset), the model is expected to perform worse than if it had been trained on all available data. This causes the validation error to overestimate the error we'd expect from a model trained on the full dataset.

2. On a theoretical note, the estimate of the test error obtained via the validation error can be highly variable, depending heavily on which specific observations end up in the training set versus the validation set.

Both issues are amplified with smaller datasets: less data for training makes problem (1) worse, and the specific random split has proportionally more influence on the result, making problem (2) worse too.

##### K-fold Cross-validation

*Widely used approach* for estimating test error.

K-fold cross-validation is a powerful technique to get a more unbiased estimate of the test error on new data. The procedure works as follows:

1) The dataset is divided into $K$ different subsets, each one the same size (i.e. number of observations).
2) A random subset $k$ is chosen from the $K$ subsets. This one will be used as validation error, while the other ones as training error.
3) We fit the model using the training subsets, then we compute the validation error on the $k$ subset.  While doing that, we also take into account the subset size: $$\text{MSE}_{k} = \sum_{i \in C_k}(y_i - \hat{y}_i)^{2}/n_k$$ where $C_k$ is the set of all the observations in the $k$-th subset, and $n_k$ is its size.
4) We repeat this process using all the subsets as validation set, one at the time. We then compute for each subset a validation error. 
5) The k-fold CV estimate is computed by averaging these values: $$\text{CV}_K =  \sum_{k=1}^{K} \frac{n_k}{n}\text{MSE}_{k}$$
*LOOCV* might be useful, still typically it might not shake up the data enough. Minuto 6.52