---
name: ebuilder-component-eb-html-viewer
description: 'Deep component skill for eb-html-viewer. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-html-viewer in eBuilder component YAML.'
---

# eb-html-viewer Component Skill

## General Docs

Use eb-html-viewer to render HTML content in a read-only TinyMCE container. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBHtmlViewer

## Main Props

| name    | type   | description        |
| ------- | ------ | ------------------ |
| content | string | Specifies content. |
| height  | number | Specifies height.  |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-html-viewer
    props:
      height: 260
      content: |
        <h1>Hello HTML Viewer</h1>
        <p>
          This component renders HTML using TinyMCE in read-only mode.
        </p>
        <ul>
          <li>Bold text: <strong>enabled</strong></li>
          <li>Italic text: <em>enabled</em></li>
          <li>Links: <a href="https://example.com" target="_blank" rel="noreferrer">example.com</a></li>
        </ul>
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
