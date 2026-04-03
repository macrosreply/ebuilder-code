---
name: ebuilder-component-eb-typography
description: 'Deep component skill for eb-typography. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-typography in eBuilder component YAML.'
---

# eb-typography Component Skill

## General Docs

Use `eb-typography` for building heading, paragraph, order list or normal text with variety styles There are variety styles you can apply on the text content `eb-typography` should be used with localed text (texts are added in `locale/(en|de).*.yml` files) to have translated text (with placeholder injection) in components which don't support locale yet Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBTypography

## Main Props

| name                | type                                                        | description                     |
| ------------------- | ----------------------------------------------------------- | ------------------------------- |
| class               | string                                                      | CSS class overrides.            |
| code                | boolean                                                     | Whether code.                   |
| contentType         | "title" \| "text" \| "paragraph" \| "order-list"            | Specifies content type.         |
| copyable            | boolean                                                     | Whether copyable.               |
| copyIconTitle       | { hover: string; copied: string; }                          | Specifies copy icon title.      |
| copyText            | string                                                      | Specifies copy text.            |
| delete              | boolean                                                     | Whether delete.                 |
| disabled            | boolean                                                     | Whether disabled.               |
| ellipsis            | boolean                                                     | Whether ellipsis.               |
| keepSpace           | boolean                                                     | Whether keep space.             |
| keyboard            | boolean                                                     | Whether keyboard.               |
| level               | 1 \| 2 \| 3 \| 4 \| 5                                       | Specifies level.                |
| mark                | boolean                                                     | Whether mark.                   |
| maxLines            | number                                                      | Specifies max lines.            |
| multiLines          | boolean                                                     | Whether multi lines.            |
| noTranslation       | boolean                                                     | Whether no translation.         |
| on.click            | eBuilder events emit                                        | Callback for click.             |
| orderList           | { data: Record []; field: string; } \| { data: string[]; }  | Specifies order list.           |
| strong              | boolean                                                     | Whether strong.                 |
| text                | string                                                      | Specifies text.                 |
| titleNoBottomMargin | boolean                                                     | Whether title no bottom margin. |
| type                | "secondary" \| "success" \| "warning" \| "danger" \| "link" | Specifies type.                 |
| underline           | boolean                                                     | Whether underline.              |
| values              | Record                                                      | Specifies values.               |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-typography
    props:
      contentType: title
      level: 1
      text: h1. eb-typography
  - name: eb-typography
    props:
      contentType: title
      level: 2
      text: h2. eb-typography
  - name: eb-typography
    props:
      contentType: title
      level: 3
      text: h3. eb-typography
  - name: eb-typography
    props:
      contentType: title
      level: 4
      text: h4. eb-typography
  - name: eb-typography
    props:
      contentType: title
      level: 5
      text: h5. eb-typography
```

### Story Example 2

```yaml
children:
  - name: eb-typography
    props:
      contentType: text
      text: eb-typography (default)
  - name: eb-typography
    props:
      contentType: text
      text: eb-typography (secondary)
      type: secondary
  - name: eb-typography
    props:
      contentType: text
      text: eb-typography (success)
      type: success
  - name: eb-typography
    props:
      contentType: text
      text: eb-typography (warning)
      type: warning
  - name: eb-typography
    props:
      contentType: text
      text: eb-typography (danger)
      type: danger
  - name: eb-typography
    props:
      contentType: text
      text: eb-typography (underline)
      underline: true
  - name: eb-typography
    props:
      contentType: text
      text: eb-typography (mark)
      mark: true
  - name: eb-typography
    props:
      contentType: text
      text: eb-typography (strong)
      strong: true
  - name: eb-typography
    props:
      contentType: text
      text: eb-typography (delete)
      delete: true
  - name: eb-typography
    props:
      contentType: text
      text: eb-typography (disabled)
      disabled: true
  - name: eb-typography
    props:
      contentType: text
      text: eb-typography (code)
      code: true
  - name: eb-typography
    props:
      contentType: text
      text: eb-typography (keyboard)
      keyboard: true
  - name: eb-typography
    props:
      contentType: text
      text: eb-typography (link)
      type: link
```

### Story Example 3

```yaml
children:
  - name: eb-typography
    props:
      text: This is a copyable content.
      copyable: true
```

### Story Example 4

```yaml
children:
  - name: eb-typography
    props:
      text: Clickable content
      on:
        click:
          emit:
            name: eb-toast
            params:
              message: Clicked
```

### Story Example 5

```yaml
children:
  - name: eb-typography
    props:
      text: typography_locale
      values:
        placeholder: Placeholder value
```

### Story Example 6

```yaml
children:
  - name: eb-typography
    props:
      contentType: order-list
      orderList:
        field: text
        data:
          - text: First item
          - text: Second item
          - text: Third item
```

### Story Example 7

```yaml
children:
  - name: eb-typography
    props:
      text: These are short, famous texts in English from classic sources like the Bible or Shakespeare. Some texts have word definitions and explanations to help you. Some of these texts are written in an old style of English. Try to understand them, because the English that we speak today is based on what our great, great, great, great grandparents spoke before!
      contentType: paragraph
```

### Story Example 8

```yaml
children:
  - name: eb-typography
    props:
      text: These are short, famous texts in English from classic sources like the Bible or Shakespeare. Some texts have word definitions and explanations to help you. Some of these texts are written in an old style of English. Try to understand them, because the English that we speak today is based on what our great, great, great, great grandparents spoke before!
      ellipsis: true
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
