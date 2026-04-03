---
name: ebuilder-component-eb-grid-items
description: 'Deep component skill for eb-grid-items. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-grid-items in eBuilder component YAML.'
---

# eb-grid-items Component Skill

## General Docs

Use `eb-grid-items` for building grid UI of items with loading more feature. You need to config `datasource` prop, you can use `type: query` or `type: task` to load data from server. To enable loading more feature, you need to set `loadMore` prop, it contains the name of item field that you want the component to load more items which have the continue values of this field value in last item (`eb-grid-items` will build `eb-query` `orders` and `filters` based on this config). Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBGridItems

## Main Props

| name              | type                                                                                    | description                      |
| ----------------- | --------------------------------------------------------------------------------------- | -------------------------------- |
| cache             | { key: string; }                                                                        | Specifies cache.                 |
| dataSource        | { type: "task"; name: string; params?: Record ; } \| (QueryConfig & { type: "query"; }) | Specifies data source.           |
| eventToAddItem    | string                                                                                  | Specifies event to add item.     |
| eventToDeleteItem | string                                                                                  | Specifies event to delete item.  |
| eventToRefresh    | string                                                                                  | Specifies event to refresh.      |
| eventToUpdateItem | string                                                                                  | Specifies event to update item.  |
| itemKey           | string                                                                                  | Specifies item key.              |
| itemSpan          | { xs?: number; sm?: number; md?: number; lg?: number; xl?: number; xxl?: number; }      | Specifies item span.             |
| loadMore          | { field: string; direction: "ASC" \| "DESC"; }                                          | Specifies load more.             |
| numOfItemsPerLoad | number                                                                                  | Specifies num of items per load. |
| style             | CSSProperties                                                                           | Inline style overrides.          |

## YAML Examples

### Story Example 1

```yaml
- name: eb-grid-items
  props:
    style:
      height: 500px
    itemKey: id
    dataSource:
      type: query
      name: eb-grid-items-default
    loadMore:
      field: createdAt
      direction: ASC
    numOfItemsPerLoad: 10
    itemSpan:
      xs: 24
      sm: 12
      md: 8
      lg: 6
      xl: 4
      xxl: 3
  children:
    - name: div
      slot: item
      props:
        style:
          padding: var(--padding-md)
          border: 1px solid var(--color-border)
          margin: var(--padding-md)
      children:
        - name: div
          children: ${{ $ctx.$parent.$parent.item.fullName }}
        - name: div
          children: ${{ $ctx.$parent.$parent.item.createdAt }}
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
