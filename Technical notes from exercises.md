I will write don here some notes regarding the exercises done by myself. The notes will involve the mistakes made using the specific libraries, with which I have to become really good and comfortable working with.

##### Scikit-learn

Scikit-learn is de facto library for classical machine learning using tabular data. It is extremely useful as it is optimised for machine learning models training, testing and predict, with minimal effort from the user. 

1) The `.predict()` method takes as an input a 2D pandas dataframe. This means that you cannot provide a Pandas Series as an input to this methods, but rather you have to take a 2D dataframe with dimensions *(n,1)*. In order to do so, you have to provide a list to the pandas dataframe containing only one element, the name of the column you want to slice. 
2) Both the train test split functions, the average model functions and the metrics functions are actually inside `scikit-learn`;
3