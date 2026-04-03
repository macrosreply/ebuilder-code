---
name: ebuilder-component-eb-button
description: 'Deep component skill for eb-button. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-button in eBuilder component YAML.'
---

# eb-button Component Skill

## General Docs

Use `eb-button` for showing a button for single action or dropdown with multiple menu item actions There are some built-in styles ... ... and sizes you can use to build your button at state you want Add icons into button with `icon` property Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBButton

## Main Props

| name         | type                                                              | description              |
| ------------ | ----------------------------------------------------------------- | ------------------------ |
| authorize    | AuthorizeAction                                                   | Specifies authorize.     |
| class        | string                                                            | CSS class overrides.     |
| danger       | boolean                                                           | Whether danger.          |
| disable      | boolean                                                           | Whether disable.         |
| dropdown     | EBButtonDropdown                                                  | Specifies dropdown.      |
| icon         | string                                                            | Specifies icon.          |
| on.click     | eBuilder events emit                                              | Callback for click.      |
| on.longPress | eBuilder events emit                                              | Callback for long press. |
| size         | "large" \| "middle" \| "small"                                    | Specifies size.          |
| style        | CSSProperties                                                     | Inline style overrides.  |
| text         | string                                                            | Specifies text.          |
| title        | string                                                            | Specifies title.         |
| type         | "primary" \| "ghost" \| "dashed" \| "link" \| "text" \| "default" | Specifies type.          |

## Nested Props

### EBButtonDropdown

| name         | type                                                                                      | description              |
| ------------ | ----------------------------------------------------------------------------------------- | ------------------------ |
| placement    | "bottomLeft" \| "bottomCenter" \| "bottomRight" \| "topLeft" \| "topCenter" \| "topRight" | Specifies placement.     |
| trigger      | "click" \| "hover" \| "contextmenu"                                                       | Specifies trigger.       |
| noArrowIcon  | boolean                                                                                   | Whether no arrow icon.   |
| items        | EBButtonDropdownItem[] \| (() => EBButtonDropdownItem[])                                  | Specifies items.         |
| overlayClass | string                                                                                    | Specifies overlay class. |
| selectedKeys | string[]                                                                                  | Specifies selected keys. |

### EBButtonDropdownItem

| name        | type                   | description                |
| ----------- | ---------------------- | -------------------------- |
| key         | string                 | Specifies key.             |
| text        | string                 | Specifies text.            |
| router-link | string                 | Specifies ['router link']. |
| title       | string                 | Specifies title.           |
| icon        | string                 | Specifies icon.            |
| hide        | boolean                | Whether hide.              |
| disabled    | boolean                | Whether disabled.          |
| subItems    | EBButtonDropdownItem[] | Specifies sub items.       |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-button
    props:
      text: eb-button
```

### Story Example 2

```yaml
children:
  - name: eb-button
    props:
      text: Small
      size: small
  - name: eb-button
    props:
      text: Middle (default)
      size: middle
  - name: eb-button
    props:
      text: Large
      size: middle
```

### Story Example 3

```yaml
children:
  - name: eb-button
    props:
      text: Default
      type: default
  - name: eb-button
    props:
      text: Primary
      type: primary
  - name: eb-button
    props:
      text: Ghost
      type: ghost
  - name: eb-button
    props:
      text: Dashed
      type: dashed
  - name: eb-button
    props:
      text: Link
      type: link
  - name: eb-button
    props:
      text: Text
      type: text
  - name: eb-button
    props:
      text: Disabled Button
      type: primary
      disable: true
  - name: eb-button
    props:
      text: Change Style Button
      type: primary
      style:
        backgroundColor: red
        color: white
```

### Story Example 4

```yaml
children:
  - name: eb-button
    props:
      text: Button with icon & text
      icon: fas,users
  - name: eb-button
    props:
      title: Button with icon only
      icon: fas,users
```

### Story Example 5

```yaml
children:
  - name: eb-button
    props:
      text: Dropdown Button
      dropdown:
        placement: topLeft
        items:
          - key: item 1
            icon: fas,user-tag
            text: 1st menu item
            title: 1st menu item
            on:
              click:
                emit: clicked 1st menu item
          - key: item 2
            icon: fas,user-tag
            text: 2nd menu item
            title: 2nd menu item
            on:
              click:
                emit: clicked 2nd menu item
          - key: item 3
            icon: fas,user-tag
            text: 3rd menu item
            title: 3rd menu item
            subItems:
              - key: item 3-1
                icon: fas,user-tag
                text: Menu item 3-1
                title: Menu item 3-1
                on:
                  click:
                    emit: clicked 3-1 menu item
              - key: item 3-2
                icon: fas,user-tag
                text: Menu item 3-2
                title: Menu item 3-2
                on:
                  click:
                    emit: clicked 3-2 menu item
```

### Story Example 6

```yaml
children:
  - name: eb-button
    props:
      on:
        click:
          emit: button clicked
    children:
      - name: eb-typography
        props:
          contentType: text
          text: Button content with eb-typography
          type: warning
```

### Story Example 7

```yaml
children:
  - name: eb-button
    props:
      text: Click or Long Press
      type: ghost
      on:
        click:
          emit: button clicked
        longPress:
          emit: button long pressed
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
