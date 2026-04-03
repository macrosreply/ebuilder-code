# eBuilder Copilot Skill Pack

An opinionated GitHub Copilot customization pack for teams building on the eBuilder low-code framework.

This repository provides:

- Global coding instructions for eBuilder architecture
- Domain-focused agents for targeted YAML/config work
- Reusable skills that enforce safe, schema-aware output

The goal is simple: keep Copilot responses aligned with eBuilder conventions so generated changes are practical, consistent, and production-oriented.

## What This Repository Contains

### 1) Global Instructions

- `.github/copilot-instructions.md`
- Defines architecture priorities, folder ownership, naming conventions, and anti-patterns
- Enforces a YAML-first approach before escalating to `externals/` or `dll/`

### 2) Specialized Agents

Located in `.github/agents/`:

- `ebuilder-configs.agent.md`
- `ebuilder-resource.agent.md`
- `ebuilder-task.agent.md`
- `ebuilder-sql.agent.md`
- `ebuilder-components.agent.md`
- `ebuilder-externals.agent.md`
- `ebuilder-locale.agent.md`
- `ebuilder-dll-integration.agent.md`

Use these when you want Copilot to operate with tighter scope and stronger domain guardrails.

### 3) Skills

Located in `.github/skills/`.

Skills provide tested playbooks for recurring eBuilder work, including:

- Core architecture decisions
- Schema-safe YAML generation
- Task/resource/SQL safety patterns
- UI component YAML structure
- Event emit/listener patterns
- Locale and i18n enforcement

## eBuilder Architecture Principles Used

This pack assumes the standard eBuilder folder model and follows these priorities:

1. YAML in `configs/` for data flow, resources, policies, and settings
2. YAML in `components/` for UI composition and event wiring
3. `externals/` TypeScript only when YAML becomes hard to maintain
4. `dll/` integration only for heavy backend logic

## Expected Naming Conventions

- Tasks: `task-<feature>`
- Queries: `query-<entity>`
- Mutations: `mutation-<action>-<entity>`
- Locale files: `<lang>.<module>.yml`

## Quick Start

1. Clone this repository.
2. Copy or merge `.github/copilot-instructions.md` into your target eBuilder project.
3. Copy relevant agent files from `.github/agents/`.
4. Copy relevant skill folders from `.github/skills/`.
5. In Copilot Chat, prompt with explicit intent and target area (for example: SQL mutation, resource endpoint, or component event flow).

## Team Setup In VS Code

Use this section to onboard team members so everyone gets the same Copilot behavior.

1. Install Visual Studio Code.
2. Install GitHub Copilot and GitHub Copilot Chat extensions in VS Code.
3. Open your target eBuilder project folder in VS Code.
4. Copy this repository's `.github/` folder into the root of that project.
5. Reload VS Code window:

   ```text
   Command Palette -> Developer: Reload Window
   ```

6. Open Copilot Chat in that project and test with a prompt like:

   ```text
   Create a task-user-approve flow in configs/task.user.yml following eBuilder YAML-first rules.
   ```

7. Confirm Copilot follows these behaviors:

- Prioritizes YAML in `configs/` and `components/`
- Uses eBuilder naming conventions (`task-*`, `query-*`, `mutation-*`)
- Avoids generic React/Vue scaffolding for eBuilder-specific requests
- Uses locale keys instead of hardcoded UI text

If the behavior looks generic, verify the `.github/copilot-instructions.md`, `.github/agents/`, and `.github/skills/` paths are present in the opened workspace root.

## Example Prompts

### Configs / Task / SQL

- "Create a `task-user-approve` flow in `configs/task.user.yml` and wire it to existing mutation targets."
- "Add a `query-user-by-id` in `configs/sql.user.yml` using parameter binding and safe response mapping."
- "Create a REST endpoint in `configs/resource.user.yml` for GET `/users/:id` that targets `query-user-by-id`."

### Components

- "Update `components/pages/user-detail.yml` to load profile data on `on.load` and handle `eb-error`/`eb-done`."
- "Refactor this form to reusable `formFields` and consume via `ebSharedFormFields`."

### Locale

- "Replace hardcoded labels in `components/pages/settings.yml` with locale keys and sync `locale/en.settings.yml` and `locale/vi.settings.yml`."

## Safety Guardrails Enforced

- Prefer extending existing domain files over creating unnecessary new files
- Keep business/data logic out of component markup
- Use parameter binding in SQL (no string-concatenated user input)
- Ensure resource/task targets map only to valid names
- Keep user-facing text locale-driven instead of hardcoded

## When To Use Externals Or DLL

Use `externals/` when UI logic becomes complex, repetitive, or unreadable in YAML.

Use `dll/` when backend integrations or heavy business logic exceed what task YAML should express.

## Repository Layout

```text
.github/
  copilot-instructions.md
  agents/
    *.agent.md
  skills/
    */SKILL.md
```

## Contribution Guidelines

When adding or updating skills/agents:

1. Keep scope explicit: define use-cases and non-goals.
2. Include deterministic checklists where possible.
3. Align with current eBuilder schema and folder responsibilities.
4. Prefer concise, enforceable rules over long generic guidance.

## License

This repository is for internal use only.

Unauthorized copying, distribution, modification, or use outside approved internal contexts is prohibited.
