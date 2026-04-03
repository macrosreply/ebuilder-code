---
name: ebuilder-component-eb-layout-master-tabs
description: 'Deep component skill for eb-layout-master-tabs. USE FOR: prop-safe YAML generation, nested prop authoring, and event wiring for eb-layout-master-tabs in eBuilder component YAML.'
---

# eb-layout-master-tabs Component Skill

## General Docs

No dedicated \*.docs.mdx content is available. Guidance is inferred from \*.types.ts, \*.stories.ts (when present), and real YAML usage in eClient repositories. Source directory in ebuilder_engine_repository: ebuilder-js-sdk/packages/Vue/src/layouts/EBLayoutMasterTabs

## YAML Examples

### Minimal Example

```yaml
children:
  - name: eb-layout-master-tabs
    slot: else
    props:
      headerLogo: header-logo.svg
      headerTag: eClient
      userProfileLinks:
        - policy: eClient
          on:
            click:
              emit:
                name: eb-router-push
                params:
                  path: /profile
      extraUserMenuItems:
        - key: admin
          text: eClientAdmin
          on:
            click:
              emit:
                name: eb-open-url
                params:
                  url: ${{ `${EB.constant.ECLIENTADMIN_PUBLIC_HOST_URL}/eClientAdmin` }}
                  newTab: true
        - key: api
          text: root_user_menu_api
          on:
            click:
              emit:
                name: eb-open-url
                params:
                  url: ${{ `${location.origin + location.pathname}docs/swagger.html` }}
                  newTab: true
        - key: help
          text: common_help
          router-link: /help
        - key: about
          text: about_heading
          router-link: /about
      items:
        - key: ${{ `inbox-${$rootStore.currentUser.id ?? 'anonymous'}` }}
          icon: fas,home
          title: common_inbox
          router-link: |
            ${{ $rootStore.currentUser.id ? `/inbox/${$rootStore.currentUser.id}/open-documents` : '/home' }}
      headerMenuItems:
        - key: enhanced-search
          hide: ${{ !$rootVars.isLoggedIn }}
          icon: fas,magnifying-glass-plus
          title: adv_search_heading
          on:
            click:
              emit:
                - name: eb-router-push
                  params:
                    path: ${{ `/search/${EB.utils.uuid()}` }}
      dynamicItemsRules:
        - key: search-{searchId}
          text: search_tab_title
          icon: fas,magnifying-glass
          path: /search/:searchId
          policy: eClient
        - key: profile
          text: common_profile
          icon: fas,address-card
          path: /profile
          policy: eClient
        - key: case-{caseId}
          text: |
            ${{ `${EB.intl.formatMessage('case_tab_title')} {caseId}` }}
          icon: fas,bars-progress
          path: /case/:caseId
          policy: eClient
        - key: letter-{letterId}
          text: |
            ${{ `${EB.intl.formatMessage('letter_tab_title')} {letterId}` }}
          icon: fas,bars-progress
          path: /letter/:letterId
          policy: eClient
        - key: set-{setId}
          text: |
            ${{ `${EB.intl.formatMessage('set_tab_title')} {setId}` }}
          icon: fas,bars
          path: /set/:setId
          policy: eClient
        - key: file-{fileNr}
          text: |
            ${{ `${EB.intl.formatMessage('file_tab_title')} {fileNr}` }}
          icon: fas,paperclip
          path: /file/:fileNr/:categoryId?
          policy: eClient
        - key: inbox-{inboxId}
          text: |
            ${{ `${EB.intl.formatMessage('inbox_tab_title')} {inboxId}` }}
          icon: fas,inbox
          path: /inbox/:inboxId
          policy: eClient
        - key: help
          text: common_help
          icon: fas,circle-question
          path: /help
        - key: about
          text: about_heading
          icon: fas,circle-exclamation
          path: /about
      masterSearch:
        maxWidth: 800
        hide: ${{ !$rootVars.isLoggedIn }}
        placeholder: master_search_quick_search
        customSuggestions: ${{ $rootMemo.customSuggestions }}
        searchDelay: 1000
        enterToSearch: ${{ EB.constant.FEATURES.QUICK_SEARCH.pressEnterToSearch }}
        searchOption:
          default: |
            ${{ EB.constant.FEATURES.QUICK_SEARCH.enableSearchAll ? 'all' : EB.constant.FEATURES.QUICK_SEARCH.searchOptions[0] }}
          select:
            style:
              width: 100px
              margin-right: 0
            showSearch: false
            showClear: false
            source:
              data: |
                ${{
                  [
                    ...(EB.constant.FEATURES.QUICK_SEARCH.enableSearchAll
                      ? [
                          {
                            label: EB.intl.formatMessage('common_all'),
                            value: 'all'
                          }
                        ]
                      : []),
                    ...EB.constant.FEATURES.QUICK_SEARCH.searchOptions.map((option) => ({
                      label: EB.intl.formatMessage(`common_${option}`),
                      value: option
                    }))
                  ]
                }}
              label: label
              value: value
              autoSelectOnFirstLoad: true
        on:
          suggest:
            emit:
              name: evt-submit-search-suggest
              params:
                searchTerm: ${{ $event.searchTerm }}
                searchOption: ${{ $event.searchOption }}
      on:
        close:
          emit:
            - name: eb-manual
              handler: |
                ${{
                  if (
                    $event.key.startsWith('case-') ||
                    $event.key.startsWith('file-') ||
                    $event.key.startsWith('inbox-')
                  ) {
                    EB.storage.stateStore.removeStore(`tab-info-state-${$event.key}-*`)
                  }
                }}
    children:
      - name: div
        slot: master-search-hint
        props:
          style:
            margin-top: var(--padding-sm)
        children:
          - name: eb-typography
            props:
              text: master_search_hint
              type: secondary
              style:
                display: block
          - name: eb-if
            props:
              checks:
                - ${{ !!$rootVars.loadingSuggestions }}
            children:
              - name: eb-loading
                props:
                  label: master_search_quick_searching
                  inline: true
              - name: eb-if
                slot: else
                props:
                  checks:
                    - ${{ $rootVars.hasSearchTerm && !$rootVars.searchResults.length }}
                children:
                  - name: eb-typography
                    props:
                      text: master_search_quick_search_no_results
      - name: div
        slot: master-search-extra
        props:
          style:
            margin-top: var(--padding-md)
            padding-top: var(--padding-md)
            border-top: 1px solid var(--base-border)
        children:
          - name: div
            props:
              style:
                display: flex
                align-items: center
                justify-content: space-between
            children:
              - name: div
                children:
                  - name: eb-if
                    props:
                      checks:
                        - ${{ $rootVars.viewingAllOf }}
                    children:
                      - name: eb-button
                        props:
                          type: default
                          text: common_close
                          icon: fas,xmark
                          danger: true
                          on:
                            click:
                              emit:
                                - name: eb-manual
                                  handler: |
                                    ${{
                                      $rootVars.viewingAllOf = null
                                    }}
              - name: eb-button
                props:
                  text: adv_search_heading
                  icon: fas,magnifying-glass-plus
                  on:
                    click:
                      emit:
                        - name: eb-router-push
                          params:
                            path: ${{ `/search/${EB.utils.uuid()}` }}
                        - name: eb-layout-master-search-close-modal
          - name: div
            props:
              style:
                display: |
                  ${{ $rootVars.viewingAllOf ? 'block' : 'none' }}
                border-top: 1px solid var(--base-border)
                margin-top: var(--padding-md)
            children:
              - name: eb-if
                props:
                  checks:
                    - ${{ $rootVars.viewingAllOf === 'inbox' }}
                children:
                  - name: all-searchable-inboxes
                    props:
                      searchTerm: ${{ $rootVars.currentSearchTerm }}
              - name: eb-if
                props:
                  checks:
                    - ${{ $rootVars.viewingAllOf === 'file' }}
                children:
                  - name: all-searchable-files
                    props:
                      searchTerm: ${{ $rootVars.currentSearchTerm }}
              - name: eb-if
                props:
                  checks:
                    - ${{ $rootVars.viewingAllOf === 'case' }}
                children:
                  - name: all-searchable-cases
                    props:
                      searchTerm: ${{ $rootVars.currentSearchTerm }}
              - name: eb-if
                props:
                  checks:
                    - ${{ $rootVars.viewingAllOf === 'set' }}
                children:
                  - name: all-searchable-sets
                    props:
                      searchTerm: ${{ $rootVars.currentSearchTerm }}
              - name: eb-if
                props:
                  checks:
                    - ${{ $rootVars.viewingAllOf === 'document' }}
                children:
                  - name: all-searchable-documents
                    props:
                      searchTerm: ${{ $rootVars.currentSearchTerm }}
      - name: eb-loading
        props:
          visible: ${{ $rootVars.isLoggedIn && !$rootStore.currentUser.rights }}
      - name: eb-if
        props:
          checks:
            - ${{ !$rootVars.isLoggedIn || ($rootVars.isLoggedIn && $rootStore.currentUser.rights) }}
        children:
          - name: right-view-wrapper
            children:
              - name: eb-router-view
```

## Authoring Notes

- Prefer locale keys for user-facing strings in labels/placeholders/tooltips.
- Keep business logic in configs tasks/resources/queries; keep component YAML focused on UI composition and event wiring.
- For event listeners defined as on.event_name, wire emit chains declaratively and keep handlers short.
