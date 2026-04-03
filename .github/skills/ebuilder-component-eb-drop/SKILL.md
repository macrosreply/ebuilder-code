---
name: ebuilder-component-eb-drop
description: 'Deep component skill for eb-drop. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-drop in eBuilder component YAML.'
---

# eb-drop Component Skill

## General Docs

Use `eb-drop` to enable drag-and-drop files or URLs over a target element. Basic usage with accepted file types and a drop target. Enable/disable drop mode dynamically by emitting events. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBDrop

## Main Props

| name           | type                 | description                 |
| -------------- | -------------------- | --------------------------- |
| acceptFile     | boolean              | Whether accept file.        |
| acceptUrl      | boolean              | Whether accept url.         |
| dragOverDesc   | string               | Specifies drag over desc.   |
| eventToDisable | string               | Specifies event to disable. |
| eventToEnable  | string               | Specifies event to enable.  |
| fileTypes      | string               | Specifies file types.       |
| maxFiles       | number               | Specifies max files.        |
| on.dropFiles   | eBuilder events emit | Callback for drop files.    |
| on.dropUrl     | eBuilder events emit | Callback for drop url.      |
| targetSelector | string               | Specifies target selector.  |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: div
    props:
      id: eb-drop-target
      style:
        height: 220px
        border: 1px dashed #8c8c8c
        border-radius: 6px
        display: flex
        align-items: center
        justify-content: center
    children: Drag files or URLs over this area
  - name: eb-drop
    props:
      targetSelector: '#eb-drop-target'
      acceptFile: true
      acceptUrl: false
      fileTypes: .png,.jpg,.jpeg,image/*
      maxFiles: 5
      on:
        dropFiles:
          emit:
            - name: eb-toast
              params:
                message: Dropped files
```

### Story Example 2

```yaml
children:
  - name: eb-button
    props:
      text: Disable drop
      on:
        click:
          emit: evt-disable-drop
  - name: eb-button
    props:
      text: Enable drop
      on:
        click:
          emit: evt-enable-drop
  - name: div
    props:
      id: eb-drop-target-toggle
    children: Drag files here
  - name: eb-drop
    props:
      targetSelector: '#eb-drop-target-toggle'
      acceptFile: true
      eventToDisable: evt-disable-drop
      eventToEnable: evt-enable-drop
      on:
        dropFiles:
          emit:
            name: eb-toast
            params:
              message: Dropped files
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
