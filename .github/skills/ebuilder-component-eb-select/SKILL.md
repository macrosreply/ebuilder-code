---
name: ebuilder-component-eb-select
description: 'Deep component skill for eb-select. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-select in eBuilder component YAML.'
---

# eb-select Component Skill

## General Docs

Use `eb-select` to select single state or multiple state from multiple options. Simple `eb-select` usage with simple options config in `option` object. Each property of that object is an option, property name is option value and property value is option label. `eb-select` supports 2 types of multiple selections, with `mode: multiple` users can select option from list only Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/form/EBSelect

## Main Props

| name            | type                                                                                                                                                                                                | description                                                                                                                                                                                        |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| autoFocus       | boolean                                                                                                                                                                                             | Whether auto focus.                                                                                                                                                                                |
| disable         | boolean                                                                                                                                                                                             | Whether disable.                                                                                                                                                                                   |
| eventToRefresh  | string                                                                                                                                                                                              | Specifies event to refresh.                                                                                                                                                                        |
| fixedTopOptions | { label?: string; value: string \| number; }[]                                                                                                                                                      | Specifies fixed top options.                                                                                                                                                                       |
| mode            | "multiple" \| "tags"                                                                                                                                                                                | Specifies mode.                                                                                                                                                                                    |
| options         | Record<string \| number, string> \| { label: string; value: string \| number; }[]                                                                                                                   | Specifies options.                                                                                                                                                                                 |
| popupClassName  | string                                                                                                                                                                                              | Specifies popup class name.                                                                                                                                                                        |
| rawObject       | boolean                                                                                                                                                                                             | If true, the value will be returned as a raw object instead of a value of the `value` field. This is useful when you want to get the entire object from the source data instead of just the value. |
| showClear       | boolean                                                                                                                                                                                             | Whether show clear.                                                                                                                                                                                |
| showRefresh     | boolean                                                                                                                                                                                             | Whether show refresh.                                                                                                                                                                              |
| showSearch      | boolean                                                                                                                                                                                             | Whether show search.                                                                                                                                                                               |
| source          | { data?: Record<string, unknown>[]; query?: QueryConfig; task?: TaskConfig; label: string \| (($entry: unknown) => string); value: string; autoSelectOnFirstLoad?: boolean; params?: Record<...>; } | Specifies source.                                                                                                                                                                                  |
| style           | CSSProperties                                                                                                                                                                                       | Inline style overrides.                                                                                                                                                                            |
| valueSeparator  | string                                                                                                                                                                                              | Specifies value separator.                                                                                                                                                                         |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: select
          ebFormControl:
            name: eb-select
            label: Select
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
        - name: select
          ebFormControl:
            name: eb-select
            label: Select
            props:
              mode: multiple
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
        - name: select
          ebFormControl:
            name: eb-select
            label: Select
            props:
              mode: tags
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
        - name: select
          defaultValue: '0;2;3'
          ebFormControl:
            name: eb-select
            label: Select
            props:
              mode: multiple
              valueSeparator: ';'
              options:
                0: Option 1
                1: Option 2
                2: Option 3
                3: Option 4
```

### Story Example 5

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: hideSearch
          ebFormControl:
            name: eb-select
            label: Hidden Search
            props:
              showSearch: false
              options:
                0: Option 1
                1: Option 2
                2: Option 3
                3: Option 4
        - name: autoFocusSelect
          ebFormControl:
            name: eb-select
            label: Auto Focus Select
            props:
              autoFocus: true
              options:
                0: Option 1
                1: Option 2
                2: Option 3
                3: Option 4
        - name: disableSelect
          ebFormControl:
            name: eb-select
            label: Disabled Select
            props:
              disable: true
              options:
                0: Option 1
                1: Option 2
                2: Option 3
                3: Option 4
        - name: styleSelect
          ebFormControl:
            name: eb-select
            label: Style Select
            props:
              style:
                fontSize: 20px
                color: orange
              options:
                0: Option 1
                1: Option 2
                2: Option 3
                3: Option 4
```

### Story Example 6

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: data_source
          ebFormControl:
            name: eb-select
            label: Select
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

### Story Example 7

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: data_source
          ebFormControl:
            name: eb-select
            label: Select
            props:
              source:
                label: name
                value: id
                query:
                  name: get-options-for-select
                  params:
                    userId: 1
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
