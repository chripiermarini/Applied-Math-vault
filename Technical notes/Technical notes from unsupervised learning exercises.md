For unsupervised learning, we have focused on 2 models: KMeans and Gaussian Mixture Model.

Notes on the GMMs have been made on paper, focusing on the main idea (presence of subpopulations, governed by distinct gaussian distributions of which the GMM is a convex combination). 

A few points to note:

For ***KMeans***:

1) On *KMeans*, the objective is to tie each point to the its closest centroid. In this case, we are not able to compute likelihood, but rather the inertia formula (sum of euclidean distances between each point and its assigned centroid).
2) Hyperparameter tuning for *KMeans* algorithm regards specifically the desired number of centroids (hence number of regions). We select the number that minimizes the inertia, as discussed above.
3) Regions are then expected to be defined with straight lines.
4) Finally, KMeans can be negatively affected by bad initialization of the centroid. One thing that is by default implemented in the library is the number of different initializations to 10.

For ***Gaussian Mixture Models***:

1) In this case as well, the number of assumed distribution is an hyperparameter that needs to be tuned.
2) In this case, we are not using neither the inertia nor the *silhouette*, but we can use directly the loglikelihood (rather, the negative loglikelihood since the argument of the logarithm is usually less than 0).
3) Regions in this case are not straight defined, but rather they have a specific ellipsoid, with mean as the center and the eigenvectors of the covariance being the axes.