---
name: ebuilder-emit-event-patterns
description: 'Generation playbook for eBuilder event-driven UI YAML. USE FOR: on.emit listener registration, on.load/on.unload/on.watch usage, single-event emit, multi-step emit chains with depends/checks, eb-error/eb-done handling, and built-in eb-* events wiring. DO NOT USE FOR: SQL/task/resource authoring.'
---

# eBuilder Emit and Event Patterns Playbook

## Purpose

Use this skill when implementing event-driven behavior in `components/**/*.yml`.

Sources:

- `ebuilder-docs/UI.md` sections on `on.load`, `on.unload`, `on.emit`, `on.watch`
- repository event usage in current `components/pages/*.yml` and `components/components/**/*.yml`

## Root Event Config Table

| Root key    | Type         | When it runs              | Typical use                                        |
| ----------- | ------------ | ------------------------- | -------------------------------------------------- |
| `on.load`   | object/array | Component mounted         | Initial fetch, subscriptions, bootstrap state.     |
| `on.unload` | object/array | Component unmounted       | Cleanup, unsubscribe, reset state.                 |
| `on.emit`   | array        | While mounted             | Register named listeners for custom/system events. |
| `on.watch`  | array        | On reactive value changes | Trigger events when props/state values change.     |

## Lifecycle Events: `on.load` and `on.unload`

Pattern:

```yaml
on:
  load:
    - emit:
        - name: evt-load-data
          params:
            id: ${{ $rootProps.itemId }}
  unload:
    - emit:
        - name: evt-cleanup
          params:
            id: ${{ $rootProps.itemId }}
```

## Watch Pattern

```yaml
on:
  watch:
    - value: ${{ $rootProps.isActive }}
      handler:
        emit:
          - name: evt-activated
            checks:
              - ${{ $event.value === true }}
          - name: evt-deactivated
            checks:
              - ${{ $event.value === false }}
```

## Listener Registration (`on.emit`)

Pattern:

```yaml
on:
  emit:
    - name: evt-load-file-details
      debounce: 300
      handler:
        emit:
          - name: eb-manual
            params:
              handler: ${{ () => { /* update state */ } }}
          - name: eb-query
            params:
              name: query-file-details
```

Rules:

1. Use stable `evt-*` event naming for app-specific events.
2. Keep handler responsibilities cohesive.
3. If a handler grows too much, move logic to `externals/`.
4. We can use `debounce` in `on.emit` handlers to prevent rapid re-firing, but use it judiciously.

## Emit Forms

### 1. Single event (string)

```yaml
emit: evt-refresh
```

### 2. Single event (object)

```yaml
emit:
  name: eb-query
  params:
    name: query-profile
```

### 3. Multiple events (chain array)

```yaml
emit:
  - name: eb-mutation
    key: saveProfile
    params:
      name: mutation-update-profile
      data: ${{ $event.model }}
  - name: eb-toast
    depends:
      - saveProfile
    params:
      type: success
      message: profile_saved
  - name: evt-refresh-profile
    depends:
      - saveProfile
```

## Emit Item Config Table

| Property  | Type               | Required                               | Notes                                         |
| --------- | ------------------ | -------------------------------------- | --------------------------------------------- |
| `name`    | string             | Yes                                    | Event name to emit (`evt-*` or `eb-*`).       |
| `key`     | string             | No                                     | Step alias for referencing output in `$emit`. |
| `params`  | object             | No                                     | Event payload/config.                         |
| `depends` | string/array       | No                                     | Emit only after listed steps finish.          |
| `checks`  | array              | No                                     | Conditional guards. False skips this step.    |
| `handler` | inline js/function | For `eb-manual`, `eb-error`, `eb-done` | Manual callback body.                         |
| `delay`   | number             | No                                     | Delay in ms before emitting this event.       |

## Chain Context Variables

| Variable | Scope                     | Purpose                                                                                                                                                                                      |
| -------- | ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `$event` | Current listener/callback | Incoming payload and component event context.                                                                                                                                                |
| `$emit`  | Emit chain                | Access prior step results by key/name.                                                                                                                                                       |
| `$error` | `eb-error` only           | Captured error from chain execution. There is `$error.errorMessageInResponse` prop for API errors. If it's not available, normally we use default error message `__eb_default_error_message` |

## Dependency Result Access (`$emit.<dependency>`)

In lower steps, you can read results from upper dependencies through `$emit`.

You can use this in:

1. `checks`
2. `params`
3. `handler` of manual steps

Examples:

