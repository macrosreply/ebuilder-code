---
name: ebuilder-component-eb-descriptions
description: 'Deep component skill for eb-descriptions. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-descriptions in eBuilder component YAML.'
---

# eb-descriptions Component Skill

## General Docs

Display multiple read-only fields in groups. Commonly displayed on the details page. \*\*\* We recommend using `eb-descriptions` with `key` prop to refresh descriptions when the data changes. Simple usage Descriptions with border and background color with `bordered: true` Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBDescriptions

## Main Props

| name         | type                                                                                                        | description                                                                                                                    |
| ------------ | ----------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| bordered     | boolean                                                                                                     | Whether bordered.                                                                                                              |
| colon        | boolean                                                                                                     | Whether colon.                                                                                                                 |
| column       | number \| { xs?: number; sm?: number; md?: number; lg?: number; xl?: number; xxl?: number; xxxl?: number; } | Specifies column.                                                                                                              |
| contentStyle | CSSProperties                                                                                               | Specifies content style.                                                                                                       |
| items        | EBDescriptionItem[] \| (() => EBDescriptionItem[])                                                          | Specifies items.                                                                                                               |
| labelStyle   | CSSProperties                                                                                               | Specifies label style.                                                                                                         |
| layout       | "horizontal" \| "vertical"                                                                                  | Specifies layout.                                                                                                              |
| mobileSafe   | boolean                                                                                                     | eb-descriptions will switch to use `layout: vertical` and `column: 1` when the screen width is less than 576px (xs breakpoint) |
| size         | "default" \| "middle" \| "small"                                                                            | Specifies size.                                                                                                                |

## Nested Props

### EBDescriptionItem

| name         | type                                                   | description                 |
| ------------ | ------------------------------------------------------ | --------------------------- |
| label        | string                                                 | Specifies label.            |
| span         | number                                                 | Specifies span.             |
| labelStyle   | CSSProperties                                          | Specifies label style.      |
| contentStyle | CSSProperties                                          | Specifies content style.    |
| hide         | boolean                                                | Whether hide.               |
| children     | { default: () => unknown[]; label?: () => unknown[]; } | Nested child configuration. |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-descriptions
    props:
      items:
        - label: Role
          children: Admin permission
        - label: User
          children: User information
        - label: Profile
          children: Profile information
        - label: Email
          children: example@macrosreply.de
        - label: Phone
          children: 0987654321
        - label: Address
          children: 1 Macrosreply De
```

### Story Example 2

```yaml
children:
  - name: eb-descriptions
    props:
      key: ${{ $rootVars.reloadKey }}
      bordered: true
      items:
        - label: Role
          children: Admin permission
        - label: User
          children: User information
        - label: Profile
          children: Profile information
        - label: Email
          children: example@macrosreply.de
        - label: Phone
          children: 0987654321
        - label: Address
          children: 1 Macrosreply De
```

### Story Example 3

```yaml
children:
  - name: eb-descriptions
    props:
      key: ${{ $rootVars.reloadKey }}
      bordered: true
      layout: horizontal
      items:
        - label: Role
          children: Admin permission
        - label: User
          children: User information
        - label: Profile
          children: Profile information
        - label: Email
          children: example@macrosreply.de
        - label: Phone
          children: 0987654321
        - label: Address
          children: 1 Macrosreply De
  - name: eb-descriptions
    props:
      key: ${{ $rootVars.reloadKey }}
      bordered: true
      layout: vertical
      items:
        - label: Role
          children: Admin permission
        - label: User
          children: User information
        - label: Profile
          children: Profile information
        - label: Email
          children: example@macrosreply.de
        - label: Phone
          children: 0987654321
        - label: Address
          children: 1 Macrosreply De
```

### Story Example 4

```yaml
children:
  - name: eb-descriptions
    props:
      key: ${{ $rootVars.reloadKey }}
      bordered: true
      items:
        - label: Role
          children: Admin permission
        - label: User
          children: User information
        - label: Profile
          children: Profile information
        - label: Email
          children: example@macrosreply.de
        - label: Phone
          children: '0987654321'
        - label: Address
          children: 1 Macrosreply De
  - name: eb-descriptions
    props:
      key: ${{ $rootVars.reloadKey }}
      bordered: true
      size: middle
      items:
        - label: Role
          children: Admin permission
        - label: User
          children: User information
        - label: Profile
          children: Profile information
        - label: Email
          children: example@macrosreply.de
        - label: Phone
          children: '0987654321'
        - label: Address
          children: 1 Macrosreply De
  - name: eb-descriptions
    props:
      key: ${{ $rootVars.reloadKey }}
      bordered: true
      size: small
      items:
        - label: Role
          children: Admin permission
        - label: User
          children: User information
        - label: Profile
          children: Profile information
        - label: Email
          children: example@macrosreply.de
        - label: Phone
          children: '0987654321'
        - label: Address
          children: 1 Macrosreply De
```

### Story Example 5

```yaml
children:
  - name: eb-descriptions
    props:
      key: ${{ $rootVars.reloadKey }}
      bordered: true
      labelStyle:
        color: #4cc235
        fontSize: 15px
        fontWeight: bold
      contentStyle:
        fontSize: 15px
        fontWeight: 600
        textAlign: center
      items:
        - label: Role
          children: Admin permission
        - label: User
          children: User information
        - label: Profile
          children: Profile information
        - label: Email
          children: example@macrosreply.de
        - label: Phone
          children: '0987654321'
        - label: Address
          children: 1 Macrosreply De
```

### Story Example 6

```yaml
children:
  - name: eb-descriptions
    props:
      key: ${{ $rootVars.reloadKey }}
      bordered: true
      column: 4
      items:
        - label: Role
          children: Admin permission
        - label: User
          children: User information
        - label: Profile
          children: Profile information
        - label: Email
          children: example@macrosreply.de
        - label: Phone
          children: '0987654321'
        - label: Address
          children: 1 Macrosreply De
```

### Story Example 7

```yaml
children:
          - name: eb-descriptions
            props:
              key: ${{ $rootVars.reloadKey }}
              bordered: true
              column: 2
              items:
                - label: Role
                  children: Admin permission
                - label: User
                  children: User information
                - label: Profile
                  span: 4
                  children: Profile information (span: 4)
                - label: Email
                  span: 4
                  children: example@macrosreply.de (span: 4)
                - label: Phone
                  children: '0987654321'
                - label: Address
                  children: 1 Macrosreply De
```

### Story Example 8

```yaml
children:
  - name: eb-descriptions
    props:
      key: ${{ $rootVars.reloadKey }}
      bordered: true
      items:
        - label: Role
          labelStyle:
            color: red
          contentStyle:
            color: green
          children: Admin permission
        - label: User
          labelStyle:
            color: blue
          contentStyle:
            color: lightblue
          children: User information
        - label: Profile
          children: Profile information
        - label: Email
          children: example@macrosreply.de
        - label: Phone
          children: '0987654321'
        - label: Address
          children: 1 Macrosreply De
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
