---
name: ebuilder-component-html-standard
description: 'Playbook for generating schema-safe standard HTML nodes inside eBuilder component YAML. USE FOR: html tags (div/span/section/label/etc.), slot placement, props.style object, native DOM event handlers (onClick/onInput/onChange) with inline JS, and eb-* mixed with html nodes. DO NOT USE FOR: SQL/resource/task authoring in configs/*.yml.'
---

# eBuilder Standard HTML in Component YAML Playbook

## Purpose

Use this skill when generating standard HTML nodes inside `components/**/*.yml`.

Examples:

- `name: div`
- `name: span`
- `name: section`
- `name: label`

This skill defines how to write html tags safely in eBuilder YAML and how to wire native html events.

## Scope

- Target files: `components/**/*.yml`
- Works together with built-in `eb-*` components
- Excludes: `configs/*.yml`, `externals/*`, `dll/*`

## Node Shape for Standard HTML

```yaml
- name: div
  slot: extra
  props:
    style:
      display: flex
      align-items: center
      gap: var(--padding-xs)
    onClick: |
      ${{
        ($event) => {
          $event.stopPropagation()
        }
      }}
  children:
    - name: span
      children: some_locale_key
```

## Rules

1. Use standard html tag name in `name` (`div`, `span`, `p`, `section`, `header`, `footer`, `label`, `small`, etc.).
2. Use `slot` only when targeting a supported slot on the parent component.
3. Put visual styles under `props.style` as an object.
4. For native html events, use camelCase prop names such as `onClick`, `onInput`, `onChange`, `onFocus`, `onBlur`.
5. Use block scalar (`|`) with `${{ ... }}` for event handler functions.
6. Keep handler logic small; move complex logic to `externals/`.
7. Prefer locale keys over hardcoded user-facing text in `children`.

## Event Handler Pattern

```yaml
props:
  onClick: |
    ${{
      ($event) => {
        $event.stopPropagation()
        // optional: emit custom event
        $emit('evt-my-click', { source: 'html-node' })
      }
    }}
```

Notes:

- Use `$event` for native event object access.
- Keep side effects explicit and predictable.
- Avoid long imperative blocks in YAML.

## Common Patterns

### Wrapper Container

```yaml
- name: div
  props:
    style:
      display: flex
      flex-direction: column
      gap: var(--padding-sm)
  children:
    - name: eb-typography
      props:
        text: section_title
```

### Prevent Parent Click/Toggle

```yaml
- name: div
  props:
    onClick: |
      ${{
        ($event) => {
          $event.stopPropagation()
        }
      }}
```

### Inline Text Composition

```yaml
- name: div
  children:
    - name: span
      children: attribute_label
    - name: span
      props:
        style:
          margin-left: var(--padding-xs)
      children: attribute_value
```

## Guardrails

- Do not put SQL/task/resource logic in html node handlers.
- Do not use unsupported root keys just because html nodes are present.
- Do not replace existing `eb-*` component capabilities with raw html unless needed.
- Do not hardcode translatable labels/messages.

## Deterministic Checklist

1. File is under `components/`.
2. `name` uses a valid html tag or `eb-*` component name.
3. `props.style` is object-based and readable.
4. Native event handler uses `${{ ... }}` and small, safe logic.
5. Slot usage matches parent component contract.
6. User-facing text uses locale keys.
