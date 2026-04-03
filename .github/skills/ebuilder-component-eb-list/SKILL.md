---
name: ebuilder-component-eb-list
description: 'Deep component skill for eb-list. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-list in eBuilder component YAML.'
---

# eb-list Component Skill

## General Docs

An `eb-list` can be used to display content related to a single subject. The content can consist of multiple elements of varying type and size For rendering `eb-list`, `children` now will be used for rendering each element. From its context, you can get element value with `$ctx.entry` or element index with `$ctx.entryIndex` Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBList

## Main Props

| name       | type                                       | description                 |
| ---------- | ------------------------------------------ | --------------------------- |
| border     | boolean                                    | Whether border.             |
| children   | { default?: ($event: unknown) => void; }   | Nested child configuration. |
| footerText | string                                     | Specifies footer text.      |
| headerText | string                                     | Specifies header text.      |
| rowKey     | string                                     | Specifies row key.          |
| source     | { data?: Record []; query?: QueryConfig; } | Specifies source.           |
| style      | CSSProperties                              | Inline style overrides.     |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-list
    props:
      rowKey: id
      source:
        data:
          - id: 1
            title: Item 1
          - id: 2
            title: Item 2
          - id: 3
            title: Item 3
          - id: 4
            title: Item 4
          - id: 5
            title: Item 5
    children:
      - name: span
        children: ${{ `(#${$ctx.$parent.entry.id}) ${$ctx.$parent.entry.title}` }}
```

### Story Example 2

```yaml
children:
  - name: eb-list
    props:
      headerText: Header list
      footerText: Footer list
      rowKey: id
      source:
        data:
          - id: 1
            title: Item 1
          - id: 2
            title: Item 2
          - id: 3
            title: Item 3
          - id: 4
            title: Item 4
          - id: 5
            title: Item 5
    children:
      - name: span
        children: ${{ `(#${$ctx.$parent.entry.id}) ${$ctx.$parent.entry.title}` }}
```

### Story Example 3

```yaml
children:
  - name: eb-list
    props:
      style:
        padding: 10px
        border: '1px dashed blue'
      border: false
      rowKey: id
      source:
        data:
          - id: 1
            title: Item 1
          - id: 2
            title: Item 2
          - id: 3
            title: Item 3
          - id: 4
            title: Item 4
          - id: 5
            title: Item 5
    children:
      - name: span
        children: ${{ `(#${$ctx.$parent.entry.id}) ${$ctx.$parent.entry.title}` }}
```

### Story Example 4

```yaml
children:
  - name: eb-list
    props:
      rowKey: id
      source:
        query:
          name: get-list-item
          params: null
    children:
      - name: span
        children: ${{ `(#${$ctx.$parent.entry.id}) ${$ctx.$parent.entry.title}` }}
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
