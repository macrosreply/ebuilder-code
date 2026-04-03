---
name: ebuilder-component-eb-monaco-editor
description: 'Deep component skill for eb-monaco-editor. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-monaco-editor in eBuilder component YAML.'
---

# eb-monaco-editor Component Skill

## General Docs

Use `eb-monaco-editor` to render multiple line input form field with code styles and formatting. Simple usages of `eb-monaco-editor` By default `eb-monaco-editor` renders in container with 200px height and word wrap enabled, you can set `height` and `wordWrap` properties to build UI you want. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/form/EBMonacoEditor

## Main Props

| name                   | type                                                                               | description                                                                                                                                     |
| ---------------------- | ---------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| disable                | boolean                                                                            | Whether disable.                                                                                                                                |
| editorOptions          | Record<string, unknown>                                                            | Specifies editor options.                                                                                                                       |
| eventToAppendText      | string                                                                             | Specifies event to append text.                                                                                                                 |
| height                 | string \| number                                                                   | Specifies height.                                                                                                                               |
| language               | string                                                                             | Specifies language.                                                                                                                             |
| provideCompletionItems | (monaco: any, model: any, position: any, context: any, token: any) => Promise<any> | Follow https://microsoft.github.io/monaco-editor/playground.html?source=v0.55.1#example-extending-language-services-completion-provider-example |
| readOnly               | boolean                                                                            | Whether read only.                                                                                                                              |
| wordWrap               | boolean                                                                            | Whether word wrap.                                                                                                                              |

## YAML Examples

### Story Example 1

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: editor
          ebFormControl:
            name: eb-monaco-editor
            label: Monaco Editor
            props:
              language: sql
```

### Story Example 2

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: editor
          ebFormControl:
            name: eb-monaco-editor
            label: Monaco Editor
            props:
              language: sql
              height: 150
              wordWrap: false
```

### Story Example 3

```yaml
children:
  - name: eb-form
    props:
      fields:
        - name: editor
          ebFormControl:
            name: eb-monaco-editor
            label: Suggestions
            props:
              language: json
              height: 150
              editorOptions:
                quickSuggestions: false
              provideCompletionItems: |
                ${{
                  (monaco, model, position) => {
                    const word = model.getWordUntilPosition(position);
                    const range = {
                      startLineNumber: position.lineNumber,
                      endLineNumber: position.lineNumber,
                      startColumn: word.startColumn,
                      endColumn: word.endColumn
                    };

                    return {
                      suggestions: [
                        {
                          label: '"react"',
                          kind: monaco.languages.CompletionItemKind.Property,
                          documentation: 'React library',
                          insertText: '"react": "^18.2.0",',
                          range
                        },
                        {
                          label: '"vue"',
                          kind: monaco.languages.CompletionItemKind.Property,
                          documentation: 'Vue library',
                          insertText: '"vue": "^3.2.37",',
                          range
                        },
                        {
                          label: '"lodash"',
                          kind: monaco.languages.CompletionItemKind.Property,
                          documentation: 'Lodash library',
                          insertText: '"lodash": "^4.17.21",',
                          range
                        }
                      ]
                    };
                  }
                }}
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
