---
name: ebuilder-component-eb-text-area
description: 'Deep component skill for eb-text-area. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-text-area in eBuilder component YAML.'
---

# eb-text-area Component Skill

## General Docs

Use `eb-textarea` in form field for a multi-line input Simple usages of `eb-textarea` `eb-textarea` resizes its height automatically based on the current content and with default configurations `minRows: 5` and `maxRows: 20`. You can update these props with values you want. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/form/EBTextArea

## Main Props

| name      | type    | description         |
| --------- | ------- | ------------------- |
| autoFocus | boolean | Whether auto focus. |
| disable   | boolean | Whether disable.    |
| maxRows   | number  | Specifies max rows. |
| minRows   | number  | Specifies min rows. |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: default
          ebFormControl:
            name: eb-textarea
            label: Default
        - name: auto_focus
          ebFormControl:
            name: eb-textarea
            label: Auto Focus TextArea
            props:
              autoFocus: true
        - name: disable
          ebFormControl:
            name: eb-textarea
            label: Disable TextArea
            props:
              disable: true
```

### Story Example 2

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: default
          defaultValue: "${{ Array.from({ length: 15 }).map((_, i) => i + 1).join('\n') }}"
          ebFormControl:
            name: eb-textarea
            label: Textarea
            props:
              minRows: 2
              maxRows: 10
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
