---
name: ebuilder-component-eb-icon
description: 'Deep component skill for eb-icon. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-icon in eBuilder component YAML.'
---

# eb-icon Component Skill

## General Docs

Use `eb-icon` for rendering a icon from [FontAwesome](fontawesome.io/icons/) or an image/a svg from eBuilder app `assets` file By default, `eb-icon` renders FontAwesome based on `icon` prop value, it should be in format `,`. For `icon-type`, there are `fas` for Solid type, `far` for Regular type and `fab` for Brand type. Ex. With `circle-info`, we have `fas,circle-info` or `far,circle-info`. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBIcon

## Main Props

| name  | type                                                           | description             |
| ----- | -------------------------------------------------------------- | ----------------------- |
| asset | string                                                         | Specifies asset.        |
| class | string                                                         | CSS class overrides.    |
| icon  | string \| object \| string[]                                   | Specifies icon.         |
| style | CSSProperties                                                  | Inline style overrides. |
| title | string                                                         | Specifies title.        |
| type  | "primary" \| "secondary" \| "success" \| "warning" \| "danger" | Specifies type.         |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-icon
    props:
      icon: circle-question
```

### Story Example 2

```yaml
children:
  - name: div
    children:
      - name: eb-icon
        props:
          icon: fas,circle-question
          style:
            fontSize: 20px
            marginRight: 10px
      - name: span
        children: fas - Solid style
  - name: div
    children:
      - name: eb-icon
        props:
          icon: far,circle-question
          style:
            fontSize: 20px
            marginRight: 10px
      - name: span
        children: far - Regular style
```

### Story Example 3

```yaml
children:
  - name: eb-icon
    props:
      icon: fas,circle-question,transform=rotate(-20deg) scale(1.2)
      style:
        fontSize: 20px
        color: blue
```

### Story Example 4

```yaml
children:
  - name: div
    children:
      - name: eb-icon
        props:
          icon: fas,circle-question
          type: primary
          style:
            fontSize: 20px
            marginRight: 10px
      - name: span
        children: primary
  - name: div
    children:
      - name: eb-icon
        props:
          icon: fas,circle-question
          type: secondary
          style:
            fontSize: 20px
            marginRight: 10px
      - name: span
        children: secondary
  - name: div
    children:
      - name: eb-icon
        props:
          icon: fas,circle-question
          type: success
          style:
            fontSize: 20px
            marginRight: 10px
      - name: span
        children: success
  - name: div
    children:
      - name: eb-icon
        props:
          icon: fas,circle-question
          type: warning
          style:
            fontSize: 20px
            marginRight: 10px
      - name: span
        children: warning
  - name: div
    children:
      - name: eb-icon
        props:
          icon: fas,circle-question
          type: danger
          style:
            fontSize: 20px
            marginRight: 10px
      - name: span
        children: danger
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
