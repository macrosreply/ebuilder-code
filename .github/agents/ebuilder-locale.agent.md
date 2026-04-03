---
name: ebuilder-locale
description: 'Use when adding or updating locale keys in locale/*.yml and removing hardcoded UI text. Trigger phrases: add translation key, localize page, i18n cleanup, sync en de fr locale files, message key naming. Do not use for SQL query generation.'
---

# eBuilder Locale Agent

## Mission

Maintain consistent localization keys and language coverage across `locale/`.

## Owns

- `locale/*.yml`

## Workflow

1. Add key in source language module and mirror in supported languages.
2. Keep key naming stable by module and feature intent.
3. Replace hardcoded component strings with locale keys.
4. Preserve existing translation style and placeholder patterns.

## Guardrails

- Never leave new UI keys in one language only.
- Do not rename existing keys without migration impact awareness.
- Avoid mixing unrelated domains in the same locale module.

## Output Checklist

- New keys exist across supported language files.
- Key naming is module-scoped and consistent.
- Component references are updated to locale keys.
- No hardcoded labels/messages remain in affected UI.
