---
name: ebuilder-component-eb-table
description: 'Deep component skill for eb-table. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-table in eBuilder component YAML.'
---

# eb-table Component Skill

## General Docs

Use `eb-table` to build a table displays a collection of structured data with sorting, pagination, filtering features Below is a basic `eb-table` usage, `eb-table` works best with `eb-query`, its sorting, fitering and pagination features use `eb-query` params. For each `eb-table` instance, you have to define `rowKey` which could be a string which is the name of Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBTable

## Main Props

| name                      | type                                                                                                                                                                                                                                                                                          | description                                                                                                                                                                                                            |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| autoHeight                | { minHeight: number; }                                                                                                                                                                                                                                                                        | Config table take the full height of its parent. We recommend to make its parent `flex: 1` or `height: fixed_value`. This prop is disabled if `preferHeight` is set.                                                   |
| cache                     | EBCacheOptions                                                                                                                                                                                                                                                                                | Specifies cache.                                                                                                                                                                                                       |
| cols                      | EBTableColProps[]                                                                                                                                                                                                                                                                             | Specifies cols.                                                                                                                                                                                                        |
| columnMinWidth            | string \| number                                                                                                                                                                                                                                                                              | Specifies column min width.                                                                                                                                                                                            |
| columnsOrderToReset       | string[]                                                                                                                                                                                                                                                                                      | Specifies columns order to reset.                                                                                                                                                                                      |
| columnsResizableByDefault | boolean                                                                                                                                                                                                                                                                                       | Whether columns resizable by default.                                                                                                                                                                                  |
| contextMenuItems          | EBTableMenuItemConfig[]                                                                                                                                                                                                                                                                       | Specifies context menu items.                                                                                                                                                                                          |
| contextMenuShortcutMode   | "none" \| "default" \| "dropdown"                                                                                                                                                                                                                                                             | Specifies context menu shortcut mode.                                                                                                                                                                                  |
| data                      | EBTableRecord[]                                                                                                                                                                                                                                                                               | Specifies data.                                                                                                                                                                                                        |
| dataSource                | { type: "task"; name: string; params?: Record ; cache?: { save?: string[]; }; } \| (QueryConfig & { type: "query"; })                                                                                                                                                                         | eb-query or eb-task to get data                                                                                                                                                                                        |
| dataTransform             | (data: unknown) => unknown                                                                                                                                                                                                                                                                    | Use this prop to edit received record objects before processing table rendering. We can use this to add new fields, change existing fields, ... to use them in rendering callbacks like column.children/rowStyles/etc. |
| defaultExpandAllRows      | boolean                                                                                                                                                                                                                                                                                       | Whether default expand all rows.                                                                                                                                                                                       |
| disableRowSelection       | boolean                                                                                                                                                                                                                                                                                       | Whether disable row selection.                                                                                                                                                                                         |
| edit                      | { mode?: "table" \| "row" \| "cell" \| "none"; eventToSubmit?: string; on?: { submit?: ($event: unknown) => void; }; }                                                                                                                                                                        | Specifies edit.                                                                                                                                                                                                        |
| enableClearAllFilters     | boolean                                                                                                                                                                                                                                                                                       | Whether enable clear all filters.                                                                                                                                                                                      |
| enableExcelExport         | boolean                                                                                                                                                                                                                                                                                       | Whether enable excel export.                                                                                                                                                                                           |
| enableRowExpandable       | boolean                                                                                                                                                                                                                                                                                       | Whether enable row expandable.                                                                                                                                                                                         |
| enableSummary             | boolean                                                                                                                                                                                                                                                                                       | Whether enable summary.                                                                                                                                                                                                |
| eventToExpand             | string                                                                                                                                                                                                                                                                                        | Specifies event to expand.                                                                                                                                                                                             |
| eventToFill               | string                                                                                                                                                                                                                                                                                        | Specifies event to fill.                                                                                                                                                                                               |
| eventToLoad               | string                                                                                                                                                                                                                                                                                        | Specifies event to load.                                                                                                                                                                                               |
| eventToRefresh            | string \| string[]                                                                                                                                                                                                                                                                            | Specifies event to refresh.                                                                                                                                                                                            |
| eventToSelect             | string                                                                                                                                                                                                                                                                                        | Specifies event to select.                                                                                                                                                                                             |
| eventToSetColumnsOrder    | string                                                                                                                                                                                                                                                                                        | Specifies event to set columns order.                                                                                                                                                                                  |
| eventToSetColumnsState    | string                                                                                                                                                                                                                                                                                        | Specifies event to set columns state.                                                                                                                                                                                  |
| eventToSetFilters         | string                                                                                                                                                                                                                                                                                        | Specifies event to set filters.                                                                                                                                                                                        |
| eventToSetOrders          | string                                                                                                                                                                                                                                                                                        | Specifies event to set orders.                                                                                                                                                                                         |
| eventToUpdateRow          | string                                                                                                                                                                                                                                                                                        | Specifies event to update row.                                                                                                                                                                                         |
| excelQueryConfigTransform | (excelQueryConfig: ExcelQueryConfig) => ExcelQueryConfig                                                                                                                                                                                                                                      | Use this prop to change excelQueryConfig before sending it in excel query request.                                                                                                                                     |
| expandedRow               | false \| { children: { default: (props: { record: unknown; index: number; }) => unknown; }; }                                                                                                                                                                                                 | Specifies expanded row.                                                                                                                                                                                                |
| expandIcons               | { expand?: string; collapse?: string; style?: Partial ; colWidth?: number; fixed?: boolean; children?: { default?: (props: { expandable: boolean; expanded: boolean; record: unknown; onExpand: (record: unknown) => void; }) => unknown; }; }                                                | Specifies expand icons.                                                                                                                                                                                                |
| filters                   | QueryFilter[]                                                                                                                                                                                                                                                                                 | No official description found; inferred from type declaration.                                                                                                                                                         |
| hideColumnIcons           | boolean                                                                                                                                                                                                                                                                                       | Whether hide column icons.                                                                                                                                                                                             |
| initialColumnsOrder       | string[]                                                                                                                                                                                                                                                                                      | Specifies initial columns order.                                                                                                                                                                                       |
| initialColumnsWidth       | Record                                                                                                                                                                                                                                                                                        | Specifies initial columns width.                                                                                                                                                                                       |
| initialFilters            | QueryFilter[]                                                                                                                                                                                                                                                                                 | Specifies initial filters.                                                                                                                                                                                             |
| initialOrders             | QueryOrder[]                                                                                                                                                                                                                                                                                  | Specifies initial orders.                                                                                                                                                                                              |
| initialVisibleColumns     | string[]                                                                                                                                                                                                                                                                                      | Specifies initial visible columns.                                                                                                                                                                                     |
| isRowExpandable           | (record: unknown) => boolean                                                                                                                                                                                                                                                                  | Callback for is row expandable.                                                                                                                                                                                        |
| menuItems                 | EBTableMenuItemConfig[]                                                                                                                                                                                                                                                                       | Specifies menu items.                                                                                                                                                                                                  |
| mobileSafe                | boolean                                                                                                                                                                                                                                                                                       | eb-table will switch to use `contextMenuShortcutMode: dropdown` and `pagination.noLabel: true` when the screen width is less than 576px (xs breakpoint)                                                                |
| multipleColsSort          | boolean                                                                                                                                                                                                                                                                                       | Whether multiple cols sort.                                                                                                                                                                                            |
| multipleSelection         | boolean                                                                                                                                                                                                                                                                                       | Whether multiple selection.                                                                                                                                                                                            |
| on.change                 | eBuilder events emit                                                                                                                                                                                                                                                                          | Callback for change.                                                                                                                                                                                                   |
| on.click                  | eBuilder events emit                                                                                                                                                                                                                                                                          | Callback for click.                                                                                                                                                                                                    |
| on.columns                | eBuilder events emit                                                                                                                                                                                                                                                                          | Callback for columns.                                                                                                                                                                                                  |
| on.filters                | eBuilder events emit                                                                                                                                                                                                                                                                          | Callback for filters.                                                                                                                                                                                                  |
| on.load                   | eBuilder events emit                                                                                                                                                                                                                                                                          | Callback for load.                                                                                                                                                                                                     |
| on.orders                 | eBuilder events emit                                                                                                                                                                                                                                                                          | Callback for orders.                                                                                                                                                                                                   |
| pagination                | { size?: number; options?: number[]; hide?: boolean; noLabel?: boolean; autoHide?: boolean; }                                                                                                                                                                                                 | Specifies pagination.                                                                                                                                                                                                  |
| params                    | Record                                                                                                                                                                                                                                                                                        | Specifies params.                                                                                                                                                                                                      |
| parentChild               | { indentSize?: number; parentRefKey: string; parentValueKey: string; defaultExpandAll?: boolean; }                                                                                                                                                                                            | Specifies parent child.                                                                                                                                                                                                |
| preferHeight              | string                                                                                                                                                                                                                                                                                        | Specifies prefer height.                                                                                                                                                                                               |
| preventUnselectRowOnClick | boolean                                                                                                                                                                                                                                                                                       | Whether prevent unselect row on click.                                                                                                                                                                                 |
| query                     | QueryConfig                                                                                                                                                                                                                                                                                   | Specifies query.                                                                                                                                                                                                       |
| queryConfigTransform      | (queryConfig: QueryConfig) => QueryConfig                                                                                                                                                                                                                                                     | Use this prop to change queryConfig (orders, filters, params) before sending it in query/task request. We can use this to edit orders, filters, params, ... but still keep the selected filters on UI                  |
| reorder                   | { disable?: boolean; draggable?: boolean; dragHandleColumn?: string; orderKey: string; pkKey?: string; mutation?: string; mutationParamsTransform?: (order: number, key: string \| number) => Record ; queryNextSort?: { ...; }; updateRecordsOrderKeyValue?: (records: Record []) => void; } | Specifies reorder.                                                                                                                                                                                                     |
| rowClass                  | string \| ((record: Record , index: number) => string)                                                                                                                                                                                                                                        | Specifies row class.                                                                                                                                                                                                   |
| rowKey                    | string \| ((record: EBTableRecord) => EBRowKeyType)                                                                                                                                                                                                                                           | Specifies row key.                                                                                                                                                                                                     |
| rowStyle                  | CSSProperties \| ((record: Record , index: number) => CSSProperties)                                                                                                                                                                                                                          | Inline styles for each row. This can be a fixed style or a function that returns a style object based on the record and index.                                                                                         |
| sectionRowIndexes         | number[]                                                                                                                                                                                                                                                                                      | Specifies section row indexes.                                                                                                                                                                                         |
| selectRowOnClick          | boolean                                                                                                                                                                                                                                                                                       | Whether select row on click.                                                                                                                                                                                           |
| selectRowOnContextMenu    | boolean                                                                                                                                                                                                                                                                                       | Whether select row on context menu.                                                                                                                                                                                    |
| size                      | "middle" \| "small" \| "large"                                                                                                                                                                                                                                                                | Specifies size.                                                                                                                                                                                                        |
| sort                      | { by: string; direction: "ASC" \| "DESC"; local?: boolean; }                                                                                                                                                                                                                                  | Single col sorting, for multiple cols sorting, please use cols[].sortByDefault and multipleColsSort                                                                                                                    |
| style                     | CSSProperties                                                                                                                                                                                                                                                                                 | Inline style overrides.                                                                                                                                                                                                |
| title                     | string \| { children: { default?: () => unknown[]; }; }                                                                                                                                                                                                                                       | Specifies title.                                                                                                                                                                                                       |
| unmountExpandedOnCollapse | boolean                                                                                                                                                                                                                                                                                       | Whether unmount expanded on collapse.                                                                                                                                                                                  |
| useMaxContentWidth        | boolean                                                                                                                                                                                                                                                                                       | Whether use max content width.                                                                                                                                                                                         |

