In the following section, we are going back to regression models, and we are going to address some potential issues that might arise when fitting the model into *badly-posed datasets* (i.e. datasets where the set of predictors/features is much higher than the number of dataset points). 

The aim is always to balance the bias-variance trade-off (introduced in [[Foundations of Statistical learning]], which arises when trying to fit a model to a dataset.

The following techniques might be applied to optimally select and tune models in order minimize the bias-variance tradeoff as much as possible. These will lead to better prediction accuracy first, and also better model interpretability later (as we can reduce the number of predictor used, and hence the assess the effect of each of the remaining ones more precisely).
##### Best subset selection

The best subset selection technique refers to a structured way to select the best optimal subset of predictors in: 

1) We start from the *null model* which has zero predictors. This model has a given performance, that might be computed through $MSE/RSS$, adjusted $R^{2}$, or possibly AIC or BIC. We point such model as the 'best model' with zero predictors ($\mathbb{M}_0$) 
2) For each $k = 1, \dots, p$: fit **all** $\binom{p}{k}$ possible models containing exactly $k$ predictors. Among these $\binom{p}{k}$ models, select the one with the lowest RSS (or highest $R^2$) as $\mathbb{M}_k$ — comparing purely on training error is valid here, since all candidate models have the same number of predictors. 
3) Having obtained $\mathbb{M}_0, \mathbb{M}_1, \dots, \mathbb{M}_p$ (one best model per subset size), select a single final model among them using a metric that accounts for model complexity — cross-validation error, AIC, BIC, or Adjusted $R^2$ — never raw RSS/$R^2$, since these always favor larger models regardless of true predictive value. 

The total number of models fit across the entire procedure is $2^p$ (every possible subset of predictors), which becomes computationally infeasible for large p. Furthermore, looking at too many models could also lead to *overfitting*. This is the key motivation for the cheaper, approximate alternative: forward (and backward) stepwise selection, which builds models incrementally rather than exhaustively.

##### Forward Stepwise Selection

Similar to best subset selection, we start from the null model $\mathbb{M}_0$. At each step, we consider adding **one** predictor to the current model — trying each of the remaining candidates — and select the one that yields the best improvement in training performance (RSS/$R^2$, valid here since all candidates being compared have the same number of predictors). This gives $\mathbb{M}_1$. We then repeat, always adding one predictor to the current best model, until all p predictors have been included, giving us $\mathbb{M}_0, \mathbb{M}_1, \dots, \mathbb{M}_p$. As with best subset selection, we then select the single best model among these using cross-validation, AIC, BIC, or Adjusted $R^2$ — not raw RSS/$R^2$.

##### Backward Stepwise Selection

This works in the opposite direction: it starts from the full model  containing all p predictors, and at each step removes the single  predictor whose removal least hurts (or most improves) the model's training performance, progressively producing models with fewer and fewer predictors. Backward selection requires $n > p$, since it needs to be able to fit the full model with all predictors first — unlike forward selection, which can be used even when $p > n$.


Best subset selection **exhaustively** tries every possible combination of predictors ($2^p$ models total), guaranteeing that the best model of each size is found. Stepwise methods (forward or backward), by contrast, apply a **greedy** algorithm: at each step, they commit to the best local choice given the model built so far, without reconsidering it later. This drastically reduces the number of models fit (only $1 + p(p+1)/2$ in total), but comes at a cost: the final model at each step depends on the specific path taken (which predictors were added/removed first), and stepwise methods are **not guaranteed** to find the true best subset of a given size — unlike best subset selection.

##### Estimating test error using two specific approaches

The reason we cannot use the training error as an estimate of the test error is that model parameters are estimated specifically to minimize the error on the training data itself — this makes the training error systematically optimistic, regardless of the model's flexibility, since fitting well on the data used for estimation does not guarantee the model has learned the true underlying pattern of the phenomenon, rather than the noise specific to that sample. 

This problem is compounded by the bias-variance trade-off: as we increase model flexibility to reduce bias, variance increases in turn, and the model increasingly overfits to the training data's noise. This widens the gap between training error (which keeps decreasing with more flexibility) and true test error (which eventually increases past some optimal flexibility level). 

To obtain a reliable estimate of test error, we have two broad options: 

1. **Adjust the training error mathematically**, using metrics like $C_p$, AIC, BIC, or Adjusted $R^2$, which add a penalty term accounting for model complexity — computationally cheap, since no refitting is required, but based on theoretical (often asymptotic) approximations. 
2. **Estimate the test error directly**, using the validation set approach or cross-validation — more computationally expensive, but makes fewer theoretical assumptions.

Aggiungi dettagli su metriche di calcolo, piú la principale limitazione che risiede nel calcolo di alcuni parametri (tipo $d$, $\sigma$ etc.).
Video 6.6


