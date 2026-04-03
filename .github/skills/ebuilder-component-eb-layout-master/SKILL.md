---
name: ebuilder-component-eb-layout-master
description: 'Deep component skill for eb-layout-master. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-layout-master in eBuilder component YAML.'
---

# eb-layout-master Component Skill

## General Docs

No dedicated \*.docs.mdx content is available. Guidance is inferred from \*.types.ts, \*.stories.ts (when present), and real YAML usage in eClient repositories. Source directory in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/layouts/EBLayoutMaster

## Main Props

| name                 | type                        | description                       |
| -------------------- | --------------------------- | --------------------------------- |
| extraUserMenuItems   | EBExtraUserMenuItem[]       | Specifies extra user menu items.  |
| headerLink           | string                      | Specifies header link.            |
| headerLogo           | string                      | Specifies header logo.            |
| headerLogoStyle      | StyleValue                  | Specifies header logo style.      |
| headerMenuItems      | HeaderMenuMenuItem[]        | Specifies header menu items.      |
| headerTag            | string                      | Specifies header tag.             |
| hideMainHeader       | boolean                     | Whether hide main header.         |
| masterSearch         | MasterSearchConfigs         | Specifies master search.          |
| noHeader             | boolean                     | Whether no header.                |
| showSecurityPolicies | string[]                    | Specifies show security policies. |
| userProfileLinks     | HeaderMenuUserProfileLink[] | Specifies user profile links.     |

## YAML Examples

### Minimal Example

```yaml
children:
  - name: eb-layout-master
    props:
      headerLink: ${{ location.pathname }}
      headerLogo: header-logo.svg
      extraUserMenuItems:
        - key: addin-config
          text: addin_config_menu
          on:
            click:
              emit:
                name: evt-open-addin-config-modal
                params:
                  selectedCategories: ${{ $rootStore.currentUser.outlookAddIn?.categoriesForTriggerAutoSuggestion ?? [] }}
        - key: about
          text: about
          on:
            click:
              emit: evt-open-about-modal
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
