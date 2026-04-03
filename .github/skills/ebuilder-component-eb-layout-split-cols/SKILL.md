---
name: ebuilder-component-eb-layout-split-cols
description: 'Deep component skill for eb-layout-split-cols. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-layout-split-cols in eBuilder component YAML.'
---

# eb-layout-split-cols Component Skill

## General Docs

Use `eb-layout-split-cols` to create a layout with multiple columns that can be resized and rearranged. This component is useful for building complex layouts with a flexible structure. Simple usages of `eb-layout-split-cols` {' '} Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/layouts/EBLayoutSplitCols

## Main Props

| name  | type                   | description      |
| ----- | ---------------------- | ---------------- |
| cache | EBLayoutSplitColsCache | Specifies cache. |
| cols  | EBLayoutSplitCol[]     | Specifies cols.  |

## Nested Props

### EBLayoutSplitCol

| name    | type    | description         |
| ------- | ------- | ------------------- |
| key     | string  | Specifies key.      |
| size    | number  | Specifies size.     |
| minSize | number  | Specifies min size. |
| maxSize | number  | Specifies max size. |
| hide    | boolean | Whether hide.       |

### EBLayoutSplitColsCache

| name | type   | description    |
| ---- | ------ | -------------- |
| key  | string | Specifies key. |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-button
    props:
      text: Toggle col3
      on:
        click:
          emit:
            handler: |
              ${{
                $rootVars.col3Visible = !$rootVars.col3Visible
              }}
  - name: eb-layout-split-cols
    props:
      cols:
        - key: col1
          minSize: 5
        - key: col2
          minSize: 5
        - key: col3
          minSize: 5
          hide: ${{ !$rootVars.col3Visible }}
        - key: col4
          minSize: 5
    children:
      - name: div
        slot: col1
        children: This is col1 content
      - name: div
        slot: col2
        children: This is col2 content
      - name: div
        slot: col3
        children: This is col3 content
      - name: div
        slot: col4
        children: This is col4 content
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