```yaml
emit:
  - name: eb-query
    key: loadDefaults
    params:
      name: query-next-priority

  - name: eb-mutation
    key: saveData
    depends:
      - loadDefaults
    params:
      name: mutation-save-item
      data: |
        ${{
          { ...$event.model, priority: $emit.loadDefaults.results[0]?.NextPriority ?? 1 }
        }}

  - name: eb-toast
    depends:
      - saveData
    checks:
      - ${{ $emit.saveData?.success !== false }}
    params:
      type: success
      message: saved_successfully
```

Note:

- Access style `$emit.<dependency>` works when the dependency is keyed and can be represented as a property.
- Access style `$emit['<dependency>']` is also valid and useful for names/keys containing characters that are not property-safe.

## `on.watch` Pattern

```yaml
on:
  watch:
    - value: ${{ $rootProps.isCounting }}
      handler:
        emit:
          - name: evt-start-counter
            checks:
              - ${{ $event.value }}
          - name: evt-stop-counter
            checks:
              - ${{ !$event.value }}
```

Use `on.watch` to react to value transitions instead of ad-hoc polling.

## Built-In Event Reference (From `Global/src/emitter/index.ts`)

This section is grounded on:

- `/Users/kentnguyen/Projects/macros-reply-ebuilder-next-v4/ebuilder-js-sdk/packages/Global/src/emitter/index.ts`

### `eb-query`

| Config key                  | Type   | Required | Notes                                             |
| --------------------------- | ------ | -------- | ------------------------------------------------- |
| `name`                      | string | Yes      | Query name.                                       |
| `inputParams`/query options | object | No       | Standard query config options passed to DB layer. |

Emit sample:

```yaml
emit:
  - name: eb-query
    key: loadUsers
    params:
      name: query-users
      inputParams:
        inboxId: ${{ $rootProps.inboxId }}
```

Result usage (`$emit`):

```yaml
emit:
  - handler: |
      ${{
        $rootVars.users = $emit.loadUsers.results || []
        // For eb-query with responseType: single, singleOrNull, first, firstOrNull, user `.result` instead of `.results`
        $rootVars.user = $emit.loadUsers.result
      }}
```

### `eb-query-excel`

| Config key          | Type   | Required | Notes                                  |
| ------------------- | ------ | -------- | -------------------------------------- |
| `name`              | string | Yes      | Query name for excel export.           |
| excel query options | object | No       | Passed to `db.queryToExcelWithConfig`. |

Emit sample:

```yaml
emit:
  - name: eb-query-excel
    params:
      name: query-users
      inputParams:
        inboxId: ${{ $rootProps.inboxId }}
```

Result usage (`$emit`): no stable payload for chaining. Treat as side-effect event.

### `eb-mutation`

| Config key       | Type    | Required | Notes                                       |
| ---------------- | ------- | -------- | ------------------------------------------- |
| `name`           | string  | Yes      | Mutation name.                              |
| mutation params  | object  | No       | Payload/options for mutation.               |
| `__without_spin` | boolean | No       | Skip spinner in non-chain usage.            |
| `__without_mask` | boolean | No       | Skip blocking mask while spinner is active. |

Emit sample:

```yaml
emit:
  - name: eb-mutation
    key: saveUser
    params:
      name: mutation-update-user
      data: ${{ $event.model }}
```

Result usage (`$emit`):

```yaml
checks:
  - ${{ $emit.saveUse.result != null }}
```

### `eb-task`

| Config key       | Type    | Required | Notes                                            |
| ---------------- | ------- | -------- | ------------------------------------------------ |
| `name`           | string  | Yes      | Task name.                                       |
| `params`         | object  | No       | Task payload. Files should be in `params.files`. |
| `cache`          | object  | No       | Cache save/invalid options for task execution.   |
| `headers`        | object  | No       | Extra request headers.                           |
| `noCancel`       | boolean | No       | Forwarded to task.execute.                       |
| `keepAlive`      | boolean | No       | Forwarded to task.execute.                       |
| `__without_spin` | boolean | No       | Skip spinner in non-chain usage.                 |
| `__without_mask` | boolean | No       | Skip blocking mask while spinner is active.      |

Emit sample:

```yaml
emit:
  - name: eb-task
    key: exportPdf
    params:
      name: task-export-report
      params:
        itemId: ${{ $rootProps.itemId }}
```

Result usage (`$emit`):

```yaml
emit:
  - name: eb-open-url
    depends: [exportPdf]
    params:
      url: ${{ EB.task.getDownloadByKeyUrl($emit.exportPdf.result.downloadKey) }}
```

### `eb-authorize`

