---
name: ebuilder-component-eb-card
description: 'Deep component skill for eb-card. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-card in eBuilder component YAML.'
---

# eb-card Component Skill

## General Docs

The card can be used to display content related to a single subject. The content can consist of multiple elements. A basic card with `icon`, `title` and `description` Enabled lift up style when hovering card with prop `hoverable` Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBCard

## Main Props

| name        | type                 | description            |
| ----------- | -------------------- | ---------------------- |
| description | string               | Specifies description. |
| hoverable   | boolean              | Whether hoverable.     |
| icon        | string               | Specifies icon.        |
| on.click    | eBuilder events emit | Callback for click.    |
| title       | string               | Specifies title.       |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-card
    props:
      title: Card title
      description: Card description
      on:
        click:
          emit:
            name: eb-toast
            params:
              message: Clicked on card
```

### Story Example 2

```yaml
children:
  - name: eb-card
    props:
      title: Card title
      description: Card description
      icon: fas,user
      on:
        click:
          emit:
            name: eb-toast
            params:
              message: Clicked on card
```

### Story Example 3

```yaml
children:
  - name: eb-card
    props:
      title: Card title
      description: Card description
      hoverable: true
      on:
        click:
          emit:
            name: eb-toast
            params:
              message: Clicked on card
```

### Story Example 4

```yaml
children:
  - name: eb-card
    props:
      title: Card actions
      description: Card actions
      hoverable: true
      icon: fas,cloud
    children:
      - name: span
        slot: card-action-1
        children:
          - name: eb-icon
            props:
              icon: fas,list
      - name: span
        slot: card-action-2
        children:
          - name: eb-icon
            props:
              icon: fas,bookmark
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
