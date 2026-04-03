---
name: ebuilder-app-entry-yaml
description: 'Generation playbook for special eBuilder app entry components (`components/app*.yml`). USE FOR: defining supported languages, router tree, required `eb-config-provider` with app-level `defaultProps`, and root layout bootstrap (`eb-layout-master` + `eb-router-view`) while preserving standard component capabilities. DO NOT USE FOR: SQL/task/resource authoring in `configs/*.yml`.'
---

# eBuilder App Entry YAML Playbook

## Purpose

Use this skill when creating or editing app entry components such as:

- `components/app.yml`
- `components/app-office.yml`
- other `components/app-*.yml` entry variants

These files are special: they are first-loaded components that bootstrap app runtime, language loading, component registration, and router setup.

Sources:

- `ebuilder-docs/UI.md` section `### app.yml`

## What Makes `app*.yml` Special

Compared with regular component YAML, app entry files must define app bootstrap concerns in addition to normal component features:

1. supported languages (`languages`)
2. include registry for route/root components (`includes`)
3. route tree (`router`)
4. root defaults through required `eb-config-provider`

## Required App Entry Structure

| Root key    | Required | Notes                                                                 |
| ----------- | -------- | --------------------------------------------------------------------- |
| `languages` | Yes      | Language codes available in this app entry.                           |
| `includes`  | Yes      | Register route/root components referenced by `router` and `children`. |
| `router`    | Yes      | App navigation tree for `eb-router-view`.                             |
| `children`  | Yes      | Should contain login and root `eb-config-provider` composition.       |

Common optional keys from normal components can still be used when needed (`authorize`, `style`, `on`, `rootVars`, etc.).

## `languages` Rules

| Rule                           | Guidance                                       |
| ------------------------------ | ---------------------------------------------- |
| Declare explicit list          | Example: `[en, de]`.                           |
| Keep aligned with locale files | Values should map to generated locale bundles. |
| Avoid dead language entries    | Remove unsupported/unshipped language codes.   |

Example:

```yaml
languages: [en, de]
```

## `includes` Rules

| Rule                           | Guidance                                                       |
| ------------------------------ | -------------------------------------------------------------- |
| Include every routed component | Each `router[*].component` should resolve to an include alias. |
| Keep naming stable             | Prefer semantic aliases such as `root`, `dashboard`, `help`.   |
| Use YAML component paths       | Typically `./*.yml` in app-level entry files.                  |

Example:

```yaml
includes:
  login: ./login.yml
  root: ./root.yml
  dashboard: ./dashboard.yml
  help: ./help.yml
```

## `router` Rules

| Property    | Required    | Notes                                                     |
| ----------- | ----------- | --------------------------------------------------------- |
| `path`      | Yes         | Route path (`/`, child segment, or empty redirect entry). |
| `component` | Usually     | Include alias name for rendered component.                |
| `children`  | No          | Nested route array.                                       |
| `name`      | Recommended | Named route for navigation clarity.                       |
| `redirect`  | Optional    | Redirect target route/path.                               |

Guidance:

1. Keep one top-level root route (`path: /`) with nested children where appropriate.
2. Ensure `component` names map to `includes` aliases.
3. Prefer explicit fallback redirect for empty child path.

Example:

```yaml
router:
  - path: /
    component: root
    children:
      - name: dashboard
        path: dashboard
        component: dashboard
      - path: ''
        redirect: /dashboard
```

## Required `eb-config-provider` at Root

`eb-config-provider` must exist at root of app entry composition and should define app-level `defaultProps` for built-in components.

| Path                                   | Required    | Notes                                         |
| -------------------------------------- | ----------- | --------------------------------------------- |
| `children[*].name: eb-config-provider` | Yes         | Required wrapper in root app tree.            |
| `children[*].props.defaultProps`       | Recommended | Central place for default behavior/UX tuning. |

Expected root composition pattern:

```yaml
children:
  - name: login
  - name: eb-config-provider
    props:
      defaultProps:
        ebTable:
          pagination:
            size: 50
            options: [50, 100, 250, 500]
        ebForm:
          showRequiredMark: true
          colon: false
    children:
      - name: eb-layout-master
        children:
          - name: eb-router-view
```

## App-Level `defaultProps` Guidance

Use app entry file to keep defaults centralized and consistent.

Typical targets:

- `ebTable` (pagination, export, mobile-safe behavior)
- `ebForm` (required mark, colon)
- `ebSelect` (refresh behavior)
- `ebDescriptions`, `ebBreadcrumb`, and other shared UI primitives

## Navigation Notes

- Use `eb-router-link` for declarative links in component tree.
- Use emit events `eb-router-push` / `eb-router-replace` for programmatic navigation.
- Keep route-level authorization concerns consistent with security policy setup.

## Anti-Patterns

- Missing `languages` in app entry file.
- Defining `router` entries that reference non-included components.
- Omitting root `eb-config-provider`.
- Scattering component defaults into many pages instead of `defaultProps` in app entry.

## Deterministic Checklist

1. Confirm target file is an app entry (`components/app*.yml`).
2. Ensure `languages`, `includes`, `router`, and `children` are present and coherent.
3. Verify each router component maps to an include alias.
4. Ensure root tree contains `eb-config-provider` and `eb-router-view` under layout wrapper.
5. Keep app-wide built-in component defaults in `defaultProps`.
6. Validate navigation fallback route/redirect behavior.
7. Ensure user-facing labels remain locale-key based.
