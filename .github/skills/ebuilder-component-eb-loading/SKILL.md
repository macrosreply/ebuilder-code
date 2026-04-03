---
name: ebuilder-component-eb-loading
description: 'Deep component skill for eb-loading. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-loading in eBuilder component YAML.'
---

# eb-loading Component Skill

## General Docs

Use `eb-loading` for showing styled feedback messages. Normal usages Custom styles Show/hide loading icon Block/release a content on loading Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBLoading

## Main Props

| name     | type          | description             |
| -------- | ------------- | ----------------------- |
| inline   | boolean       | Whether inline.         |
| label    | string        | Specifies label.        |
| spinning | boolean       | Whether spinning.       |
| style    | CSSProperties | Inline style overrides. |
| visible  | boolean       | Whether visible.        |

## YAML Examples

### Story Example 1

```yaml
- name: eb-loading
```

### Story Example 2

```yaml
- name: eb-loading
  props:
    label: Loading resources...
    style:
      font-size: 30px
      color: red
```

### Story Example 3

```yaml
rootVars:
  isLoading: true

children:
  - name: eb-button
    props:
      text: "${{ $rootVars.isLoading ? 'Stop' : 'Start' }}"
      style:
        margin-right: 15px
      on:
        click:
          emit:
            name: eb-manual
            handler: |
              ${{
                () => {
                  $rootVars.isLoading = !$rootVars.isLoading
                }
              }}
  - name: eb-loading
    props:
      visible: ${{ $rootVars.isLoading }}
```

### Story Example 4

```yaml
rootVars:
  isLoading: true
children:
  - name: div
    children:
      - name: eb-button
        props:
          text: "${{ $rootVars.isLoading ? 'Stop' : 'Start' }}"
          style:
            margin-right: 15px
          on:
            click:
              emit:
                name: eb-manual
                handler: |
                  ${{
                    () => {
                      $rootVars.isLoading = !$rootVars.isLoading
                    }
                  }}
  - name: eb-loading
    props:
      spinning: ${{ $rootVars.isLoading }}
    children:
      - name: div
        props:
          height: 100px
          line-height: 100px
          text-align: center
        children: Content is blocked on loading
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