## Nested Props

### EBCacheOptions

| name | type          | description     |
| ---- | ------------- | --------------- |
| key  | string        | Specifies key.  |
| mode | EBStorageType | Specifies mode. |

### EBRowKeyType

| name           | type                                                                                                                                                             | description                                                                                                                            |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| toString       | (() => string) \| ((radix?: number) => string)                                                                                                                   | Returns a string representation of a string. Returns a string representation of an object.                                             |
| valueOf        | (() => string) \| (() => number)                                                                                                                                 | Returns the primitive value of the specified object.                                                                                   |
| toLocaleString | (() => string) \| { (locales?: string \| string[], options?: NumberFormatOptions): string; (locales?: LocalesArgument, options?: NumberFormatOptions): string; } | Returns a date converted to a string using the current locale. Converts a number to a string by using the current or specified locale. |

### EBTableColProps

| name                 | type                                                                                                                                                                 | description                                                                                                |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| key                  | string                                                                                                                                                               | Specifies key.                                                                                             |
| dataIndex            | string                                                                                                                                                               | Specifies data index.                                                                                      |
| hide                 | boolean                                                                                                                                                              | Whether hide.                                                                                              |
| hideToggleable       | boolean                                                                                                                                                              | Whether hide toggleable.                                                                                   |
| ellipsis             | boolean                                                                                                                                                              | Whether ellipsis.                                                                                          |
| multiLines           | boolean                                                                                                                                                              | Whether multi lines.                                                                                       |
| maxLines             | number                                                                                                                                                               | Specifies max lines.                                                                                       |
| edit                 | any                                                                                                                                                                  | Specifies edit.                                                                                            |
| resizable            | boolean                                                                                                                                                              | Whether resizable.                                                                                         |
| title                | string \| { default?: string; hover?: string; inFiltersAndColumnsPopUp?: string; children?: { default?: () => unknown[]; }; }                                        | Specifies title.                                                                                           |
| width                | string \| number                                                                                                                                                     | Specifies width.                                                                                           |
| sorter               | boolean \| ((firstEle: unknown, secondEle: unknown) => number) \| { compare: (firstEle: unknown, secondEle: unknown) => number; multiple: number; }                  | Specifies sorter.                                                                                          |
| sortByDefault        | { direction: "ASC" \| "DESC"; priority: number; }                                                                                                                    | Specifies sort by default.                                                                                 |
| filter               | boolean \| EBTableFilterConfig                                                                                                                                       | Disable filter for this column with `false`, enable default filter with `true` or use custom filter config |
| filterByDefault      | Omit                                                                                                                                                                 | Specifies filter by default.                                                                               |
| align                | string                                                                                                                                                               | Specifies align.                                                                                           |
| children             | { default?: (event: { text: unknown; record: unknown; index: number; }) => unknown; edit?: (event: { text: unknown; record: unknown; index: number; }) => unknown; } | Nested child configuration.                                                                                |
| transform            | (event: { record: unknown; index: number; }) => Promise                                                                                                              | Callback for transform.                                                                                    |
| type                 | `string` `number` `datetime` `date` `time` `boolean`                                                                                                                 | Specifies type.                                                                                            |
| summary              | false \| { children: { default: () => unknown; }; }                                                                                                                  | Specifies summary.                                                                                         |
| minWidth             | string \| number                                                                                                                                                     | Specifies min width.                                                                                       |
| maxWidth             | string \| number                                                                                                                                                     | Specifies max width.                                                                                       |
| customFilterDropdown | boolean                                                                                                                                                              | This prop is calculated inside component                                                                   |
| renderer             | (event: { text: unknown; record: unknown; index: number; }) => unknown                                                                                               | This prop is calculated inside component                                                                   |
| editRenderer         | (event: { text: unknown; record: unknown; index: number; }) => unknown                                                                                               | This prop is calculated inside component                                                                   |
| colEdit              | EBColEdit                                                                                                                                                            | This prop is calculated inside component                                                                   |
| customCell           | (record: Record , rowIndex: number, column: unknown) => unknown                                                                                                      | This prop is calculated inside component                                                                   |
| customHeaderCell     | (column: unknown) => unknown                                                                                                                                         | This prop is calculated inside component                                                                   |

