I will write don here some notes regarding the exercises done by myself. The notes will involve the mistakes made using the specific libraries, with which I have to become really good and comfortable working with.

#### Best practice

For each model:

1. Grid/Random search with k-fold CV → ONLY on the training set
2. Take the configuration with the best CV score
3. Refit that configuration on the full training set

At the end, for all models together:  

4. Evaluate each "final" model ONCE on the test set  
5. Compare test set results across models
##### Scikit-learn

Scikit-learn is de facto library for classical machine learning using tabular data. It is extremely useful as it is optimised for machine learning models training, testing and predict, with minimal effort from the user. 

1) The `.predict()` method takes as an input a 2D pandas dataframe. This means that you cannot provide a Pandas Series as an input to this methods, but rather you have to take a 2D dataframe with dimensions *(n,1)*. In order to do so, you have to provide a list to the pandas dataframe containing only one element, the name of the column you want to slice. 
2) Both the train test split functions, the average model functions and the metrics functions are actually inside `scikit-learn`;
3) Careful about the `train_test_split` method: the order is always `x_train, x_test,y_train, y_test`. An important caveat to always remember when dealing with classification is that sample spaces must be equally distributed to make the classification training fruitful (if training does not have enough 1-labeled points, we might not be able to classify at all).
4) If you want to use categorical predictors or variables into our model, it is important to encode them as 0/1 instead of Yes/No. The same does not apply for the response variable, as sklearn automatically converts it to numerical values.
###### Cross validation

Please check how the 'Kfold' object works in Python from *sklearn*.
##### One hot encoding

When using discrete set of values as predictors, it might be useful to encode the discrete features (e.g. names, colors, whatever) in different possible formats. One easy format would be *one hot encoding*, for which we create one column for each value that the categorical feature can have, and then we set either True or False. Easy to use technique, does not create any ordering relation for the models (meaning, if we put 1,2,3,4 for each possibly entry, we might confuse the model into believing there is a similarity between the 2nd and 3rd class), might be expensive if the number of possible entries is too high.
##### Random Forest and Bagging

The Random Forest method takes two important parameters as input: 

1) *max_depth*, which is the maximum depth (number of splits) for each tree that is trained;
2) *n_estimators*, which is the number of different trees that is trained/compose the forest;
3) *learning rate*, which is the shrinking parameter.
4) *bootstrap*, which is True as default.

--- 
##### Simulation

When performing Discrete Event Simulation, the sequence of event that must be performed is the following.

First, we need to understand what is the probability distribution that best describes the underlying process. This can be done by the means of multiple tools: 
	- *Kolmogorov-Smirnov tests*, which are used to compare the empirical cumulative distribution function with the theoretical distribution function that we are assuming to be true. It computes the vertical distance between the two distributions.
	- *Q-Q plot*: quantile-quantile plots, where we plot the observed quantile we obtained with the ones we are expecting, drawn from the same distribution. If they are coming from the same distribution, it means that our hypothesis is correct, we should see all the blue points along the diagonal of the positive square.
	- *Testing the properties of specific distributions*: i.e. hazard rate per memorylessness of the exponential distribution.

***Important note***: in practice, we would never find a first good distribution on the first try. Usually, we either have to segment the distribution based on specific features (peak/off-peak, hourly level, origin, etc.).

Another good practice would be to apply fitting on multiple distributions at the same time, while also using AIC/BIC based comparisons. Generally speaking, we both want ot use the AIC value (the smaller, the better, as it gives more importance to simpler models) and the KS p-value. The smaller the p-value, the higher the evidence against the null hypothesis that the empirical distribution follows the theoretical distribution.

TODO: small refresher on Gamma, Exponential, Weibull and Normal/Gaussian distribution.

#### SimPy

Technical notes on the fuse of SimPy simulation framework.

*Time simulation*: SimPy has no built-in unit of time. The simulation clock (`env.now`) is just a float that advances — what it represents (minutes, seconds, hours) is purely a convention chosen by the modeler through the scale of the parameters.

In this project, minutes were implicitly chosen as the unit (`mean_interarrival=2.0`, `sim_duration=480.0` for an 8-hour shift). Changing units later requires rescaling _every_ time-related parameter consistently (interarrival rates, transit time parameters, rush-hour windows, simulation duration) — SimPy itself enforces nothing here.

 ***`env.process()` vs `env.run()`***

