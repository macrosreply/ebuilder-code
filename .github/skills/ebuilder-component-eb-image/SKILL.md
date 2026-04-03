---
name: ebuilder-component-eb-image
description: 'Deep component skill for eb-image. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-image in eBuilder component YAML.'
---

# eb-image Component Skill

## General Docs

Use `eb-image` for rendering an image from eBuilder asset or with absolute image url Absolute image url Asset from eBuilder asset Image preview feature, click on image to see image Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBImage

## Main Props

| name     | type                 | description             |
| -------- | -------------------- | ----------------------- |
| alt      | string               | Specifies alt.          |
| asset    | string               | Specifies asset.        |
| height   | number               | Specifies height.       |
| on.click | eBuilder events emit | Callback for click.     |
| preview  | boolean              | Whether preview.        |
| style    | CSSProperties        | Inline style overrides. |
| title    | string               | Specifies title.        |
| width    | number               | Specifies width.        |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-image
    props:
      asset: https://fastly.picsum.photos/id/20/3670/2462.jpg?hmac=CmQ0ln-k5ZqkdtLvVO23LjVAEabZQx2wOaT4pyeG10I
      alt: eb-image
      width: 500
```

### Story Example 2

```yaml
children:
  - name: eb-image
    props:
      asset: Natur.png
      alt: eb-image
      width: 500
```

### Story Example 3

```yaml
children:
  - name: eb-image
    props:
      asset: https://fastly.picsum.photos/id/16/2500/1667.jpg?hmac=uAkZwYc5phCRNFTrV_prJ_0rP0EdwJaZ4ctje2bY7aE
      alt: eb-image
      width: 500
      preview: true
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
