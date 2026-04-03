---
name: ebuilder-component-eb-radio
description: 'Deep component skill for eb-radio. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-radio in eBuilder component YAML.'
---

# eb-radio Component Skill

## General Docs

Use `eb-radio` to select a single state from multiple options. Simple `eb-radio` usage with simple options config in `option` object. Each property of that object is an option, property name is option value and property value is option label. There are 2 `eb-radio` styles: `button` and `radio` Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/form/EBRadio

## Main Props

| name    | type                                                                                                    | description        |
| ------- | ------------------------------------------------------------------------------------------------------- | ------------------ |
| disable | boolean                                                                                                 | Whether disable.   |
| mode    | "button" \| "radio"                                                                                     | Specifies mode.    |
| options | Record<string \| number, string>                                                                        | Specifies options. |
| source  | { data?: Record<string, unknown>[]; query?: QueryConfig; label: string; icon?: string; value: string; } | Specifies source.  |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: radio
          ebFormControl:
            name: eb-radio
            label: Radio
            props:
              options:
                0: Radio 1
                1: Radio 2
                2: Radio 3
```

### Story Example 2

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: button
          ebFormControl:
            name: eb-radio
            label: Button
            props:
              mode: button
              options:
                0: Button 1
                1: Button 2
                2: Button 3
        - name: radio
          ebFormControl:
            name: eb-radio
            label: Radio
            props:
              mode: radio
              options:
                0: Radio 1
                1: Radio 2
                2: Radio 3
        - name: disabledButton
          ebFormControl:
            name: eb-radio
            label: Disabled buttons
            props:
              mode: button
              disable: true
              options:
                0: Button 1
                1: Button 2
                2: Button 3
        - name: disabledRadio
          ebFormControl:
            name: eb-radio
            label: Disabled radios
            props:
              mode: radio
              disable: true
              options:
                0: Radio 1
                1: Radio 2
                2: Radio 3
```

### Story Example 3

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: data_source
          ebFormControl:
            name: eb-radio
            label: Radio
            props:
              mode: radio
              source:
                label: name
                value: id
                data:
                  - name: LIFE INSURANCE
                    id: 100
                  - name: HEALTH INSURANCE B.
                    id: 101
                  - name: HEALTH INSURANCE L.
                    id: 102
```

### Story Example 4

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: data_source
          ebFormControl:
            name: eb-radio
            label: Radio
            props:
              mode: radio
              source:
                label: name
                value: id
                query:
                  name: get-options-for-radio
                  params:
                    userId: 1
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
