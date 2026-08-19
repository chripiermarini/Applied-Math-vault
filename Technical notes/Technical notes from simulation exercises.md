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
