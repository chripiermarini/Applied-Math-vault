##### Confusion matrix and ROC curve

Whatever it is the algorithm that we apply for classification, we still need to assess the performances of the model. In order to do so, we might want to use some specific techniques.

1) *Accuracy*: straightforward proportion of the number of cases which have been correctly classified. 

2) *Confusion matrix*: a tabular display of 2x2 dimensions in the binary case, where we display the record counts by their predicted and actual classification status. In simpler terms, we state how many true positives and negatives the model properly exposed, as well as the false positives (predicted positives but actually negatives) and false negatives we have. From this table, other metrics can be then displayed:

	![[Screenshot 2026-07-23 alle 11.03.44.png|626]]

	In this table, other measurements are also displayed, which can be of variable importance. Specifically, it might be very important to correctly specify which data belongs to a 'rare class' (i.e. a class in which we a smaller amount of samples w.r.t the others).
	
3) *Precision, Recall/Sensitivity and Specificity*: those are three specific metrics, which are complementary to each other, yet providing different perspectives of the model performances. 
	
	Precision is computed as $$\frac{TP}{TP + FP}$$ and it measures the *accuracy* of predicting a positive (capacity of filtering out what is not positive).
	
	Recall is computed as $$\frac{TP}{TP + FN}$$ and it measures the strength of predicting a positive (capacity of correctly recognizing what is positive out of noise).
	
	Finally, *specificity* is computed as $$\frac{TN}{TN + FP}$$ and it measures the accuracy of predicting a *negative* (capacity of correctly recognizing what is negative out of noise).
	
##### ROC curve

*ROC curve*: *Receiving Operating Characteristics* curve, it plots in 2D graph the curve which represent of how much specificity of the model we sacrifice in order to have an improvement in the recall/sensitivity. We would like to have curve which is as high as possible. 
 
 In order to build such curve, the procedure is as follows: 
 
1) First, we fit the training data in the classifier, and then use our model on test data, which will provide us with the expected probability for each point to belong to class 1. 
   
2) Then, we sort the data points based on such probabilities (from 0 to 1). 
   
3) We select a threshold value, $t$, that will increase from 0 to 1. All the data points which probability is higher than $t$ are labeled positives, negative otherwise. 
   
4) We compute the Recall and the Specificity for that particular t. This becomes a point in the graph.
   
5) We finally repeat such procedure for all values from $t=0$ to $t=1$. 

Finally, we can also summarize the visualization results into a practical statistic, which is the *AUC* or Area Under the Curve, as the integral of the curve between 0 and 1. 