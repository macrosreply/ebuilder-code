---
name: ebuilder-component-eb-layout-sidebar-menu
description: 'Deep component skill for eb-layout-sidebar-menu. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-layout-sidebar-menu in eBuilder component YAML.'
---

# eb-layout-sidebar-menu Component Skill

## General Docs

No dedicated \*.docs.mdx content is available. Guidance is inferred from \*.types.ts, \*.stories.ts (when present), and real YAML usage in eClient repositories. Source directory in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/layouts/EBLayoutSidebarMenu

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
| items              | EBLayoutSidebarMenuItem[]  | Specifies items.               |
| sidebarCollapsible | boolean                    | Whether sidebar collapsible.   |
| width              | number                     | Specifies width.               |

## Nested Props

### EBLayoutSidebarMenuItem

| name        | type                      | description            |
| ----------- | ------------------------- | ---------------------- |
| key         | string                    | Specifies key.         |
| router-link | string                    | Specifies router link. |
| text        | string                    | Specifies text.        |
| icon        | string                    | Specifies icon.        |
| subItems    | EBLayoutSidebarMenuItem[] | Specifies sub items.   |
| authorize   | AuthorizeAction           | Specifies authorize.   |
| hide        | boolean                   | Whether hide.          |

## YAML Examples

### Minimal Example

```yaml
children:
  - name: eb-layout-sidebar-menu
    props:
      width: 250
      collapsedCacheKey: root-sidebar-menu
      collapsedByDefault: true
      items:
        - key: open-documents
          text: root_menu_open_documents
          icon: fas,file
          router-link: ${{ `/inbox/${$route.params.inboxId}/open-documents` }}
        - key: open-checklists
          text: root_menu_open_checklists
          icon: fas,list-check
          router-link: ${{ `/inbox/${$route.params.inboxId}/open-checklists` }}
          authorize:
            policy: eClient
            checks:
              - ${{ EB.externals.auth.authorizeCheck('editChecklist', EB.constant.OBJECT_IDX.INBOX) }}
        - key: open-workflow-todos
          text: root_menu_open_workflow_todos
          icon: fas,diagram-project
          router-link: ${{ `/inbox/${$route.params.inboxId}/open-workflow-todos` }}
          authorize:
            policy: eClient
            checks:
              - ${{ EB.externals.auth.authorizeCheck('editToDo', EB.constant.OBJECT_IDX.INBOX) }}
        - key: follow-ups
          text: root_menu_follow_ups
          icon: fas,calendar-days
          router-link: ${{ `/inbox/${$route.params.inboxId}/follow-ups` }}
        - key: processed-documents
          text: root_menu_processed_documents
          icon: fas,check
          router-link: ${{ `/inbox/${$route.params.inboxId}/processed-documents` }}
        - key: open-archives
          text: root_menu_open_archives
          icon: fas,box-archive
          router-link: ${{ `/inbox/${+$route.params.inboxId}/open-archives` }}
        - key: queries
          text: root_menu_queries
          icon: fas,magnifying-glass
          router-link: ${{ `/inbox/${+$route.params.inboxId}/queries` }}
    children:
      - name: div
        props:
          style:
            flex: 1
            overflow: hidden
        children:
          - name: eb-router-view
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
