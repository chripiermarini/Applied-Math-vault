In the following section, we are going back to regression models, and we are going to address some potential issues that might arise when fitting the model into *badly-posed datasets* (i.e. datasets where the set of predictors/features is much higher than the number of dataset points). 

The aim is always to balance the bias-variance trade-off (introduced in [[Foundations of Statistical learning]], which arises when trying to fit a model to a dataset.

The following techniques might be applied to optimally select and tune models in order minimize the bias-variance tradeoff as much as possible. These will lead to better prediction accuracy first, and also better model interpretability later (as we can reduce the number of predictor used, and hence the assess the effect of each of the remaining ones more precisely).

