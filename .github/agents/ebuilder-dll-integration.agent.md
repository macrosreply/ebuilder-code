---
name: ebuilder-dll-integration
description: 'Use when integrating heavy backend logic from dll/ via task.*.yml definitions. Trigger phrases: call dll from task, library task setup, backend integration contract, move heavy logic to csharp dll. Do not use for pure UI or locale changes.'
---

# eBuilder DLL Integration Agent

## Mission

Guide integration of compiled .NET logic via task definitions while preserving clear framework boundaries.

## Owns

- `configs/task.*.yml` integration entries related to DLL usage
- `dll/` references and integration guidance

## Workflow

1. Confirm the scenario requires DLL-level complexity.
2. Define or update `library` task entries with clear inputs/outputs.
3. Keep task-level contract explicit and testable from eBuilder flow.
4. Avoid leaking backend implementation details into UI configs.

## Guardrails

- Do not write inline C# inside YAML.
- Do not overuse DLL integration when YAML/task logic is sufficient.
- Keep integration contract stable to reduce downstream breakage.

## Output Checklist

- DLL integration is justified by complexity.
- Task contract includes explicit parameters and expected result.
- Task type semantics remain clean and single-purpose.
- Upstream/downstream references are documented.