| Config key              | Type   | Required | Notes                                           |
| ----------------------- | ------ | -------- | ----------------------------------------------- |
| authorize action object | object | Yes      | Same shape as `EB.auth.authorize` action input. |

Emit sample:

```yaml
emit:
  - name: eb-authorize
    key: canDelete
    params:
      policy: eClient
      permissions:
        deleteItem: Modify
```

Result usage (`$emit`):

```yaml
checks:
  - ${{ $emit.canDelete === true }}
```

### `eb-submit-login`

| Config key        | Type   | Required | Notes                         |
| ----------------- | ------ | -------- | ----------------------------- |
| login credentials | object | Yes      | Forwarded to `EB.auth.login`. |

Emit sample:

```yaml
emit:
  - name: eb-submit-login
    key: loginResult
    params:
      policy: eClient
      username: ${{ $event.model.username }}
      password: ${{ $event.model.password }}
```

Result usage (`$emit`):

```yaml
checks:
  - ${{ $emit.loginResult?.success === true }}
```

### `eb-confirm`

| Config key | Type   | Required | Notes                         |
| ---------- | ------ | -------- | ----------------------------- |
| `message`  | string | Yes      | Locale key/text.              |
| `values`   | object | No       | Message interpolation values. |

Emit sample:

```yaml
emit:
  - name: eb-confirm
    key: confirmDelete
    params:
      message: delete_confirm_message
      values:
        name: ${{ $event.record.name }}
```

Result usage (`$emit`):

```yaml
checks:
  - ${{ $emit.confirmDelete === true }}
```

### `eb-alert`

| Config key | Type              | Required | Notes                         |
| ---------- | ----------------- | -------- | ----------------------------- |
| `message`  | string            | Yes      | Locale key/text.              |
| `values`   | object            | No       | Message interpolation values. |
| `type`     | `success`/`error` | No       | Defaults to `success`.        |

Emit sample:

```yaml
emit:
  - name: eb-alert
    params:
      message: validation_failed
      type: error
```

Result usage (`$emit`): no stable payload for chaining.

### `eb-toast`

| Config key | Type              | Required | Notes                         |
| ---------- | ----------------- | -------- | ----------------------------- |
| `message`  | string            | Yes      | Locale key/text.              |
| `values`   | object            | No       | Message interpolation values. |
| `type`     | `success`/`error` | No       | Defaults to `success`.        |

Emit sample:

```yaml
emit:
  - name: eb-toast
    params:
      message: save_success
      type: success
```

Result usage (`$emit`): no stable payload for chaining.

### `eb-set-interval`

| Config key    | Type     | Required | Notes                        |
| ------------- | -------- | -------- | ---------------------------- |
| `key`         | string   | Yes      | Interval key to clear later. |
| `interval`    | number   | Yes      | Milliseconds.                |
| `on.interval` | function | Yes      | Callback executed each tick. |

Emit sample:

```yaml
emit:
  - name: eb-set-interval
    params:
      key: refresh-users
      interval: 30000
      on:
        interval:
          emit:
            - name: evt-refresh-users
```

Result usage (`$emit`): no stable payload for chaining.

### `eb-clear-interval`

| Config key | Type   | Required | Notes                               |
| ---------- | ------ | -------- | ----------------------------------- |
| `key`      | string | Yes      | Interval key previously registered. |

Emit sample:

```yaml
emit:
  - name: eb-clear-interval
    params:
      key: refresh-users
```

Result usage (`$emit`): no stable payload for chaining.

### `eb-manual`

| Config key | Type      | Required | Notes                                                              |
| ---------- | --------- | -------- | ------------------------------------------------------------------ |
| `handler`  | inline js | Yes      | Callback logic; wrapper function is optional in newer engine.      |
| `params`   | object    | No       | Additional params merged into `$emit` context for handler runtime. |

Emit sample:

```yaml
emit:
  - key: preparePayload
    handler: |
      ${{
        return {
          data: $event.model,
          changedAt: Date.now(),
        };
      }}
```

Result usage (`$emit`):

```yaml
params:
  data: ${{ $emit.preparePayload.data }}
```

### `eb-new-browser-tab`

| Config key     | Type   | Required | Notes                             |
| -------------- | ------ | -------- | --------------------------------- |
| `url`          | string | Yes      | URL to open.                      |
| `downloadName` | string | No       | Appended as encoded path segment. |

Emit sample:

```yaml
emit:
  - name: eb-new-browser-tab
    params:
      url: ${{ EB.task.getDownloadByKeyUrl($emit.exportPdf.result.downloadKey) }}
      downloadName: report.pdf
```

