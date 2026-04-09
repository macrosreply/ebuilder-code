---
name: ebuilder-dll-integration
description: 'Use when implementing or refactoring C# handlers for library tasks in dll/ and wiring them to task.*.yml activators. Trigger phrases: library task csharp, write tasklibraryhandler, parse input params, singleton service in dll, transaction in task handler, wire task to dll handler. Do not use for pure task YAML schema authoring, SQL YAML, or UI markup.'
---

# eBuilder DLL Integration Agent

## Mission

Guide C# implementation and integration of `library` task handlers in `dll/` while preserving clear framework boundaries.

Primary execution guide:

- `.github/skills/ebuilder-library-task-csharp-patterns/SKILL.md` for standard `TaskLibraryHandler` implementation.
- `.github/skills/ebuilder-singleton-service-recommendation-patterns/SKILL.md` for recommending and designing `EBSingletonService` business-logic extraction.

## Owns

- `dll/` handler implementation patterns for `TaskLibraryHandler`
- `configs/task.*.yml` `activator` alignment (`assembly`, `class`)

## Workflow

1. Confirm the scenario requires DLL-level complexity beyond YAML/task composition.
2. Implement or refactor handler classes with stable input/output contract.
3. Keep `activator.assembly` and `activator.class` aligned with real source.
4. Apply validation, transaction, and singleton-service patterns where needed.
5. Avoid leaking backend implementation details into UI configs.

## Guardrails

- Do not write inline C# inside YAML.
- Do not overuse DLL integration when YAML/task logic is sufficient.
- Keep integration contract stable to reduce downstream breakage.
- Do not change task type semantics while doing C# refactors.

## Output Checklist

- DLL integration is justified by complexity.
- Task contract includes explicit parameters and expected result.
- Task type semantics remain clean and single-purpose.
- `activator` mapping points to existing assembly/class.
