---
name: ebuilder-architecture-core
description: 'Core eBuilder architecture and folder responsibility guidance. USE FOR: decide target folder, structure a new feature end-to-end, map UI-task-sql flow, enforce eBuilder naming conventions, avoid non-framework scaffolding. DO NOT USE FOR: deep SQL optimization details or translation-only workflows.'
---

# eBuilder Architecture Core

## Purpose

Use this skill to decide where code/config belongs and to keep feature work aligned with eBuilder architecture.

## Core Rules

- Follow 8-folder boundaries strictly.
- Start with YAML-first design.
- Escalate to `externals/` or `dll/` only with clear complexity reasons.
- Keep data, logic, and UI responsibilities separated.

## Deterministic Checklist

1. Pick target folder from `configs/`, `components/`, `externals/`, `dll/`, `locale/`, `templates/`.
2. Confirm naming follows task/query/mutation/locale conventions.
3. Ensure UI flow maps to task/query/mutation targets.
4. Verify no suggestion creates files outside the framework structure.
5. Add localization implications for UI text changes.
