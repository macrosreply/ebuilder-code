---
name: ebuilder-component-eb-kanban-board
description: 'Deep component skill for eb-kanban-board. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-kanban-board in eBuilder component YAML.'
---

# eb-kanban-board Component Skill

## General Docs

Use `eb-kanban-board` to build a Kanban view with columns and swimlanes, and load cards from `eb-query` or `eb-task`. Basic Kanban board with: - columns driven by query filters (`todo`, `in-progress`, `done`) - multiple swimlanes using a shared `swimlaneBase` - custom card rendering in `item` slot - `item`: render each card item. Access current item with `$ctx.item`. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBKanbanBoard

## Main Props

| name                   | type                    | description                           |
| ---------------------- | ----------------------- | ------------------------------------- |
| cache                  | { key: string; }        | Specifies cache.                      |
| cols                   | EBKanbanBoardColumn[]   | Specifies cols.                       |
| eventToUpdateHeaderPos | string                  | Specifies event to update header pos. |
| numOfItemsPerLoad      | number                  | Specifies num of items per load.      |
| style                  | CSSProperties           | Inline style overrides.               |
| swimlaneBase           | Partial                 | Specifies swimlane base.              |
| swimlanes              | EBKanbanBoardSwimlane[] | Specifies swimlanes.                  |

## Nested Props

### EBKanbanBoardColumn

| name    | type          | description        |
| ------- | ------------- | ------------------ |
| key     | string        | Specifies key.     |
| title   | string        | Specifies title.   |
| filters | QueryFilter[] | Specifies filters. |

### EBKanbanBoardSwimlane

| name              | type                                                                                    | description                      |
| ----------------- | --------------------------------------------------------------------------------------- | -------------------------------- |
| swimlaneKey       | string                                                                                  | Specifies swimlane key.          |
| key               | string                                                                                  | Specifies key.                   |
| title             | string                                                                                  | Specifies title.                 |
| itemKey           | string                                                                                  | Specifies item key.              |
| colorKey          | string                                                                                  | Specifies color key.             |
| dataSource        | { type: "task"; name: string; params?: Record ; } \| (QueryConfig & { type: "query"; }) | Specifies data source.           |
| loadMore          | { field: string; direction: "ASC" \| "DESC"; }                                          | Specifies load more.             |
| numOfItemsPerLoad | number                                                                                  | Specifies num of items per load. |
| data              | Record                                                                                  | Specifies data.                  |
| eventToUpdateItem | string                                                                                  | Specifies event to update item.  |
| eventToDeleteItem | string                                                                                  | Specifies event to delete item.  |
| eventToAddItem    | string                                                                                  | Specifies event to add item.     |

## YAML Examples

### Story Example 1

```yaml
- name: eb-kanban-board
  props:
    style:
      height: 520px
    cache:
      key: eb-kanban-board-default
    eventToUpdateHeaderPos: eb-kanban-update-header-pos
    cols:
      - key: todo
        title: Todo
        filters:
          - field: status
            op: EQ
            value: todo
      - key: in-progress
        title: In Progress
        filters:
          - field: status
            op: EQ
            value: in-progress
      - key: done
        title: Done
        filters:
          - field: status
            op: EQ
            value: done
    swimlaneBase:
      itemKey: id
      colorKey: color
      dataSource:
        type: query
        name: eb-kanban-board-default
    swimlanes:
      - key: owner-1
        title: Owner 1
        data:
          subtitle: Marketing
        dataSource:
          type: query
          name: eb-kanban-board-default
          params:
            ownerId: 1
      - key: owner-2
        title: Owner 2
        data:
          subtitle: Engineering
        dataSource:
          type: query
          name: eb-kanban-board-default
          params:
            ownerId: 2
  children:
    - name: div
      slot: item
      props:
        style:
          padding: var(--padding-sm)
          border: 1px solid var(--color-split)
          borderRadius: var(--border-radius)
          backgroundColor: '#ffffff'
      children:
        - name: div
          props:
            style:
              fontWeight: bold
              marginBottom: 4px
          children: ${{ $ctx.$parent.$parent.item.title }}
        - name: div
          props:
            style:
              color: var(--color-text-secondary)
          children: ${{ $ctx.$parent.$parent.item.owner }}
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
