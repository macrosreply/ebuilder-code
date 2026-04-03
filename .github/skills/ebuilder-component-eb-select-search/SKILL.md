---
name: ebuilder-component-eb-select-search
description: 'Deep component skill for eb-select-search. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-select-search in eBuilder component YAML.'
---

# eb-select-search Component Skill

## General Docs

Use `eb-select-search` need use `eb-query` to load options from database in `source` property `eb-select-search` works almost the same as `eb-select` so you can use `eb-select-search` in the same ways as `eb-select`. The differences between `eb-select` and `eb-select-seach` are: - `eb-select` loads and renders all options which are returned from `eb-query` and do filters on browser side only. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/form/EBSelectSearch

## Main Props

| name                  | type                                                                                                           | description                         |
| --------------------- | -------------------------------------------------------------------------------------------------------------- | ----------------------------------- |
| autoFocus             | boolean                                                                                                        | Whether auto focus.                 |
| disable               | boolean                                                                                                        | Whether disable.                    |
| eventToRefresh        | string                                                                                                         | Specifies event to refresh.         |
| filterOperator        | QueryFilterOperator                                                                                            | Specifies filter operator.          |
| minSearchTermLength   | number                                                                                                         | Specifies min search term length.   |
| mode                  | "multiple"                                                                                                     | Specifies mode.                     |
| numberOfSearchResults | number                                                                                                         | Specifies number of search results. |
| searchMode            | "filter" \| "param"                                                                                            | Specifies search mode.              |
| showRefresh           | boolean                                                                                                        | Whether show refresh.               |
| source                | { query: QueryConfig; label: string \| (($entry: unknown) => string); value: string; searchOnField?: string; } | Specifies source.                   |
| style                 | CSSProperties                                                                                                  | Inline style overrides.             |
| valueSeparator        | string                                                                                                         | Specifies value separator.          |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: querySelect
          ebFormControl:
            name: eb-select-search
            label: Select search
            props:
              searchMode: filter
              filterOperator: c
              minSearchTermLength: 3
              source:
                label: name
                value: id
                query:
                  name: get-options-for-select-search
                  params: null
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
