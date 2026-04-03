---
name: ebuilder-component-eb-collapse
description: 'Deep component skill for eb-collapse. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-collapse in eBuilder component YAML.'
---

# eb-collapse Component Skill

## General Docs

A content area which can be collapsed and expanded. A simple `eb-collapse` usage By default, `eb-collapse` rendes panel content lazily after clicking on header, you can set `forceRender: true` if you want to force it render at initial time You can customize UI with `style` prop Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBCollapse

## Main Props

| name            | type             | description                                                      |
| --------------- | ---------------- | ---------------------------------------------------------------- |
| cache           | { key: string; } | Specifies cache.                                                 |
| eventToToggle   | string           | Name of event which eb-collapse should subscribe to toggle state |
| expandByDefault | boolean          | Render expanded state at initial time                            |
| forceRender     | boolean          | Whether force render.                                            |
| style           | CSSProperties    | Inline style overrides.                                          |
| title           | string           | Specifies title.                                                 |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-collapse
    props:
      title: Panel header
    children:
      - name: div
        children: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam
```

### Story Example 2

```yaml
children:
  - name: eb-collapse
    props:
      title: Panel header
      forceRender: true
    children:
      - name: div
        children: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam
```

### Story Example 3

```yaml
children:
  - name: eb-collapse
    props:
      title: Panel header
      style:
        margin: 20px
        padding: 10px
        fontWeight: bold
        fontSize: larger
    children:
      - name: div
        children: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam
```

### Story Example 4

```yaml
children:
  - name: eb-collapse
    props:
      expandByDefault: true
    children:
      - name: span
        slot: header
        props:
          style:
            color: red
            font-weight: bold
        children:
          - name: eb-icon
            props:
              icon: fas,star
              style:
                color: red
                margin-right: 5px
          - name: span
            children: This is a customized panel header
      - name: div
        children: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam
```

### Story Example 5

```yaml
children:
  - name: eb-button
    props:
      text: Toggle
      on:
        click:
          emit: toggle-collapse
  - name: eb-button
    props:
      text: 'state: keep-open'
      on:
        click:
          emit: toggle-collapse
          params:
            state: keep-open
  - name: eb-button
    props:
      text: 'state: keep-close'
      on:
        click:
          emit: toggle-collapse
          params:
            state: keep-close
  - name: eb-collapse
    props:
      title: Panel header
      eventToToggle: toggle-collapse
    children:
      - name: div
        children: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
