Finite Markov Decision Processes (from now one, *MDPs*), represent the mathematical formalization of the reinforcement learning problem, and for sequential decision making problems in general. Actions do not just influence immediate rewards, but also subsequent situations. 

We could also begin from the multi-armed bandit problems, which simplify the reinforcement learning problem by not including the *state* information within the formula. In a multi-armed bandit scenario, we only take actions by moving the arms, without including any state information (it is *stateless* per say). We skip the formulation of the problem for now, moving there in the future.

##### The agent-environment framework

Markov decision processes are a straightforward framework of the problem of learning by reaching a goal. The learner and decision maker is called *agent*, and everything it interacts with is called *environment*. They interact continuously, since the agent interacts with the environment, and the environment produces a reward to the agent. From an ontological point of view, we should always consider anything that it is outside of control from the agent as part of the environment. As an example, for a human agent we should always consider the skeleton, heart rate, and other features outside of his arbitrary control as part of the environment.

![[Screenshot 2026-08-18 alle 14.46.29.png|664]]

From figure, we can see that the MDP starts from the agent choosing a specific action $a_t$ (action at time $t$) which affects the environment. Given the current state $S_t$ of the environment, a reward $R_t$ is provided to the agent. Furthermore, the action changes the environment as well, transitioning from state $S_t$ to $S_{t+1}$. The cycle then continues, and the sequence of states, actions and rewards taken is called *trajectory*.

In a *finite* state, the sets of actions $\mathcal{A}$ , states $\mathcal{S}$ and rewards $\mathcal{R}$ are finite as well. This is crucial: from a mathematical standpoint, we can develop a probabilistic approach of the MDP problem, given that in a finite state both rewards and states have well defined **discrete probability distributions**. Hence, given a current state $s_t$ and current action taken by the agent $a_t$, we can compute the probability of the reward $r_t$ and future state $s_{t+1}$ as following:

$$
P(r_t, s_{t+1}) = P(R = r_t, S = s_{t+1} | A = a_t, S = s_t).
$$

Thus, it is important to notice how the P function defines the *dynamics* of the environment **completely**. Furthermore, we will also assume that the posterior probabilities of current reward and future state will only depend on the current state and the current action, and nothing else. This property is called *Markov* property. Markov property is not only applied on the decision process, but also on the state itself, as it must include all information about all aspects if the past agent-environment interaction that make difference in the future.

##### The state-transition probabilities

Given that we know exactly and properly the dynamics of the environment (i.e. the probability function $P$), one can obtain all the *state-transition probabilities*, which define all the major information regarding the environment, such as probability of each single reward and state. 
For example:

$$
p(s_{t+1}|a_t, s_t)  = \sum_{r \in \mathcal{R}}p(s_{t+1}, r | a_t, s_t)
$$
We can also compute the expected rewards for the *state-action pairs* as a two argument function:

$$
r(s,a) \doteq \mathbb{E}[R_t \mid S_{t-1}=s, A_{t-1}=a] = \sum_{r \in \mathcal{R}} r \sum_{s' \in \mathcal{S}} p(s',r \mid s,a)

$$
And the expected reward given the state-action-next state:

$$
r(s,a,s') \doteq \mathbb{E}[R_t \mid S_{t-1}=s, A_{t-1}=a, S_t=s'] = \sum_{r \in \mathcal{R}} r \frac{p(s',r \mid s,a)}{p(s' \mid s,a)}
$$

##### Goals and rewards

The formal definition for goals and rewards is tied to the actual role of the above concepts within a Reinforcement Learning environment. The goals is the maximisation of the expected value of the cumulative sum of a scalar signal (the *reward*), which is provided by the environment when an agent executes an action.