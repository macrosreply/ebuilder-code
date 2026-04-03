---
name: ebuilder-component-eb-password
description: 'Deep component skill for eb-password. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-password in eBuilder component YAML.'
---

# eb-password Component Skill

## General Docs

Use `eb-password` in form field for a input type of password. Simple usages of `eb-password` Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/form/EBPassword

## Main Props

| name      | type    | description         |
| --------- | ------- | ------------------- |
| autoFocus | boolean | Whether auto focus. |
| disable   | boolean | Whether disable.    |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: password
          ebFormControl:
            name: eb-password
            label: Password
        - name: autoFocus
          ebFormControl:
            name: eb-password
            label: Auto Focus Password
            props:
              autoFocus: true
        - name: disable
          ebFormControl:
            name: eb-password
            label: Disable Password
            props:
              disable: true
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
