This is a personal folder containing all my personal notes regarding Machine Learning, following religiously the [Introduction to Statistical Learning course](https://www.youtube.com/watch?v=LvySJGj-88U&list=PLoROMvodv4rPP6braWoRt5UCXYZ71GZIQ) and book from Hastie/Tibshirani.

In the following, I will put some hyperlinks referring to the main chapters of the book.

1) [[Introduction to the course]]
2) [[Foundations of Statistical learning]]
3) [[Regression]]
4) [[Classification]]
5) [[Model validation]]
6) [[Model selection]]
7) [[Tree-based methods]]
8) [[Reinforcement Learning overview]]
9) [[Finite Markov Decision processes]]

Another important part of learning is practice what you are studying. Please also use the [Hands-on Machine learning book](http://14.139.161.31/OddSem-0822-1122/Hands-On_Machine_Learning_with_Scikit-Learn-Keras-and-TensorFlow-2nd-Edition-Aurelien-Geron.pdf). The following technical exercises are available as well:

1. [[Technical notes from exercises]] 
2. [[Technical notes from unsupervised learning]]
##### Update of Aug 5th

***Completed***

- Built the first simulation engine producing data from a two-origin, two-destination network
- Data validated: statistical tests confirm correct distributional properties (exponential interarrivals, lognormal transit times, rush-hour and lane heterogeneity)
- Built an exploratory ML notebook: feature engineering, model comparison (Naive Bayes, Logistic Regression, SVM, XGBoost) with proper train/test split, CV-based hyperparameter tuning, and honest test-set evaluation

***To review***

1. Cross-entropy loss for classification models — including its derivation from maximum likelihood, and the distinction between binary and multiclass (softmax) cross-entropy
2. Model interpretation — feature importance, plus SHAP values for per-prediction (not just aggregate) interpretability
3. Interpretation of classification results — precision/recall/ROC AUC/log-loss trade-offs, plus probability calibration (does a predicted 70% actually correspond to a true 70% frequency?)

***Next steps***

- Refactor the notebook into a proper, reusable ML pipeline: move code into modules under `src/digital_twin/models/`, with testable functions — not just automating the same notebook cells in sequence
- Add model persistence (e.g. `joblib` or XGBoost's native format), since training and inference will no longer happen in the same process/notebook once real-time prediction is introduced
- Switch to the "fake digital twin" setting: implement a model that predicts delays in real time, connecting naturally to the streaming layer (Kafka/Faust) planned for later — real-time prediction requires the model to be exposed as a callable service within the event flow, not run from a static notebook

---

In terms of priority :

1. Digital twin: working, principles, frameworks. Start building a prototype, using data from Kaggle, as an example. Build two specific models, one focusing on ML models studied in the course, and the other using multi-objective optimization.
2. Educate on coding best practices (Refactoring, Design Patterns -  Strategy Pattern, factory pattern, Observer patter, Adapter pattern, Decorator e Singleton).
3. System Design Capitoli 5-6 e 8 (if possible).
