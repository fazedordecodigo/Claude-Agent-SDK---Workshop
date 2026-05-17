# 3. Bash, Filesystem, and Context

> Time range: 15:25-20:34 and 29:20-34:07

## Bash as a general-purpose tool

A central thesis of the workshop: Bash is one of the most powerful tools for agents. Instead of creating a new tool for every case, the agent can use existing commands, CLIs, scripts, `grep`, `jq`, package managers, linters, tests, and APIs called from the command line.

This turns Bash into "programmatic tool calling". The agent does not receive only predefined buttons; it receives an environment where it can compose actions. It can discover commands with `--help`, save outputs, create temporary scripts, call APIs, and verify results.

That freedom costs care. Bash can be dangerous, slow, or noisy. That is why the workshop repeatedly returns to permissions, command parsing, sandboxing, and verification.

## Filesystem as memory and context

The filesystem appears as a context-engineering mechanism. Instead of stuffing everything into the conversation, the agent can write files, create notes, save long results, keep memory, and reuse scripts.

This idea also explains skills. A skill is basically a collection of files that the agent can read when it needs a specialized capability. It enables progressive disclosure: the agent does not load all knowledge all the time; it discovers and opens what matters for the current task.

Example: a DOCX skill may contain instructions, templates, and scripts. The agent reads the skill, generates code to manipulate the document, and validates the result. The knowledge does not need to live entirely in the initial prompt.

## Example: email agent

Question: "how much did I spend on rides this week?"

Without Bash, an agent might receive 100 emails and try to reason directly. That increases error, cost, and confusion.

With Bash/filesystem, the flow improves:

1. Search messages with terms like Uber and Lyft.
2. Save raw results to a file.
3. Extract values with a script or command.
4. Generate a table with email, date, value, and source.
5. Sum values.
6. Verify samples and duplicates.

The point is not that Bash is magic. The point is that it gives the agent instruments similar to those a person would use: search, take notes, filter, calculate, and review.

## Workshop: turning a long result into a verifiable file

Practical pattern:

```text
External tool -> raw file -> extraction script -> summary file -> final answer
```

For agents, prefer returning file paths instead of dumping huge outputs into context. This enables rereading, auditing, and incremental verification.

Exercise:

1. Choose a query that returns a lot of text.
2. Save the output in `raw-results.txt`.
3. Generate `summary.csv` with verifiable fields.
4. Answer the user by citing which fields support the conclusion.

This pattern reduces context pollution and creates an evidence trail. It also helps subagents: a subagent can work on the raw file and return only conclusions.
