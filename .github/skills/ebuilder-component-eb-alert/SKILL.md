---
name: ebuilder-component-eb-alert
description: 'Deep component skill for eb-alert. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-alert in eBuilder component YAML.'
---

# eb-alert Component Skill

## General Docs

Use `eb-alert` for showing styled feedback messages. `eb-alert` supports built-in style messages Custom content with slot `default` Toggle visibility with `visible` prop Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBAlert

## Main Props

| name    | type                                        | description             |
| ------- | ------------------------------------------- | ----------------------- |
| message | string                                      | Specifies message.      |
| style   | CSSProperties                               | Inline style overrides. |
| type    | "success" \| "info" \| "warning" \| "error" | Specifies type.         |
| visible | boolean                                     | Whether visible.        |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-alert
    props:
      visible: true
      type: error
      message: Error text
  - name: eb-alert
    props:
      visible: true
      type: success
      message: Success text
  - name: eb-alert
    props:
      visible: true
      type: warning
      message: Warning text
  - name: eb-alert
    props:
      visible: true
      type: info
      message: Info text
```

### Story Example 2

```yaml
- name: eb-alert
            props:
              visible: true
              type: success
            children:
              - name: eb-typography
                props:
                  style:
                    color: red
                  message: This is the customized success text
```

### Story Example 3

```yaml
rootVars:
            customVisible: true
          ...
          children:
            - name: eb-alert
              props:
                visible: ${{ $rootVars.customVisible }}
                type: success
                message: Success text
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
