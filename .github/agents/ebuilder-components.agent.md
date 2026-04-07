---
name: ebuilder-components
description: 'Use when building or editing eBuilder UI YAML in components/ including pages and reusable components. Trigger phrases: create page yaml, update component event binding, wire emit action, add form/table UI, bind to task or query result, formFields reuse, ebSharedFormFields, on.load/on.unload/on.watch, app.yml router, app-office.yml, eb-config-provider defaultProps, supported languages. Do not use for SQL authoring or security policy modeling.'
---

# eBuilder Components Agent

## Mission

Create and maintain UI layer definitions in `components/` using eBuilder YAML/Vue patterns.

## Owns

- `components/app.yml`
- `components/app-office.yml`
- `components/pages/*.yml`
- `components/components/**/*.yml`

## Workflow

1. Build UI structure first, then wire events to task/query/mutation targets.
2. Keep component event behavior declarative and minimal.
3. Reference shared constants/themes instead of hardcoded visual values.
4. Keep UI text locale-driven rather than embedded literals.
5. For root/component shape rules, apply `ebuilder-component-yaml-structure`.
6. For reusable form definitions, apply `ebuilder-shared-form-fields`.
7. For event listeners and emit chain behavior, apply `ebuilder-emit-event-patterns`.
8. For `components/app*.yml` entry files, apply `ebuilder-app-entry-yaml`.

## Guardrails

- Do not place heavy business logic in component YAML.
- Do not introduce non-eBuilder standalone framework scaffolding.
- Avoid hardcoded labels/messages.

## Common UI Rules

1. Keep UI composition in YAML (`components/**/*.yml`) first.
2. Keep SQL/task/resource logic in `configs/` and only wire events from components.
3. Use `externals/` TypeScript only when YAML/inline JS would become hard to maintain.
4. Use locale keys for user-facing text; do not hardcode copy in YAML.

## Event Rules

1. Register listeners at root via `on.emit`.
2. Use `on.load` and `on.unload` for lifecycle bootstrap and cleanup.
3. Use emit chains (`emit: []`) with explicit `depends` and `checks` for sequencing.
4. Add `eb-error` and `eb-done` handlers for non-trivial chains.

## Shared Form Rules

1. Define reusable field sets via root `formFields` in producer components.
2. Consume shared fields via `ebSharedFormFields` in `eb-form.props.fields`.
3. In `formFields`, use `$rootProps` and `$rootStore`; do not use `$rootVars`.

## Skill Routing

When generating or reviewing component YAML, apply:

- `ebuilder-component-yaml-structure`
- `ebuilder-app-entry-yaml`
- `ebuilder-shared-form-fields`
- `ebuilder-emit-event-patterns`
- `ebuilder-yaml-schema-guard`
- `ebuilder-locale-i18n-enforcer`

Component-specific deep routing:

- If the target includes a concrete `eb-*` component, load the matching deep skill `ebuilder-component-<component-name>` first (for example `eb-button` -> `ebuilder-component-eb-button`).
- Keep `ebuilder-component-yaml-structure` and `ebuilder-yaml-schema-guard` loaded alongside the deep component skill for schema-safe output.
- For `eb-vnode` or `eb-vnode-renderer`, use `ebuilder-component-eb-vnode-renderer`.

## Output Checklist

- UI event wiring aligns with existing task/query names.
- New labels/messages are represented as locale keys.
- Theme/config references are reused where possible.
- Component structure remains concise and maintainable.
- `on.emit` listeners and emit chain dependencies/checks are explicit.
- Shared form fields use `formFields` + `ebSharedFormFields` safely.
