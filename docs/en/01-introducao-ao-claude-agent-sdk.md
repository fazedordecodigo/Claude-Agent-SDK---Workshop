# 1. Introduction to the Claude Agent SDK

> Time range: 00:00-10:00

## What the workshop covers

The workshop presents the Claude Agent SDK as a way to build AI agents using the same kind of harness that powers Claude Code. The goal is not just to show a finished demo, but to think out loud about how an agent is designed: which tools it receives, how it creates context, how it acts, how it verifies its own work, and how all of that changes when the domain is not programming.

The central agenda moves through four questions:

- What is an agent?
- Why use an agentic framework?
- How do you design an agent with the SDK?
- How do you prototype a real agent while observing the loop and adjusting the environment?

The workshop is practical in tone. It does not try to sell a closed recipe. Designing agent loops still involves a lot of intuition: reading executions, noticing where the model gets lost, adjusting tools, creating deterministic feedback, and repeating.

## From single feature to autonomous agent

The speaker describes an evolution in three stages.

First came isolated LLM features: classify a text, summarize a message, return a response in a structured format. The scope was small and the execution path was defined by the application.

Then came workflows: more structured flows, such as labeling emails, indexing a RAG corpus, generating a code completion, or running a predictable process. The model still has freedom, but the application knows the input, output, and steps well.

Agents appear when the application lets the model construct a meaningful part of the trajectory. Instead of saying "call this tool and then that one", you provide a natural-language goal and a working environment. The agent decides which files to read, which commands to run, which scripts to create, which evidence to collect, and when to ask for help.

Claude Code appears as the canonical example: an agent that can work for many minutes, explore a codebase, edit files, run commands, test, and adjust.

## Why build on Claude Code

The Claude Agent SDK comes from the observation that many teams were rebuilding the same pieces:

- models;
- tools;
- execution loop;
- system prompts;
- filesystem;
- compaction;
- memory;
- skills;
- subagents;
- search;
- hooks;
- permissions;
- sandbox.

Packaging these pieces reduces repetitive work and brings production-tested opinions. The workshop's thesis is strong: for many agents, Bash and the filesystem are not technical details; they are part of product design.

Official note: the current documentation describes the Agent SDK as a library for creating agents that read files, run commands, search, edit code, and use the same loop/context as Claude Code in Python or TypeScript. Source: [Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview).

## Workshop: recognizing an agentic problem

Use this filter before choosing the SDK:

1. Can the user express the goal in natural language?
2. Does the path to the answer vary with context?
3. Does the agent need to gather information before acting?
4. Is there a reasonable way to verify the result?
5. Can the action be protected by permissions, sandboxing, or human review?

If the answer is "yes" for most of these, the problem looks agentic. If input and output are fixed and the path is predictable, a traditional workflow or atomic tool may be better.

Practical example: "how much did I spend on rides this week?" sounds simple, but it requires searching emails, identifying Uber/Lyft receipts, extracting values, summing them, handling duplicate receipts, and verifying that each value really is a ride. The agent benefits from tools for searching, saving intermediate results, and auditing its own answer.
