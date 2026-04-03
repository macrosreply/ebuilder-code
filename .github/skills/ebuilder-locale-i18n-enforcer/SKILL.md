---
name: ebuilder-locale-i18n-enforcer
description: 'Localization enforcement for eBuilder locale files and UI bindings. USE FOR: add i18n keys, enforce locale key naming, synchronize language coverage, replace hardcoded component text. DO NOT USE FOR: SQL/task configuration unrelated to text localization.'
---

# eBuilder Locale i18n Enforcer

## Purpose

Use this skill to keep localization complete and consistent whenever UI text changes.

## Enforcement Rules

- New UI labels/messages must map to locale keys.
- Add equivalent keys for supported languages.
- Keep naming module-based and intention-revealing.
- Preserve placeholder semantics across language variants.

## Deterministic Checklist

1. Identify all UI text introduced or changed.
2. Add or update locale keys in module-appropriate files.
3. Mirror keys across supported languages.
4. Replace hardcoded strings with locale references in components.
5. Check for naming drift and duplicate-key risk.
