---
name: ebuilder-component-eb-markdown
description: 'Deep component skill for eb-markdown. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-markdown in eBuilder component YAML.'
---

# eb-markdown Component Skill

## General Docs

Use `eb-markdown` for rendering markdown content with styling capabilities. Source files in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/components/EBMarkdown

## Main Props

| name         | type                 | description              |
| ------------ | -------------------- | ------------------------ |
| on.linkClick | eBuilder events emit | Callback for link click. |
| showingRaw   | boolean              | Whether showing raw.     |
| source       | string               | Specifies source.        |

## YAML Examples

### Story Example 1

````yaml
- name: eb-markdown
  props:
    style:
      padding: var(--padding-sm)
      border: 1px dashed var(--color-border)
    source: |
      # Hello Markdown! 🎉
      ## This is a subtitle 🚀

      Welcome to the Markdown component. Here are some examples:

      - This is a bullet point ✅
      - Another bullet point 📌

      [This is a normal link](https://example.com) 🔗

      [This is an eb-emit link: /eb-emit?eb-event-name=eb-toast&type=success&message=link+clicked+from+eb-markdown](/eb-emit?eb-event-name=eb-toast&type=success&message=link+clicked+from+eb-markdown) 🔗

      > This is a blockquote 💬

      **Bold text** and *italic text* are also supported! ⭐

      ```
      // Some code block
      const greeting = "Hello World!";
      console.log(greeting);
      ```

      Mermaid diagrams are also supported:

      ```mermaid
      graph TD;
        A[Start] --> B{Is it working?};
        B -- Yes --> C[Great!];
        B -- No --> D[Fix it!];
        D --> B;
        C --> E[End];
      ```


      ```mermaid
      timeline
        title History of Social Media Platform
        2002 : LinkedIn
        2004 : Facebook
              : Google
        2005 : YouTube
        2006 : Twitter
        2010 : Instagram
        2011 : Snapchat
        2016 : TikTok
      ```

      Enjoy using Markdown! 🎨
````

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
