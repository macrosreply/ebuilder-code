---
name: ebuilder-component-yaml-structure
description: 'Generation playbook for components/**/*.yml in eBuilder. USE FOR: root component shape, includes/children/slot patterns, rootStore/rootVars/rootMemo usage, authorizations, singleton/style, app.yml routing/languages, and schema-safe component YAML output. DO NOT USE FOR: SQL/task/resource authoring in configs/*.yml.'
---

# eBuilder Component YAML Structure Playbook

## Purpose

Use this skill when creating or reviewing UI YAML in `components/` so output is:

- aligned with `ebuilder-docs/UI.md`
- valid against component schema expectations (`ebuilder-docs/schemas`)
- consistent with repository patterns in `components/pages` and `components/components`

## Scope

- File targets: `components/**/*.yml`, including `components/app.yml` and `components/app-office.yml`
- Excludes: `configs/*.yml`, `externals/*`, `dll/*`

## Root-Level Properties

Only use root keys that are supported by eBuilder component transformation.

| Property     | Type                | Required | Notes                                                 |
| ------------ | ------------------- | -------- | ----------------------------------------------------- |
| `authorize`  | object/array        | No       | Component-level access policy.                        |
| `singleton`  | boolean             | No       | Render only one instance (useful for modal includes). |
| `includes`   | object              | No       | Register reusable YAML/Vue components.                |
| `style`      | string (multi-line) | No       | LESS/CSS style block for this component.              |
| `rootStore`  | object              | No       | Pinia-backed reactive state bound by `storeKey`.      |
| `rootVars`   | object              | No       | Local reactive state values.                          |
| `rootMemo`   | object              | No       | Computed values derived from root state/props.        |
| `formFields` | array               | No       | Reusable form field definitions.                      |
| `on`         | object              | No       | Root events: `emit`, `load`, `unload`, `watch`.       |
| `children`   | array/string        | Usually  | Render tree, text node, or slot projection.           |

## Node-Level (`children[]`) Properties

| Property   | Type         | Required | Notes                                                  |
| ---------- | ------------ | -------- | ------------------------------------------------------ |
| `name`     | string       | Yes      | Built-in `eb-*`, html tag, or included component name. |
| `props`    | object       | No       | Props binding, supports inline JS via `${{ ... }}`.    |
| `slot`     | string       | No       | Target parent slot name (default when omitted).        |
| `children` | array/string | No       | Nested nodes, text content, or slot content.           |

## Includes Patterns

Supported include forms:

- Simple path form:

```yaml
includes:
  inbox-table: ./components/item/v-items-table.yml
```

- Object form with loading control:

```yaml
includes:
  comment-fields:
    path: /components/common/form/comment-fields.yml
    async: false
```

Rules:

- Include names should be stable and intention-revealing.
- Use existing component files first before creating new ones.
- Prefer YAML includes over externals for structural UI composition.

## Root State Rules

| State block | Reactive type | Primary use                           |
| ----------- | ------------- | ------------------------------------- |
| `rootVars`  | `reactive`    | Local per-instance mutable state.     |
| `rootStore` | `Pinia store` | Shared cross-component/global state.  |
| `rootMemo`  | `computed`    | Derived values from root state/props. |

Guardrails:

- Put transient component state in `rootVars`.
- Put shared or route-scoped state in `rootStore`.
- Keep `rootMemo` free of side effects.

## `app.yml` Special Rules

`components/app.yml` and `components/app-office.yml` are entry components.

Required concerns:

1. `languages` list must match available locale files.
2. `includes` should register route components and root layout components.
3. `router` defines application navigation map.
4. `children` should include login/bootstrap wrappers and main layout.

Do not place business SQL/task logic directly in `app.yml`; only event wiring.

## Inline JavaScript Rules

- Use `${{ ... }}` only for expressions/logic that cannot be represented declaratively.
- Keep inline handlers short; move complex logic to `externals/`.
- Access context safely: `$rootProps`, `$rootVars`, `$rootStore`, `$rootMemo`, `$route`, `$event`, `$ctx`.

## Common Mistakes To Avoid

- Using unsupported root keys.
- Hardcoding user-facing strings instead of locale keys.
- Putting heavy business logic in component YAML.
- Referencing undefined includes or event names.
- Overusing inline JS when declarative props are sufficient.

## Deterministic Checklist

1. Confirm file belongs in `components/` and not `configs/`.
2. Validate root keys and node keys against supported shape.
3. Ensure includes names and paths are valid and reusable.
4. Confirm route/page structure is consistent with existing app patterns.
5. Ensure UI labels are locale-key based.
6. Validate event names and data flow readability.
