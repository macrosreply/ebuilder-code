---
name: ebuilder-component-eb-image-viewer
description: 'Deep component skill for eb-image-viewer. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-image-viewer in eBuilder component YAML.'
---

# eb-image-viewer Component Skill

## General Docs

Use `eb-image-viewer` to display one or more images in an inline gallery. The component can show an optional title and exposes an `extra` slot for supplementary content in the header. Basic usage of `eb-image-viewer` with a list of absolute image URLs. Display a header by setting the `title` prop. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBImageViewer

## Main Props

| name   | type     | description                                             |
| ------ | -------- | ------------------------------------------------------- |
| images | string[] | Absolute image URLs rendered by the inline viewer.      |
| title  | string   | Optional header text message id shown above the viewer. |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: div
    props:
      style:
        width: 100%
        height: 480px
    children:
      - name: eb-image-viewer
        props:
          images:
            - https://fastly.picsum.photos/id/20/3670/2462.jpg?hmac=CmQ0ln-k5ZqkdtLvVO23LjVAEabZQx2wOaT4pyeG10I
            - https://fastly.picsum.photos/id/16/2500/1667.jpg?hmac=uAkZwYc5phCRNFTrV_prJ_0rP0EdwJaZ4ctje2bY7aE
            - https://fastly.picsum.photos/id/48/5000/3333.jpg?hmac=fyGjd90XW1d5XFo_Hf2qBfjD1SL8nR8Pq2K2s6Frlbc
```

### Story Example 2

```yaml
children:
  - name: div
    props:
      style:
        width: 100%
        height: 480px
    children:
      - name: eb-image-viewer
        props:
          title: Image gallery
          images:
            - https://fastly.picsum.photos/id/20/3670/2462.jpg?hmac=CmQ0ln-k5ZqkdtLvVO23LjVAEabZQx2wOaT4pyeG10I
            - https://fastly.picsum.photos/id/16/2500/1667.jpg?hmac=uAkZwYc5phCRNFTrV_prJ_0rP0EdwJaZ4ctje2bY7aE
            - https://fastly.picsum.photos/id/48/5000/3333.jpg?hmac=fyGjd90XW1d5XFo_Hf2qBfjD1SL8nR8Pq2K2s6Frlbc
```

### Story Example 3

```yaml
children:
  - name: div
    props:
      style:
        width: 100%
        height: 480px
    children:
      - name: eb-image-viewer
        props:
          title: Project attachments
          images:
            - https://fastly.picsum.photos/id/20/3670/2462.jpg?hmac=CmQ0ln-k5ZqkdtLvVO23LjVAEabZQx2wOaT4pyeG10I
            - https://fastly.picsum.photos/id/16/2500/1667.jpg?hmac=uAkZwYc5phCRNFTrV_prJ_0rP0EdwJaZ4ctje2bY7aE
            - https://fastly.picsum.photos/id/48/5000/3333.jpg?hmac=fyGjd90XW1d5XFo_Hf2qBfjD1SL8nR8Pq2K2s6Frlbc
        children:
          - name: span
            slot: extra
            children: 3 images
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