### EBTableMenuItemConfig

| name                   | type                                                                                                                                     | description                                                    |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| key                    | string                                                                                                                                   | No official description found; inferred from type declaration. |
| text                   | string                                                                                                                                   | No official description found; inferred from type declaration. |
| title                  | string                                                                                                                                   | No official description found; inferred from type declaration. |
| icon                   | string                                                                                                                                   | No official description found; inferred from type declaration. |
| hide                   | boolean                                                                                                                                  | No official description found; inferred from type declaration. |
| disabled               | boolean                                                                                                                                  | No official description found; inferred from type declaration. |
| group                  | string                                                                                                                                   | No official description found; inferred from type declaration. |
| labelForGroup          | string                                                                                                                                   | No official description found; inferred from type declaration. |
| authorize              | AuthorizeAction                                                                                                                          | No official description found; inferred from type declaration. |
| subItems               | EBContextMenuItemConfig [] \| (($event: { record: unknown; records: unknown; all: unknown; }) => EBContextMenuItemConfig [] \| Promise ) | No official description found; inferred from type declaration. |
| ebTableMenuItemHide    | EBContextDataChecker                                                                                                                     | Specifies eb table menu item hide.                             |
| ebTableMenuItemDisable | EBContextDataChecker                                                                                                                     | Specifies eb table menu item disable.                          |

