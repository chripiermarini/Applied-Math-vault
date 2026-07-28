
The LOOCV method is a specific validation technique in which we select  one point from the dataset and exclude it from training. The model is  trained on the remaining n-1 points, and we compute the validation  error (MSE) using the excluded point. We repeat this for every point 
in the dataset, and average the n resulting validation errors.

This method has very low bias, since each of the n models is trained  on almost the entire dataset (n-1 points). However, it suffers from  high variance in the final estimate: because the n training sets used  across iterations are nearly identical (differing by just one point  each), the resulting MSE values are highly correlated with one another — and averaging highly correlated quantities doesn't reduce variance as effectively as averaging independent ones would. On top 
of this, the computational cost can be extremely high, due to the large number of iterations (n) required.

Given these limitations, k-Fold cross-validation offers a helpful generalization: instead of fixing the number of iterations to n (one per data point), it uses k folds of multiple points each as 
validation sets, with k chosen freely (typically 5 or 10). LOOCV is, in fact, simply the special case of k-Fold CV where k = n.