Tree-based methods are common machine learning methods that can be used for both classification and regression. The main idea is segment the predictor space into multiple smaller subspaces based on rules. Given this separation, we then provide a predict from the model using the mode or average of the observations in the subspace the new sample falls into. The models are 'tree-based' since the splitting rules are actually applied in a sequential order. They are also called *decision trees*.

They might small and easily interpretable. Main con is that they might not be competitive with the best supervised learning.
###### Practical example to understand stratification

By observing the data, we might see that we can also divide the predictor space based on values of the predictor themselves.


![[Screenshot 2026-07-31 alle 15.46.44.png|478]]

*How can we compute these limits*? Tree-based algorithms are used for that! The main point is that these values are actually well-interpretable.

##### Foundations of tree-based models

*As a useful terminology*, we have **terminal nodes** being the actual regions in which we divide the predictor space into. The **internal nodes** are the non-terminal nodes.

*How do these work*? We divide the predictor space into $J$ distinct and non-overlapping regions, being $R_1, R_2 \dots, R_j$. We might also consider those in terms of *boxes*, meaning that the predictor region can be also divided into high-dimensional rectangles. The goal is to find the boxes that minimise the RSS, given by the formula $$\sum_{j=1}^{J}\sum_{i \in R_j}\left(y_i -\hat{y}_{R_j}\right)^{2},$$ where the second term is the mean response for the training observation with the $j$th box. Unfortunately, this calculation is computationally intractable, hence, we usually take a *top-down, greedy* approach that is known to be recursive binary splitting.

Being top down, it starts from the root of the nose and then successively splits the predictor space, being greedy it always select the best option for the current state, not the future one.

VIDEO 8.2
