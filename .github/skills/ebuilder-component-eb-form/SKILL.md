---
name: ebuilder-component-eb-form
description: 'Deep component skill for eb-form. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-form in eBuilder component YAML.'
---

# eb-form Component Skill

## General Docs

A form consists of one or more form fields whose type includes input, textarea, checkbox, radio, select, date, editor, and more. Simple `eb-form` usage for normal login form with required `user` and `password` fields together with a hidden field `policy`, they are defined in prop `fields`. As you can see, we are using `model.custom` to prefill value of `policy` field. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBForm

## Main Props

| name             | type                                   | description                   |
| ---------------- | -------------------------------------- | ----------------------------- |
| cache            | { key: string; }                       | Specifies cache.              |
| colon            | boolean                                | Whether colon.                |
| cols             | Record                                 | Specifies cols.               |
| eventToReset     | string                                 | Specifies event to reset.     |
| eventToSetValue  | string                                 | Specifies event to set value. |
| eventToSubmit    | string                                 | Specifies event to submit.    |
| fields           | FieldItem[]                            | Specifies fields.             |
| labelCol         | ColSpan                                | Specifies label col.          |
| layout           | "horizontal" \| "vertical" \| "inline" | Specifies layout.             |
| model            | { custom?: Record ; }                  | Specifies model.              |
| on.submit        | eBuilder events emit                   | Callback for submit.          |
| showRequiredMark | boolean                                | Whether show required mark.   |
| style            | CSSProperties                          | Inline style overrides.       |
| wrapperCol       | ColSpan                                | Specifies wrapper col.        |

## Nested Props

### FieldItem

| name          | type                                                     | description                                                                                              |
| ------------- | -------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| name          | string                                                   | Specifies name.                                                                                          |
| style         | CSSProperties                                            | Inline style overrides.                                                                                  |
| defaultValue  | unknown                                                  | Specifies default value.                                                                                 |
| depends       | string[]                                                 | Specifies depends.                                                                                       |
| key           | number                                                   | We need this for force rerendering controls while resolving dependencies fields                          |
| control       | FieldItemControl                                         | We need it for typing. We shouldn't bind this prop directly. Please use `ebFormControl` function instead |
| ebFormControl | (event: { $dependencies: Record ; }) => FieldItemControl | Callback for eb form control.                                                                            |

### FieldItemControl

| name     | type                                                                                                                                                 | description                 |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------- |
| label    | string                                                                                                                                               | Specifies label.            |
| desc     | string                                                                                                                                               | Specifies desc.             |
| name     | string                                                                                                                                               | Specifies name.             |
| props    | Record                                                                                                                                               | Specifies props.            |
| rules    | FormRule[]                                                                                                                                           | Specifies rules.            |
| spans    | ColSpans                                                                                                                                             | Specifies spans.            |
| class    | string                                                                                                                                               | CSS class overrides.        |
| children | { extra?: (props: { getFieldValue: (fieldName: string) => unknown; setFieldValue: (fieldName: string, fieldValue: unknown) => void; }) => unknown; } | Nested child configuration. |

### ColSpans

| name | type   | description    |
| ---- | ------ | -------------- |
| xs   | number | Specifies xs.  |
| sm   | number | Specifies sm.  |
| md   | number | Specifies md.  |
| lg   | number | Specifies lg.  |
| xl   | number | Specifies xl.  |
| xxl  | number | Specifies xxl. |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: div
    children:
      - name: eb-form
        props:
          model:
            custom:
              policy: eWorkplace
          fields:
            - name: policy
              ebFormControl:
                name: eb-hidden
            - name: username
              ebFormControl:
                label: Username
                name: eb-text
                rules:
                  - required: true
            - name: password
              ebFormControl:
                label: Passwords
                name: eb-password
                rules:
                  - required: true
          eventToSubmit: simple-login-form
          on:
            submit:
              emit:
                name: eb-toast
                params:
                  message: ${{ `Form submitted: policy=${$event.model.policy}, username=${$event.model.username}, password=${$event.model.password}`}}
```

### Story Example 2

```yaml
children:
  - name: div
    children:
      - name: eb-form
        props:
          labelCol:
            md:
              span: 14
              offset: 5
            sm:
              span: 24
          wrapperCol:
            md:
              span: 14
              offset: 5
            sm:
              span: 24
          model:
            custom:
              policy: eWorkplace
          fields:
            - name: policy
              ebFormControl:
                name: eb-hidden
            - name: username
              ebFormControl:
                label: Username
                name: eb-text
                rules:
                  - required: true
            - name: password
              ebFormControl:
                label: Passwords
                name: eb-password
                rules:
                  - required: true
          eventToSubmit: layout-with-label-col-and-wrapper-col
          on:
            submit:
              emit:
                name: eb-toast
                params:
                  message: ${{ `Form submitted: policy=${$event.model.policy}, username=${$event.model.username}, password=${$event.model.password}`}}
