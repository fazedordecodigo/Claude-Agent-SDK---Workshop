# 9. Workshop: Pokemon Agent

> Time range: 86:00-105:20

## Prototype goal

The workshop uses Pokemon as the example domain because it combines a rich API, lots of data, specific rules, and open-ended questions. The user can ask about types, generations, abilities, moves, or competitive team building.

The goal is to build an agent that can talk about Pokemon and use real data, not just the model's memorized knowledge.

## Tools vs Claude Code

The speaker compares two approaches.

In the first, a traditional application uses specific tools: `getPokemon`, `getSpecies`, `getAbility`, `getMove`, and others. This works for expected questions, but scales poorly if the API has many entities. The model also needs to choose among several similar tools.

In the second, Claude Code receives a small TypeScript library or data access and writes scripts to answer. For the question "which are the generation 2 water Pokemon?", it can fetch lists, iterate, check types, and produce the answer.

The pedagogical point: generated code can combine calls, filter, and verify at scale, without manually creating a tool for every path.

## Analyzing competitive data

The example moves on to textual data about competitive Pokemon. The corpus contains information about who pairs with whom, counters, roles, and relationships.

The agent needs to:

- search for `Venusaur`;
- find related entries;
- identify common teammates;
- separate counters from partners;
- create a script to analyze co-occurrences;
- suggest a team with justification.

This case is interesting because it is not only an API query. It is semi-structured text analysis. The agent uses search, scripts, and synthesis.

## Workshop: team around Venusaur

Example prompt:

```text
I want to build a competitive team around Venusaur. Use the available data, identify recurring partners, common threats, and suggest an initial composition with justification.
```

Expected flow:

1. Search mentions of Venusaur.
2. Extract nearby snippets.
3. Identify Pokemon cited as partners.
4. Identify counters and threats.
5. Generate a table with role, reason, and textual source.
6. Propose a composition.
7. Call out uncertainties and next tests.

A good answer should not just list names. It should explain roles: support, type coverage, offensive pressure, response to counters, and team gaps.

Core learning: when data is in files, the agent can build its own analysis tool. The design should make that easy with accessible data, allowed scripts, and verification.