### EBTableFilterConfig

| name                      | type                                                 | description                                                     |
| ------------------------- | ---------------------------------------------------- | --------------------------------------------------------------- |
| options                   | Record<string \| number, string>                     | Config for filter type options.                                 |
| type                      | 'select' or 'radio'                                  | Specifies type.                                                 |
| selectSearch              | EBTableFilterTypeSelectSearchConfig                  | Config for filter type select-search.                           |
| dataIndex                 | string                                               | Use this prop to forward filter values to other field in query. |
| distinct                  | boolean                                              | Whether distinct.                                               |
| distinctAdditionalFilters | QueryFilter[]                                        | Specifies distinct additional filters.                          |
| dataIndexType             | `string` `number` `datetime` `date` `time` `boolean` | Use this prop to parse selected filter values to correct type.  |
| supportOps                | QueryFilterOperator[]                                | Specifies support ops.                                          |

### EBTableFilterTypeSelectSearchConfig

| name                  | type                                    | description                                          |
| --------------------- | --------------------------------------- | ---------------------------------------------------- |
| query                 | QueryConfig                             | Specifies query.                                     |
| label                 | string \| (($entry: unknown) => string) | Specifies label.                                     |
| value                 | string                                  | Specifies value.                                     |
| searchOnField         | string                                  | Specifies search on field.                           |
| loadAll               | boolean                                 | This prop should not be used with searchMode: param. |
| searchMode            | 'filter' \| 'param'                     | Specifies search mode.                               |
| filterOperator        | QueryFilterOperator                     | Specifies filter operator.                           |
| numberOfSearchResults | number                                  | Specifies number of search results.                  |
| minSearchTermLength   | number                                  | Specifies min search term length.                    |
| showRefresh           | boolean                                 | Whether show refresh.                                |