- **`env.process(generator)`** _registers_ a generator function as an active process in the environment. Nothing inside it executes yet — it's scheduled, not run.
- **`env.run(until=...)`** is the engine that actually advances the clock, waking up registered processes in the correct chronological order until the stop condition is reached.

A single `env.run()` call can trigger execution of many processes registered via multiple `env.process()` calls — including processes that register _further_ processes dynamically during the run (e.g. an origin generator spawning a new package process every time a package arrives).

***Parallel processes starting at different times***

Calling `env.process()` does not block — it registers the process and immediately returns control. To start a second process later than a first one, simply let simulated time pass (`yield env.timeout(...)`) before registering it.

python

```python
def process_a(env):
    print(f"A starts at t={env.now}")
    yield env.timeout(5)
    print(f"A ends at t={env.now}")

def process_b(env):
    print(f"B starts at t={env.now}")
    yield env.timeout(3)
    print(f"B ends at t={env.now}")

def scheduler(env):
    env.process(process_a(env))   # starts immediately, t=0
    yield env.timeout(2)           # wait 2 time units
    env.process(process_b(env))   # starts at t=2, while A is still running

env = simpy.Environment()
env.process(scheduler(env))
env.run(until=10)
```

Output:

```
A starts at t=0
B starts at t=2
B ends at t=5
A ends at t=5
```

The two processes coexist in simulated time; SimPy interleaves them automatically based on when each `yield` wakes up. This is exactly the mechanism behind an origin generator spawning a new package process on every arrival — each package lives its lifecycle in parallel with all previously spawned packages, none waiting on the others.

***Fork-join: waiting for multiple sub-processes to complete***

`env.process()` returns a `Process` object, which can itself be `yield`-ed — yielding on a process waits exactly until that process finishes.

python

```python
def sub_process_1(env):
    yield env.timeout(4)
    print(f"Sub-process 1 done at t={env.now}")

def sub_process_2(env):
    yield env.timeout(7)
    print(f"Sub-process 2 done at t={env.now}")

def parent_process(env):
    print(f"Parent starts at t={env.now}")
    
    p1 = env.process(sub_process_1(env))
    p2 = env.process(sub_process_2(env))
    
    yield p1
    yield p2
    
    print(f"Parent resumes, both children done, t={env.now}")

env = simpy.Environment()
env.process(parent_process(env))
env.run(until=20)
```

Output:

```
Parent starts at t=0
Sub-process 1 done at t=4
Sub-process 2 done at t=7
Parent resumes, both children done, t=7
```

The parent resumes at `t=7` (the maximum of the two), not `t=4` — because it waits for both, sequentially.

***Cleaner variant: `env.all_of([...])`***

For more than two sub-processes, or when order of completion doesn't matter:

python

```python
def parent_process(env):
    p1 = env.process(sub_process_1(env))
    p2 = env.process(sub_process_2(env))
    
    yield env.all_of([p1, p2])   # waits for ALL, regardless of completion order
    print(f"All done, t={env.now}")
```

`env.all_of([...])` is equivalent to yielding on each process in sequence, but more readable with many sub-processes and doesn't impose an order — if `sub_process_2` finished before `sub_process_1`, it would still work correctly.

SimPy also provides `env.any_of([...])` for the opposite case: proceed as soon as **at least one** sub-process finishes (a "race condition" pattern — e.g. the first of several alternative routes to reach a destination wins).

##### Real-time inference feature

The ML model needs to be embedded into a stream of events, and hence
consumed in real time. To support this, we build a system that ingests
events as they occur, one at a time — not in batches.

During training, feature engineering operates over the entire historical
dataset at once (batch processing) — this is how the current pipeline
works, and remains appropriate offline.

For real-time inference, the flow is different: as soon as a single
`departed_from_origin` event occurs, the required features (origin,
destination, hour of day, is_rush) are extracted directly from that one
event — no batching, no waiting for other events, no need to look at the
package's full history. That single event is immediately passed to the
already-loaded model's `predict` method, producing a delay-risk prediction
for that specific package right away.

##### Notes for system design

1) Functional and non-functional requisites
2) Diagram of components
3) Key decision and why (Architecture Decision Records)
4) Explicit trade offs.