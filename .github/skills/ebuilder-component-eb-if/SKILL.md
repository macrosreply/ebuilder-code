---
name: ebuilder-component-eb-if
description: 'Deep component skill for eb-if. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-if in eBuilder component YAML.'
---

# eb-if Component Skill

## General Docs

Use `eb-if` for conditional rendering. Conditionally rendering a block with array of conditions You can use a function which returns `boolean` to handler complex logics `eb-if` supports `else` slot for showing a block when conditions are failed Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBIf

## Main Props

| name      | type                         | description          |
| --------- | ---------------------------- | -------------------- |
| authorize | AuthorizeAction              | Specifies authorize. |
| checks    | (() => boolean) \| boolean[] | Specifies checks.    |

## YAML Examples

### Story Example 1

```yaml
rootVars:
  index: 2
  shouldShow: true

children:
  - name: eb-if
    props:
      checks:
        -  ${{ $rootVars.index === 2 }}
        -  ${{ $rootVars.shouldShow }}
    children:
      - name: div
        children: `if` content
```

### Story Example 2

```yaml
rootVars:
  index: 2
  shouldShow: true

children:
  - name: eb-if
    props:
      checks: |
        ${{
          () => {
            return $rootVars.index === 2 && $rootVars.shouldShow;
          }
        }}
    children:
      - name: div
        children: `if` content
```

### Story Example 3

```yaml
rootVars:
  index: 2
  shouldShow: true

children:
  - name: eb-button
    props:
      text: "${{ $rootVars.shouldShow ? 'Else' : 'If' }}"
      style:
        margin-bottom: 15px
      on:
        click:
          emit:
            name: eb-manual
            handler: |
              ${{
                () => {
                  $rootVars.shouldShow = !$rootVars.shouldShow
                }
              }}
  - name: eb-if
    props:
      checks:
        -  ${{ $rootVars.index === 2 }}
        -  ${{ $rootVars.shouldShow }}
    children:
      - name: div
        children: `if` content
      - name: div
        slot: else
        children: `else` content
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
