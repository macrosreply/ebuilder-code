---
name: ebuilder-component-eb-text
description: 'Deep component skill for eb-text. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-text in eBuilder component YAML.'
---

# eb-text Component Skill

## General Docs

Use `eb-text` in form field for a normal text input Simple usages of `eb-text` `eb-text` supports 4 slots `addonAfter`, `addonBefore`, `prefix`, `suffix`, you can use them to customize the input field {' '} `eb-text` supports 3 events `blur` `keyUp` `keyDown`, in these events context you can access `$event` to get HTML event attributes. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/form/EBText

## Main Props

| name       | type                  | description                                                                                                                                          |
| ---------- | --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| autoFocus  | boolean               | Whether auto focus.                                                                                                                                  |
| disable    | boolean               | Whether disable.                                                                                                                                     |
| mask       | { pattern?: string; } | Specifies mask.                                                                                                                                      |
| on.blur    | eBuilder events emit  | Listener for blur event.                                                                                                                             |
| on.keyDown | eBuilder events emit  | Listener for keyDown event.                                                                                                                          |
| on.keyUp   | eBuilder events emit  | Listener for keyUp event.                                                                                                                            |
| trigger    | "blur" \| "change"    | Input event trigger type. Value 'blur' will trigger on blur event, 'change' will trigger on change event. If not specified, the default is 'change'. |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-form
    props:
      model:
        custom:
          maskDefault: '+7(111)222-33-44'
      fields:
        - name: default
          ebFormControl:
            name: eb-text
            label: Default Text
        - name: defaultTriggerBlur
          ebFormControl:
            name: eb-text
            label: 'Text (trigger: blur)'
        - name: auto_focus
          ebFormControl:
            name: eb-text
            label: Auto Focus Text
            props:
              autoFocus: true
        - name: disable
          ebFormControl:
            name: eb-text
            label: Disable Text
            props:
              disable: true
        - name: maskDefault
          ebFormControl:
            name: eb-text
            label: 'Masked Text: +{7}(000)000-00-00'
            props:
              mask:
                pattern: '+{7}(000)000-00-00'
              placeholder: '+7(___)___-__-__'
```

### Story Example 2

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: addonAfter
          ebFormControl:
            name: eb-text
            label: addonAfter slot
            children:
              - name: eb-icon
                slot: addonAfter
                props:
                  icon: fas,circle-info
        - name: addonBefore
          ebFormControl:
            name: eb-text
            label: addonBefore slot
            children:
              - name: eb-icon
                slot: addonBefore
                props:
                  icon: fas,circle-info
        - name: prefix
          ebFormControl:
            name: eb-text
            label: prefix slot
            children:
              - name: eb-icon
                slot: prefix
                props:
                  icon: fas,circle-info
        - name: suffix
          ebFormControl:
            name: eb-text
            label: suffix slot
            children:
              - name: eb-icon
                slot: suffix
                props:
                  icon: fas,circle-info
```

### Story Example 3

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: blur
          ebFormControl:
            name: eb-text
            label: Blur
            props:
              on:
                blur:
                  emit:
                    name: eb-toast
                    params:
                      message: '${{ `On blur - value: ${$event.target?.value}` }}'
        - name: key_up
          ebFormControl:
            name: eb-text
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
            name: eb-text
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
