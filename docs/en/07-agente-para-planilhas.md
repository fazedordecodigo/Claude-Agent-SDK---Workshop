# 7. Spreadsheet Agent

> Time range: 50:42-65:17 and 72:53-77:53

## Searching spreadsheets and large datasets

The spreadsheet example asks: what is the revenue in 2026? It sounds simple, but a real spreadsheet can have multiple sheets, irregular headers, years in columns, metrics in rows, formulas, subtotals, and hidden data.

The common mistake is loading everything into context. The workshop recommends thinking like a person: first look at the visible area, search for relevant terms, navigate sheets, record references, and go deeper only where there is signal.

For large datasets, accuracy drops if the agent tries to read everything. It is better to create search interfaces:

- search text;
- list sheets;
- read ranges;
- convert CSV into SQLite;
- query through SQL;
- expose XLSX XML when useful;
- keep a scratchpad with references.

## Interfaces the model already knows

Models tend to operate better with familiar formats. That is why a creative part of design is converting the problem into an "in-distribution" interface.

Examples:

- spreadsheet to SQL;
- range `B3:F20`;
- normalized CSV;
- structured XML;
- table with metadata;
- sheet-level summary.

If the agent knows SQL, use that. If it knows spreadsheet ranges, expose ranges. If it knows how to read XML, use XML when it makes sense. The goal is to reduce the distance between proprietary data and formats the model understands well.

## Verification and reversibility

For spreadsheets, verification can include:

- checking source cells;
- comparing a sum with a total;
- detecting null values;
- validating formulas;
- limiting queried ranges;
- keeping a copy/checkpoint before editing;
- recording every change.

Reversibility matters. Code is easy to undo with Git. Spreadsheets, UI, and external systems accumulate state. If the agent changes the wrong cell, there needs to be a checkpoint, history, or undo operation.

## Workshop: answering revenue in 2026

Recommended flow:

1. List sheets.
2. Search for `receita`, `revenue`, `2026`, and synonyms.
3. Read ranges near the results.
4. Identify whether years are in columns and metrics are in rows.
5. Save a scratchpad with candidates: sheet, range, cell, value.
6. Verify whether the value is a total, subtotal, or item.
7. Answer with value and reference.

Example of a good answer:

```text
The 2026 revenue appears to be in `Forecast!E12`, value BRL 1,203,000. I verified that the row represents "Total revenue" and that column E corresponds to 2026. I also compared it with the subtotal of range E8:E11.
```

This format makes the verification path clear. The user can audit it.
