# 2. Agents vs Workflows

> Time range: 11:28-24:59

## When to use a workflow

A workflow is the best choice when input, output, and process are well defined. One cited example is GitHub Actions-style automation: receive a PR, generate a summary, run validations, publish a result.

The Claude Agent SDK can also be used in workflows, especially when there are variable parts in the middle. An issue triage bot looks like a workflow, but may need to clone a repository, search files, start a container, reproduce a bug, and only then generate a structured classification. In that case, the outside shape is a workflow; the middle is agentic.

## When to use an agent

An agent is appropriate when the user wants to speak in natural language and let the system act flexibly. Examples:

- querying business data;
- building dashboards;
- investigating bugs;
- automating Slack or GitHub;
- exploring internal datasets;
- creating documents, spreadsheets, or scripts.

The difference is not "has an LLM" vs "does not have an LLM". The difference is who chooses the trajectory. In a classic workflow, the application leads. In an agent, the application provides tools, initial context, and limits; the agent leads part of the journey.

## The loop: context, action, and verification

The presented loop has three phases:

1. Gather context.
2. Take action.
3. Verify the work.

For Claude Code, gathering context means locating relevant files. For an email agent, it means finding the right messages. For a finance agent, it may mean discovering the correct sheet, table, period, and metric.

Taking action involves tools, Bash, scripts, edits, or API calls. Verification is what separates a good-looking demo from a reliable agent. Code is a strong domain because it compiles, runs tests, and supports diffs. Deep research is harder: citations help, but they are not equivalent to a compiler.

Plans fit between context and action when the problem needs step-by-step reasoning. They increase control and auditability, but also add latency.

## Workshop: designing an agent loop

Choose a task and fill in:

| Phase | Question | Example |
| --- | --- | --- |
| Context | What does the agent need to find? | This week's receipt emails |
| Action | What does it need to do? | Extract values, normalize currency, sum |
| Verification | How do we know it is correct? | File with each receipt, value, and source line |

Then ask: does the agent really need all context handed to it up front? In the workshop, the recommendation is to let the agent gather context whenever possible. Instead of dumping a pile of documents into the context window, give the agent instruments to search, filter, save evidence, and return to it.

Official note: the SDK documentation lists native tools for reading, editing, Bash, glob, grep, web search, and other capabilities, as well as hooks, subagents, MCP, permissions, and sessions. Source: [Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview).
