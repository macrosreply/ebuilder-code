---
name: ebuilder-component-eb-brand-logo
description: 'Deep component skill for eb-brand-logo. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-brand-logo in eBuilder component YAML.'
---

# eb-brand-logo Component Skill

## General Docs

Use `eb-brand-logo` for showing Macros Reply brand logo You can change logo dimension with `height` property Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBBrandLogo

## Main Props

| name   | type    | description       |
| ------ | ------- | ----------------- |
| color  | boolean | Whether color.    |
| height | number  | Specifies height. |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-brand-logo
```

### Story Example 2

```yaml
children:
  - name: eb-brand-logo
    props:
      height: 50
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
