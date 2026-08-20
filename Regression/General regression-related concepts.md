##### General regression-related concepts

- *Outliers*:
		Outliers are data points for which $y_i$ is far from the value predicted by the model. These values may not be informative, in the sense that they can come from error in the data registration, recording, or single event that do not represent the usual behavior of the phenomenon.

- *Non-constant variance of error terms*:
		It might be possible that prediction errors (or *residuals*) are *heteroskedastic*, which means that the variance of the error is not constant. This might happen when the variance actually increases with the scale of the response itself. There are some refined techniques to apply when this happens (e.g. *weighted least squares*), but one easy trick would be to transform the response Y using the logarithmic or squared function (i.e. $\log(Y) \text{ or } \sqrt{Y}$) (of course, before fitting the data).

- *Difference between the Residual sum of Squared and the Least Squared*:
		Simple: the first one is the function that must be minimized in order to compute the optimal parameters (i.e. $\text{RSS} = \sum_{i=1}^{n} e_{i}^{2}$), while the second one is the actual criterion/method, which requires to minimize the RSS itself.

- *High leverage points*:
		High leverage points are observations whose predictor values (x_i) are unusual or extreme relative to the rest of the dataset, not necessarily unusual in y. This is not an actual problem for the model (as the observation is usually valid, it is just that its $x$ is far from the average), but you just need to be careful of the value as it might cause issues for the numerical side to the least squared method.

- *Collinearity*:
		A phenomenon that appears when multiple predictors have an high correlation one another. This can pose problems in the regression setting as it might be difficult to interpret the effects of each predictor, since they usually increase or decrease together as they are correlated. One simple technique to apply would be to just not use one predictor for the regression itself.