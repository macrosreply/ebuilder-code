---
name: ebuilder-component-eb-layout-sidebar
description: 'Deep component skill for eb-layout-sidebar. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-layout-sidebar in eBuilder component YAML.'
---

# eb-layout-sidebar Component Skill

## General Docs

No dedicated \*.docs.mdx content is available. Guidance is inferred from \*.types.ts, \*.stories.ts (when present), and real YAML usage in eClient repositories. Source directory in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/layouts/EBLayoutSidebar

## Main Props

| name               | type                       | description                    |
| ------------------ | -------------------------- | ------------------------------ |
| class              | string                     | CSS class overrides.           |
| collapsedByDefault | boolean                    | Whether collapsed by default.  |
| collapsedCacheKey  | string                     | Specifies collapsed cache key. |
| contentPadding     | string \| number           | Specifies content padding.     |
| eventToToggle      | string                     | Specifies event to toggle.     |
| header             | boolean \| HeaderMenuProps | Specifies header.              |
| hideDefaultTrigger | boolean                    | Whether hide default trigger.  |
| sidebarCollapsible | boolean                    | Whether sidebar collapsible.   |
| width              | number                     | Specifies width.               |

## YAML Examples

### Minimal Example

```yaml
children:
  - name: eb-layout-sidebar
    props:
      width: 300
      sidebarCollapsible: true
      hideDefaultTrigger: true
      eventToToggle: evt-adv-search-toggle-sidebar
    children:
      - name: div
        slot: sidebar
        props:
          style:
            height: 100%
            display: flex
            flex-direction: column
            overflow: hidden
        children:
          - name: div
            props:
              style:
                display: flex
                align-items: center
                justify-content: space-between
                padding: var(--padding-sm) var(--padding-md)
                border-bottom: 1px solid var(--color-split)
            children:
              - name: eb-typography
                props:
                  contentType: title
                  level: 5
                  text: adv_search_form_title
                  titleNoBottomMargin: true
              - name: eb-button
                props:
                  icon: fas,window-maximize
                  style:
                    transform: rotate(270deg)
                  type: text
                  on:
                    click:
                      emit: evt-adv-search-toggle-sidebar
          - name: div
            props:
              style:
                flex: 1
                overflow-x: hidden
                overflow-y: auto
                margin: var(--padding-sm) var(--padding-sm) 0 var(--padding-sm)
            children:
              - name: advanced-search-form
                props:
                  searchId: ${{ $route.params.searchId }}
      - name: div
        slot: collapsed-sidebar
        props:
          style:
            display: flex
            align-items: center
            justify-content: space-between
            padding: var(--padding-sm) var(--padding-md)
            border-bottom: 1px solid var(--color-split)
        children:
          - name: eb-button
            props:
              icon: fas,magnifying-glass
              type: text
              on:
                click:
                  emit: evt-adv-search-toggle-sidebar
      - name: advanced-search-results
        props:
          searchId: ${{ $route.params.searchId }}
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
