---
name: ebuilder-component-eb-config-provider
description: 'Deep component skill for eb-config-provider. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-config-provider in eBuilder component YAML.'
---

# eb-config-provider Component Skill

## General Docs

No dedicated _.docs.mdx content is available. Guidance is inferred from _.types.ts, \*.stories.ts (when present), and real YAML usage in eClient repositories. Source directory in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBConfigProvider

## Main Props

| name         | type                                                                                                                 | description              |
| ------------ | -------------------------------------------------------------------------------------------------------------------- | ------------------------ |
| defaultProps | { ebTable?: Partial >; ebSelect?: Partial ; ebDescriptions?: Partial ; ebForm?: Partial ; ebBreadcrumb?: Partial ... | Specifies default props. |

## YAML Examples

### App Usage

```yaml
children:
  - name: eb-config-provider
    props:
      defaultProps:
        ebTable:
          pagination:
            size: 50
            options: [50, 100, 250, 500]
          enableExcelExport: true
          enableClearAllFilters: true
          multipleColsSort: true
          useMaxContentWidth: true
          mobileSafe: true
          columnsResizableByDefault: true
        ebSelect:
          showRefresh: true
        ebDescriptions:
          mobileSafe: true
        ebForm:
          showRequiredMark: true
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
