---
name: ebuilder-component-eb-hover-overlay
description: 'Deep component skill for eb-hover-overlay. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-hover-overlay in eBuilder component YAML.'
---

# eb-hover-overlay Component Skill

## General Docs

Use `eb-hover-overlay` to show floating card popped by hovering on a target Simple `eb-hover-overlay` usage with popup title and content There are 12 `placement` options available you can use to change position of the tooltip relative to the target Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBHoverOverlay

## Main Props

| name          | type                                                                                                                                                           | description                |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------- |
| arrow         | boolean                                                                                                                                                        | Whether arrow.             |
| eventToToggle | string                                                                                                                                                         | Specifies event to toggle. |
| placement     | "top" \| "left" \| "right" \| "bottom" \| "topLeft" \| "topRight" \| "bottomLeft" \| "bottomRight" \| "leftTop" \| "leftBottom" \| "rightTop" \| "rightBottom" | Specifies placement.       |
| trigger       | "hover" \| "click"                                                                                                                                             | Specifies trigger.         |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-hover-overlay
    props:
      placement: bottomLeft
    children:
      - name: eb-button
        props:
          type: primary
          text: Hover me
      - name: b
        slot: hover-overlay-title
        children:
          - Hover Overlay Title
      - name: span
        slot: hover-overlay-content
        children:
          - Hover Overlay Content
```

### Story Example 2

```yaml
children:
  - name: div
    props:
      style:
        text-align: center
        marginTop: 80px
    children:
      - name: div
        props:
          style:
            display: inline-block
        children:
          - name: div
            props:
              style:
                whiteSpace: nowrap
            children:
              - name: div
                children:
                  - name: eb-hover-overlay
                    props:
                      placement: topLeft
                    children:
                      - name: eb-button
                        props:
                          style:
                            width: 70px
                            textAlign: center
                            marginRight: 8px
                            marginBottom: 8px
                          type: primary
                          text: TL
                      - name: b
                        slot: hover-overlay-title
                        children:
                          - Hover Overlay topLeft Title
                      - name: span
                        slot: hover-overlay-content
                        children:
                          - Hover Overlay topLeft Content
                  - name: eb-hover-overlay
                    props:
                      placement: top
                    children:
                      - name: eb-button
                        props:
                          style:
                            width: 70px
                            textAlign: center
                            marginRight: 8px
                            marginBottom: 8px
                          type: primary
                          text: Top
                      - name: b
                        slot: hover-overlay-title
                        children:
                          - Hover Overlay top Title
                      - name: span
                        slot: hover-overlay-content
                        children:
                          - Hover Overlay top Content
                  - name: eb-hover-overlay
                    props:
                      placement: topRight
                    children:
                      - name: eb-button
                        props:
                          style:
                            width: 70px
                            textAlign: center
                            marginRight: 8px
                            marginBottom: 8px
                          type: primary
                          text: TR
                      - name: b
                        slot: hover-overlay-title
                        children:
                          - Hover Overlay topRight Title
                      - name: span
                        slot: hover-overlay-content
                        children:
                          - Hover Overlay topRight Content
          - name: div
            props:
              style:
                width: 70px
                float: left
            children:
              - name: eb-hover-overlay
                props:
                  placement: leftTop
                children:
                  - name: eb-button
                    props:
                      style:
                        width: 70px
                        textAlign: center
                        marginRight: 8px
                        marginBottom: 8px
                      type: primary
                      text: LT
                  - name: b
                    slot: hover-overlay-title
                    children:
                      - Hover Overlay leftTop Title
                  - name: span
                    slot: hover-overlay-content
                    children:
                      - Hover Overlay leftTop Content
              - name: eb-hover-overlay
                props:
                  placement: left
                children:
                  - name: eb-button
                    props:
                      style:
                        width: 70px
                        textAlign: center
                        marginRight: 8px
                        marginBottom: 8px
                      type: primary
                      text: Left
                  - name: b
                    slot: hover-overlay-title
                    children:
                      - Hover Overlay left Title
                  - name: span
                    slot: hover-overlay-content
                    children:
                      - Hover Overlay left Content
              - name: eb-hover-overlay
                props:
                  placement: leftBottom
                children:
                  - name: eb-button
                    props:
                      style:
                        width: 70px
                        textAlign: center
                        marginRight: 8px
                        marginBottom: 8px
                      type: primary
                      text: LB
                  - name: b
                    slot: hover-overlay-title
                    children:
                      - Hover Overlay leftBottom Title
                  - name: span
                    slot: hover-overlay-content
                    children:
                      - Hover Overlay leftBottom Content
          - name: div
            props:
              style:
                width: 70px
                marginLeft: 315px
            children:
              - name: eb-hover-overlay
                props:
                  placement: rightTop
                children:
                  - name: eb-button
                    props:
                      style:
                        width: 70px
                        textAlign: center
                        marginRight: 8px
                        marginBottom: 8px
                      type: primary
                      text: RT
                  - name: b
                    slot: hover-overlay-title
                    children:
                      - Hover Overlay rightTop Title
                  - name: span
                    slot: hover-overlay-content
                    children:
                      - Hover Overlay rightTop Content
              - name: eb-hover-overlay
                props:
                  placement: right
                children:
                  - name: eb-button
                    props:
                      style:
                        width: 70px
                        textAlign: center
                        marginRight: 8px
                        marginBottom: 8px
                      type: primary
                      text: Right
                  - name: b
                    slot: hover-overlay-title
                    children:
                      - Hover Overlay right Title
                  - name: span
                    slot: hover-overlay-content
                    children:
                      - Hover Overlay right Content
              - name: eb-hover-overlay
                props:
                  placement: rightBottom
                children:
                  - name: eb-button
                    props:
                      style:
                        width: 70px
                        textAlign: center
                        marginRight: 8px
                        marginBottom: 8px
                      type: primary
                      text: RB
                  - name: b
                    slot: hover-overlay-title
                    children:
                      - Hover Overlay rightBottom Title
                  - name: span
                    slot: hover-overlay-content
                    children:
                      - Hover Overlay rightBottom Content
          - name: div
            children:
              - name: eb-hover-overlay
                props:
                  placement: bottomLeft
                children:
                  - name: eb-button
                    props:
                      style:
                        width: 70px
                        textAlign: center
                        marginRight: 8px
                        marginBottom: 8px
                      type: primary
                      text: BL
                  - name: b
                    slot: hover-overlay-title
                    children:
                      - Hover Overlay bottomLeft Title
                  - name: span
                    slot: hover-overlay-content
                    children:
                      - Hover Overlay bottomLeft Content
              - name: eb-hover-overlay
                props:
                  placement: bottom
                children:
                  - name: eb-button
                    props:
                      style:
                        width: 70px
                        textAlign: center
                        marginRight: 8px
                        marginBottom: 8px
                      type: primary
                      text: Bottom
                  - name: b
                    slot: hover-overlay-title
                    children:
                      - Hover Overlay bottom Title
                  - name: span
                    slot: hover-overlay-content
                    children:
                      - Hover Overlay bottom Content
              - name: eb-hover-overlay
                props:
                  placement: bottomRight
                children:
                  - name: eb-button
                    props:
                      style:
                        width: 70px
                        textAlign: center
                        marginRight: 8px
                        marginBottom: 8px
                      type: primary
                      text: BR
                  - name: b
                    slot: hover-overlay-title
                    children:
                      - Hover Overlay bottomRight Title
                  - name: span
                    slot: hover-overlay-content
                    children:
                      - Hover Overlay bottomRight Content
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
