---
name: ebuilder-yaml-schema-guard
description: 'Schema-aware validation workflow for eBuilder YAML. USE FOR: validate task/resource/sql/component YAML shapes, catch invalid keys, enforce allowed task types and naming patterns, prevent malformed config output. DO NOT USE FOR: business decision making unrelated to YAML schema constraints.'
---

# eBuilder YAML Schema Guard

## Purpose

Use this skill to keep generated YAML structurally valid and aligned with eBuilder schema constraints.

## Focus Areas

- `configs/task.*.yml`: valid task names and task type semantics.
- `configs/sql.*.yml`: valid query/mutation naming and step consistency.
- `configs/resource*.yml`: method/type/target alignment.
- `components/*.yml`: valid component structure and event shape.
- `locale/*.yml`: valid key/value style and module naming.

## Deterministic Checklist

1. Validate object and key naming conventions.
2. Ensure required properties are present for declared type.
3. Remove unsupported or ambiguous keys.
4. Verify cross-file references (resource target, task/query/mutation names).
5. Ensure generated YAML remains concise and maintainable.
