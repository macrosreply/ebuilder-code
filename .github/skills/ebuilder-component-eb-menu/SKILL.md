---
name: ebuilder-component-eb-menu
description: 'Deep component skill for eb-menu. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-menu in eBuilder component YAML.'
---

# eb-menu Component Skill

## General Docs

A versatile menu for navigation. Menu type `tab` navigation. Menu type `horizontal` navigation. Menu type `vertical` navigation. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBMenu

## Main Props

| name  | type                                | description             |
| ----- | ----------------------------------- | ----------------------- |
| items | EBMenuItem[]                        | Specifies items.        |
| style | CSSProperties                       | Inline style overrides. |
| type  | "tab" \| "horizontal" \| "vertical" | Specifies type.         |

## Nested Props

### EBMenuItem

| name        | type                       | description                                                                                                          |
| ----------- | -------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| key         | string                     | Specifies key.                                                                                                       |
| router-link | string                     | Specifies router link.                                                                                               |
| text        | string                     | Specifies text.                                                                                                      |
| icon        | string                     | Specifies icon.                                                                                                      |
| isActive    | (route: string) => boolean | function to determine if the menu item is active, it receives the route of the menu item and should return a boolean |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-menu
    props:
      type: tab
      items:
        - key: home
          router-link: /home
          text: Home
          icon: fas,house
        - key: accounts
          router-link: /accounts
          text: Accounts
          icon: fas,circle-info
        - key: contract
          router-link: /contract
          text: Contract
          icon: fas,address-book
        - key: user
          router-link: /user
          text: User
          icon: fas,user
        - key: other
          router-link: /other
          text: Other
          icon: fas,sliders
```

### Story Example 2

```yaml
children:
  - name: eb-menu
    props:
      type: horizontal
      items:
        - key: home
          router-link: /home
          text: Home
          icon: fas,house
        - key: accounts
          router-link: /accounts
          text: Accounts
          icon: fas,circle-info
        - key: contract
          router-link: /contract
          text: Contract
          icon: fas,address-book
        - key: user
          router-link: /user
          text: User
          icon: fas,user
        - key: other
          router-link: /other
          text: Other
          icon: fas,sliders
```

### Story Example 3

```yaml
children:
  - name: eb-menu
    props:
      type: vertical
      items:
        - key: home
          router-link: /home
          text: Home
          icon: fas,house
        - key: accounts
          router-link: /accounts
          text: Accounts
          icon: fas,circle-info
        - key: contract
          router-link: /contract
          text: Contract
          icon: fas,address-book
        - key: user
          router-link: /user
          text: User
          icon: fas,user
        - key: other
          router-link: /other
          text: Other
          icon: fas,sliders
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
