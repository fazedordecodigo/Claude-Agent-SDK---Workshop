# Claude Agent SDK — Workshop Documentation

> **Source:** ~2-hour workshop about the Claude Agent SDK (Anthropic)
> **Presenter:** Anthropic engineer
> **Topic:** Building Agents with the Claude Agent SDK
> **Translation:** Transcript translated and formatted as technical documentation

---

## Table of Contents

1. [Introduction: What Is the Claude Agent SDK](#1-introduction-what-is-the-claude-agent-sdk)
2. [Evolution of AI Capabilities](#2-evolution-of-ai-capabilities)
3. [Components of an Agent Harness](#3-components-of-an-agent-harness)
4. [Why the Claude Agent SDK?](#4-why-the-claude-agent-sdk)
5. [Bash as the Most Powerful Tool](#5-bash-as-the-most-powerful-tool)
6. [Workflows vs Agents](#6-workflows-vs-agents)
7. [Designing an Agent Loop](#7-designing-an-agent-loop)
8. [Tools vs Bash vs Code Generation](#8-tools-vs-bash-vs-code-generation)
9. [Skills](#9-skills)
10. [Subagents and Context Management](#10-subagents-and-context-management)
11. [Security and Guardrails](#11-security-and-guardrails)
12. [Verification and Hooks](#12-verification-and-hooks)
13. [Design Example: Spreadsheet Agent](#13-design-example-spreadsheet-agent)
14. [Live Coding: Pokemon Agent](#14-live-coding-pokemon-agent)
15. [Hosting and Sandbox](#15-hosting-and-sandbox)
16. [Agent Monetization](#16-agent-monetization)
17. [Context Considerations](#17-context-considerations)
18. [General Best Practices](#18-general-best-practices)

---

## 1. Introduction: What Is the Claude Agent SDK

The **Claude Agent SDK** is a development kit built on top of **Claude Code** that lets you create AI agents in an opinionated and structured way.

### 1.1 Motivation

While building agents internally at Anthropic, the team realized it kept rebuilding the same pieces. The SDK packages those pieces so they can be reused.

### 1.2 Who It Is For

- Software engineers building coding agents
- Finance, data, and marketing professionals automating tasks
- Anyone who needs to create non-coding agents

### 1.3 Popular Applications

- Software engineering agents (reliability, security)
- Bug triage agents
- Dashboard builders
- Office agents
- Agents for finance, legal, and healthcare

---

## 2. Evolution of AI Capabilities

The speaker draws a clear evolutionary line:

### 2.1 Single LLM Call

- **Example:** "Classify this into categories and return JSON"
- Most basic use: one prompt, one response
- No state, no loop, no tools

### 2.2 Workflows

- **Example:** "Take this email, label it, and based on the result, index it in RAG"
- More structured, with well-defined steps
- Fixed inputs and outputs
- Similar to GitHub Actions

### 2.3 Agents

- **Example:** Claude Code: you do not restrict what it can do
- You converse in natural language
- The agent builds its own context
- It decides its own action trajectory
- It works autonomously for long periods (10, 20, 30 minutes)
- **Claude Code was described as the first true agent**

---

## 3. Components of an Agent Harness

The SDK packages all these components:

### 3.1 Models

- The foundation of the system: the agent's brain

### 3.2 Tools

- The obvious first step: add tools to the model
- Custom or prebuilt tools (filesystem, search, etc.)

### 3.3 Execution Loop

- Tools run in a continuous loop
- The agent decides which tool to call at each step

### 3.4 Prompts

- The central agent prompt and auxiliary prompts
- Prompt engineering to steer behavior

### 3.5 File System

- **Main mechanism for context engineering**
- The agent uses the file system to store and retrieve information
- Context is not only in the prompt; it is also in the available files

### 3.6 Skills

- Recently launched capability
- Collections of instructions and expertise that the agent can load

### 3.7 Other Components

- Subagents
- Web search
- Context compression
- Memory
- Hooks

---

## 4. Why the Claude Agent SDK?

### 4.1 Built on Claude Code

- When Claude Code launched, engineers started using it
- Then finance, data, and marketing teams followed
- People used Claude Code for non-coding tasks
- When building non-coding agents, the team kept coming back to Claude Code

### 4.2 The Bash Principle

> "Spoiler: Bash is the reason we can use Claude Code for non-coding tasks."

Bash allows any action on a Unix system: the agent can literally do anything through it.

### 4.3 Lessons Learned at Scale

- Tool-use errors
- Context compression
- Best practices discovered across millions of executions
- The SDK is **opinionated**: it incorporates Anthropic's best practices

### 4.4 Analogy: React vs jQuery

> "The Agent SDK is like React for agent frameworks: we build our own things on top of it, so we know it is real. And all the boring parts are things we faced ourselves."

---

## 5. Bash as the Most Powerful Tool

### 5.1 Why Bash?

One of the SDK's main opinions is that **Bash is the most powerful tool an agent can have**.

### 5.2 What Bash Enables

- Store tool-call results in files
- Persist memory
- Dynamically generate and run scripts
- Compose capabilities (`grep`, `jq`, `ffmpeg`, `curl`)
- Use existing software (`npm`, `pip`, system packages)
- Pipeline processing

### 5.3 Bash vs Traditional Tools

If you were designing an agent harness without Bash:

- You would create a search tool, a read tool, an execution tool
- Every new use case would require a new tool
- With Bash: `grep` already searches, `npm` already installs, `curl` already does HTTP

### 5.4 Practical Example: Email Agent

**Scenario:** The user asks "How much did I spend on Uber/Lyft rides this week?"

**Without Bash:**

- The agent receives 100 emails as JSON
- It tries to understand each one manually
- High cognitive load, low precision

**With Bash:**

- It uses the Gmail search API to filter
- It saves results to a file
- It uses `grep` to extract prices
- It sums values with a script
- It verifies the work by checking each price
- It stores everything in a file for future reference

### 5.5 Bash for Non-Coding Agents

- Generate scripts to fetch weather APIs
- Use `ffmpeg` to process video
- Use `jq` to analyze JSON
- Compose data pipelines

### 5.6 Custom Scripts in Bash

- Place scripts in the file system
- Instruct the agent with `--help` for progressive discovery
- The agent progressively discovers subcommands

---

## 6. Workflows vs Agents

### 6.1 When to Use Workflows

- Predefined flows with well-defined inputs and outputs
- Repetitive processes, such as GitHub issue triage
- Use with Structured Outputs

### 6.2 When to Use Agents

- When you want to converse in natural language
- When the agent needs to act flexibly
- Examples: business data analysis, dashboards, question answering

### 6.3 Both Are Possible in the SDK

- The SDK supports both workflows and agents
- The presentation's main focus: agents

---

## 7. Designing an Agent Loop

### 7.1 The Three Parts of the Loop

```text
+-----------------+
|  Gather         |
|  Context        |
+--------+--------+
         |
         v
+-----------------+
|  Take           |
|  Action         |
+--------+--------+
         |
         v
+-----------------+
|  Verify         |
|  Work           |
+-----------------+
```

### 7.2 Gather Context

- For Claude Code: find the necessary files
- For an email agent: find the relevant emails
- **A step that is often underestimated or skipped**

### 7.3 Take Action

- Does the agent have the right tools?
- Code generation and Bash are more flexible
- The agent decides which action to take

### 7.4 Verify Work

- **Key question:** "Can you verify the agent's work?"
- If yes, it is an excellent candidate for an agent
- Code -> can compile, run, and test -> verifiable
- Deep research -> harder -> use source citations

### 7.5 Practical Tip

> "Read the transcripts repeatedly. Every time the agent runs, read what it did. Ask yourself: 'What is it doing? Why is it doing this? How can I help it?'"

---

## 8. Tools vs Bash vs Code Generation

### 8.1 Three Forms of Action

| Characteristic | Tools | Bash | Code Gen |
|---|---|---|---|
| **Structure** | Extremely structured | Composable | Highly dynamic |
| **Reliability** | High | Medium | Variable |
| **Context use** | High (many tools) | Low | Low |
| **Latency** | Low | Medium | High |
| **Discovery** | None (defined) | Progressive (`--help`) | Dynamic |
| **Composition** | Not composable | Highly composable | Highly composable |

### 8.2 When to Use Each One

#### Tools

- Atomic actions that need high control
- Non-destructive or irreversible actions
- **Example:** write file (the user sees and approves)
- **Example:** send email

#### Bash

- Composable actions
- **Example:** navigate directories, Git, check errors
- **Example:** file-based memory system

#### Code Generation

- Highly dynamic and flexible logic
- API composition
- Data analysis
- Advanced search

### 8.3 Practical Recommendation

```text
Sensitive database with user data?  -> Use Tool (protection + validation)
Database with dynamic SQL?          -> Use Bash or Code Gen (more flexible)
Send email?                         -> Use Tool (irreversible action)
Analyze spreadsheet?                -> Use Code Gen + Bash
```

---

## 9. Skills

### 9.1 What Skills Are

Skills are collections of files that provide specialized instructions to the agent. They are a form of **progressive context disclosure**.

### 9.2 How They Work

- They are simply folders with files the agent can read
- The agent runs `cd` into the skill directory and reads the instructions
- They allow the agent to execute long and complex tasks

### 9.3 Examples of Skills

- **Frontend Design Skill:** detailed instructions from one of Anthropic's best frontend engineers
- **Document Generation Skills:** how to use code gen to create specific files

### 9.4 Skills vs API

- Skills are useful when the agent needs to discover how to do something
- If the agent always queries a specific API, a `.ts` or `.py` file may be better
- Skills introduce the idea of "file system as context"

### 9.5 Future of Skills

- Claude Code has a plugin marketplace
- It is evolving (still V0)
- It is more a discovery system than a monetization system
- Over time, models will do more tasks alone; skills fill gaps for out-of-distribution tasks

---

## 10. Subagents and Context Management

### 10.1 Why Use Subagents

- Manage context efficiently
- Run tasks that do not need to pollute the main agent's context
- Parallel processing

### 10.2 Usage Patterns

- **Search subagent:** receives the question, researches, returns summarized results
- **Verification subagent:** adversarially analyzes the main agent's work
- **Parallel processing:** read multiple spreadsheets simultaneously

### 10.3 How Claude Code Implements This

- Claude Code has the best subagent experience, especially with Bash
- Parallel subagents with Bash require care around race conditions
- The harness handles all system engineering: spawn, communication, collection

### 10.4 Locked Room Analogy

> "It is like someone locks you in a room and gives you tasks. Would you rather have a stack of papers or a computer with Google, tools, and freedom? The agent also prefers the computer."

---

## 11. Security and Guardrails

### 11.1 Defense in Depth

Each layer has some defenses; together, they are expected to block everything.

### 11.2 Model Layer

- Model alignment (safety training)
- Recent paper on reward hacking (recommended)

### 11.3 Harness Layer

- Permissions and prompting
- **AST parser in Bash:** reliably knows what Bash is doing
- Not something you want to build from scratch

### 11.4 Sandbox Layer

- Even if the agent is compromised, what can it actually do?
- Network sandbox
- File-system sandbox
- Operation isolation

### 11.5 The Security Triad

> "If someone takes control of your agent, they still need to extract information. If you isolate it in a sandbox: network, files, execution, it becomes much harder."

### 11.6 Sandbox Providers

- Modal, E2B, Fly.io: all have built-in security levels
- Do not host on your personal computer or with production secrets

---

## 12. Verification and Hooks

### 12.1 Importance of Verification

- The more verifiable the work, the better the agent candidate
- Start with as many **deterministic** rules as possible
- Then add subagent-based verification

### 12.2 Examples of Deterministic Verification

- **Null pointers:** does the code compile? Are there obvious errors?
- **Claude Code:** if the agent tries to write a file it has not read, the harness rejects it
- **Rules:** "do not fetch more than 10,000 rows at a time"

### 12.3 Hooks

- A way to perform deterministic verification on specific events
- Trigger after each tool call
- **Example:** after each call, check whether the spreadsheet was changed by the user
- **Example:** force the agent to always write scripts instead of answering directly

### 12.4 Verification in Subagents

- Use subagents in a separate context for adversarial verification
- "Act as a junior McKinsey analyst reviewing this work"
- **Careful:** avoid context pollution; use a new session

---

## 13. Design Example: Spreadsheet Agent

### 13.1 Problem

Build an agent that can:

- Search data in spreadsheets
- Execute actions (insert, update)
- Verify the work

### 13.2 How to Search (Gather Context)

Several creative approaches were discussed:

| Approach | Description |
|---|---|
| **CSV + grep** | Convert to CSV and use grep/text search |
| **SQLite** | Convert the spreadsheet into a SQL database; the agent knows SQL very well |
| **Formulas (A1:B5)** | Use the range syntax the agent already knows |
| **XML** | `.xlsx` files are XML; the agent understands XML search |
| **Google APIs** | Use the spreadsheet tool's native APIs |
| **Metadata** | Preprocess the spreadsheet to add search metadata |

> "Think in transformations. If you can convert a data source into an interface the agent knows well (like SQL), you have won."

### 13.3 How to Take Action

- Insert rows via SQL or APIs
- Edit cells through formulas or scripts
- Similar approaches to context gathering

### 13.4 How to Verify

- Check null pointers
- State checkpoints (undo/redo)
- Store state between checkpoints
- Allow the user to "go back in time"

### 13.5 Challenges with Large Spreadsheets

- Millions of rows -> accuracy decreases
- Do not load everything into context at once
- The agent should navigate like a human: view first rows, then search
- Use a scratch pad (a new sheet) for notes

---

## 14. Live Coding: Pokemon Agent

### 14.1 Goal

Build an agent that can talk about Pokemon, build competitive teams, and query the PokeAPI.

### 14.2 Approach 1: Claude Code + Code Generation

1. Prompt Claude Code: "Fetch the PokeAPI and create a TypeScript library"
2. Claude Code automatically generates interfaces, types, and functions
3. The agent writes scripts in the `examples/` directory and runs them
4. Example: "Show me all generation 2 water Pokemon"

### 14.3 Approach 2: Tools Only

- Define tools: `getPokemon`, `getAbility`, `getType`, `getMove`
- Create a simple chat interface
- The agent decides which tool to call
- **Limitation:** too many tools confuse the model

### 14.4 Competitive Team Example

- Data from a text file with Pokemon synergies
- Claude Code writes a script to analyze the data
- It searches counters, teammates, base stats
- It generates team and moveset suggestions

### 14.5 Claude Code Playing Pokemon

- Access to the GBA emulator's internal memory
- RAM search to find party, map, items
- Pokemon Red is highly "in-distribution" (heavily reverse engineered)

---

## 15. Hosting and Sandbox

### 15.1 Deployment Options

#### Local Application

- Claude Code runs on the user's computer
- Ideal for initial distribution
- "Local applications may become the default with AI"

#### Cloud Sandbox

- Providers: E2B, Modal, Fly.io
- Just call `sandbox.start()` and communicate with it
- Each user has their own sandbox

### 15.2 Example with Agent.ts

```typescript
// Conceptual example
import { Sandbox } from 'sandbox-provider'
const sandbox = await Sandbox.start()
// The agent runs inside the sandbox
// Communication via API
```

### 15.3 Adaptive UI

- Run a dev server inside the sandbox
- The agent edits code and the server live-refreshes
- The user interacts with the generated site
- This is how Lovable and other site builders work

---

## 16. Agent Monetization

### 16.1 Initial Considerations

- Agents are costly (models, compute)
- Design monetization **from the beginning**; it is hard to go back

### 16.2 Pricing Models

| Model | When to use |
|---|---|
| **Subscription** | Frequent and predictable usage |
| **Per token/usage** | Sporadic usage |
| **Hybrid** | Rate limits + overage billing |

### 16.3 How Claude Code Does It

- Rate limits for moderate users
- Usage-based pricing for users who exceed limits

### 16.4 Advice

> "Number one: make sure you are solving a problem people want to pay for. Then choose the pricing model."

---

## 17. Context Considerations

### 17.1 When to Compact

- In Claude Code, the speaker rarely needs compression
- Strategy: clear context often
- State is in files; `git diff` shows the changes

### 17.2 For Agents with Non-Technical Users

- Users do not know what a "context window" is
- On each new question, compact automatically
- Store user preferences
- Application state, such as a spreadsheet, already contains a lot of context

### 17.3 Practical Tip

> "You do not need 1 million context tokens. You need **good context management**."

---

## 18. General Best Practices

### 18.1 Rapid Prototyping

1. Start with Claude Code: give it APIs and instructions
2. Observe what Claude Code does
3. Iterate on the behavior
4. When it is good, turn it into an agent with the SDK

### 18.2 Simplicity vs Ease

> "Building an agent should be **simple**, but simple is not the same as **easy**."

- The agent code does not need to be huge
- It needs to be elegant
- It needs to be what the **model wants**: work with the model, not against it

### 18.3 Constant Rewriting

- Review and rewrite agent code every ~6 months
- Capabilities change quickly
- "You write code 10x faster, so throw away code 10x faster too"
- Startups have an advantage: short incubation cycles vs established companies

### 18.4 Working with the Model

- Read transcripts
- Discover what the model prefers
- Adapt the interface to be "in-distribution"
- Turn out-of-distribution problems into something the model knows well

### 18.5 Reuse Across Agents

- Different paradigm from web apps (1 app -> 1M users)
- Agents are usually 1:1 (one container per user)
- Communication between agents: HTTP requests, do not reinvent
- Creative example: a virtual forum where agents post topics and replies

### 18.6 Semantic Search vs grep

- The model is better with `grep` than with semantic search
- The model **was not trained** for semantic search
- For large codebases: good context management + hooks + linting

---

## Glossary

| Term | Meaning |
|---|---|
| **Agent Harness** | Infrastructure around the model: tools, loop, prompts |
| **Progressive Context Disclosure** | Revealing information as needed, such as Bash `--help`, skills, and filesystem |
| **In-Distribution** | Tasks the model knows well from training |
| **Code Gen** | The agent writes and runs code dynamically |
| **Hooks** | Deterministic functions that trigger on agent loop events |
| **Swiss Cheese Defense** | Multiple security layers that protect the system together |
| **AST Parser** | Abstract syntax tree parser, used to understand what Bash is doing |
| **Context Pollution** | When unnecessary information overloads the agent context |

---

> **Note:** This documentation was generated from the transcript of a ~2-hour workshop about the Claude Agent SDK, presented by an Anthropic engineer. The transcript was produced with Whisper (tiny model) and then translated/formatted. Small inaccuracies may exist due to automatic transcription quality.
