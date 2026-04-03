---
name: ebuilder-component-eb-router-prompt
description: 'Deep component skill for eb-router-prompt. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-router-prompt in eBuilder component YAML.'
---

# eb-router-prompt Component Skill

## General Docs

No dedicated \*.docs.mdx content is available. Guidance is inferred from \*.types.ts, \*.stories.ts (when present), and real YAML usage in eClient repositories. Source directory in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBRouterPrompt

## Main Props

| name    | type                         | description        |
| ------- | ---------------------------- | ------------------ |
| checks  | (() => boolean) \| boolean[] | Specifies checks.  |
| message | string                       | Specifies message. |

## YAML Examples

### Minimal Example

```yaml
children:
  - name: eb-router-prompt
    props:
      message: attribute_query_leave_confirm
      checks:
        - ${{ $rootVars.appliedEntity || $rootVars.appliedAttributesQuery }}
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
