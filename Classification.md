Classification tasks arise when we want to build a model to compute/predict a response that can come from a finite, discrete set (e.g. "red", "blue", "green" - which can be mapped to 1,2 or 3 for example). 

In this instance, as seen in the [[Introduction to the course]], we would want to build a model that, given a out-of-sample value, provides the probabilities of that new sample to fall into each of the class. Rationally, we would choose the class with the highest probability associated. 
 ![[Screenshot 2026-07-22 alle 16.28.21.png]]

In this instance, linear regression could be not useful, as the linear model can provide a negative value as response. Also, linear regression does not work properly even for multi-class classification (i.e. more than two possible responses). For these reasons, we actually use the so-called *Logistic regression*.

The models that are practically used in classification are the following:

1. [[Logistic regression]]
2. [[Linear discriminant analysis]]
3. [[Quadratic Discriminant Analysis]]
4. [[Naive Bayes]]
5. [[K-Nearest Neighbors (classification)]]

In order to assess whether a classification model is working properly or not, we have multiple tools at disposal. Specifically, notes on [[Performances of classification models]] are available.

---
#####  Study notes:

1) Comprehension is fine: classification as a task results much more difficult, as it is based on probabilities and distribution of which my technical skills remain a little bit weaker than the rest. A prob/stat refresher is highly recommended.
2) Please also review again all the formulas of the above models. Everything needs to be clear.
