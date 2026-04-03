---
name: ebuilder-component-eb-context-menu
description: 'Deep component skill for eb-context-menu. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-context-menu in eBuilder component YAML.'
---

# eb-context-menu Component Skill

## General Docs

Use `eb-context-menu` when you want to cover context menu (right click context menu) on a specific UI block. `eb-context-menu` wraps its default slot `children` inside a container, you can use `containerStyle` or `containerClass` to adjust its styles to fit page layout. Basic usage of `eb-context-menu` with `disabled` and `hide` items Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBContextMenu

## Main Props

| name           | type                                               | description                                   |
| -------------- | -------------------------------------------------- | --------------------------------------------- |
| class          | string                                             | CSS class overrides.                          |
| containerClass | string                                             | [deprecated] Container CSS class overrides    |
| containerStyle | CSSProperties                                      | [deprecated] Container inline style overrides |
| id             | string                                             | Identifier for item.                          |
| items          | EBContextMenuItem[] \| (() => EBContextMenuItem[]) | Specifies items.                              |
| on.click       | eBuilder events emit                               | Callback for click.                           |
| style          | CSSProperties                                      | Inline style overrides.                       |

## Nested Props

### EBContextMenuItem

| name        | type                | description                |
| ----------- | ------------------- | -------------------------- |
| key         | string              | Specifies key.             |
| text        | string              | Specifies text.            |
| router-link | string              | Specifies ['router link']. |
| title       | string              | Specifies title.           |
| icon        | string              | Specifies icon.            |
| hide        | boolean             | Whether hide.              |
| disabled    | boolean             | Whether disabled.          |
| subItems    | EBContextMenuItem[] | Specifies sub items.       |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-context-menu
    props:
      style:
        textAlign: center
        background: #f7f7f7
        height: 200px
        lineHeight: 200px
        color: #777
        border: 1px solid blue
        borderStyle: dashed
      items:
        - key: 1
          text: User
          title: User
          icon: fas,user
          on:
            click:
              emit:
                - name: eb-toast
                  params:
                    message: Clicked User
        - key: 2
          text: Information
          title: Information
          hide: true
          icon: fas,circle-info
          on:
            click:
              emit:
                - name: eb-toast
                  params:
                    message: Clicked Information
        - key: 3
          text: Address
          title: Address
          disabled: true
          icon: fas,address-card
          on:
            click:
              emit:
                - name: eb-toast
                  params:
                    message: Clicked Address
    children:
      - name: span
        children: Right Click to show dropdown menu
```

### Story Example 2

```yaml
children:
  - name: eb-context-menu
    props:
      id: context-menu-root
      class: eb-context-menu-container-demo
      style:
        textAlign: center
        background: '#f7f7f7'
        height: 160px
        lineHeight: 160px
        color: '#777'
        border: 1px dashed
      items:
        - key: 1
          text: Open
          title: Open
          icon: fas,folder-open
        - key: 2
          text: Delete
          title: Delete
          icon: fas,trash-can
    children:
      - name: span
        children: Right Click to show dropdown menu
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
