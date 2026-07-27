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
*LOOCV* is a nice special case, meaning that for least-squares or polynomial regression, we have a closed form where we have $$CV_{(n)} = \frac{1}{n}\sum_{i=1}^{n}\left(\frac{y_i - \hat{y}_i}{1 - h_i}\right)^2$$
##### Cross-validation for classification problems

Usually equal, with the main difference being having the $MSE_i$ substituted by the $\text{Err}_k$ , where $\text{Err}_k = \sum_{i\in C_k}I(y_i \neq y_i)/n_k$ . The estimated standard deviation of $CV_k$ is similar to the previous formula (using of course the classification error).

Cross-validation is a form of procedure which can be mistakenly applied in multiple situations. Let's picture this situation:

1) We have 5000 different predictors for only 50 sample. We want to develop a classification model which works, hence we need to select the predictors which have the highest correlation with the class labels.
2) To apply correctly cross validation, we need to separate the validation set *before* selecting the best predictors. If the choice relies on correlation, we first select the subset of predictors from training set using correlation, and only after we have selected such predictors we filter the unwanted ones from the validation set itself.

The point is to avoid to select the predictors using also the validation set. This would lead to unfair high performance of the model when computing CV error.

##### Bootstrap method

Bootstrap is a powerful tool used to quantify the uncertainty associated with  a given estimator or statistical learning method, without requiring any distributional assumption on the underlying data.

The idea is to generate $B$ bootstrap datasets by sampling **with replacement** from the original training dataset, each of the same size $n$ as the original.  For each bootstrap dataset, we recompute the estimate of the parameter of  interest, obtaining $\hat\alpha^{*1}, \hat\alpha^{*2}, \dots, \hat\alpha^{*B}$.

We then compute the standard error of the original estimate $\hat\alpha$ using the sample standard deviation of these B bootstrap estimates:

$$SE_B(\hat\alpha) = \sqrt{\frac{1}{B-1} \sum_{r=1}^{B} \left(\hat\alpha^{*r} - \overline{\hat\alpha^*}\right)^2}$$

where $\overline{\hat\alpha^*} = \frac{1}{B}\sum_{r=1}^{B} \hat\alpha^{*r}$ is the mean of the bootstrap estimates.

Bootstrap method is not always applicable as described: it might be possible that some instances do not respect basic assumptions of the method (e.g. *time series*, where there is an important correlation between data points, and the order is actually important). In those instances, some corrections might be applied (like performing *block bootstrap sampling*, extracting 'slices' of the time series).

Another important application of bootstrap method would be to generate a *Bootstrap Percentile confidence intervals*, using the percentiles generated by the bootstrap estimates to compute a valid confidence interval with given confidence (e.g. taking the 5th and 95th percentile to compute the extremes of the 90% confidence interval)

Although bootstrap method is quite useful, it should not be used to compute estimate of the prediction error, and the reason for this is due to the *overlapping* between the training set (the bootstrap dataset) and the validation set (the original data). The probability of having an overlap between dataset is usually about two third, and the conditions to ensure non-overlapping between datasets are actually hard to satisfy. Hence, it is not prolific to use it in that instance.