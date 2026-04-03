---
name: ebuilder-component-eb-pdf-viewer
description: 'Deep component skill for eb-pdf-viewer. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-pdf-viewer in eBuilder component YAML.'
---

# eb-pdf-viewer Component Skill

## General Docs

`eb-pdf` is a component for displaying PDF files. It uses the PDF.js library to render the PDF files. It supports all basic features like pagination, zooming, rotation, find, text selection, and more. Basic usage of `eb-pdf` component. Slot `extra` and `metadata` for custom content. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBPdfViewer

## Main Props

| name                  | type                                                   | description                                                                                                    |
| --------------------- | ------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------- |
| data                  | ArrayBuffer                                            | Specifies data.                                                                                                |
| disableKeyboard       | boolean                                                | Disable keyboard shortcuts. Default is `false`.                                                                |
| documentTitle         | string                                                 | Deprecated prop, are not supported in eb-pdf                                                                   |
| eventToFocus          | string                                                 | Event name to focus on a page. Event param should be `object` `{ page: page index (starts from 1) }` '         |
| extraMenuItems        | (() => EBPdfViewerMenuItem[]) \| EBPdfViewerMenuItem[] | Extra menu items to be added to the 3-dot menu in the toolbar.                                                 |
| file                  | File                                                   | Specifies file.                                                                                                |
| fileName              | string                                                 | Specifies file name.                                                                                           |
| metadataWidth         | number                                                 | The width of the right panel for showing `metadata` slot content. Default is 200.                              |
| mode                  | "legacy" \| "modern"                                   | `legacy` or `modern`. Specify pdfjs prebuilt package to use. Default is `legacy`.                              |
| on.attachmentSelected | eBuilder events emit                                   | Callback for attachment selected.                                                                              |
| on.download           | eBuilder events emit                                   | Callback for download.                                                                                         |
| on.load               | eBuilder events emit                                   | Callback for load.                                                                                             |
| on.pageChanged        | eBuilder events emit                                   | Callback for page changed.                                                                                     |
| on.thumbnailRendered  | eBuilder events emit                                   | Callback for thumbnail rendered.                                                                               |
| showThumbnailOnLoad   | boolean                                                | Deprecated prop, are not supported in eb-pdf                                                                   |
| sidebarInitialView    | "none" \| "thumbs"                                     | Default sidebar state, `none` means to close, `thumbs` means to open and show thumbnails. Default is `thumbs`. |
| task                  | TaskConfig                                             | eb-task configuration to fetch the PDF file.                                                                   |
| thumbnailWidth        | number                                                 | The width of the left panel for showing thumbnails. Default is 200.                                            |
| url                   | string                                                 | Specifies url.                                                                                                 |

## Nested Props

### EBPdfViewerMenuItem

| name        | type                  | description                |
| ----------- | --------------------- | -------------------------- |
| key         | string                | Specifies key.             |
| text        | string                | Specifies text.            |
| router-link | string                | Specifies ['router link']. |
| title       | string                | Specifies title.           |
| icon        | string                | Specifies icon.            |
| hide        | boolean               | Whether hide.              |
| disabled    | boolean               | Whether disabled.          |
| subItems    | EBPdfViewerMenuItem[] | Specifies sub items.       |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: div
    props:
      style:
        width: 100%
        height: 600px
    children:
      - name: eb-pdf
        props:
          url: https://mozilla.github.io/pdf.js/web/compressed.tracemonkey-pldi-09.pdf
```

### Story Example 2

```yaml
children:
  - name: div
    props:
      style:
        width: 100%
        height: 600px
    children:
      - name: eb-pdf
        props:
          url: https://mozilla.github.io/pdf.js/web/compressed.tracemonkey-pldi-09.pdf
        children:
          - name: div
            slot: extra
            children:
              - name: div
                children: Extra content
          - name: div
            slot: metadata
            children:
              - name: div
                children: Metadata content
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
