---
name: ebuilder-component-eb-checkbox
description: 'Deep component skill for eb-checkbox. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-checkbox in eBuilder component YAML.'
---

# eb-checkbox Component Skill

## General Docs

Use `eb-checkbox` to render a single checkbox in form. For now we don't support multiple checkboxes yet, please use `eb-select` for multiple selections form field. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/form/EBCheckbox

## Main Props

| name        | type                            | description                     |
| ----------- | ------------------------------- | ------------------------------- |
| disable     | boolean                         | Whether disable.                |
| inlineLabel | boolean                         | Whether inline label.           |
| options     | (string \| number \| boolean)[] | mapping value for [true, false] |
| title       | string                          | Specifies title.                |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: default
          ebFormControl:
            name: eb-checkbox
            label: Default
            props:
              title: Default checkbox field title
              options: [1, 0]
        - name: checked
          defaultValue: 1
          ebFormControl:
            name: eb-checkbox
            label: Checked
            props:
              options: [1, 0]
        - name: disabled_checkbox
          defaultValue: 1
          ebFormControl:
            name: eb-checkbox
            label: Disabled Checkbox
            props:
              disable: true
              options: [1, 0]
        - name: custom
          on:
            change:
              emit:
                name: eb-toast
                params:
                  message: ${{ `Checkbox field value has changed from ${$event.oldValue} to ${$event.newValue}` }}
          ebFormControl:
            name: eb-checkbox
            label: Custom values for checked and unchecked states
            props:
              options: ['Checked', 'Unchecked']
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