Result usage (`$emit`): no stable payload for chaining.

### `eb-open-url`

| Config key | Type    | Required | Notes                      |
| ---------- | ------- | -------- | -------------------------- |
| `url`      | string  | Yes      | URL to open.               |
| `newTab`   | boolean | No       | Open in new tab when true. |

Emit sample:

```yaml
emit:
  - name: eb-open-url
    params:
      url: /help
      newTab: false
```

Result usage (`$emit`): no stable payload for chaining.

### `eb-router-push`

| Config key | Type   | Required | Notes          |
| ---------- | ------ | -------- | -------------- |
| `name`     | string | No       | Route name.    |
| `path`     | string | No       | Route path.    |
| `params`   | object | No       | Router params. |
| `query`    | object | No       | Router query.  |

Emit sample:

```yaml
emit:
  - name: eb-router-push
    params:
      path: /inbox
      query:
        id: ${{ $rootProps.inboxId }}
```

Result usage (`$emit`): router result may be available but should not be relied on for business logic.

### `eb-router-replace`

| Config key | Type   | Required | Notes          |
| ---------- | ------ | -------- | -------------- |
| `name`     | string | No       | Route name.    |
| `path`     | string | No       | Route path.    |
| `params`   | object | No       | Router params. |
| `query`    | object | No       | Router query.  |

Emit sample:

```yaml
emit:
  - name: eb-router-replace
    params:
      path: /dashboard
```

Result usage (`$emit`): router result may be available but should not be relied on for business logic.

### `eb-route-redirect` (Deprecated)

| Config key               | Type   | Required | Notes                                      |
| ------------------------ | ------ | -------- | ------------------------------------------ |
| same as `eb-router-push` | object | No       | Deprecated alias; prefer `eb-router-push`. |

Emit sample:

```yaml
emit:
  - name: eb-route-redirect
    params:
      path: /dashboard
```

Result usage (`$emit`): same caveat as router push.

### `eb-doc-title`

| Config key | Type   | Required | Notes                                     |
| ---------- | ------ | -------- | ----------------------------------------- |
| `title`    | string | Yes      | Locale key/text, combined with app title. |

Emit sample:

```yaml
emit:
  - name: eb-doc-title
    params:
      title: page_inbox
```

Result usage (`$emit`): no stable payload for chaining.

### `eb-reload-dev-app`

| Config key | Type | Required | Notes                                                |
| ---------- | ---- | -------- | ---------------------------------------------------- |
| none       | -    | No       | Triggers dev reload task and page reload on success. |

Emit sample:

```yaml
emit:
  - name: eb-reload-dev-app
    key: reloadDone
```

Result usage (`$emit`):

```yaml
checks:
  - ${{ $emit.reloadDone === true }}
```

### `eb-cache-save`

| Config key | Type     | Required | Notes                  |
| ---------- | -------- | -------- | ---------------------- |
| `key`      | object   | Yes      | Cache key object.      |
| `data`     | unknown  | Yes      | Data to store.         |
| `tags`     | string[] | Yes      | Tags for invalidation. |

Emit sample:

```yaml
emit:
  - name: eb-cache-save
    params:
      key:
        module: inbox
        id: ${{ $rootProps.inboxId }}
      data: ${{ $rootVars.inboxDetails }}
      tags: [inbox-details]
```

Result usage (`$emit`): no stable payload for chaining.

### `eb-cache-invalid`

| Config key | Type     | Required | Notes                     |
| ---------- | -------- | -------- | ------------------------- |
| `tags`     | string[] | Yes      | Cache tags to invalidate. |

Emit sample:

```yaml
emit:
  - name: eb-cache-invalid
    params:
      tags: [inbox-details, inbox-list]
```

Result usage (`$emit`): no stable payload for chaining.

### `eb-state-store-remove`

| Config key | Type     | Required | Notes                                |
| ---------- | -------- | -------- | ------------------------------------ |
| `key`      | string[] | Yes      | Store keys to remove.                |
| `match`    | function | No       | Predicate to remove matching stores. |

Emit sample:

```yaml
emit:
  - name: eb-state-store-remove
    params:
      key:
        - inbox-details-1
        - inbox-details-2
```

Result usage (`$emit`): no stable payload for chaining.

### `eb-post-message`

| Config key | Type    | Required | Notes                          |
| ---------- | ------- | -------- | ------------------------------ |
| `to`       | string  | Yes      | `parent` or iframe element id. |
| `type`     | string  | Yes      | Message type.                  |
| `data`     | unknown | No       | Payload (cloned before send).  |

