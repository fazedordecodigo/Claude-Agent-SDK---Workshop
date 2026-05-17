# 10. Production, Hooks, Hosting, and Monetization

> Time range: 99:31-112:02

## From prototype to SDK

After prototyping in Claude Code, the next step is turning the behavior into an application. In the example, the agent file has only a few lines: it defines the working directory, calls the SDK, configures allowed tools, and runs the loop.

The hard part is not writing a lot of code. It is discovering:

- which files the agent needs;
- which scripts should exist;
- which permissions are safe;
- how to approve tools;
- how to store sessions;
- which checks run at each step;
- how to expose the result to the user.

Official note: the documentation shows programmatic usage through CLI, Python, and TypeScript, including `claude -p` for non-interactive execution and options such as allowed tools and output format. Source: [Run Claude Code programmatically](https://code.claude.com/docs/en/headless).

## Hooks for deterministic control

Hooks are extension points for validating, blocking, logging, or inserting context during the agent cycle.

In the workshop, hooks appear as answers to several problems:

- prevent an answer without running a script when the task requires data;
- verify a spreadsheet after each action;
- insert changes made by the user between tool calls;
- reinforce the rule "read before writing";
- create an audit trail;
- run intermediate validations.

Official note: the documentation lists hooks as callbacks on agent lifecycle events, such as before/after tool use, session start/end, and prompt submission. Source: [Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview).

## Local hosting, sandbox, and adaptive UI

Two ways to distribute agents are mentioned:

1. Local application: the user installs it and the agent operates on their computer.
2. Remote sandbox: each user receives an isolated environment with filesystem and commands.

For adaptive interfaces, the workshop suggests running a dev server inside the sandbox. The agent can edit code, the UI reloads, and the user interacts with an experience built for the task. This pattern resembles website builders: sandbox + server + live editing.

This architecture changes the product. The UI can stop being fixed and become something the agent adjusts to the task: Pokemon team builder, spreadsheet panel, data visualization, or review tool.

## Cost and product

Agents can be expensive because strong models work longer, use tools, and take several iterations. The workshop recommends thinking about monetization early.

Product questions:

- Is the problem valuable enough to pay for the cost?
- Is usage frequent or occasional?
- Does subscription, usage limit, or consumption-based billing make sense?
- Does the agent need more expensive models for every step?
- Which checks avoid rework?

Pricing and usage promises are hard to undo. Therefore, technical design and the business model need to talk from the beginning.

## Closing: practical principles

- Start with the problem the user would pay to solve.
- Prototype in Claude Code before freezing architecture.
- Give the agent filesystem, Bash, and well-organized data.
- Use atomic tools for sensitive actions.
- Verify deterministically whenever possible.
- Use subagents for parallel work and QA.
- Isolate with sandbox and scoped credentials.
- Read execution transcripts as the main improvement instrument.
