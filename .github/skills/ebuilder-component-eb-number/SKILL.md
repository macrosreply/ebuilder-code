---
name: ebuilder-component-eb-number
description: 'Deep component skill for eb-number. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-number in eBuilder component YAML.'
---

# eb-number Component Skill

## General Docs

Use `eb-number` to binding input numberic-only in form Some usages of `eb-number` You can set range Min/Max value Display value within it's situation with `formatter`, and we usually use `parser` at the same time. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/form/EBNumber

## Main Props

| name         | type                        | description                                                                                               |
| ------------ | --------------------------- | --------------------------------------------------------------------------------------------------------- |
| autoFocus    | boolean                     | Whether auto focus.                                                                                       |
| disable      | boolean                     | Whether disable.                                                                                          |
| formatter    | (value: unknown) => unknown | Callback for formatter.                                                                                   |
| hideControls | boolean                     | Show hide number +/- control buttons of browser and keyboard up/down to increase/descrease number feature |
| max          | number                      | Specifies max.                                                                                            |
| min          | number                      | Specifies min.                                                                                            |
| on.blur      | eBuilder events emit        | Listener for blur event.                                                                                  |
| on.enter     | eBuilder events emit        | Listener for enter event.                                                                                 |
| on.keyDown   | eBuilder events emit        | Listener for keyDown event.                                                                               |
| on.keyUp     | eBuilder events emit        | Listener for keyUp event.                                                                                 |
| parser       | (value: unknown) => unknown | Callback for parser.                                                                                      |
| suffix       | string                      | Specifies suffix.                                                                                         |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: number
          ebFormControl:
            name: eb-number
            label: Number
```

### Story Example 2

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: basic
          ebFormControl:
            name: eb-number
            label: Basic
            props:
              hideControl: true
        - name: hideControl
          ebFormControl:
            name: eb-number
            label: Hide browser up/down controls
            props:
              hideControl: true
        - name: autoFocus
          ebFormControl:
            name: eb-number
            label: Auto Focus
            props:
              autoFocus: true
        - name: disable
          ebFormControl:
            name: eb-number
            label: Disabled
            props:
              disable: true
        - name: suffix
          ebFormControl:
            name: eb-number
            label: Suffix
            props:
              suffix: %
```

### Story Example 3

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: minMax
          ebFormControl:
            name: eb-number
            label: Number
            props:
              min: 10
              max: 100
```

### Story Example 4

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: format
          ebFormControl:
            name: eb-number
            label: Number
            props:
              formatter: "${{ (value) => value ? value + '%' : '' }}"
              parser: "${{ (value) => value.replace('%', '') }}"
```

### Story Example 5

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: blur
          ebFormControl:
            name: eb-number
            label: Blur
            props:
              on:
                blur:
                  emit:
                    name: eb-toast
                    params:
                      message: '${{ `On blur - value: ${$event.target?.value}` }}'
        - name: press_enter
          ebFormControl:
            name: eb-number
            label: Press Enter
            props:
              on:
                enter:
                  emit:
                    name: eb-toast
                    params:
                      message: '${{ `On press enter: key ${$event.key}` }}'
        - name: key_up
          ebFormControl:
            name: eb-number
            label: Key Up
            props:
              on:
                keyUp:
                  emit:
                    name: eb-toast
                    params:
                      message: '${{ `On key up - key ${$event.key}` }}'
        - name: key_down
          ebFormControl:
            name: eb-number
            label: Key Down
            props:
              on:
                keyDown:
                  emit:
                    name: eb-toast
                    params:
                      message: '${{ `On key down - key ${$event.key}` }}'
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
