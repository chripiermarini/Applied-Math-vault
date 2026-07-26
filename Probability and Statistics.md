
Useful concepts from statistics:

Relevant for [[Classification]] tasks:

1) *Prior and posterior probabilities*: these are important concepts since they are related to the internal theory of classification models. Let's assume we want to draw some conclusions to $y$ given an evidence $x$. Let's then assume that there exists a relationship between $x$ and $y$ (i.e. they are not independent). We can then use the *Bayes' theorem*: $$P(y \mid x) = \frac{P(x \mid y) \, P(y)}{P(x)}$$
	Let's define one by one all the elements of the above probability equation. The first element is the evidence $P(x)$, while the *prior* probability is $P(y)$. The name comes from the fact that it represents the probability of $y$ before observing the evidence $x$. We then have the *likelihood* $p(y \mid x)$ , which represents the relationship between the evidence $x$ and the value we want to compute $y$. Finally, we have the *posterior* probability $p(y \mid x)$, which is the probability of the event y after the evidence $x$.

2) *Normality of the conditional distributions*:  for each class, check whether the predictor's distribution (visually and/or statistically) resembles a Gaussian. 
    
	A low p-value (e.g. < 0.05) means you reject the null — statistically, the data isn't normal. With large sample sizes, visual inspection (histogram + KDE) is often more informative in practice than the p-value alone, since these tests become hypersensitive to negligible deviations.

| Test | Null hypothesis | Notes |
|---|---|---|
| Shapiro-Wilk | Data comes from a normal distribution | Generally the most powerful normality test overall, but can be overly sensitive with large n (even trivial deviations get flagged) |
| Jarque-Bera | Data has normal skewness (0) and kurtosis (0, excess) | Explicitly decomposes the deviation into skewness and kurtosis — useful for understanding how the distribution deviates, not just whether it does |
| Anderson-Darling | Data comes from a normal distribution | Gives more weight to the tails than Shapiro-Wilk or K-S |

3) *Equal variance across classes (homoschedasticity*: this is the assumption that specifically distinguishes LDA (shared covariance) from QDA (class-specific covariance). It is **not** the same thing as comparing kurtosis across classes — kurtosis (tail heaviness relative to variance) and variance (spread) are distinct properties. The correct test here is:

|Test|Null hypothesis|Notes|
|---|---|---|
|**Levene's test**|All groups have equal variance|Robust alternative to Bartlett's test|
|**Brown-Forsythe** (Levene with `center='median'`)|All groups have equal variance|More robust to outliers/heavy tails — preferable when normality is in doubt|
|**Bartlett's test**|All groups have equal variance|Very sensitive to non-normality; only reliable if data is close to Gaussian|
  