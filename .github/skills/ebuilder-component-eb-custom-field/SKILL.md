---
name: ebuilder-component-eb-custom-field
description: 'Deep component skill for eb-custom-field. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-custom-field in eBuilder component YAML.'
---

# eb-custom-field Component Skill

## General Docs

Use `eb-custom-field` to create custom form fields with custom rendering logic. This component allows you to define completely custom field controls by providing a render function that returns custom components. The `EBCustomField` component is a powerful wrapper that enables you to create custom form field controls with full access to form state and field manipulation functions. It's particularly useful when you need to create specialized input controls that don't fit the standard form field patterns. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/form/EBCustomField

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-form
    props:
      model:
        custom:
          simpleCustom: 'Hello World'
      fields:
        - name: simpleCustom
          ebFormControl:
            label: Simple Custom Field
            name: eb-custom-field
            children:
              - name: div
                props:
                  style:
                    display: flex
                    alignItems: center
                    gap: 8px
                children:
                  - name: input
                    props:
                      type: text
                      class: ant-input
                      value: ${{ $ctx.$parent.value }}
                      onInput: ${{ (e) => $ctx.$parent.setValue(e.target.value) }}
                  - name: button
                    props:
                      type: button
                      class: ant-btn ant-btn-primary
                      onClick: ${{ () => $ctx.$parent.setValue('') }}
                    children: Clear
```

### Story Example 2

```yaml
children:
            - name: eb-form
              props:
                model:
                  custom:
                    advancedCustom:
                      color: "#1890ff"
                      text: "Custom Text"
                fields:
                  - name: advancedCustom
                    ebFormControl:
                      label: Advanced Custom Field
                      name: eb-custom-field
                      children:
                        - name: div
                          children:
                            - name: input
                              props:
                                type: text
                                value: ${{ $ctx.$parent.value.text }}
                                onInput: ${{ (e) => $ctx.$parent.setValue({ ...$ctx.$parent.value, text: e.target.value }) }}
                            - name: input
                              props:
                                type: color
                                value: ${{ $ctx.$parent.value.color }}
                                onChange: ${{ (e) => $ctx.$parent.setValue({ ...$ctx.$parent.value, color: e.target.value }) }}
                            - name: div
                              props:
                                style:
                                  border: 2px solid ${{ $ctx.$parent.value.color }}
                                  color: ${{ $ctx.$parent.value.color }}
                                  fontWeight: bold
                              children: ${{ $ctx.$parent.$parent.value.text }}
```

### Story Example 3

```yaml
children:
  - name: eb-form
    props:
      model:
        custom:
          name: 'John Doe'
          email: 'john@example.com'
          summary: ''
      fields:
        - name: name
          ebFormControl:
            label: Name
            name: eb-text
        - name: email
          ebFormControl:
            label: Email
            name: eb-text
        - name: summary
          ebFormControl:
            label: Dynamic Summary Generator
            name: eb-custom-field
            children:
              - name: div
                children:
                  - name: button
                    props:
                      type: button
                      onClick: ${{ () => $ctx.$parent.setValue(`Hello, my name is ${$ctx.$parent.getFieldValue('name')} and you can reach me at ${$ctx.$parent.getFieldValue('email')}.`) }}
                    children: Generate Summary
                  - name: textarea
                    props:
                      value: ${{ $ctx.$parent.value }}
                      onInput: ${{ (e) => $ctx.$parent.setValue(e.target.value) }}
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
