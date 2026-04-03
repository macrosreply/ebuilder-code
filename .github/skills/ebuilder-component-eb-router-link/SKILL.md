---
name: ebuilder-component-eb-router-link
description: 'Deep component skill for eb-router-link. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-router-link in eBuilder component YAML.'
---

# eb-router-link Component Skill

## General Docs

Use `eb-router-link` to render links to other pages or websites Simple `eb-router-link` usage It supports all `target` attribute options of a href `eb-router-link` will check if `to` prop value is absolute url or relative url to render itss content and target Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBRouterLink

## Main Props

| name      | type                 | description             |
| --------- | -------------------- | ----------------------- |
| authorize | AuthorizeAction      | Specifies authorize.    |
| class     | string               | CSS class overrides.    |
| on.click  | eBuilder events emit | Callback for click.     |
| style     | CSSProperties        | Inline style overrides. |
| target    | string               | Specifies target.       |
| text      | string               | Specifies text.         |
| title     | string               | Specifies title.        |
| to        | string               | Specifies to.           |
| visible   | boolean              | Whether visible.        |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-router-link
    props:
      text: Router link
      to: /home/companies
```

### Story Example 2

```yaml
children:
  - name: eb-router-link
    props:
      to: /self
      text: 'target: _self'
      target: '_self'
  - name: eb-router-link
    props:
      to: /blank
      text: 'target: _blank'
      target: _blank
  - name: eb-router-link
    props:
      to: /top
      text: 'target: _top'
      target: _top
  - name: eb-router-link
    props:
      to: /parent
      text: 'target: _parent'
      target: _parent
```

### Story Example 3

```yaml
children:
  - name: eb-router-link
    props:
      to: https://www.google.com
      text: 'External link "to: https://www.google.com"'
  - name: eb-router-link
    props:
      to: /help
      text: 'Router path "to: /help"'
```

### Story Example 4

```yaml
children:
  - name: eb-router-link
    props:
      to: /custom-router-link
    children:
      - name: eb-typography
        props:
          type: success
          text: Custom link content
```

### Story Example 5

```yaml
children:
  - name: eb-router-link
    props:
      to: /custom-router-link
      text: Custom Style
      style:
        padding: 10px
        backgroundColor: green
        color: yellow
  - name: eb-router-link
    props:
      to: /visible
      text: Visibility
      visible: false
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
