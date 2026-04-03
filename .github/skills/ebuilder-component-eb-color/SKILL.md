---
name: ebuilder-component-eb-color
description: 'Deep component skill for eb-color. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-color in eBuilder component YAML.'
---

# eb-color Component Skill

## General Docs

Use `eb-color` to render a input type color in form. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/form/EBColor

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
        - name: default
          defaultValue: '#ffffff'
          ebFormControl:
            name: eb-color
            label: Default
            props:
              autoFocus: true
        - name: disable
          defaultValue: '#ace411'
          ebFormControl:
            name: eb-color
            label: Disable
            props:
              disable: true
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
