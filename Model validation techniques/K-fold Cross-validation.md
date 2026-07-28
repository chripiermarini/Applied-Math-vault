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