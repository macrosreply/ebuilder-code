---
name: ebuilder-component-eb-for-in
description: 'Deep component skill for eb-for-in. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-for-in in eBuilder component YAML.'
---

# eb-for-in Component Skill

## General Docs

Use `eb-for-in` for list rendering Rendering a list of items, `children` now will be used for rendering each element in array. If you are using an array of objects, you can set `entryKey` prop to specify the key while rendering each element. From its context, you can get element value with `$ctx.entry` or element index with `$ctx.entryIndex` Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBForIn

## Main Props

| name     | type                    | description          |
| -------- | ----------------------- | -------------------- |
| array    | any[]                   | Specifies array.     |
| entryKey | string                  | Specifies entry key. |
| filter   | (entry: any) => boolean | Callback for filter. |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-row
    props:
      style:
        padding: 10px
        gap: 15px
        backgroundColor: #fff
    children:
      - name: eb-for-in
        props:
          array:
            - key: 1
              title: Home
              description: Home description
              icon: fas,house
            - key: 2
              title: Image
              description: Image description
              icon: fas,image
            - key: 3
              title: Phone
              description: Phone description
              icon: fas,phone
            - key: 4
              title: File
              description: File description
              icon: fas,file
        children:
          - name: eb-card
            props:
              title: ${{ `#${$ctx.entryIndex} ${$ctx.entry.title}` }}
              description: ${{ $ctx.entry.description }}
              icon: ${{ $ctx.entry.icon }}
              hoverable: true
            children:
              - name: div
                slot: card-action-1
                children:
                  - name: eb-icon
                    props:
                      icon: fas,circle-info
                      style:
                        fontSize: 12px
              - name: div
                slot: card-action-2
                children:
                  - name: eb-icon
                    props:
                      icon: fas,pen
                      style:
                        fontSize: 12px
```

### Story Example 2

```yaml
children:
  - name: eb-row
    props:
      style:
        padding: 10px
        gap: 15px
        backgroundColor: #fff
    children:
      - name: eb-for-in
        props:
          filter: |
            ${{ (entry) => entry.key >= 2 }}
          array:
            - key: 1
              title: Home
              description: Home description
              icon: fas,house
            - key: 2
              title: Image
              description: Image description
              icon: fas,image
            - key: 3
              title: Phone
              description: Phone description
              icon: fas,phone
            - key: 4
              title: File
              description: File description
              icon: fas,file
        children:
          - name: eb-card
            props:
              title: ${{ `#${$ctx.entryIndex} ${$ctx.entry.title}` }}
              description: ${{ $ctx.entry.description }}
              icon: ${{ $ctx.entry.icon }}
              hoverable: true
            children:
              - name: div
                slot: card-action-1
                children:
                  - name: eb-icon
                    props:
                      icon: fas,circle-info
                      style:
                        fontSize: 12px
              - name: div
                slot: card-action-2
                children:
                  - name: eb-icon
                    props:
                      icon: fas,pen
                      style:
                        fontSize: 12px
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