```

### Story Example 3

```yaml
children:
  - name: div
    children:
      - name: eb-form
        props:
          cols:
            sm: 1
            md: 2
          model:
            custom:
              policy: eWorkplace
          fields:
            - name: policy
              ebFormControl:
                name: eb-hidden
            - name: username
              ebFormControl:
                label: Username
                name: eb-text
                rules:
                  - required: true
            - name: password
              ebFormControl:
                label: Passwords
                name: eb-password
                rules:
                  - required: true
          eventToSubmit: simple-login-form
          on:
            submit:
              emit:
                name: eb-toast
                params:
                  message: ${{ `Form submitted: policy=${$event.model.policy}, username=${$event.model.username}, password=${$event.model.password}`}}
```

### Story Example 4

```yaml
children:
  - name: div
    children:
      - name: eb-form
        props:
          fields:
            - name: age
              style:
                background-color: darkgray
                padding: 5px
              on:
                change:
                  emit:
                    name: eb-toast
                    params:
                      message: ${{ `Input has changed: ${$event.oldValue} to ${$event.newValue}` }}
              ebFormControl:
                label: Age
                name: eb-number
          eventToSubmit: simple-form-field
          on:
            submit:
              emit:
                name: eb-toast
                params:
                  message: ${{ `Form submitted: age=${$event.model.age}`}}
```

### Story Example 5

```yaml
children:
  - name: div
    children:
      - name: eb-form
        props:
          fields:
            - name: shouldShowAge
              ebFormControl:
                label: Should show Age field
                name: eb-checkbox
                props:
                  options: [1, 0]
            - name: age
              depends:
                - shouldShowAge
              ebFormControl:
                label: Age
                name: "${{ $dependencies.shouldShowAge ? 'eb-number' : 'eb-hidden' }}"
```

### Story Example 6

```yaml
children:
  - name: div
    children:
      - name: eb-form
        props:
          model:
            custom:
              policy: eWorkplace
          fields:
            - name: policy
              ebFormControl:
                name: eb-hidden
            - name: username
              ebFormControl:
                label: Username
                name: eb-text
                rules:
                  - required: true
            - name: password
              ebFormControl:
                label: Passwords
                name: eb-password
                rules:
                  - required: true
          eventToSubmit: slots
          on:
            submit:
              emit:
                name: eb-toast
                params:
                  message: ${{ `Form submitted: policy=${$event.model.policy}, username=${$event.model.username}, password=${$event.model.password}`}}
        children:
          - name: eb-alert
            slot: header
            props:
              visible: true
              type: warning
              message: Something went wrong
          - name: eb-alert
            slot: footer
            props:
              visible: true
              type: success
              message: By submitting this form you agree to our privacy policy
              style:
                margin-top: var(--padding-md)
```

### Story Example 7

```yaml
children:
  - name: div
    children:
      - name: eb-form
        props:
          layout: inline
          model:
            custom:
              policy: eWorkplace
          fields:
            - name: policy
              ebFormControl:
                name: eb-hidden
            - name: username
              ebFormControl:
                label: Username
                name: eb-text
                rules:
                  - required: true
            - name: password
              ebFormControl:
                label: Passwords
                name: eb-password
                rules:
                  - required: true
          eventToSubmit: slots
          on:
            submit:
              emit:
                name: eb-toast
                params:
                  message: ${{ `Form submitted: policy=${$event.model.policy}, username=${$event.model.username}, password=${$event.model.password}`}}
        children:
          - name: eb-alert
            slot: header
            props:
              visible: true
              type: warning
              message: Something went wrong
              style:
                width: 100%
                margin-bottom: var(--padding-md)
          - name: eb-alert
            slot: footer
            props:
              visible: true
              type: success
              message: By submitting this form you agree to our privacy policy
              style:
                margin-top: var(--padding-md)
                width: 100%
```

### Story Example 8

```yaml
children:
  - name: div
    children:
      - name: eb-form
        props:
          layout: horizontal
          labelCol:
            span: 6
          wrapperCol:
            span: 14
          model:
            custom:
              policy: eWorkplace
          fields:
            - name: policy
              ebFormControl:
                name: eb-hidden
            - name: username
              ebFormControl:
                label: Username
                name: eb-text
                rules:
                  - required: true
            - name: password
              ebFormControl:
                label: Passwords
                name: eb-password
                rules:
                  - required: true
          eventToSubmit: slots
          on:
            submit:
              emit:
                name: eb-toast
                params:
                  message: ${{ `Form submitted: policy=${$event.model.policy}, username=${$event.model.username}, password=${$event.model.password}`}}
        children:
          - name: eb-alert
            slot: header
            props:
              visible: true
              type: warning
              message: Something went wrong
          - name: eb-alert
            slot: footer
            props:
              visible: true
              type: success
              message: By submitting this form you agree to our privacy policy
              style:
                margin-top: var(--padding-md)
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
