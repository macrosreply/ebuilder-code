---
name: ebuilder-component-eb-grid
description: 'Deep component skill for eb-grid. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-grid in eBuilder component YAML.'
---

# eb-grid Component Skill

## General Docs

In most business situations, Ant Design Vue needs to solve a lot of information storage problems within the design area, so based on 12 Grids System, we divided the design area into 24 sections. We name the divided area 'box'. We suggest four boxes for horizontal arrangement at most, one at least. Boxes are proportional to the entire screen as shown in the picture above. To ensure a high level of visual comfort, we customize the typography inside of the box based on the box unit. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBGrid

## Main Props

| name   | type                       | description                                                          |
| ------ | -------------------------- | -------------------------------------------------------------------- |
| flex   | string \| number           | Specifies flex.                                                      |
| lg     | number \| BreakpointObject | Specifies lg.                                                        |
| md     | number \| BreakpointObject | Specifies md.                                                        |
| offset | number                     | the number of cells to offset Col from the left                      |
| order  | number                     | raster order, used in `flex` layout mode                             |
| pull   | number                     | the number of cells that raster is moved to the left                 |
| push   | number                     | the number of cells that raster is moved to the right                |
| sm     | number \| BreakpointObject | Specifies sm.                                                        |
| span   | number                     | raster number of cells to occupy, `0` corresponds to `display: none` |
| xl     | number \| BreakpointObject | Specifies xl.                                                        |
| xs     | number \| BreakpointObject | Specifies xs.                                                        |
| xxl    | number \| BreakpointObject | Specifies xxl.                                                       |
| xxxl   | number \| BreakpointObject | Specifies xxxl.                                                      |

## Nested Props

### BreakpointObject

| name   | type   | description                                                    |
| ------ | ------ | -------------------------------------------------------------- |
| span   | number | No official description found; inferred from type declaration. |
| offset | number | No official description found; inferred from type declaration. |
| push   | number | No official description found; inferred from type declaration. |
| pull   | number | No official description found; inferred from type declaration. |
| order  | number | No official description found; inferred from type declaration. |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-row
    children:
      - name: eb-col
        props:
          span: 6
          order: 4
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightseagreen
            children: 1 - col-order-4
      - name: eb-col
        props:
          span: 6
          order: 3
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightblue
            children: 2 - col-order-2
      - name: eb-col
        props:
          span: 6
          order: 2
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightseagreen
            children: 3 - col-order-4
      - name: eb-col
        props:
          span: 6
          order: 1
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightblue
            children: 4 - col-order-1
  - name: p
    children: Fill rest
  - name: eb-row
    children:
      - name: eb-col
        props:
          flex: 300px
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightseagreen
            children: col width 300px
      - name: eb-col
        props:
          flex: auto
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightblue
            children: col fill rest
  - name: p
    children: Raw flex style
  - name: eb-row
    children:
      - name: eb-col
        props:
          flex: 1 1 200px
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightseagreen
            children: col 1 1 200px
      - name: eb-col
        props:
          flex: 0 1 300px
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightblue
            children: col 0 1 300px
```

### Story Example 2

```yaml
children:
  - name: p
    children: Percentage columns
  - name: eb-row
    children:
      - name: eb-col
        props:
          flex: 1
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightseagreen
            children: col-1/5
      - name: eb-col
        props:
          flex: 4
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightblue
            children: col-4/5
  - name: p
    children: Fill rest
  - name: eb-row
    children:
      - name: eb-col
        props:
          flex: 300px
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightseagreen
            children: col width 300px
      - name: eb-col
        props:
          flex: auto
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightblue
            children: col fill rest
  - name: p
    children: Raw flex style
  - name: eb-row
    children:
      - name: eb-col
        props:
          flex: 1 1 200px
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightseagreen
            children: col 1 1 200px
      - name: eb-col
        props:
          flex: 0 1 300px
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightblue
            children: col 0 1 300px
```

### Story Example 3

```yaml
children:
  - name: eb-row
    children:
      - name: eb-col
        props:
          span: 8
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightseagreen
            children: col-8
      - name: eb-col
        props:
          span: 8
          offset: 8
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightblue
            children: col-6
  - name: eb-row
    children:
      - name: eb-col
        props:
          span: 6
          offset: 6
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightseagreen
            children: col-6 offset-6
      - name: eb-col
        props:
          span: 6
          offset: 6
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightblue
            children: col-6 offset-6
  - name: eb-row
    children:
      - name: eb-col
        props:
          span: 12
          offset: 6
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightseagreen
            children: col-12 offset 6
```

### Story Example 4

```yaml
children:
  - name: eb-row
    children:
      - name: eb-col
        props:
          xs: 2
          sm: 4
          md: 6
          lg: 8
          xl: 10
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightseagreen
            children: Col 1
      - name: eb-col
        props:
          xs: 20
          sm: 16
          md: 12
          lg: 8
          xl: 4
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightblue
            children: Col 2
      - name: eb-col
        props:
          xs: 2
          sm: 4
          md: 6
          lg: 8
          xl: 10
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightseagreen
            children: Col 3
```

### Story Example 5

```yaml
children:
  - name: eb-row
    children:
      - name: eb-col
        props:
          span: 18
          push: 6
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightseagreen
            children: Col 18 Push 6
      - name: eb-col
        props:
          span: 6
          pull: 18
        children:
          - name: div
            props:
              style:
                padding: 10px
                backgroundColor: lightblue
            children: Col 6 Pull 18
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
