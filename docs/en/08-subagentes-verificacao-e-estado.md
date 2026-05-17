# 8. Subagents, Verification, and State

> Time range: 66:03-82:14

## Subagents as context management

Subagents are useful when a task requires a lot of intermediate work, but the main agent only needs the final result.

Examples:

- one subagent searches one spreadsheet tab;
- another summarizes documents;
- another verifies security;
- another performs adversarial QA;
- another researches external sources.

The benefit is reducing context pollution. The main agent does not need to carry all logs, attempts, and raw results. It receives a synthesis with evidence.

In large spreadsheets, the main agent can split reading by sheet. Each subagent returns findings, uncertainties, and references. The main agent consolidates and decides whether more investigation is needed.

## Deterministic verification first

The workshop emphasizes: verify deterministically whenever possible.

Before asking another model to review, use rules:

- does it compile?
- do tests pass?
- does the query have a limit?
- was the file read before editing?
- does the range exceed the maximum size?
- are there null values?
- does the output format match the schema?

Then, when rules are not enough, use a QA subagent. Model-based verification should preferably run in a separate context, to avoid excessive sympathy for the produced work.

## State, checkpoints, and undo

State is one of the central difficulties of agents. In code, Git helps. In a UI or spreadsheet, an error can create confusing state: add the wrong item to a cart, delete a cell, move data, change configuration.

Good practices:

- create a checkpoint before edits;
- record every action;
- allow rollback;
- separate reading from writing;
- ask for approval for irreversible actions;
- turn complex systems into reversible operations when possible.

Official note: the current documentation includes checkpointing to revert file changes in the Agent SDK. Source: [Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview).

## Workshop: splitting reading and QA

Example: an agent answers a question about a spreadsheet with three sheets.

Split:

1. Subagent A: read sheet 1 and return candidates.
2. Subagent B: read sheet 2 and return candidates.
3. Subagent C: read sheet 3 and return candidates.
4. Main agent: consolidate evidence.
5. QA subagent: review the conclusion without seeing the full conversation.

Return contract for subagents:

```markdown
## Finding
- Candidate answer:
- Source:
- Confidence:
- Gaps:
- Suggested next verification:
```

This contract prevents loose answers and makes consolidation more reliable.
