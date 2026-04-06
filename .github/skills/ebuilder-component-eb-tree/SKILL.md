---
name: ebuilder-component-eb-tree
description: "Deep component skill for eb-tree. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-tree in eBuilder component YAML."
---

# eb-tree Component Skill

## General Docs

Use `eb-tree` for showing parent-children data in tree view style Tree with initialized data, you can notice that you can use `subs` to define child nodes of a node. In this way, you can build the whole tree without using async loading for each branch. Tree without initialized data, `eb-tree` will trigger `loadData` with `null` `$treeNode` Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBTree

## Main Props

| name             | type                                                                                                                                       | description                                                                                                                                                                |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| autoHeight       | { minHeight: number; offset?: number; }                                                                                                    | Config eb-tree take the full height of its parent. We recommend to make its parent `flex: 1` or `height: fixed_value`. This prop is disabled if `height` is set.           |
| contextMenuItems | EBTreeMenuItemConfig[]                                                                                                                     | Specifies context menu items.                                                                                                                                              |
| data             | EBTreeItem[]                                                                                                                               | Fixed data to show on tree                                                                                                                                                 |
| ebTreeDataSource | ($treeNode: EventDataNode, $searchTerm: string) => ({ type: "query"; } & QueryConfig) \| { type: "task"; name: string; params?: Record ; } | Function to get eb-query or eb-task to get data                                                                                                                            |
| eventToExpand    | string                                                                                                                                     | Name of event which tree should subscribe to select expand a node. Event param should be { key: node_key or node_key[] or null, state: keep-open \| keep-close \| toggle } |
| eventToSelect    | string                                                                                                                                     | Name of event which tree should subscribe to select a node. Event param should be { key: node_key or node_key[] or null }                                                  |
| expandOnSelect   | boolean                                                                                                                                    | expand node by selecting on title (default: true) otherwise expand only when click icon                                                                                    |
| height           | number                                                                                                                                     | Height of the tree, set if you want to use virtual scroll                                                                                                                  |
| isLeaf           | (item: EBTreeItem) => boolean                                                                                                              | Callback for is leaf.                                                                                                                                                      |
| isSelectable     | (item: EBTreeItem) => boolean                                                                                                              | Callback for is selectable.                                                                                                                                                |
| loadData         | ($treeNode: EventDataNode) => Promise                                                                                                      | Custom function to get data                                                                                                                                                |
| nodeKey          | string                                                                                                                                     | Name of property should be used for node key                                                                                                                               |
| noDeselect       | boolean                                                                                                                                    | prevent deselecting a selected node on click                                                                                                                               |
| on.select        | eBuilder events emit                                                                                                                       | Callback for select.                                                                                                                                                       |
| search           | EBTreeSearchConfig                                                                                                                         | The property we should search on                                                                                                                                           |
| showLine         | boolean                                                                                                                                    | show vertical tree line                                                                                                                                                    |
| treeType         | string                                                                                                                                     | `directory` (default) or `normal`                                                                                                                                          |

## Nested Props

### EBTreeItem

| name            | type             | description                                                                          |
| --------------- | ---------------- | ------------------------------------------------------------------------------------ |
| subs            | EBTreeItem[]     | Specifies subs.                                                                      |
| children        | EBTreeItem[]     | Nested child configuration.                                                          |
| key             | string \| number | No official description found; inferred from type declaration.                       |
| title           | any              | No official description found; inferred from type declaration.                       |
| checkable       | boolean          | No official description found; inferred from type declaration.                       |
| disabled        | boolean          | No official description found; inferred from type declaration.                       |
| disableCheckbox | boolean          | No official description found; inferred from type declaration.                       |
| icon            | any              | No official description found; inferred from type declaration.                       |
| isLeaf          | boolean          | No official description found; inferred from type declaration.                       |
| selectable      | boolean          | No official description found; inferred from type declaration.                       |
| switcherIcon    | any              | No official description found; inferred from type declaration.                       |
| class           | string           | Set style of TreeNode. This is not recommend if you don't have any force requirement |
| style           | CSSProperties    | No official description found; inferred from type declaration.                       |
| slots           | Record           | No official description found; inferred from type declaration.                       |

### EBTreeMenuItemConfig