### EBTableRecord

| name          | type            | description                 |
| ------------- | --------------- | --------------------------- |
| indexInParent | number          | Specifies index in parent.  |
| children      | EBTableRecord[] | Nested child configuration. |

## YAML Examples

### Story Example 1

```yaml
- name: eb-table
  props:
    query:
      name: eb-table-default
    rowKey: id
    eventToRefresh: refresh-eb-table-default
    cache:
      key: eb-table-default
    cols:
      - key: id
        dataIndex: id
        title: Id
        sorter: true
        type: number
      - key: fullName
        dataIndex: fullName
        title: fullName
        sorter: true
      - key: email
        dataIndex: email
        title: Email
        sorter: false
      - key: gender
        dataIndex: gender
        title: Gender
        sorter: true
      - key: ipAddress
        dataIndex: ipAddress
        title: IP Address
        sorter: false
      - key: age
        dataIndex: age
        title: Age
        sorter: true
        type: number
      - key: createdAt
        dataIndex: createdAt
        title: Created at
        sorter: true
        type: date
      - key: online
        dataIndex: online
        title: Online
        sorter: false
        type: boolean
        maxWidth: 300
        ellipsis: true
    pagination:
      size: 20
      options: [10, 20, 50]
    sort:
      by: name
      direction: ASC
    preferHeight: 300px
    title: EB-Table Title
    enableExcelExport: false
    useMaxContentWidth: true
    enableClearAllFilters: true
```

