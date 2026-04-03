---
name: ebuilder-component-eb-file
description: 'Deep component skill for eb-file. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-file in eBuilder component YAML.'
---

# eb-file Component Skill

## General Docs

Use `eb-text` in form field for a files drag and drop selection field. Simple usages of `eb-file` Disabled state with option `disable : true` `eb-file` supports multiple files selection together with files validations, by default only 1 file in any type and less than 10MB is accepted. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/form/EBFile

## Main Props

| name     | type    | description          |
| -------- | ------- | -------------------- |
| accept   | string  | Specifies accept.    |
| disable  | boolean | Whether disable.     |
| maxFiles | number  | Specifies max files. |
| maxSize  | number  | Specifies max size.  |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: file
          ebFormControl:
            name: eb-file
            label: File
```

### Story Example 2

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: file
          ebFormControl:
            name: eb-file
            label: File
            props:
              disable: true
```

### Story Example 3

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: file
          ebFormControl:
            name: eb-file
            label: File
            props:
              maxFiles: 5
              maxSize: 2
              accept: image/*
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