| name                  | type                                                                                                      | description                                                    |
| --------------------- | --------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| key                   | string                                                                                                    | No official description found; inferred from type declaration. |
| text                  | string                                                                                                    | No official description found; inferred from type declaration. |
| title                 | string                                                                                                    | No official description found; inferred from type declaration. |
| icon                  | string                                                                                                    | No official description found; inferred from type declaration. |
| hide                  | boolean                                                                                                   | No official description found; inferred from type declaration. |
| disabled              | boolean                                                                                                   | No official description found; inferred from type declaration. |
| group                 | string                                                                                                    | No official description found; inferred from type declaration. |
| labelForGroup         | string                                                                                                    | No official description found; inferred from type declaration. |
| authorize             | AuthorizeAction                                                                                           | No official description found; inferred from type declaration. |
| subItems              | EBContextMenuItemConfig [] \| (($event: { node: EBTreeItem; }) => EBContextMenuItemConfig [] \| Promise ) | No official description found; inferred from type declaration. |
| ebTreeMenuItemHide    | EBContextDataChecker                                                                                      | Specifies eb tree menu item hide.                              |
| ebTreeMenuItemDisable | EBContextDataChecker                                                                                      | Specifies eb tree menu item disable.                           |

### EBTreeSearchConfig

| name    | type    | description        |
| ------- | ------- | ------------------ |
| enabled | boolean | Specifies enabled. |

## YAML Examples

### Story Example 1

```yaml
- name: eb-tree
  props:
    loadData: |
      ${{
        ($treeNode) => {
          const key = $treeNode?.eventKey || 'root';
          return new Promise((resolve) => {
            setTimeout(() => {
              resolve([
                { title: `Folder ${key}-0`, key: `${key}-0` },
                { title: `Folder ${key}-1`, key: `${key}-1` }
              ]);
            }, 2000);
          });
        }
      }}
    data:
      - title: Folder 1
        key: 0
      - title: Folder 2
        key: 1
      - title: Folder 3
        key: 2
        subs:
          - title: Folder 3-1
            key: 3
          - title: Folder 3-2
            key: 4
    on:
      select:
        name: eb-router-push
        params:
          path: "${{ `/explorer/folder/${$event.item.key}` }}"
  children:
    - name: div
      slot: title
      children: ${{ $ctx.$parent.title }}
```

### Story Example 2

```yaml
- name: eb-tree
  props:
    loadData: |
      ${{
        ($treeNode) => {
          const key = $treeNode?.eventKey || 'root';
          return new Promise((resolve) => {
            setTimeout(() => {
              resolve([
                { title: `Folder ${key}-0`, key: `${key}-0` },
                { title: `Folder ${key}-1`, key: `${key}-1` }
              ]);
            }, 2000);
          });
        }
      }}
    on:
      select:
        name: eb-router-push
        params:
          path: "${{ `/explorer/folder/${$event.item.key}` }}"
  children:
    - name: div
      slot: title
      children: ${{ $ctx.$parent.title }}
```

### Story Example 3

```yaml
- name: eb-tree
  props:
    loadData: |
      ${{
        ($treeNode) => {
          const key = $treeNode?.eventKey || 'root';
          return new Promise((resolve) => {
            setTimeout(() => {
              resolve([
                { title: `Folder ${key}-0`, key: `${key}-0` },
                { title: `Folder ${key}-1`, key: `${key}-1` }
              ]);
            }, 2000);
          });
        }
      }}
    on:
      select:
        emit:
          name: eb-router-push
          params:
            path: "${{ `/explorer/folder/${$event.item.key}` }}"
    search:
      enabled: true
      propToSearch: title
  children:
    - name: div
      slot: title
      children: ${{ $ctx.$parent.title }}
```

### Story Example 4

```yaml
- name: eb-tree
  props:
    loadData: |
      ${{
        ($treeNode) => {
          const key = $treeNode?.eventKey || 'root';
          return new Promise((resolve) => {
            setTimeout(() => {
              resolve([
                { title: `Folder ${key}-0`, key: `${key}-0`, isLeaf: true },
                { title: `Folder ${key}-1`, key: `${key}-1`, isLeaf: false }
              ]);
            }, 2000);
          });
        }
      }}
    data:
      - title: Folder 1
        isLeaf: true
        key: 0
      - title: Folder 2
        isLeaf: false
        key: 1
      - title: No icon
        isLeaf: false
        key: 2
    isLeaf: ${{ item => Boolean(item.isLeaf) }}
    on:
      select:
        emit:
          name: eb-router-push
          params:
            path: "${{ `/explorer/folder/${$event.item.key}` }}"
  children:
    - name: div
      slot: title
      children:
        - name: b
          props:
            color: red
          children: ${{ $ctx.$parent.$parent.title }}
    - name: eb-icon
      slot: icon
      props:
        icon: "${{ $ctx.title === 'No icon' ? null : $ctx.isLeaf ? 'fas,copy' : $ctx.expanded ? 'fas,folder-open' : 'fas,folder' }}"
```

