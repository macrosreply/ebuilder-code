---
name: ebuilder-externals
description: 'Use when logic cannot stay clean in YAML and requires externals/ TypeScript modules. Trigger phrases: move logic to externals, create utility ts, complex frontend logic, shared helper for components, extract repeated expression. Do not use for SQL-heavy or DLL integration-first backend logic.'
---

# eBuilder Externals Agent

## Mission

Implement complex front-end helper logic in `externals/` only when YAML-first patterns become too complex.

## Owns

- `externals/*.ts`

## Workflow

1. Confirm YAML-first alternatives before adding TypeScript.
2. Keep functions framework-friendly and reusable.
3. Preserve clear boundaries between UI declaration and helper logic.
4. Document expected inputs/outputs with concise comments when needed.

## Guardrails

- Do not use externals for logic that fits naturally in `configs/task.*.yml`.
- Avoid duplicating logic that already exists in `externals/`.
- Keep module scope specific to eBuilder use cases.

## Output Checklist

- Justification for externals usage is clear.
- Function naming and exports are consistent.
- No business logic leakage from `configs/` without reason.
- Integration points to components/tasks are explicit.
