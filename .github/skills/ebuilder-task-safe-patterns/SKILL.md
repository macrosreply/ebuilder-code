---
name: ebuilder-task-safe-patterns
description: 'Generation playbook for configs/task.*.yml in eBuilder. USE FOR: create task-* definitions, select correct task type (library/forward-task/upload-files/schedule/webhook/publish-signal), enforce authorize and upload constraints, validate activator and forwardTo blocks, and align task YAML with task docs, schema, and dll handlers. DO NOT USE FOR: SQL definitions in configs/sql.*.yml or standalone C# implementation without task config changes.'
---

# eBuilder Task Generation Playbook

## Purpose

Use this skill when generating or reviewing `configs/task.*.yml` so output is:

- aligned with `ebuilder-docs/Task.md`
- compatible with `ebuilder.configs.task.schema.json`
- consistent with this repository's current task and DLL conventions

## Scope

- File target: `configs/task.*.yml`
- Root key: `tasks`
- Task names: `task-[a-zA-Z0-9-]+`
- Keep task entries small, explicit, and type-focused

## Repo Grounded Conventions

Patterns already used in this repository and should be preserved:

- Shared auth anchors at top of task files (for example `.eclient-auth: &eclient-auth`).
- `library` tasks usually bind to:
  - `activator.assembly: eClient/eClient.dll` or `eWorkflow/eWorkflow.dll`
  - `activator.class: <Namespace>.Tasks.<ClassName>`
- `forward-task` tasks define `forwardTo.host`, `forwardTo.app`, `forwardTo.task`, `forwardTo.authPolicies`.
- `publish-signal` tasks define `signal.channel[]` and `signal.signal { name, data }`.
- Some existing tasks use top-level `maxFileSize` and `maxFiles` for upload-related limits; preserve local project pattern when extending nearby tasks.

## Task Types And Required Blocks

Select exactly one `type` per task and include its matching required block.

| Task Type        | Source                                                            | Required Block                                 |
| ---------------- | ----------------------------------------------------------------- | ---------------------------------------------- |
| `library`        | schema + docs + repo                                              | `activator`                                    |
| `forward-task`   | schema + docs + repo                                              | `forwardTo`                                    |
| `upload-files`   | schema + docs                                                     | `upload`                                       |
| `schedule`       | docs (schema enum includes type but omits schedule object fields) | `schedule`                                     |
| `webhook`        | docs + schema variant                                             | `webhook` (docs) or `request` (schema variant) |
| `publish-signal` | schema + repo                                                     | `signal`                                       |
| `healthz`        | docs only                                                         | `activator`                                    |

Compatibility notes:

- `healthz` appears in docs but is not included in this schema's `type` enum.
- `webhook` docs use `webhook`, while schema defines `request` for HTTP-style dispatch config.
- Keep using the shape already validated in the target project.

## Configuration Properties (Complete Tables)

### Root Object

| Property | Type   | Required | Notes                                   |
| -------- | ------ | -------- | --------------------------------------- |
| `tasks`  | object | Yes      | Keys must match `^task-[a-zA-Z0-9-]+$`. |

### Task Object (All Properties)

| Property      | Type                     | Required                               | Applies To           | Notes                                                                                                                     |
| ------------- | ------------------------ | -------------------------------------- | -------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `type`        | enum                     | Yes                                    | all                  | `library`, `webhook`, `schedule`, `forward-task`, `upload-files`, `publish-signal` (schema). Docs also mention `healthz`. |
| `authorize`   | object or array          | No                                     | all                  | Single policy rule or list of policy rules.                                                                               |
| `fileAccept`  | string[]                 | No                                     | upload-capable tasks | MIME allow-list for incoming files.                                                                                       |
| `maxFiles`    | integer (min 1)          | No                                     | upload-capable tasks | Max number of files.                                                                                                      |
| `maxFileSize` | number/string expression | No                                     | repo pattern         | Seen in current repo task files as top-level limit.                                                                       |
| `activator`   | object                   | Type-required                          | `library`, `healthz` | DLL binding metadata.                                                                                                     |
| `upload`      | object                   | Type-required                          | `upload-files`       | Upload target options.                                                                                                    |
| `forwardTo`   | object                   | Type-required                          | `forward-task`       | Target engine/app/task routing.                                                                                           |
| `request`     | object                   | Type-required for schema-style webhook | `webhook`            | Schema HTTP dispatch structure.                                                                                           |
| `webhook`     | object                   | Type-required for docs-style webhook   | `webhook`            | Docs dispatch structure with headers/body.                                                                                |
| `schedule`    | object                   | Type-required                          | `schedule`           | Cron and chained task execution settings.                                                                                 |
| `signal`      | object                   | Type-required                          | `publish-signal`     | Channel + signal payload.                                                                                                 |

