---
name: ebuilder-component-eb-vnode
description: 'Deep component skill for eb-vnode. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-vnode in eBuilder component YAML.'
---

# eb-vnode Component Skill

## General Docs

No dedicated _.docs.mdx content is available. Guidance is inferred from _.types.ts, \*.stories.ts (when present), and real YAML usage in eClient repositories. Source directory in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBVNodeRenderer

## Main Props

| name  | type                         | description      |
| ----- | ---------------------------- | ---------------- |
| nodes | VNode [] \| (() => VNode []) | Specifies nodes. |

## YAML Examples

### Minimal Example

```yaml
children:
  - name: eb-vnode
    props:
      nodes: |
        ${{
          () => {
            const ebIcon = Vue.resolveComponent('eb-icon') as string;
            return [
              Vue.h(ebIcon, { icon: 'fas,bookmark' }),
              Vue.h(ebIcon, { icon: 'fas,user' }),
              Vue.h(ebIcon, { icon: 'fas,house' }),
              Vue.h(ebIcon, { icon: 'fas,check' }),
              Vue.h(ebIcon, { icon: 'fas,heart' })
            ];
          }
        }}
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