### Story Example 5

```yaml
- name: eb-tree
  props:
    loadData: |
      ${{
        ($treeNode) => {
          const key = $treeNode?.eventKey || 'root';
          return new Promise((resolve) => {
            setTimeout(() => {
              resolve([
                { title: `Folder ${key}-0`, key: `${key}-0` },
                { title: `Folder ${key}-1`, key: `${key}-1` }
              ]);
            }, 2000);
          });
        }
      }}
    data:
      - title: Folder 1
        key: 0
      - title: Folder 2
        key: 1
    contextMenuItems:
      - key: copy
        text: Copy
        title: Copy
        icon: fas,copy
      - key: search
        text: Search
        title: Search
        icon: fas,search
        subItems:
          - key: search-all
            text: Search all
            title: Search all files in all folders
            ebTreeMenuItemHide:
              - ${{ $event.node.key.endsWith('0') }}
          - key: search-direct
            text: Search direct
            title: Search all direct files in this folder
      - key: info
        text: Info
        title: Info
        icon: fas,info
        ebTreeMenuItemDisable:
          - ${{ $event.node.key.endsWith('1') }}
      - key: delete
        text: Delete
        title: Delete
        icon: fas,trash
        disabled: true
    on:
      select:
        emit:
          name: eb-router-push
          params:
            path: "${{ `/explorer/folder/${$event.item.key}` }}"
  children:
    - name: div
      slot: title
      children: ${{ $ctx.$parent.title }}
```

### Story Example 6

```yaml
- name: div
  props:
    style:
      margin-bottom: var(--padding-md)
  children:
    - name: eb-button
      props:
        text: 'Select "Folder 2"'
      on:
        click:
          emit:
            name: sb-tree-select
            params:
              key: "1"
- name: eb-tree
  props:
    loadData: |
      ${{
        ($treeNode) => {
          const key = $treeNode?.eventKey || 'root';
          return new Promise((resolve) => {
            setTimeout(() => {
              resolve([
                { title: `Folder ${key}-0`, key: `${key}-0` },
                { title: `Folder ${key}-1`, key: `${key}-1` }
              ]);
            }, 2000);
          });
        }
      }}
    data:
      - title: Folder 1
        key: 0
      - title: Folder 2
        key: 1
    eventToSelect: sb-tree-select
    on:
      select:
        emit:
          name: eb-router-push
          params:
            path: "${{ `/explorer/folder/${$event.item.key}` }}"
  children:
    - name: div
      slot: title
      children: ${{ $ctx.$parent.title }}
```

### Story Example 7

```yaml
rootVars:
  containerHeight: 100px

children:
  - name: div
    props:
      style:
        width: 400px
    children:
      - name: div
        props:
          style:
            margin-bottom: var(--padding-md)
        children:
          - name: eb-button
            props:
              text: Container height: 100px
            on:
              click:
                emit:
                  name: eb-manual
                  handler: |
                    ${{
                      () => {
                        $rootVars.containerHeight = '100px'
                      }
                    }}
          - name: eb-button
            props:
              text: Container height: 150px
            on:
              click:
                emit:
                  name: eb-manual
                  handler: |
                    ${{
                      () => {
                        $rootVars.containerHeight = '150px'
                      }
                    }}
          - name: eb-button
            props:
              text: Container height: 200px
            on:
              click:
                emit:
                  name: eb-manual
                  handler: |
                    ${{
                      () => {
                        $rootVars.containerHeight = '200px'
                      }
                    }}
      - name: div
        props:
          style:
            height: ${{ $rootVars.containerHeight }}
        children:
          - name: eb-tree
            props:
              loadData: |
                ${{
                  ($treeNode) => {
                    const key = $treeNode?.eventKey || 'root';
                    return new Promise((resolve) => {
                      setTimeout(() => {
                        resolve([
                          { title: `Folder ${key}-0`, key: `${key}-0` },
                          { title: `Folder ${key}-1`, key: `${key}-1` }
                        ]);
                      }, 2000);
                    });
                  }
                }}
              autoHeight:
                minHeight: 120
              data:
                - title: Folder 1
                  key: 0
                - title: Folder 2
                  key: 1
              on:
                select:
                  emit:
                    name: eb-router-push
                    params:
                      path: '${{ `/explorer/folder/${$event.item.key}` }}'
            children:
              - name: div
                slot: title
                children: ${{ $ctx.$parent.title }}
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
