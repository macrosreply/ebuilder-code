---
name: ebuilder-component-eb-modal
description: 'Deep component skill for eb-modal. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-modal in eBuilder component YAML.'
---

# eb-modal Component Skill

## General Docs

Use `eb-modal` to create a new floating layer over the current page. You can control `eb-modal` visibility with `emit` its configured `eventToOpen` and `eventToClose` events, in `eb-modal` `children` you can use data passed in `eventToOpen` event to build UI or to handle business logics. Basic `eb-modal` usage included getting data passed when opening `$ctx.openParams.*` Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBModal

## Main Props

| name             | type                 | description                                          |
| ---------------- | -------------------- | ---------------------------------------------------- |
| cancelButton     | EBButtonModal        | Specifies cancel button.                             |
| class            | string               | CSS class overrides.                                 |
| controlledIsOpen | boolean              | Whether the open state is controlled by model value. |
| desc             | string               | Specifies desc.                                      |
| eventToClose     | string               | Specifies event to close.                            |
| eventToOpen      | string               | Specifies event to open.                             |
| fullscreen       | boolean              | Whether fullscreen.                                  |
| okButton         | EBButtonModal        | Specifies ok button.                                 |
| on.close         | eBuilder events emit | Callback for close.                                  |
| on.open          | eBuilder events emit | Callback for open.                                   |
| title            | string               | Specifies title.                                     |
| width            | string \| number     | Specifies width.                                     |

## Nested Props

### EBButtonModal

| name     | type                                                              | description       |
| -------- | ----------------------------------------------------------------- | ----------------- |
| text     | string                                                            | Specifies text.   |
| type     | "link" \| "default" \| "primary" \| "ghost" \| "dashed" \| "text" | Specifies type.   |
| shape    | "default" \| "circle" \| "round"                                  | Specifies shape.  |
| hide     | boolean \| ((openParams: unknown) => boolean)                     | Specifies hide.   |
| disabled | boolean                                                           | Whether disabled. |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-button
    props:
      text: Open modal
      on:
        click:
          emit:
            name: open-modal
            params:
              team: eBuilder
              company: Macros Reply
  - name: eb-modal
    props:
      title: Modal title
      desc: Modal description
      eventToOpen: open-modal
      eventToClose: close-modal
      width: 500
      okButton:
        text: OK
        on:
          click:
            emit: close-modal
      cancelButton:
        text: Cancel
        on:
          click:
            emit: close-modal
    children:
      - name: div
        children: 'Data passed when opening modal:'
      - name: div
        children: '${{ `Team: ${$ctx.openParams?.team}` }}'
      - name: div
        children: '${{ `Team: ${$ctx.openParams?.company}` }}'
```

### Story Example 2

```yaml
children:
  - name: eb-button
    props:
      text: Open modal
      on:
        click:
          emit:
            name: open-modal-fullscreen
            params:
              team: eBuilder
              company: Macros Reply
  - name: eb-modal
    props:
      title: Modal title
      desc: Modal description
      eventToOpen: open-modal-fullscreen
      eventToClose: close-modal-fullscreen
      fullscreen: true
      okButton:
        text: OK
        on:
          click:
            emit: close-modal-fullscreen
      cancelButton:
        text: Cancel
        on:
          click:
            emit: close-modal-fullscreen
    children:
      - name: div
        children: 'Data passed when opening modal:'
      - name: div
        children: '${{ `Team: ${$ctx.openParams?.team}` }}'
      - name: div
        children: '${{ `Team: ${$ctx.openParams?.company}` }}'
```

### Story Example 3

```yaml
children:
  - name: eb-button
    props:
      text: Open modal
      on:
        click:
          emit:
            name: open-modal-default-footer-actions
            params:
              team: eBuilder
              company: Macros Reply
  - name: eb-modal
    props:
      title: Modal title
      desc: Modal description
      eventToOpen: open-modal-default-footer-actions
      eventToClose: close-modal
      width: 500
      okButton:
        hide: true
      cancelButton:
        text: Cancel
        on:
          click:
            emit:
              name: eb-toast
              params:
                message: Clicked on cancel button
    children:
      - name: div
        children: 'Data passed when opening modal:'
      - name: div
        children: '${{ `Team: ${$ctx.openParams?.team}` }}'
      - name: div
        children: '${{ `Team: ${$ctx.openParams?.company}` }}'
```

### Story Example 4

```yaml
children:
  - name: eb-button
    props:
      text: Open modal
      on:
        click:
          emit:
            name: open-modal-title-desc-params
            params:
              team: eBuilder
              company: Macros Reply
              __modal_title__: Title from openParams
              __modal_desc__: Desc from openParams
  - name: eb-modal
    props:
      title: Modal title
      desc: Modal description
      eventToOpen: open-modal-title-desc-params
      eventToClose: close-modal
      width: 500
      okButton:
        text: OK
        on:
          click:
            emit: close-modal
      cancelButton:
        text: Cancel
        on:
          click:
            emit: close-modal
    children:
      - name: div
        children: 'Data passed when opening modal:'
      - name: div
        children: '${{ `Team: ${$ctx.openParams?.team}` }}'
      - name: div
        children: '${{ `Team: ${$ctx.openParams?.company}` }}'
```

### Story Example 5

```yaml
children:
  - name: eb-button
    props:
      text: Open modal
      on:
        click:
          emit:
            name: open-modal-custom-footer
            params:
              team: eBuilder
              company: Macros Reply
  - name: eb-modal
    props:
      title: Modal title
      desc: Modal description
      eventToOpen: open-modal-custom-footer
      eventToClose: close-modal
      width: 500
      okButton:
        text: OK
        on:
          click:
            emit: close-modal
      cancelButton:
        text: Cancel
        on:
          click:
            emit: close-modal
    children:
      - name: div
        children: 'Data passed when opening modal:'
      - name: div
        children: '${{ `Team: ${$ctx.openParams?.team}` }}'
      - name: div
        children: '${{ `Team: ${$ctx.openParams?.company}` }}'
      - name: eb-button
        slot: footer-left
        props:
          text: Actions on left
      - name: eb-button
        slot: footer-right
        props:
          text: Actions on right
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
