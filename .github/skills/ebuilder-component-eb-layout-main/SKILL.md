---
name: ebuilder-component-eb-layout-main
description: 'Deep component skill for eb-layout-main. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-layout-main in eBuilder component YAML.'
---

# eb-layout-main Component Skill

## General Docs

No dedicated \*.docs.mdx content is available. Guidance is inferred from \*.types.ts, \*.stories.ts (when present), and real YAML usage in eClient repositories. Source directory in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/layouts/EBLayoutMain

## Main Props

| name           | type             | description                |
| -------------- | ---------------- | -------------------------- |
| contentPadding | string \| number | Specifies content padding. |

## YAML Examples

### App Usage

```yaml
- name: eb-layout-main
  props:
    contentPadding: 0
    style:
      height: 100vh
      width: 100vw
  children:
    - name: eb-loading
      props:
        visible: ${{ $rootMemo.isLoggedIn && !$rootStore.currentUser.rights }}
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
