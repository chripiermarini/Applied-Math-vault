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
##### Advanced features:

*Reusable prompts*: the possibility to provide the same prompt multiple times, depending on the use-case (e.g. reviewing a code review).

*Parallel agents*: models can take 20-ish minutes, hence it might be useful to use multiple parallel agents at the same time, on the same codebase. In order to do so, we can use the command `git worktree`to set up multiple branches.

*MCPs*: Model Context Protocol, that allow to connect the coding harness with other tools, (e.g. Notion MCP connector, that allows Claude to access read and modify Notion files).

*Context management*: context is important as the agent takes the limited context window as input for the next response. Hence one can *clear* the context window, *rewinding* some steps of the conversation to keep the context focused, *compact* the context in order to reduce the current context window while maintaining the most important information from previous conversation

*llms.txt*: if one wants to use specific additional documents at inference time, and the LLM does not have the knowledge for that doc, one can add either in the context or in the `llm.txt` location. This allows to have amore compacted context.

*AGENTS.md*: markdown file in which we can store the information/context about a repository in which the file lives. In this way, we can just provide such files instead of having the agent review every time the same repo. For example, Claude uses `CLAUDE.md` to put the context of the repo.

*Skills*: the agents.md might be extremely big as well, and this reduces the context window inevitably. Instead of supplying all the information as context in the AGENTS.md file, one can list to the agent upfront the list of skills and tools it can use immediately.

*Subagents*: when an agent is provided with a big complex task, it usually breaks it down into multiple subtasks and assigns each one of them to *subagents* in order to solve those. This is good because each subagent focuses only on one task, has its own context, and provide to the parent agent only the relevant information, in order not to pollute the parent agent context.