##### Hyperparameter tuning

As usual, we rely on cross validation to optimally select the tuning parameter among a set of discrete values. Basically, what we do is define a discrete set of tuning parameters, and for each one we apply a cross validation procedure and compute the CV error (as the average of the errors for each K-fold). We then select the tuning parameter with the smallest CV error.

To perform hyperparameter tuning, there exist multiple libraries who work fine:

1. [Optuna](https://optuna.readthedocs.io/en/stable/)
2. [Hyperopt](https://github.com/hyperopt/hyperopt)

That said, generally speaking Bayesian Optimization is the optimization framework who works best for this type of task.