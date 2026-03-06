---
name: eBuilder
description: eBuilder is a framework from Macros Reply for building web applications. The skill can assist actions related to eBuilder application development like implementing UI with eb-component or Vue component with external TS module supports, handling eb-query eb-mutation or eb-task and more.
---

# Overview

eBuilder is a full-stack, low-code framework built with modern technologies that enables developers to rapidly build enterprise applications with minimal coding.
It uses YAML configuration files for UI and business logic, complemented by C# and JavaScript for complex scenarios.

# Application Structure

Each eBuilder app contains 8 standard folders:

- components - YAML/Vue files transformed to UI components
- configs - YAML configuration files (security, UI, SQL, tasks)
- externals - ES6/TypeScript modules
- assets - Images, manifests, and resource files (versioned)
- dll - .NET libraries for complex business logic
- locale - YAML language files (multi-language support)
- templates - Text files with eBuilder template syntax for generating email, HTML, PDF, ... content.

## `locale` Folder

The `locale` folder contains YAML files for different languages, enabling multi-language support in the application.
The files are named according to the language code (e.g., `en.*.yaml` for English, `de.*.yaml` for German) and structured as key-value pairs where the key is a unique identifier for the text and the value is the translated string in the respective language like this:

```yaml
# en.yaml
languages:
  name: Name
  language: Language

# de.yaml
languages:
  name: Name
  language: Sprache
```

Each language should have the same set of files and keys to ensure consistency across translations, e.g if there is `en.inboxes.yml` then there should also be `de.inboxes.yml` with the same keys but translated values.

# eBuilder Skill

The eBuilder skill can assist with various tasks related to eBuilder app development, including:

- Implementing UI with eb-component or Vue component with external TS module supports
- Handling eb-query, eb-mutation, or eb-task for data fetching and manipulation
- Writing YAML configuration for security, UI, SQL, and tasks
- Creating templates for generating content like email, HTML, PDF, etc.
- Providing guidance on best practices for eBuilder app development
- Debugging and troubleshooting common issues in eBuilder apps

# Instructions

## Create new language key

To create a new language key, follow these steps:

- Identify the key you want to add and its corresponding value in current supported languages (.i.e existing files in `locale` folder).
- Add the new key-value pair to each language file in the `locale` folder, ensuring that the key is consistent across all files and the value is appropriately translated for each language.
  For example, if you want to add a new key `welcome_message`, you would add it to each language file like this:

  ```yaml
  # File: locale/en.yaml
  languages:
    welcome_message: Welcome to our application!

  # File: locale/de.yaml
  languages:
    welcome_message: Willkommen zu unserer Anwendung!
  ```

- Make sure that changed YAML files are properly formatted and valid to avoid syntax errors in the application.
- Save the changes to the language files.

## Create new language file set to support new language

To create new language files, follow these steps:

- Determine the language code for the new language you want to add (e.g., `es` for Spanish).
- Determine the current supported language files in the `locale` folder to use as a reference for the keys and structure.
- Clone one of the existing language files (e.g., `en.*.yaml`) and rename it to match the new language code (e.g., `es.*.yaml`).
- Translate the values for each key in the new language files while keeping the keys consistent with the existing files.
  For example, if you are creating a Spanish language files, you would translate the values like this when you are being asked to create a new language file for Spanish:

  ```yaml
  # File: locale/en.common.yaml
  languages:
    name: Name
  ```

  then a new file is created for Spanish language with the same keys but translated values:

  ```yaml
  # File: locale/es.common.yaml
  languages:
    name: Nombre
  ```

- Make sure that changed YAML files are properly formatted and valid to avoid syntax errors in the application.
- Save the new language files in the `locale` folder.
