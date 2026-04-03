---
name: ebuilder-component-eb-checkbox-group
description: 'Deep component skill for eb-checkbox-group. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-checkbox-group in eBuilder component YAML.'
---

# eb-checkbox-group Component Skill

## General Docs

Use `eb-checkbox-group` for selecting multiple values from several options. Simple `eb-checkbox-group` usage with simple options config in `option` object. Each property of that object is an option, property name is option value and property value is option label. Form field is options separated by a character string, then you can use `valueSeparator` to make it Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/form/EBCheckboxGroup

## Main Props

| name           | type                                                                                                                                                                           | description                 |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------- |
| disable        | boolean                                                                                                                                                                        | Whether disable.            |
| eventToRefresh | string                                                                                                                                                                         | Specifies event to refresh. |
| options        | Record<string \| number, string> \| { label: string; value: string \| number; }[]                                                                                              | Specifies options.          |
| source         | { data?: Record<string, unknown>[]; query?: QueryConfig; task?: TaskConfig; label: string \| (($entry: unknown) => string); value: string; params?: Record<string, unknown>; } | Specifies source.           |
| spans          | ColSpans                                                                                                                                                                       | Specifies spans.            |
| valueSeparator | string                                                                                                                                                                         | Specifies value separator.  |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: checkboxGroup
          ebFormControl:
            name: eb-checkbox-group
            label: CheckboxGroup
            props:
              options:
                0: Option 1
                1: Option 2
                2: Option 3
                3: Option 4
```

### Story Example 2

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: checkboxGroup
          defaultValue: '0;2;3'
          ebFormControl:
            name: eb-checkbox-group
            label: CheckboxGroup
            props:
              mode: multiple
              valueSeparator: ';'
              options:
                0: Option 1
                1: Option 2
                2: Option 3
                3: Option 4
```

### Story Example 3

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: disable
          ebFormControl:
            name: eb-checkbox-group
            label: Disabled
            props:
              disable: true
              options:
                0: Option 1
                1: Option 2
                2: Option 3
                3: Option 4
```

### Story Example 4

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: data_source
          ebFormControl:
            name: eb-checkbox-group
            label: CheckboxGroup
            props:
              source:
                label: name
                value: id
                data:
                  - name: Option 1
                    id: 0
                  - name: Option 2
                    id: 1
                  - name: Option 3
                    id: 2
                  - name: Option 4
                    id: 3
```

### Story Example 5

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: data_source
          ebFormControl:
            name: eb-checkbox-group
            label: CheckboxGroup
            props:
              source:
                label: name
                value: id
                query:
                  name: get-options-for-checkbox-group
                  params:
                    userId: 1
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