### `authorize` Object

| Property      | Type     | Required | Notes                                                     |
| ------------- | -------- | -------- | --------------------------------------------------------- |
| `policy`      | string   | No       | Policy key in `configs/security.yml`.                     |
| `roles`       | string[] | No       | Role allow-list under the policy.                         |
| `permissions` | object[] | No       | Object entries whose values are `Modify`, `Show`, `Deny`. |

### `activator` Object

| Property   | Type   | Required | Notes                                           |
| ---------- | ------ | -------- | ----------------------------------------------- |
| `assembly` | string | Yes      | DLL path (or project-resolved assembly in dev). |
| `class`    | string | Yes      | Fully qualified class name for handler/checker. |

### `upload` Object (`upload-files`)

| Property           | Type   | Required     | Notes                                                    |
| ------------------ | ------ | ------------ | -------------------------------------------------------- |
| `folderPath`       | string | Yes (schema) | Absolute upload folder path or app data folder strategy. |
| `maxFileSize`      | number | Yes (schema) | Max single-file size (docs describe MB semantics).       |
| `acceptExtensions` | string | Yes (schema) | Extension allow-list format used by runtime.             |

### `forwardTo` Object (`forward-task`)

| Property       | Type   | Required | Notes                                             |
| -------------- | ------ | -------- | ------------------------------------------------- |
| `host`         | string | Yes      | eBuilder engine base URL.                         |
| `app`          | string | Yes      | Target eBuilder app name.                         |
| `task`         | string | Yes      | Target task name in target app.                   |
| `authPolicies` | object | Yes      | Policy-token map, commonly API key style headers. |

### `request` Object (Schema Webhook Shape)

| Property | Type   | Required | Notes                        |
| -------- | ------ | -------- | ---------------------------- |
| `url`    | string | Yes      | Destination URL.             |
| `method` | string | Yes      | HTTP method.                 |
| `params` | object | No       | Structured parameter groups. |

### `request.params` Object

| Property      | Type   | Required | Notes                                                     |
| ------------- | ------ | -------- | --------------------------------------------------------- |
| `querystring` | object | No       | Query key-values. Additional props accept `any`.          |
| `path`        | object | No       | Path parameter key-values. Additional props accept `any`. |
| `body`        | object | No       | Body key-values. Additional props accept `any`.           |

### `webhook` Object (Docs Webhook Shape)

| Property  | Type   | Required | Notes                                |
| --------- | ------ | -------- | ------------------------------------ |
| `url`     | string | Yes      | Webhook URL.                         |
| `method`  | string | Yes      | Typically `POST`, `PUT`, or `PATCH`. |
| `headers` | object | No       | Header key-values.                   |
| `body`    | object | No       | JSON payload template.               |

### `schedule` Object (Docs Shape)

| Property   | Type     | Required | Notes                                   |
| ---------- | -------- | -------- | --------------------------------------- |
| `cronExp`  | string   | Yes      | Cron expression.                        |
| `timezone` | string   | Yes      | Time zone used by cron evaluation.      |
| `steps`    | object[] | Yes      | Ordered executions of downstream tasks. |

### `schedule.steps[]` Object (Docs Shape)

| Property  | Type   | Required | Notes                              |
| --------- | ------ | -------- | ---------------------------------- |
| `name`    | string | Yes      | Target task name to run.           |
| `context` | object | No       | Optional runtime context override. |
| `params`  | object | No       | Prefilled task params.             |

### `schedule.steps[].context` Object (Docs Shape)

| Property   | Type   | Required | Notes                                                          |
| ---------- | ------ | -------- | -------------------------------------------------------------- |
| `user`     | object | No       | Usually includes fixed scheduler user info (for example `id`). |
| `language` | string | No       | Runtime language override.                                     |

### `signal` Object (`publish-signal`)

| Property  | Type     | Required | Notes                           |
| --------- | -------- | -------- | ------------------------------- |
| `channel` | string[] | Yes      | Destination channels.           |
| `signal`  | object   | Yes      | Signal event payload container. |

### `signal.signal` Object (`publish-signal`)

