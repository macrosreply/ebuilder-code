---
name: ebuilder-component-eb-monaco-viewer
description: 'Deep component skill for eb-monaco-viewer. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-monaco-viewer in eBuilder component YAML.'
---

# eb-monaco-viewer Component Skill

## General Docs

Use `eb-monaco-viewer` to show code a readonly code content or differences between 2 contents in nice way in diff-mode. `eb-monaco-viewer` would render editor based on its container size, so please wrap it inside an element and control size of wrapper. Single readonly content. Use diff-mode for showing differences between 2 contents with `diffMode: true` Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBMonacoViewer

## Main Props

| name             | type    | description                   |
| ---------------- | ------- | ----------------------------- |
| content          | string  | Specifies content.            |
| contentTitle     | string  | Specifies content title.      |
| diffContent      | string  | Specifies diff content.       |
| diffContentTitle | string  | Specifies diff content title. |
| diffMode         | boolean | Whether diff mode.            |
| language         | string  | Specifies language.           |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: div
    props:
      style:
        width: 100%
        height: 200px
    children:
      - name: eb-monaco-viewer
        props:
          language: javascript
          content: console.log('Hello, World!');
          contentTitle: Javascript
```

### Story Example 2

```yaml
children:
  - name: div
    props:
      style:
        width: 100%
        height: 200px
    children:
      - name: eb-monaco-viewer
        props:
          language: yaml
          diffMode: true
          content: |
            children:
            - name: eb-monaco-viewer
              props:
                language: javascript
                content: console.log('Hello, World!');
                contentTitle: Javascript
          contentTitle: Base
          diffContent: |
            children:
            - name: eb-monaco-viewer
              props:
                language: javascript
                content: console.log('Hello, World!');
                contentTitle: Javascript - Hello World!
          diffContentTitle: Head
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
