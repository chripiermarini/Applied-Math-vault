Notes taken from the [agentic coding](https://missing.csail.mit.edu/2026/agentic-coding/) lecture from ['the Missing semester of your CS education'](https://missing.csail.mit.edu/). These notes are taken from using Claude code as AI. Full lecture notes are available [here](https://missing.csail.mit.edu/2026/agentic-coding/).
##### What is a coding agent?

A coding agent is just a conversational AI with specific coding tools that allow to read, modify and execute commands in specific programming language. The importance is that one ca use natural language to provide tasks to the agent.

It is possible to define the security settings of the coding agent in order to either expand its autonomy or increase its control in order not to execute undesirable commands.

With Claude, one can input the command `/cost` to verify how much has been spent using Claude Code (if one has that type of subscription).
##### How do agents harness work?

It is a very broad and hot topic, but in general there are a few relevant aspects to make sure they are clear. 

The agent is composed by two parts: one is the LLM (Large Language Models) and agent coding harness tool. The LLM works using a probability distribution:

Completion y given prompts x obtaining the probability distribution of the response $\pi_\theta(y | x)$ . Then, the response is sampled from the prob. $y \approx \pi_\theta(y | x)$ . The models have a limited context window, meaning both $y$ and $x$ are limited.

As a conversational chat, the model turns markers to information:

- user: what is 1 + 1? (this is *a query*)
- system: the response is 2.
- user: ... (another query)

For each response of the system, the context of the conversation (i.e. all the other exchanges) are used as input for the next word sampled from the distribution. 

This represents the first half of a coding agent. The second half is the agent harness, which is capable of having different tasks, like file reading, executing Bash, etc. It might be possible to have separated agent harness and LLMs, in order to use different underlying models to perform language modelling with the same agent harness. 

Usually both LLMs and agent harness are run on the cloud and the user receives only the information, still it is possible to use open source packages to have parts of this architecture locally.
##### Different use-case: linters, compilers, unit tests

A great use-case to make sure the coding agent has worked properly is to write down unit tests that we want our code to pass successfully. If it does not pass, we query the agent to check the code and fix it.

It is possible to allow the agent to `git commit`, and then check the full history of commits to see what an agent has done to the codebase. Agents are particularly good at understanding entire codebases, and summarize them.

(Resume video at 41:36).