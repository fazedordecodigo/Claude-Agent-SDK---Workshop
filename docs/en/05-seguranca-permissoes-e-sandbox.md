# 5. Security, Permissions, and Sandbox

> Time range: 12:41-15:25 and 44:47-47:35

## Defense in depth

Giving Bash to an agent is powerful, but requires defense in depth. The workshop calls this a "Swiss cheese" approach: no layer is perfect alone, but several barriers reduce risk.

Layers mentioned:

- model alignment;
- harness prompts and instructions;
- Bash command parser/analysis;
- per-tool permissions;
- sandbox;
- scoped credentials;
- read and write limits;
- verification and feedback.

The goal is not to blindly trust the model. It is to design an environment where even a bad action has limited reach.

## Permissions and dangerous tools

Permissions should reflect impact. Reading a file is different from writing one. Running a test is different from deleting a database. Sending an email is different from drafting one.

In agent design, separate:

- free actions;
- actions that require approval;
- blocked actions;
- actions allowed only in a sandbox;
- actions with credential-reduced scope.

For databases, the speaker suggests thinking about agent-specific keys and proxies. Instead of giving broad credentials, create an access path with rules: read-only, allowed tables, masking, row limits, or feedback when the agent tries something forbidden.

## Sandbox and isolation

Sandbox answers the question: if someone takes control of the agent, what can they do?

A good sandbox:

- isolates the filesystem;
- limits network when necessary;
- contains no production secrets;
- allows state to be discarded;
- supports auditing;
- makes checkpoints easier.

The workshop also mentions sandbox providers and per-user containers. The architecture changes: instead of one web app for thousands of users, agents can operate in individual environments, with their own state and files.

## Workshop: designing guardrails

Take an agentic task and list risks by layer.

Example: an agent that queries an internal database.

| Layer | Guardrail |
| --- | --- |
| Credential | Read-only user, no writes |
| Query | Row limit and timeout |
| Sensitive data | Masking in the proxy |
| Bash | Block destructive commands |
| Filesystem | Isolated temporary directory |
| Response | Cite query and sample used |

Then test the bad path: "delete the table", "extract all emails", "run without limit", "ignore permissions". The agent should receive a clear error and continue safely.

Official note: the SDK documentation includes dedicated sections for permissions, hooks, checkpointing, hosting, and secure deployment. Source: [Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview).