Emit sample:

```yaml
emit:
  - name: eb-post-message
    params:
      to: parent
      type: open-item
      data:
        id: ${{ $rootProps.itemId }}
```

Result usage (`$emit`): no stable payload for chaining.

### `eb-message-received`

| Config key       | Type   | Required | Notes                                                                     |
| ---------------- | ------ | -------- | ------------------------------------------------------------------------- |
| listener payload | object | Runtime  | Emitted by window message listener with `{ orgEvent, data, type, from }`. |

Listener sample:

```yaml
on:
  emit:
    - name: eb-message-received
      handler:
        emit:
          - handler: |
              ${{
                if ($event.type === 'open-item') {
                  $rootVars.itemId = $event.data?.id;
                }
              }}
```

Result usage (`$emit`): this is usually a listener event, not a chain result source.

### `eb-signal-subscribe`

| Config key | Type     | Required | Notes                  |
| ---------- | -------- | -------- | ---------------------- |
| `channel`  | string[] | Yes      | Channels to subscribe. |

Emit sample:

```yaml
emit:
  - name: eb-signal-subscribe
    params:
      channel: [team-inbox]
```

Result usage (`$emit`): no stable payload for chaining.

### `eb-signal-unsubscribe`

| Config key | Type     | Required | Notes                    |
| ---------- | -------- | -------- | ------------------------ |
| `channel`  | string[] | Yes      | Channels to unsubscribe. |

Emit sample:

```yaml
emit:
  - name: eb-signal-unsubscribe
    params:
      channel: [team-inbox]
```

Result usage (`$emit`): no stable payload for chaining.

### `eb-signal-received`

| Config key       | Type   | Required | Notes                                                                                                                                                                                                    |
| ---------------- | ------ | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| listener payload | object | Runtime  | Raised when signal events are published to subscribed channels. there is `name` `channel` `isFromCurrentConnection` and `data` props which are name and data of the signal and the channel it belongs to |

Listener sample:

```yaml
on:
  emit:
    - name: eb-signal-received
      handler:
        emit:
          - name: evt-refresh-items
```

Result usage (`$emit`): this is usually a listener event, not a chain result source.

### `eb-error` and `eb-done` (Chain Hooks)

| Hook       | Required config | Notes                                             |
| ---------- | --------------- | ------------------------------------------------- |
| `eb-error` | `handler`       | Catch handler called with `(emitResults, error)`. |
| `eb-done`  | `handler`       | Finalizer called with `(emitResults)` always.     |

Emit sample:

```yaml
emit:
  - name: eb-query
    key: loadData
    params:
      name: query-data
  - name: eb-error
    handler: |
      ${{
        // $emit contains completed step outputs; $error is error object
        $rootVars.errorMessage = $error.errorMessageInResponse ?? '__eb_default_error_message'
      }}
  - name: eb-done
    handler: |
      ${{
        // always executed at chain end
      }}
```

Result usage (`$emit`): both hooks receive full chain outputs from completed steps.

## `eb-manual` Shorthand (Newer Engine Behavior)

In newer engine versions:

1. `name: eb-manual` can be omitted in emit items (manual is treated as default type).
2. `handler` should be inline JS callback logic that receives `$emit` as first argument.
3. The wrapper callback form is optional; engine generates it during YAML-to-JS transformation.

Long form:

```yaml
emit:
  - name: eb-manual
    handler: |
      ${{
        ($emit) => {
          // manual logic
        }
      }}
```

Short form (preferred):

```yaml
emit:
  - handler: |
      ${{
        // manual logic
        // use $emit.<dependency> directly
      }}
```

## Try/Catch Pattern In Emit Chains

Use `eb-error` and `eb-done` at the end of a chain:

```yaml
emit:
  - name: eb-query
    key: loadData
    params:
      name: query-data
  - name: eb-error
    handler: |
      ${{
        // handle error with $error and partial $emit results
      }}
  - name: eb-done
    handler: |
      ${{
        // always runs after chain finishes
      }}
```

## Deterministic Checklist

1. Pick correct root event hook (`load`, `unload`, `emit`, `watch`).
2. Name custom events consistently (`evt-*`).
3. Use chain `key` + `depends` for explicit order.
4. Use `$emit.<dependency>` (or `$emit['dependency']`) in lower-step checks/params/manual handlers.
5. Use `checks` for conditional branches.
6. Add `eb-error` and `eb-done` for non-trivial chains.
7. Use the short `eb-manual` form where readable.
8. Keep side effects predictable and easy to trace.
