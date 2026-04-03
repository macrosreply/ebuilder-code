---
name: ebuilder-component-eb-search
description: 'Deep component skill for eb-search. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-search in eBuilder component YAML.'
---

# eb-search Component Skill

## General Docs

Use `eb-search` to provide a search input field with built-in autocomplete functionality. Simple usages of `eb-search` `eb-search` supports 3 events `select`, `search`, `enter`, in these events context you can get selected value via `$event.value` property. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/form/EBSearch

## Main Props

| name       | type                                | description                                                                                           |
| ---------- | ----------------------------------- | ----------------------------------------------------------------------------------------------------- |
| autoFocus  | boolean                             | Whether auto focus.                                                                                   |
| disable    | boolean                             | Whether disable.                                                                                      |
| icon       | string                              | Icon to show next to the search input. Default is a magnifying glass. Set to 'null' to hide the icon. |
| on.search  | eBuilder events emit                | Listener for search event.                                                                            |
| on.select  | eBuilder events emit                | Listener for select event.                                                                            |
| on.suggest | eBuilder events emit                | Listener for suggest event.                                                                           |
| options    | { value: string; label: string; }[] | Specifies options.                                                                                    |
| size       | "small" \| "middle" \| "large"      | Size of the search input. Possible values are 'small', 'middle', and 'large'. Default is 'middle'.    |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: default
          ebFormControl:
            name: eb-search
            label: Default search
            props:
              icon: 'fas,magnifying-glass-plus'
              autoFocus: true
              options:
                - value: 'option1'
                  label: 'Option 1'
                - value: 'option2'
                  label: 'Option 2'
                - value: 'option3'
                  label: 'Option 3'
              on:
                suggest:
                  emit:
                    name: eb-toast
                    params:
                      message: ${{ `on.suggest ${$event.value}` }}
                select:
                  emit:
                    name: eb-toast
                    params:
                      message: ${{ `on.select ${$event.value}` }}
                enter:
                  emit:
                    name: eb-toast
                    params:
                      message: ${{ `on.enter ${$event.value}` }}
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