### Story Example 2

```yaml
- name: eb-table
  props:
    query:
      name: eb-table-default
    rowKey: id
    eventToRefresh: refresh-eb-table-default
    cache:
      key: eb-table-default
    cols:
      - key: id
        dataIndex: id
        title: Id
        sorter: true
        type: number
        resizable: true
      - key: fullName
        dataIndex: fullName
        title: fullName
        sorter: true
        resizable: true
      - key: email
        dataIndex: email
        title: Email
        sorter: false
        resizable: true
      - key: gender
        dataIndex: gender
        title: Gender
        sorter: true
        resizable: true
      - key: ipAddress
        dataIndex: ipAddress
        title: IP Address
        sorter: false
        resizable: true
      - key: age
        dataIndex: age
        title: Age
        sorter: true
        type: number
        resizable: true
      - key: createdAt
        dataIndex: createdAt
        title: Created at
        sorter: true
        type: date
        resizable: true
      - key: online
        dataIndex: online
        title: Online
        sorter: false
        type: boolean
        ellipsis: true
        resizable: true
    pagination:
      size: 20
      options: [10, 20, 50]
    sort:
      by: name
      direction: ASC
    preferHeight: 300px
    title: EB-Table Title
    enableExcelExport: false
    useMaxContentWidth: true
    enableClearAllFilters: true
```

### Story Example 3

```yaml
- name: eb-table
  props:
    query:
      name: eb-table-default
    rowKey: id
    eventToRefresh: refresh-eb-table-default
    cache:
      key: eb-table-default
    cols:
      - key: id
        dataIndex: id
        title: Id
        sorter: true
        type: number
        resizable: true
      - key: fullName
        dataIndex: fullName
        title: fullName
        sorter: true
        resizable: true
      - key: email
        dataIndex: email
        title: Email
        sorter: false
        resizable: true
      - key: gender
        dataIndex: gender
        title: Gender
        sorter: true
        resizable: true
      - key: ipAddress
        dataIndex: ipAddress
        title: IP Address
        sorter: false
        resizable: true
      - key: age
        dataIndex: age
        title: Age
        sorter: true
        type: number
        resizable: true
      - key: createdAt
        dataIndex: createdAt
        title: Created at
        sorter: true
        type: date
        resizable: true
      - key: online
        dataIndex: online
        title: Online
        sorter: false
        type: boolean
        ellipsis: true
        resizable: true
    pagination:
      size: 20
      options: [10, 20, 50]
    sort:
      by: name
      direction: ASC
    preferHeight: 300px
    hideColumnIcons: true
    title: EB-Table Title
    enableExcelExport: false
    useMaxContentWidth: true
    enableClearAllFilters: true
```