| Property | Type   | Required | Notes                                               |
| -------- | ------ | -------- | --------------------------------------------------- |
| `name`   | string | Yes      | Signal/event name.                                  |
| `data`   | object | Yes      | Payload data object; additional props accept `any`. |

### `healthz` Type (Docs-Only Notes)

| Property    | Type   | Required | Notes                                                               |
| ----------- | ------ | -------- | ------------------------------------------------------------------- |
| `type`      | string | Yes      | Must be `healthz` in docs model.                                    |
| `activator` | object | Yes      | Reuses `assembly` + `class`; class should implement `IHealthCheck`. |

## Common Properties

Allowed shared properties used across task types:

- `type`
- `authorize` (single rule object or array of rules)
- `fileAccept`
- `maxFiles`
- `maxFileSize` (project pattern)

Security expectations:

- Add `authorize` for any sensitive operation.
- Keep policy names and role names consistent with `configs/security.yml`.

## Generation Order

1. Pick existing target file in `configs/task.*.yml` by domain.
2. Choose task `type` with minimal required behavior.
3. Define `authorize` and upload limits if needed.
4. Add type-specific block (`activator`, `forwardTo`, `signal`, `upload`, `schedule`, or webhook/request).
5. Validate naming and required keys.
6. Cross-check references to:

- DLL class for `library`
- remote app/task for `forward-task`
- referenced task names for `schedule`

## YAML Blueprints

### Library Task

```yaml
tasks:
  task-example-library:
    authorize:
      policy: eClient
    type: library
    activator:
      assembly: eClient/eClient.dll
      class: eClient.Tasks.ExampleLibraryTask
```

## `library` Task Implementation Patterns (C#)

Use `library` when YAML/task composition is not enough and you need custom C# business flow.

### Minimal Handler Sample

```cs
using System.Collections.Generic;
using System.Net;
using System.Threading.Tasks;
using EBuilder.Core.Models;
using EBuilder.Core.Providers;
using Microsoft.AspNetCore.Http;

namespace Task.Project.Namespace;

public class TaskName : TaskLibraryHandler
{
    public TaskName()
        : base() { }

    public override Task<TaskLibraryHandlerResult> Execute(object parameters, List<IFormFile> files)
    {
        return Task.FromResult(new TaskLibraryHandlerResult
        {
            Body = new { data = "data in json response" },
            StatusCode = HttpStatusCode.OK
        });
    }
}
```

### Input Parsing And Validation Sample

Prefer typed input parsing with `ParseInputParams<T>(parameters)` and validation attributes.

```cs
using EBuilder.Core.Validation;

public class InputParams
{
    [EBInputParamProp("nonNullableString")]
    [EBInputParamNullable(false)]
    [EBInputParamRequired]
    public string NonNullableString { get; set; }

    [EBInputParamProp("nullableInt")]
    public int? NullableInt { get; set; }

    [EBInputParamProp("dateOnly")]
    [EBInputParamDateTime(dateOnly: true, useLocalTimeZone: true)]
    public DateTime? DateOnly { get; set; }
}

public override async Task<TaskLibraryHandlerResult> Execute(object parameters, List<IFormFile> files)
{
    var input = ParseInputParams<InputParams>(parameters);

    return new TaskLibraryHandlerResult
    {
        Body = new { input.NonNullableString, input.NullableInt, input.DateOnly }
    };
}
```

### Transaction Pattern In `library` Tasks

Use eBuilder datasource helpers and provider APIs for transaction-safe SQL work.

```cs
var (connection, transaction, dataSourceConfig, queryBuilderProvider) = ConnectDataSource();
var databaseProvider = GetDatabaseProvider(dataSourceConfig.Type);

using (connection)
using (transaction)
{
    try
    {
        var rows = await databaseProvider.Query<MyRow>(
            AppContext,
            dataSourceConfig,
            connection,
            transaction,
            queryBuilderProvider,
            "SELECT id AS \"id\" FROM my_table WHERE id = ${id}",
            new Dictionary<string, object> { ["id"] = 1 }
        );

        transaction.Commit();
        return new TaskLibraryHandlerResult { Body = new { rows } };
    }
    catch
    {
        transaction.Rollback();
        throw;
    }
}
```

### Preferred Singleton Service Pattern (`EBSingletonService`)

Current preferred approach for reusable integrations is registering singleton services in DLL projects with `EBSingletonService` and consuming them from tasks.

