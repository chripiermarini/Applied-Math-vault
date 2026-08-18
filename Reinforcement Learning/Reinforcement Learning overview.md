Reinforcement learning is a mathematical and machine learning-related framework where the aim is to compute the *optimal sequence of actions in order to maximise a numerical reward signal*. The hardest challenge related to reinforcement learning lies is the idea of computing the action that do not only maximises the current expected return, but also the return gained from future actions as well (i.e. we do not want to maximise the current the return, but the total return from all actions). To summarize, w want to compute the optimal tradeoff between *exploration* and *exploitation*. 

The standard setting for a reinforcement learning framework is the use of a *goal seeking agent* which moves within a (possibly infinite) *environment*. The agents have explicit *goals*, can sense aspects of the environment and can choose actions to influence their environments. All the interactions of the agent are supposed to be aiming at achieving a certain goal despite the *uncertainty* of the environment. 

#### Elements of Reinforcement Learning

Beyond the solely agent and the environment, it is possible to identify four main subelements of a reinforcement learning system:

1) A **policy**, which is a set of rules defining how an agent behaves at a given time, within the environment. A policy is just a mapping from perceived states of the environment to actions to be taken when in those states. It formalizes the *stimulus-response* mechanism of the agent.
2) A **reward signal**, which defines the goal of the reinforcement learning problem. Every time the agent performs a step, it receives a reward from the environment as a form of single number. The goal of the agent is to maximise the total reward received.
3) A **value function**, which represents the expected total reward from the agent given the current state of the agent itself. In other words, if the reward represents the immediate gain from an action, the value function represents an estimate of the total future gain the agent can forecast after taking that action (so it is more the *long term* reward expectation).
4) A *model of the environment*, which is the representation of the environment within the RL setting. It allows to infer how the model will behave after receiving an action from the agent.

#### Main limitations of Reinforcement Learning

Critically important is to define the concept of *state*, as a summary of the overall amount of information that the agent has w.r.t the environment settings at a given time. It represents the input to the reward function and the value function (being the short term and long term reward signal that the agent takes).

Most of the reinforcement methods of the book involve the estimation of the value function, even though not all of the state of the art algorithms require to do so. Evolutionary methods, such as genetic algorithms, genetic programming, simulated annealing, apply multiple static policies each interacting over and extended period of time with a separate instance of the environment. The policies that obtain the most reward are carried over to the next generation of policies (as well as random variations of them), and the process repeats. Those work well when the search space of the policies is narrow and we do not have information of the environment.