### Story Example 4

```yaml
- name: eb-table
  props:
    query:
      name: eb-table-default
    rowKey: id
    eventToRefresh: refresh-eb-table-default
    cache:
      key: eb-table-default
    cols:
      - key: id
        dataIndex: id
        title: Id
        sorter: true
        type: number
      - key: fullName
        dataIndex: fullName
        title: fullName
        sorter: true
      - key: email
        dataIndex: email
        title: Email
        sorter: false
      - key: gender
        dataIndex: gender
        title: Gender
        sorter: true
      - key: ipAddress
        dataIndex: ipAddress
        title: IP Address
        sorter: false
      - key: age
        dataIndex: age
        title: Age
        sorter: true
        type: number
      - key: createdAt
        dataIndex: createdAt
        title: Created at
        sorter: true
        type: date
      - key: online
        dataIndex: online
        title: Online
        sorter: false
        type: boolean
        maxWidth: 300
        ellipsis: true
    pagination:
      size: 20
      options: [10, 20, 50]
    sort:
      by: name
      direction: ASC
    title: EB-Table Title
    enableExcelExport: false
    useMaxContentWidth: true
    enableClearAllFilters: true
```

### Story Example 5

```yaml
- name: eb-table
  props:
    query:
      name: eb-table-default
    rowKey: id
    eventToRefresh: refresh-eb-table-default
    cache:
      key: eb-table-default
    cols:
      - key: id
        dataIndex: id
        title: Id
        sorter: true
        type: number
      - key: fullName
        dataIndex: fullName
        title: fullName
        sorter: true
      - key: email
        dataIndex: email
        title: Email
        sorter: false
      - key: gender
        dataIndex: gender
        title: Gender
        sorter: true
      - key: ipAddress
        dataIndex: ipAddress
        title: IP Address
        sorter: false
      - key: age
        dataIndex: age
        title: Age
        sorter: true
        type: number
      - key: createdAt
        dataIndex: createdAt
        title: Created at
        sorter: true
        type: date
      - key: online
        dataIndex: online
        title: Online
        sorter: false
        type: boolean
        maxWidth: 300
        ellipsis: true
    pagination:
      size: 20
      options: [10, 20, 50]
    sort:
      by: name
      direction: ASC
    preferHeight: 300px
    title: EB-Table Title
    enableExcelExport: false
```

### Story Example 6

```yaml
- name: eb-table
  props:
    query:
      name: eb-table-default
    rowKey: id
    eventToRefresh: refresh-eb-table-default
    cache:
      key: eb-table-default
    cols:
      - key: id
        dataIndex: id
        title: Id
        sorter: true
        type: number
      - key: fullName
        dataIndex: fullName
        title: fullName
        sorter: true
      - key: email
        dataIndex: email
        title: Email
        sorter: false
      - key: gender
        dataIndex: gender
        title: Gender
        sorter: true
      - key: ipAddress
        dataIndex: ipAddress
        title: IP Address
        sorter: false
      - key: age
        dataIndex: age
        title: Age
        sorter: true
        type: number
      - key: createdAt
        dataIndex: createdAt
        title: Created at
        sorter: true
        type: date
      - key: online
        dataIndex: online
        title: Online
        sorter: false
        type: boolean
        maxWidth: 300
        ellipsis: true
    pagination:
      size: 20
      options: [10, 20, 50]
    sort:
      by: name
      direction: ASC
    preferHeight: 300px
    title: EB-Table Title
    enableExcelExport: false
    useMaxContentWidth: true
    enableClearAllFilters: true
    selectRowOnContextMenu: ${{ $rootVars.selectRowOnContextMenu }}
    contextMenuItems:
      - key: edit
        text: Edit
        icon: fas,pen
        ebTableMenuItemHide:
          - ${{ $event.record.id % 2 }}
        on:
          click:
            emit:
              - name: eb-toast
                params:
                  type: success
                  message: ${{ `Edit user '${$event.record.firstName} ${$event.record.lastName}'` }}
      - key: delete
        text: Delete
        icon: far,trash-alt
```

