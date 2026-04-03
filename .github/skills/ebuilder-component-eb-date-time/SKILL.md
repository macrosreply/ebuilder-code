---
name: ebuilder-component-eb-date-time
description: 'Deep component skill for eb-date-time. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-date-time in eBuilder component YAML.'
---

# eb-date-time Component Skill

## General Docs

Use `eb-datetime` to render date or datetime inputs Simple usages of date and datetime input with `eb-datetime` There are some options for you to custom UI of `eb-datetime` input To prevent dates from selection, you can use `disableDates` array prop to define a list of data ranges which you want to disable. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/form/EBDateTime

## Main Props

| name         | type                  | description              |
| ------------ | --------------------- | ------------------------ |
| autoFocus    | boolean               | Whether auto focus.      |
| customFormat | string                | Specifies custom format. |
| disable      | boolean               | Whether disable.         |
| disableDates | EBDateTimeDateRange[] | Specifies disable dates. |
| showTime     | boolean               | Whether show time.       |
| showToday    | boolean               | Whether show today.      |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: date
          ebFormControl:
            name: eb-datetime
            label: Date
        - name: date_time
          ebFormControl:
            name: eb-datetime
            label: DateTime
            props:
              showTime: true
```

### Story Example 2

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: date
          ebFormControl:
            name: eb-datetime
            label: Auto Focus Date
            props:
              autoFocus: true
        - name: hidden_today
          ebFormControl:
            name: eb-datetime
            label: Hidden Button ToDay
            props:
              showToday: false
        - name: disabled_date
          ebFormControl:
            name: eb-datetime
            label: Disabled DateTime
            props:
              disable: true
```

### Story Example 3

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: date1
          defaultValue: 2023-12-20T00:00:00
          ebFormControl:
            name: eb-datetime
            label: Disable dates between startDate and endDate
            props:
              disableDates:
                - startDate: 2023-12-13
                  endDate: 2023-12-17
        - name: date2
          defaultValue: 2023-12-20T00:00:00
          ebFormControl:
            name: eb-datetime
            label: Disable dates after startDate
            props:
              disableDates:
                - startDate: 2023-12-24
        - name: date3
          defaultValue: 2023-12-20T00:00:00
          ebFormControl:
            name: eb-datetime
            label: Disable dates before endDate
            props:
              disableDates:
                - endDate: 2023-12-06
        - name: date4
          defaultValue: 2023-12-20T00:00:00
          ebFormControl:
            name: eb-datetime
            label: Combine all
            props:
              disableDates:
                - startDate: 2023-12-13
                  endDate: 2023-12-17
                - startDate: 2023-12-24
                - endDate: 2023-12-06
        - name: date5
          defaultValue: ${{ new Date(Date.now() + 86400 * 1000) }}
          ebFormControl:
            name: eb-datetime
            label: With Date instances
            props:
              disableDates:
                - startDate: ${{ new Date(Date.now() - 86400 * 2 * 1000) }}
                  endDate: ${{ new Date(Date.now() + 86400 * 2 * 1000) }}
                - startDate: ${{ new Date(Date.now() + 86400 * 4 * 1000) }}
                - endDate: ${{ new Date(Date.now() - 86400 * 4 * 1000) }}
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
