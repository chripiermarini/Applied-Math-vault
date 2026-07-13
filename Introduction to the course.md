*Statistical learning* is considered the mathematical foundation of machine learning, the computer science field that tries to **analyze data in order to understand the underlying process and make optimal prediction/estimation**.

On a practical note, the goal is always to **build systems that automatically solve a specific task**.
Recognising spam emails, recognising handwritten data, etc.

##### Notation and definitions
In both the course and the book, a lot of linear algebra and matrix will be used. Just to make sure, variables will usually be 2-dimensional. *Independent features/variables/predictors* $x_{i,j}$ will be associated with the $i$-th observation and $j$-th feature. Matrix then will have the following notation:

$$A = \begin{pmatrix}
x_{1,1} & x_{1,2} \\
x_{2,1} & x_{2,2}
\end{pmatrix}$$
The model will then provide the so-called *response* or *dependent variable*.

There exists multiple Learning frameworks. In the first instance, we will focus on ***Supervised** **Learning***. *Supervised learning* is the stat learning framework in which we train our model to understand an underlying phenomenon to provide the correct response given a new set of input data. In the training phase, each input data has a specific label, which represents the actual realisation of the phenomenon given the input settings (e.g. when predicting wage, we already know the age, degree and gender of the person). 

Another relevant framework is the ***unsupervised learning***. In this settings, training data do not have a specific label associated to them. The task is to group/organize the data given their features, in order to understand if there is an underlying structure such that data is recognized to behave similarly. Unsupervised learning can be much difficult than supervised learning, but might act as a good pre-process step for supervised learning.

The **philosophy** should always be understanding the ideas behind each of the various techniques, in order to choose the best technique to the best situation. It is also critical to understand which are the optimal metrics to judge and evaluate with fairness the different models to choose the best for that specific task.
##### What is the difference between Machine Learning and Statistical Learning?

There is much overlap, as both fields focus on *learning* problems, e.g. supervised learning, unsupervised, self-supervised, etc.

Still there are two main differences:

1) **Machine Learning** focuses more on *large scale* instances of problems and *prediction accuracy*;
2) **Statistical Learning** focuses primarily on *models*, *their interpretability and uncertainty.*
