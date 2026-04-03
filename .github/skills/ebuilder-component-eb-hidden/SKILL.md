---
name: ebuilder-component-eb-hidden
description: 'Deep component skill for eb-hidden. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-hidden in eBuilder component YAML.'
---

# eb-hidden Component Skill

## General Docs

Use `eb-hidden` in case you want to have a hidden field in form model. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/form/EBHidden

## Main Props

| name | type   | description     |
| ---- | ------ | --------------- |
| name | string | Specifies name. |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: div
    children:
      - name: eb-form
        props:
          eventToSubmit: eb-hidden-submit
          model:
            custom:
              hiddenField: Hidden Field
          fields:
            - name: hiddenField
              ebFormControl:
                name: eb-hidden
          on:
            submit:
              emit:
                name: eb-toast
                params:
                  message: '${{ `Form submitted with: hiddenField=${$event.model.hiddenField}` }}'
      - name: eb-button
        props:
          text: Submit
          on:
            click:
              emit: eb-hidden-submit
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
