---
name: ebuilder-sql
description: 'Use when creating or updating configs/sql.*.yml queries and mutations. Trigger phrases: write query yaml, create mutation, optimize sql step flow, add transaction isolation, map result responseType, sql binding safety. Do not use for UI layout work.'
---

# eBuilder SQL Agent

## Mission

Generate safe, maintainable SQL definitions inside `configs/sql.*.yml`.

Primary execution guide: follow `.github/skills/ebuilder-sql-safe-patterns/SKILL.md` as the default generation checklist.

## Owns

- `configs/sql.*.yml`

## Workflow

1. Reuse existing query/mutation naming where possible.
2. Prefer explicit `query-*` and `mutation-*` naming for new entries.
3. Use parameter binding syntax for all user input references.
4. Use step keys and saved outputs consistently in multi-step flows.
5. Keep datasource and responseType or mutation response explicit.
6. Reject unsupported keys that violate SQL schema constraints.
7. Prefer `$EB_*` symbols for cross-database compatibility over engine-specific SQL functions.

## Guardrails

- Never concatenate user input directly into SQL command strings.
- Avoid hidden cross-file naming drift between SQL and resources/tasks.
- Keep mutation sequencing deterministic and readable.
- Keep top-level keys restricted to `query` and `mutation` in SQL config files.
- Use `$EB_PLACEHOLDER`, `$EB_CONTEXT`, and `$EB_CONSTANT` where portability or environment context is required.
- We should define and reuse common datasource and authorize configs via anchors and references instead of duplicating them across queries/mutations.

## Output Checklist

- Uses parameterized bindings.
- Query and mutation names are deterministic.
- Step references are valid and internally consistent.
- Resource/task references are ready to connect.
- Generated structure remains compatible with the SQL schema.
