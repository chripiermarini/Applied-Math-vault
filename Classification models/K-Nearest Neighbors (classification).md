
$K$-nearest neighbors can be also easily applied in this context. KNN classifies a point based on the **distance** to other observations (typically Euclidean distance). This means the **scale** of each variable directly affects the classification outcome — variables with larger numeric ranges dominate the distance calculation, regardless of their actual predictive importance.

**Example** (ISLR): with `salary` (USD) and `age` (years), a $1,000 difference in salary numerically dwarfs a 50-year difference in age — even though intuitively the age gap is far more meaningful. If `age` were measured in minutes instead of years, it would suddenly dominate instead. The result changes based on an arbitrary choice of units, which is undesirable.

 The fix is the `StandardScaler` object. It standardizes each variable to mean 0, standard deviation 1:

$$z = \frac{x - \mu}{\sigma}$$

```python
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.neighbors import KNeighborsClassifier

knn_scaled = make_pipeline(
    StandardScaler(),
    KNeighborsClassifier(n_neighbors=1)
)
knn_scaled.fit(X_train, y_train)
```

Using a `Pipeline` ensures scaling is fit only on the training data and consistently applied (via `.transform()`) to the test data — avoiding data leakage, same principle as with `PolynomialFeatures`.

On single/multiple predictors

- **Single predictor**: scaling has **no effect** on KNN results. Standardization is a linear, monotonic transformation — it preserves the relative order of pairwise distances, so the nearest neighbors stay the same regardless of scale.
- **Multiple predictors on different scales** (e.g. `Lag1`, `Lag2`, `Volume`): scaling **can** change results meaningfully, since it rebalances how much each variable contributes to the multidimensional distance calculation.

|Model|Sensitive to scale?|Why|
|---|---|---|
|**KNN**|Yes, strongly|Directly based on distances between points|
|**LDA / QDA**|Somewhat|Relies on covariance, which is scale-dependent|
|**Logistic Regression**|Mostly not for prediction, but matters with regularization|L1/L2 penalties treat all coefficients equally, so scale affects which ones get penalized more|
|**Decision Trees / Random Forest**|No|Splits are threshold-based per variable, not distance-based|
