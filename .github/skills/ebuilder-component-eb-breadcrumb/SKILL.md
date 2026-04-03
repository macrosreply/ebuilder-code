---
name: ebuilder-component-eb-breadcrumb
description: 'Deep component skill for eb-breadcrumb. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-breadcrumb in eBuilder component YAML.'
---

# eb-breadcrumb Component Skill

## General Docs

Use `eb-breadcrumb` displays the current location within a hierarchy. It allows going back to states higher up in the hierarchy. A simple usage You can customize UI with `style` prop Use `hide` in item prop to show/hide item conditionally Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBBreadcrumb

## Main Props

| name                | type               | description                                                                           |
| ------------------- | ------------------ | ------------------------------------------------------------------------------------- |
| items               | EBBreadcrumbItem[] | Specifies items.                                                                      |
| style               | Partial            | Inline style overrides.                                                               |
| updateDocumentTitle | boolean            | update document title to match the name of current breadcrumb item when route changes |

## Nested Props

### EBBreadcrumbItem

| name   | type    | description       |
| ------ | ------- | ----------------- |
| path   | string  | Specifies path.   |
| name   | string  | Specifies name.   |
| hide   | boolean | Whether hide.     |
| params | Record  | Specifies params. |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-breadcrumb
    props:
      items:
        - name: Home
          path: /home
        - name: About
          path: /about
        - name: Help
          path: /help
```

### Story Example 2

```yaml
children:
  - name: eb-breadcrumb
    props:
      style:
        fontSize: larger
        fontWeight: bold
      items:
        - name: Home
          path: /home
        - name: Setting
          path: /setting
        - name: About Us
          path: /about-us
```

### Story Example 3

```yaml
children:
  - name: eb-breadcrumb
    props:
      items:
        - name: Home
          path: /home
        - name: Setting
          path: /setting
          hide: true
        - name: About Us
          path: /about-us
```

### Story Example 4

```yaml
children:
          - name: eb-breadcrumb
            props:
              items:
                - name: Home
                  path: /home
                - name: User: {name} ({id})
                  path: /setting/:id
                  params:
                    name: Macros
                    id: 777
                - name: Profile
                  path: /profile
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