### Story Example 7

```yaml
- name: eb-table
  props:
    query:
      name: eb-table-default
    rowKey: id
    eventToRefresh: refresh-eb-table-default
    cache:
      key: eb-table-default
    cols:
      - key: id
        dataIndex: id
        title: Id
        sorter: true
        type: number
      - key: fullName
        dataIndex: fullName
        title: fullName
        sorter: true
      - key: email
        dataIndex: email
        title: Email
        sorter: false
      - key: gender
        dataIndex: gender
        title: Gender
        sorter: true
      - key: ipAddress
        dataIndex: ipAddress
        title: IP Address
        sorter: false
      - key: age
        dataIndex: age
        title: Age
        sorter: true
        type: number
      - key: createdAt
        dataIndex: createdAt
        title: Created at
        sorter: true
        type: date
      - key: online
        dataIndex: online
        title: Online
        sorter: false
        type: boolean
        maxWidth: 300
        ellipsis: true
    pagination:
      size: 20
      options: [10, 20, 50]
    sort:
      by: name
      direction: ASC
    preferHeight: 300px
    title: EB-Table Title
    enableExcelExport: false
    useMaxContentWidth: true
    enableClearAllFilters: true
    multipleSelection: true
    selectRowOnContextMenu: ${{ $rootVars.selectRowOnContextMenu }}
    contextMenuItems:
      - key: edit
        text: Edit (single)
        icon: fas,pen
        ebTableMenuItemHide:
          - ${{ $event.records.length > 1 }}
        on:
          click:
            emit:
              - name: eb-toast
                params:
                  type: success
                  message: ${{ `Edit user single '${$event.record.firstName} ${$event.record.lastName}'` }}
      - key: delete
        text: Delete (single)
        icon: far,trash-alt
      - key: edit2
        text: Edit (multiple)
        icon: fas,pen
        ebTableMenuItemHide:
          - ${{ $event.records.length <= 1 }}
        on:
          click:
            emit:
              - name: eb-toast
                params:
                  type: success
                  message: `Edit multiple users '${$event.records.map((x) => x.firstName + x.lastName)}'`
      - key: delete2
        text: Delete (multiple)
        icon: far,trash-alt
```

### Story Example 8

```yaml
- name: eb-table
  props:
    query:
      name: eb-table-default
    rowKey: id
    eventToRefresh: refresh-eb-table-default
    cache:
      key: eb-table-default
    cols:
      - key: id
        dataIndex: id
        title: Id
        sorter: true
        type: number
      - key: fullName
        dataIndex: fullName
        title: fullName
        sorter: true
      - key: email
        dataIndex: email
        title: Email
        sorter: false
      - key: gender
        dataIndex: gender
        title: Gender
        sorter: true
      - key: ipAddress
        dataIndex: ipAddress
        title: IP Address
        sorter: false
      - key: age
        dataIndex: age
        title: Age
        sorter: true
        type: number
      - key: createdAt
        dataIndex: createdAt
        title: Created at
        sorter: true
        type: date
      - key: online
        dataIndex: online
        title: Online
        sorter: false
        type: boolean
        maxWidth: 300
        ellipsis: true
    pagination:
      size: 20
      options: [10, 20, 50]
    sort:
      by: name
      direction: ASC
    on:
      change:
        emit:
          name: eb-toast
          params:
            type: info
            message: "${{ $event.record ? `Selected user '${$event.record?.firstName} ${$event.record?.lastName}'` : `Unselected all` }}"
    preferHeight: 300px
    title: EB-Table Title
    enableExcelExport: false
    useMaxContentWidth: true
    enableClearAllFilters: true
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
