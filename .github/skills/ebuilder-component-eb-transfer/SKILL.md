---
name: ebuilder-component-eb-transfer
description: 'Deep component skill for eb-transfer. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-transfer in eBuilder component YAML.'
---

# eb-transfer Component Skill

## General Docs

Use `eb-transfer` to build multiple selection input field with an intuitive and efficient way In current version, you need to use `eb-query` for selected options column, `eb-transfer` only cares about the `valueKey` value to know which options from source options column are selected. The row contents in target column are getting from source options column. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/form/EBTransfer

## Main Props

| name            | type                                                                                                                                                                                                                             | description                   |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- |
| customSearch    | (item: unknown, searchTerm: string) => boolean                                                                                                                                                                                   | Callback for custom search.   |
| disable         | boolean                                                                                                                                                                                                                          | Whether disable.              |
| draggable       | boolean                                                                                                                                                                                                                          | Whether draggable.            |
| eventToMoveItem | string                                                                                                                                                                                                                           | Specifies event to move item. |
| eventToSelect   | string                                                                                                                                                                                                                           | Specifies event to select.    |
| height          | string \| number                                                                                                                                                                                                                 | Specifies height.             |
| showSearch      | boolean                                                                                                                                                                                                                          | Whether show search.          |
| source          | { title?: string; query?: QueryConfig; options?: Record<string, unknown>[]; valueKey: string; labelKey?: string; children?: { default: (props: Record<string, unknown>) => unknown; }; enableReorder?: boolean; on?: { ...; }; } | Specifies source.             |
| target          | { title?: string; query?: QueryConfig; valueKey: string; enableReorder?: boolean; syncSourceOrder?: boolean; }                                                                                                                   | Specifies target.             |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: transfer
          ebFormControl:
            name: eb-transfer
            label: Transfer
            props:
              source:
                title: All
                valueKey: id
                labelKey: name
                query:
                  name: eb-transfer-get-options-for-source
                  params: null
              target:
                title: Selected
                valueKey: id
                query:
                  name: eb-transfer-get-options-for-target
                  params: null
```

### Story Example 2

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: transfer
          ebFormControl:
            name: eb-transfer
            label: Transfer
            props:
              height: 300
              source:
                title: All
                valueKey: id
                labelKey: name
                options:
                  - key: 0
                    name: Option 0
                    id: 0
                  - key: 1
                    name: Option 1
                    id: 1
                  - key: 2
                    name: Option 2
                    id: 2
                  - key: 3
                    name: Option 3
                    id: 3
                  - key: 4
                    name: Option 4
                    id: 4
                  - key: 5
                    name: Option 5
                    id: 5
              target:
                title: Selected
                valueKey: id
                query:
                  name: eb-transfer-get-options-for-target
                  params: null
```

### Story Example 3

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: transfer
          ebFormControl:
            name: eb-transfer
            label: Transfer
            props:
              source:
                title: All
                valueKey: id
                labelKey: name
                query:
                  name: eb-transfer-get-options-for-source
                  params: null
                children:
                  - name: span
                    children: ${{ `(#${$ctx.$parent.id}) ${$ctx.$parent.name}` }}
              target:
                title: Selected
                valueKey: id
                query:
                  name: eb-transfer-get-options-for-target
                  params: null
```

### Story Example 4

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: transfer
          ebFormControl:
            name: eb-transfer
            label: Transfer
            props:
              draggable: true
              source:
                title: All
                valueKey: id
                labelKey: name
                query:
                  name: eb-transfer-get-options-for-source
                  params: null
                children:
                  - name: span
                    children: ${{ `(#${$ctx.$parent.id}) ${$ctx.$parent.name}` }}
              target:
                title: Selected
                valueKey: id
                query:
                  name: eb-transfer-get-options-for-target
                  params: null
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
