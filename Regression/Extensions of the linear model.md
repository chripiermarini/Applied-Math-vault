##### Extensions of the linear model

It might be possible that two predictors might ***interact*** with each other, meaning that there might be effects on the variations of one predictor to another predictor. It is also called *synergy*.

How do we manage the two coefficients? We multiply them together:

$$
y = \beta_{0} + \beta_{1}x_1 + \beta_{2}x_2 + \beta_{3}(x_1 \text{ x } x_2)
$$

That means that multiple variables combined together have an interacting effect. As an example:

![[Screenshot 2026-07-22 alle 10.59.06.png]]

The interaction effect might be extremely powerful, and we also need to mention the *hierarchy* effect: if the p-value of an interactions is extremely low (hence, its effect is significant), we still keep the coefficients of the two values interacting, even if their p-value is high on paper.  

Another important extension of the linear regression are the models with ***non-linear effects***. It might be possible that the models might be polynomials to understand the different degrees of freedom of the model. To do that, we just introduce new dummy variables:

$$
y = \beta_{0} + \beta_{1}x_1 + \beta_{2}x_2^{2}
$$
In this case, the model itself has a polynomial structure, but still the model is called linear as it is linear in the coefficients. 