#### 1. Define And Register Singleton Service

```cs
using EBuilder.Core.Attributes;
using EBuilder.Core.Services;
using Microsoft.Extensions.Logging;

public interface IDocumentIntelligenceService
{
    Task<string> AnalyzeAsync(string content, CancellationToken cancellationToken = default);
}

[EBSingletonService<IDocumentIntelligenceService, DocumentIntelligenceService>()]
public class DocumentIntelligenceService : IDocumentIntelligenceService, IDisposable
{
    private readonly IAppConfigsService appConfigsService;
    private readonly IDataCachingService dataCachingService;
    private readonly ILogger<IDocumentIntelligenceService> logger;

    public DocumentIntelligenceService(
        IAppConfigsService appConfigsService,
        IDataCachingService dataCachingService,
        ILogger<IDocumentIntelligenceService> logger
    )
    {
        this.appConfigsService = appConfigsService;
        this.dataCachingService = dataCachingService;
        this.logger = logger;
    }

    public Task<string> AnalyzeAsync(string content, CancellationToken cancellationToken = default)
    {
        logger.LogInformation("Analyzing content with singleton service");
        return Task.FromResult(content);
    }

    public void Dispose() { }
}
```

#### 2. Consume Singleton In `TaskLibraryHandler`

```cs
public override async Task<TaskLibraryHandlerResult> Execute(object parameters, List<IFormFile> files)
{
    var documentIntelligenceService = GetSingletonService<IDocumentIntelligenceService>();
    var result = await documentIntelligenceService.AnalyzeAsync("sample");

    return new TaskLibraryHandlerResult
    {
        Body = new { result }
    };
}
```

#### 3. Consume Singleton In Other Contexts (example: health checks)

```cs
var documentIntelligenceService = dllService.GetEBSingletonService<IDocumentIntelligenceService>(EB.App.Name);
```

Notes:

- eBuilder scans loaded DLLs for `EBSingletonService` attributes and registers them in service collection.
- Constructor injection is supported for engine services (`ITaskService`, `IDatabaseService`, `ITemplateService`, `IDataCachingService`, etc.).
- Prefer singleton services for external API clients, expensive SDK clients, and shared cache-backed adapters.

### Handler Result Semantics

- Use `TaskLibraryHandlerResult.Body` for normal JSON response payload (`result.data`).
- Use `ErrorMessage` with appropriate `StatusCode` for predictable frontend error handling.
- For file responses, use stream fields (`BodyStreamMemory`, `BodyStreamFilePath`, `BodyStream`) according to download behavior needs.

### Forward Task

```yaml
tasks:
  task-example-forward:
    authorize:
      policy: eClient
    type: forward-task
    forwardTo:
      host: 'http://localhost:${$EB_ENV(EB_PORT, 5000)}'
      app: eBridge
      task: task-target
      authPolicies:
        api: x-api-key ${$EB_CONSTANT(EBRIDGE_API_KEY)}
```

### Publish Signal Task

```yaml
tasks:
  task-example-publish-signal:
    authorize:
      policy: eClient
    type: publish-signal
    signal:
      channel:
        - 'inbox-{{ ${inboxId} }}'
      signal:
        name: inbox-updated
        data:
          inboxId: ${inboxId}
          changedBy: ${$EB_CONTEXT(user.id)}
```

## DLL Alignment Checklist (for `library`)

Before finalizing a `library` task:

1. Confirm `activator.assembly` points to a real DLL project path under `dll/`.
2. Confirm `activator.class` namespace/class exists in source.
3. Prefer handler classes derived from `TaskLibraryHandler`.
4. If transaction participation is needed, align with handlers implementing `ITaskLibraryTransactionHandler`.
5. Keep task parameter names stable with handler `ParseInputParams<T>` mappings.

## Schema Guardrails

- Keep task names strictly `task-*`.
- Do not add unknown top-level keys under task objects.
- Keep `authorize` structure valid: `policy`, optional `roles`, optional `permissions`.
- Do not combine contradictory task intent in one definition.
- Validate required keys for selected type before returning YAML.

## Anti-Patterns

- Defining tasks in `components/` or non-`configs/task.*.yml` files.
- Creating `library` tasks without `activator`.
- Forwarding tasks without `authPolicies` for protected targets.
- Hardcoding secrets in task config instead of constants/env expressions.
- Introducing a new shape for webhook/schedule blocks without checking local validator expectations first.
