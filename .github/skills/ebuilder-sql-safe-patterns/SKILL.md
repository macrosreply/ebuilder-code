---
name: ebuilder-sql-safe-patterns
description: 'Hub for eBuilder SQL generation playbooks. Routes to specialized skills: ebuilder-eb-sql-symbols (for $EB_* symbols and portability), ebuilder-sql-query (for query-* definitions), and ebuilder-sql-mutation (for mutation-* definitions with orchestration). USE FOR: directing to correct SQL generation context.'
---

# eBuilder SQL Generation Hub

## Overview

This skill is a **routing hub** directing you to the correct **specialized SQL generation playbook** based on your task.

The original monolithic SKILL.md has been split into **three focused, maintainable skills**:

| Skill                       | Purpose                                                           | Use When                                                                                               |
| --------------------------- | ----------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| **ebuilder-eb-sql-symbols** | $EB\_\* symbols, engine-aware SQL, parameter binding, portability | Learning portable symbols, understanding cross-DB compatibility, configuring datasources/authorization |
| **ebuilder-sql-query**      | Read-only query-\* definitions, responseType, output mapping      | Generating `query-*` YAML, choosing responseType, field mapping (`propType`, `hideProps`)              |
| **ebuilder-sql-mutation**   | Multi-step data-modifying mutation-\* definitions, transactions   | Generating `mutation-*` YAML, orchestrating steps, transaction isolation, error handling               |

---

## Choose Your Skill

### 🔍 I need to understand eBuilder SQL symbols and portability

**→ Use: [ebuilder-eb-sql-symbols](./ebuilder-eb-sql-symbols/SKILL.md)**

Topics covered:

- `$EB_*` symbol reference and usage rules
- Engine-aware SQL patterns (PostgreSQL, Oracle, MSSQL, MySQL)
- Parameter binding safety
- Datasource and authorization configuration

### 📝 I need to create or review a query-\* definition

**→ Use: [ebuilder-sql-query](./ebuilder-sql-query/SKILL.md)**

Topics covered:

- Query blueprint and object properties
- Response types (`list`, `single`, `singleOrNull`, `first`, `firstOrNull`)
- Input parameters and type constraints
- Output field mapping (`propType`, `hideProps`, `customProps`)
- Pre-steps, authorization, and checks
- Error handling and callbacks
- Naming conventions and best practices

### 🔄 I need to create or review a mutation-\* definition

**→ Use: [ebuilder-sql-mutation](./ebuilder-sql-mutation/SKILL.md)**

Topics covered:

- Mutation blueprint and object properties
- Transaction isolation levels
- Multi-step orchestration with ordered execution
- Save strategies (`scalar`, `last-insert-id`, `first-row-as-object`, `all-rows`, etc)
- Step chaining and result referencing
- Guard checks (`checksBefore`, `checksAfter`)
- Error handling and callbacks
- Nested sub-steps and complex workflows

---

## Quick Reference Table

| Need                                                             | Skill                       | Key Topics                                       |
| ---------------------------------------------------------------- | --------------------------- | ------------------------------------------------ |
| Learn `$EB_COALESCE()`, `$EB_CONTEXT()`, `$EB_DATETIME_NOW`, etc | **ebuilder-eb-sql-symbols** | Symbol table, usage rules, examples              |
| Handle cross-database SQL differences                            | **ebuilder-eb-sql-symbols** | Engine-target objects, placeholders, portability |
| Write `query-item-by-id`                                         | **ebuilder-sql-query**      | Query blueprint, responseType, field mapping     |
| Configure pagination for query results                           | **ebuilder-sql-query**      | `pre` steps, `propType`, input parameters        |
| Write `mutation-update-item`                                     | **ebuilder-sql-mutation**   | Mutation blueprint, transaction isolation, steps |
| Chain multiple INSERT/UPDATE operations                          | **ebuilder-sql-mutation**   | Multi-step orchestration, save strategies        |
| Understand parameter binding safety                              | **ebuilder-eb-sql-symbols** | Parameter binding, input param schema            |
| Hide sensitive fields from output                                | **ebuilder-sql-query**      | `hideProps` for field redaction                  |
| Add error handling for 409 Conflict                              | **ebuilder-sql-mutation**   | `catchErrors` mapping, response definition       |

---

## File Structure

```
.github/skills/
├── ebuilder-sql-safe-patterns/
│   └── SKILL.md                    ← You are here (hub)
├── ebuilder-eb-sql-symbols/
│   └── SKILL.md                    ← Symbols, portability, parameters
├── ebuilder-sql-query/
│   └── SKILL.md                    ← Read-only query generation
└── ebuilder-sql-mutation/
    └── SKILL.md                    ← Data-modifying mutation generation
```

---

## Deterministic Checklist (Universal)

**Before committing any `configs/sql.*.yml` file:**

1. ✅ File location: `configs/sql.*.yml`
2. ✅ Root keys: only `query` or `mutation`
3. ✅ Query names: match `^query-[a-zA-Z0-9-]+$`
4. ✅ Mutation names: match `^mutation-[a-zA-Z0-9-]+$`
5. ✅ All user inputs bound via parameters (no interpolation)
6. ✅ `datasource` references valid entry in `configs/app.yml`
7. ✅ `authorize` policy names exist in `configs/security.yml`
8. ✅ Called tasks/queries/mutations actually exist
9. ✅ Use `$EB_*` symbols for cross-database portability
10. ✅ No deprecated keys: `param`, `op`, `value`, `throw`, `silentThrow`

---

## Why Split?

The original monolithic SKILL.md was **~700 lines**, covering:

- Symbol reference (120 lines)
- Query patterns (250 lines)
- Mutation patterns (330 lines)
- Shared configuration (configuration tables, etc)

**Splitting improves:**

- **Discoverability**: Find the exact guidance you need quickly
- **Maintainability**: Update symbols, queries, or mutations independently
- **Clarity**: Each skill has a laser-focused scope
- **Examples**: More targeted examples per skill context

---

## Still Have Doubts?

Ask yourself:

1. **"I need to understand how DB engines differ"** → `ebuilder-eb-sql-symbols`
2. **"I'm building a SELECT query"** → `ebuilder-sql-query`
3. **"I'm building an INSERT/UPDATE/DELETE chain"** → `ebuilder-sql-mutation`
4. **"I need to know what `$EB_PLACEHOLDER` does"** → `ebuilder-eb-sql-symbols`
5. **"How do I hide the password field from response?"** → `ebuilder-sql-query`
6. **"How do I handle transactions?"** → `ebuilder-sql-mutation`

If you're **still unsure**, start with **ebuilder-eb-sql-symbols** to ground yourself in terminology and safety principles. Then choose the query/mutation skill based on your operation type.
