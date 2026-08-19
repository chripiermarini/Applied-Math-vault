#### Best practice

For each model:

1. Grid/Random search with k-fold CV → ONLY on the training set
2. Take the configuration with the best CV score
3. Refit that configuration on the full training set

At the end, for all models together:  

4. Evaluate each "final" model ONCE on the test set  
5. Compare test set results across models
##### Scikit-learn

Scikit-learn is de facto library for classical machine learning using tabular data. It is extremely useful as it is optimised for machine learning models training, testing and predict, with minimal effort from the user. 

1) The `.predict()` method takes as an input a 2D pandas dataframe. This means that you cannot provide a Pandas Series as an input to this methods, but rather you have to take a 2D dataframe with dimensions *(n,1)*. In order to do so, you have to provide a list to the pandas dataframe containing only one element, the name of the column you want to slice. 
2) Both the train test split functions, the average model functions and the metrics functions are actually inside `scikit-learn`;
3) Careful about the `train_test_split` method: the order is always `x_train, x_test,y_train, y_test`. An important caveat to always remember when dealing with classification is that sample spaces must be equally distributed to make the classification training fruitful (if training does not have enough 1-labeled points, we might not be able to classify at all).
4) If you want to use categorical predictors or variables into our model, it is important to encode them as 0/1 instead of Yes/No. The same does not apply for the response variable, as sklearn automatically converts it to numerical values.
###### Cross validation

Please check how the 'Kfold' object works in Python from *sklearn*.
##### One hot encoding

When using discrete set of values as predictors, it might be useful to encode the discrete features (e.g. names, colors, whatever) in different possible formats. One easy format would be *one hot encoding*, for which we create one column for each value that the categorical feature can have, and then we set either True or False. Easy to use technique, does not create any ordering relation for the models (meaning, if we put 1,2,3,4 for each possibly entry, we might confuse the model into believing there is a similarity between the 2nd and 3rd class), might be expensive if the number of possible entries is too high.
##### Random Forest and Bagging

The Random Forest method takes two important parameters as input: 

1) *max_depth*, which is the maximum depth (number of splits) for each tree that is trained;
2) *n_estimators*, which is the number of different trees that is trained/compose the forest;
3) *learning rate*, which is the shrinking parameter.
4) *bootstrap*, which is True as default.
