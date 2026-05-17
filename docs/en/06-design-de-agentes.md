# 6. Agent Design

> Time range: 21:46-43:44 and 83:40-86:00

## Reading execution transcripts

The main design recommendation is simple: read the agent transcripts repeatedly.

When observing an execution, ask:

- What did it try first?
- Where did it gather context?
- Which tool did it choose?
- Where did it get confused?
- What feedback would have helped?
- Did it verify anything or just answer?
- Does the final answer have evidence?

This diagnosis is worth more than generic theory. Each domain has its own friction. A support, spreadsheet, security, or email agent fails in different ways.

## Progressive disclosure with skills

Skills are presented as a form of progressive disclosure. Instead of dumping an entire manual into the prompt, you place knowledge in files and let the agent open it when needed.

A skill works well when:

- the task is recurring;
- it requires specialized knowledge;
- it contains reusable steps or scripts;
- it does not need to always be in context;
- it helps the agent operate in a specific format.

The conversation also compares skills, APIs, and helper files. There is no universal rule. If the agent always needs an API, an `api.ts` or `client.py` with examples may be better. If it needs specialized instructions and templates, a skill makes sense.

## APIs, scripts, and discovery

For custom scripts and CLIs, the workshop recommends discovery through the environment itself. Put scripts in the filesystem, tell the agent they exist, and implement `--help`.

A good agentic CLI:

- explains subcommands;
- shows examples;
- returns clear errors;
- saves artifacts in files;
- uses structured output when possible;
- avoids dumping giant results into the terminal.

This approach fits the Agent SDK because Bash and filesystem live in the same environment. If the tool saves a file in one place and Bash runs in another container, the agent loses composability.

## Workshop: prototype before production

Suggested path:

1. Prototype in Claude Code.
2. Give the agent an API, scripts, and sample data.
3. Ask for real tasks.
4. Read the transcript.
5. Adjust scripts, instructions, permissions, and data format.
6. Only then turn it into an agent with the SDK.

The final agent may have little code. "Simple" does not mean "easy": the difficulty is discovering the right interface between model, data, and tools.

Example: turning a spreadsheet into SQL may be the insight that makes the agent reliable. It is not much code, but it is a good representation choice.

Official note: the programmatic usage page shows that the Agent SDK can be used through CLI, Python, or TypeScript, including non-interactive mode with `claude -p`. Source: [Run Claude Code programmatically](https://code.claude.com/docs/en/headless).
