Engineering problems usually focus on computing the optimal solution of a tradeoff between cost, performance and time. The usual science of optimization actually focuses more on the single objective optimization, that might model most of the real-world problems. It is also important to note that we can also model our physical system in order to compute optimal values with respect to multiple optimization functions. 

This chapter focuses then to provide information regarding *multi-objective optimization* (also known as *vector optimization*, or *MOO*). We will also focus on models and algorithms that allow to identify the design points that represent the best tradeoff between objectives, without committing to only one in particular.

##### Pareto optimality

The concept of Pareto optimality is mandatory in a multi-objective optimization environment. A point is Pareto optimal if it is not possible to improve one objective function without worsening at least another one. In a multi-objective optimization framework, we have the following standard model:

$$
\begin{equation}
\begin{aligned}
\min_{x} \quad & y = f(x) \\
\text{s.t.} \quad & g(x) \leq 0
\end{aligned}
\end{equation}
$$

where, in multi-objective settings, we have $y \in \mathbb{R}^{m}$, with $m > 1$. This means that each point $x$ is associated with an objective function value that is a multidimensional vector, not a scalar.

In this settings, it is said that a point $x$ *dominates* another one $x'$  if it is better than the other with respect of some dimensions, and it is not worse with respect of the remaining dimensions. In other words, the following happens:

$$
\begin{equation}
\begin{aligned}
&f_i(x) \leq f_i(x') \quad \text{for $i$ in }1:m  \\
\\

\text{and  } &f_i(x) < f_i(x') \quad \text{for some $i$}
\end{aligned}
\end{equation}

$$
A point is said to be *Pareto optimal* if there is no point that dominates it. The set of Pareto optimal points forms the *Pareto frontier*, all these points lie in the image space of the multidimensional objective function. There can be multiple ways to generate the Pareto front, that would be explained in the following sections.

![[Screenshot 2026-08-19 alle 11.52.53.png|485]]


We show below the difference between the image space of the mono dimensional objective function versus the multi dimensional one. In the first case, the optimal is just a point (the optimal one, might not be the only one but we assume this for the example below). In the multi-objective optimization framework, the lower area of the search space represents the Pareto front.

![[Screenshot 2026-08-19 alle 11.50.07.png|444]]


### Constraint-based methods for MOO

There exist multiple methods to perform multi-objective optimization and generate the Pareto front. A class of these algorithms is based on using the constraints to cut-out sections of the Pareto frontier and obtain single points in the image space.

One one side, we have the *standard constraint method*, for which we basically select $m-1$ objective functions, with only one remaining. We would cap the selected objective functions by a vector $\boldsymbol{c}$ , and minimize with respect to the only objective function remained:

$$
\begin{equation}
\begin{aligned}
\min_{x} \quad & f_1(x) \\
\text{s.t.} \quad & f_2(x) \leq \textbf{c}_1 \\
&f_2(x) \leq \textbf{c}_2 \\
&\vdots \\
&f_{m-1}(x) \leq \textbf{c}_{m-1} \\
&x \in \mathcal{X}
\end{aligned}
\end{equation}
$$

On the other side, one can use a *lexicographic method*, which orders all the objective functions based on importance/relevance. The iterative algorithm then takes the most important objective function, and compute the optimal value $y*$ with respect to that (and the original problem constraints). Then, the second most important objective function is computed, using a new constraint $f_1(x) \leq y*$ on the problem. Hence:

1st iteration:

$$
\begin{equation}
\begin{aligned}
&\min_{x} \quad & f_1(x) \\
&\text{s.t.} &x \in \mathcal{X}
\end{aligned}
\end{equation}
$$
2nd iteration:

$$
\begin{equation}
\begin{aligned}
&\min_{x} \quad & f_1(x) \\
&\text{s.t.} &x \in \mathcal{X} \\
& &f_1(x) \leq y^*_1
\end{aligned}
\end{equation}
$$

![[Screenshot 2026-08-19 alle 12.32.36.png]]