---
name: ebuilder-component-eb-tabs
description: 'Deep component skill for eb-tabs. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-tabs in eBuilder component YAML.'
---

# eb-tabs Component Skill

## General Docs

Use `eb-tabs` make it easy to switch between different views. `eb-tabs` should be used together with `eb-tab-content`. Simple usages of `eb-tabs` and `eb-tab-content` You can toggle tab visibliity with `hide` prop You can set which tab should be actived at default with `initActiveTab` prop Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBTabs

## Main Props

| name             | type                 | description                    |
| ---------------- | -------------------- | ------------------------------ |
| cache            | EBTabsCache          | Specifies cache.               |
| eventToActiveTab | string               | Specifies event to active tab. |
| initActiveTab    | string               | Specifies init active tab.     |
| on.switch        | eBuilder events emit | Callback for switch.           |
| style            | CSSProperties        | Inline style overrides.        |
| tabs             | TabPanel[]           | Specifies tabs.                |

## Nested Props

### EBTabsCache

| name | type   | description    |
| ---- | ------ | -------------- |
| key  | string | Specifies key. |

### TabPanel

| name        | type            | description           |
| ----------- | --------------- | --------------------- |
| key         | string          | Specifies key.        |
| text        | string          | Specifies text.       |
| icon        | string          | Specifies icon.       |
| hide        | boolean         | Whether hide.         |
| forceRender | boolean         | Whether force render. |
| authorize   | AuthorizeAction | Specifies authorize.  |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: div
    children:
      - name: eb-tabs
        props:
          tabs:
            - key: setting
              text: Setting
              icon: fas,gear
            - key: edit
              text: Edit
              icon: fas,pen
            - key: profile
              text: Profile
              icon: fas,user
        children:
          - name: eb-tab-content
            slot: setting
            children: Setting content
          - name: eb-tab-content
            slot: edit
            children: Edit content
          - name: eb-tab-content
            slot: profile
            children: Profile content
```

### Story Example 2

```yaml
children:
  - name: div
    children:
      - name: eb-tabs
        props:
          tabs:
            - key: setting
              text: Setting
              icon: fas,gear
            - key: edit
              text: Edit
              icon: fas,pen
              hide: true
            - key: profile
              text: Profile
              icon: fas,user
        children:
          - name: eb-tab-content
            slot: setting
            children: Setting content
          - name: eb-tab-content
            slot: edit
            children: Edit content
          - name: eb-tab-content
            slot: profile
            children: Profile content
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
