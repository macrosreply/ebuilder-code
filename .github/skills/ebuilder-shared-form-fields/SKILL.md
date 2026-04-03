---
name: ebuilder-shared-form-fields
description: 'Generation playbook for reusable form definitions in eBuilder components. USE FOR: declaring root formFields producers, consuming with ebSharedFormFields inside eb-form props.fields, parameterizing via rootProps, and keeping reusable field sets maintainable. DO NOT USE FOR: SQL/task/resource generation.'
---

# eBuilder Shared Form Fields Playbook

## Purpose

Use this skill when building reusable form definitions with `formFields` and `ebSharedFormFields`.

Primary references:

- `ebuilder-docs/UI.md` section `formFields`
- existing repository form conventions in `components/**/*.yml`

## Producer/Consumer Model

`formFields` is a producer pattern at root component level, and `ebSharedFormFields` is the consumer hook from `eb-form.props.fields`.

### Producer (`formFields`)

| Property        | Type              | Required | Notes                                          |
| --------------- | ----------------- | -------- | ---------------------------------------------- |
| `name`          | string/expression | Yes      | Must be unique within the produced field list. |
| `on`            | object            | No       | Field-level events (for example `change`).     |
| `ebFormControl` | object            | Yes      | Actual form control definition.                |

`ebFormControl` commonly includes:

| Property | Type              | Required | Notes                                                                 |
| -------- | ----------------- | -------- | --------------------------------------------------------------------- |
| `name`   | string            | Yes      | Built-in control, for example `eb-input`, `eb-select`, `eb-textarea`. |
| `label`  | locale key/string | No       | Prefer locale keys.                                                   |
| `props`  | object            | No       | Control-specific props.                                               |
| `rules`  | array             | No       | Validation rules.                                                     |

### Consumer (`ebSharedFormFields`)

```yaml
children:
  - name: eb-form
    props:
      fields:
        - ebSharedFormFields:
            name: comment-fields
            props:
              mainFieldName: comment
              required: false
```

| Property | Type   | Required | Notes                                                    |
| -------- | ------ | -------- | -------------------------------------------------------- |
| `name`   | string | Yes      | Must match included producer component alias.            |
| `props`  | object | No       | Passed to producer as `$rootProps` for field generation. |

## Data Context Rules

In generated `formFields` function:

- `$rootProps` is available (from `ebSharedFormFields.props`).
- `$rootStore` from same producer component is available.
- `$rootVars` is not available.

Guardrails:

1. Do not reference `$rootVars` in `formFields` expressions.
2. If shared state is needed, move it to `rootStore`.
3. Keep field names deterministic even when dynamic.

## Reuse Patterns

1. Extract repeated field definitions to one producer component in `components/components/...`.
2. Keep producer focused on field schema and lightweight field event handlers.
3. Pass only required parameters via `ebSharedFormFields.props`.
4. Keep consumer forms thin and compose multiple shared field sets if needed.

## Validation And Safety Rules

- Use locale keys for labels/placeholders/messages.
- Keep validation in `rules`; keep side effects in explicit event handlers.
- Avoid mutating unrelated fields unless it is intentional and documented.
- Prefer stable names for producer include aliases across pages/modals.

## Example Blueprint

```yaml
# producer: components/components/common/form/comment-fields.yml
rootStore:
  currentUser:
    storeKey: current-user
    initialState:
      userView: null

formFields:
  - name: ${{ `favorite-${$rootProps.mainFieldName}` }}
    on:
      change:
        emit:
          - handler: |
              ${{
                if (!$event.value) {
                  return
                }
                const current = $event.getFieldValue($rootProps.mainFieldName) || ''
                $event.setFieldValue($rootProps.mainFieldName, current ? `${current}\n${$event.value.text}` : $event.value.text)
              }}
    ebFormControl:
      name: eb-select
      props:
        rawObject: true

  - name: ${{ $rootProps.mainFieldName }}
    ebFormControl:
      name: eb-textarea
      rules:
        - required: ${{ $rootProps.required || false }}
```

## Deterministic Checklist

1. Producer component has root-level `formFields` only.
2. Consumer uses `ebSharedFormFields` under `eb-form.props.fields`.
3. `$rootProps` parameters are clearly defined and minimal.
4. No `$rootVars` usage inside `formFields`.
5. Labels/messages use locale keys.
6. Field events remain focused and predictable.
