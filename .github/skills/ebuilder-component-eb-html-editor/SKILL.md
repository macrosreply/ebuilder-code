---
name: ebuilder-component-eb-html-editor
description: 'Deep component skill for eb-html-editor. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-html-editor in eBuilder component YAML.'
---

# eb-html-editor Component Skill

## General Docs

Use `eb-html-editor` to edit rich text/HTML content in an `eb-form` field. Basic editor usage Fixed-height editor setup Disabled editor Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/form/EBHtmlEditor

## Main Props

| name                 | type    | description                        |
| -------------------- | ------- | ---------------------------------- |
| autoResize           | boolean | Whether auto resize.               |
| disable              | boolean | Whether disable.                   |
| eventToInsertContent | string  | Specifies event to insert content. |
| height               | number  | Specifies height.                  |
| maxHeight            | number  | Specifies max height.              |
| minHeight            | number  | Specifies min height.              |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-form
    props:
      model:
        custom:
          html: <p>Initial <strong>HTML</strong> content</p>
      fields:
        - name: html
          ebFormControl:
            name: eb-html-editor
            label: HTML editor
```

### Story Example 2

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: html
          ebFormControl:
            name: eb-html-editor
            label: HTML editor
            props:
              height: 320
              autoResize: false
```

### Story Example 3

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: html
          ebFormControl:
            name: eb-html-editor
            label: HTML editor
            props:
              disable: true
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
