# 4. Tools, Bash, and Code Generation

> Time range: 25:32-48:16

## Atomic tools

Structured tools are great when the action needs to be controlled, audited, and minimally ambiguous. The workshop cites file writing and email sending as examples.

A good atomic tool:

- has clear input and output;
- limits the error surface;
- allows human approval when necessary;
- represents an action that is hard to undo or sensitive.

Sending email, approving payment, deleting a record, or changing a production database should not depend on a free chain of commands. In those cases, a tool with an explicit schema and permissions is safer.

## Bash for composition

Bash shines when the task requires composing several actions. Examples:

- changing directories;
- using GitHub CLI;
- running linters and tests;
- calling an internal CLI;
- chaining `grep`, `jq`, `sort`, `uniq`;
- storing memory in files;
- discovering subcommands through `--help`.

The trade-off is latency and discovery. A structured tool is already described in context. A CLI discovered through Bash must be explored, which costs time, but saves tokens and increases flexibility.

## Code generation for non-code tasks

"Code generation for non-code" means creating scripts to solve business, data, or research tasks. The user asks for something in natural language; the agent writes helper code to fetch an API, clean data, analyze files, or assemble reports.

Workshop example: ask for the weather in San Francisco and an outfit recommendation. An agent can write a script to call a weather API, detect location, combine it with wardrobe data, and generate a response. The code is not the final product; it is the temporary tool.

This also applies to:

- spreadsheet analysis;
- document research;
- PowerPoint creation;
- data cleaning;
- format conversion;
- dynamic SQL queries.

## Workshop: choosing the right interface

Use this matrix:

| Need | Best candidate |
| --- | --- |
| Sensitive, irreversible, or approval-based action | Atomic tool |
| Composition of existing commands | Bash |
| Dynamic logic, analysis, or transformation | Code generation |
| Highly structured and safe query | API/tool with schema |
| Free exploration in a complex domain | Bash + scripts + files |

Design question: "does the agent need power or guarantees?"

If it needs guarantees, reduce freedom. If it needs power, provide environment and verification. In databases, for example, a tool is good when the query needs to mask sensitive data or restrict access. For exploratory analysis, SQL via script may be better, as long as credentials and permissions are well scoped.

Official note: the SDK provides built-in tools, but also supports custom tools and MCP integration. Source: [Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview).
