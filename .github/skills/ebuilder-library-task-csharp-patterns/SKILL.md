---
name: ebuilder-library-task-csharp-patterns
description: 'C# implementation playbook for eBuilder library task handlers in dll/. USE FOR: implement or refactor TaskLibraryHandler classes, parse input with ParseInputParams<T>, handle transactions, use EBSingletonService patterns, and shape TaskLibraryHandlerResult responses. DO NOT USE FOR: task YAML schema authoring in configs/task.*.yml, SQL YAML in configs/sql.*.yml, or UI component work.'
---

# eBuilder Library Task C# Playbook

## Purpose

Use this skill when `type: library` task work requires C# implementation or refactoring in `dll/`.

This skill covers:

- handler class structure
- input parsing and validation
- transaction-safe data access patterns
- singleton service registration/consumption
- handler response semantics

## Scope

- Primary folder: `dll/`
- Related contract points: `configs/task.*.yml` `activator.assembly` and `activator.class`
- Handler base type: `TaskLibraryHandler`

## Minimal Handler Sample

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

## Input Parsing And Validation

Prefer typed parsing with `ParseInputParams<T>(parameters)` and validation attributes.

```cs
using EBuilder.Core.Validation;

[EBInputParamClass]
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

## Transaction Pattern

Use eBuilder datasource helpers and provider APIs for transaction-safe SQL work.
Use `databaseProvider` to work with eb-sql commands (support all $EB\__ symbols) directly and use `DatabaseService` to work with defined eb-query and eb-mutation which are defined in `configs/sql._.yml` files.
For querying objects, remember to use EBQueryResultType and EBColumnName attributes to define the shape of the result set.

```cs
[EBQueryResultType]
public class TdTodoDto
{
    [EBColumnName("id")]
    public int? Id { get; set; }

    [EBColumnName("maxDate")]
    public DateTime? MaxDate { get; set; }

    [EBColumnName("maxRemain")]
    public int? MaxRemain { get; set; }
}

...

var (connection, transaction, dataSourceConfig, queryBuilderProvider) = ConnectDataSource();
var databaseProvider = GetDatabaseProvider(dataSourceConfig.Type);

using (connection)
using (transaction)
{
    try
    {
        // query primitive values with eb-sql
        var allItemIdsInSet = await databaseProvider.Query<int>(
            AppContext,
            dataSourceConfig,
            connection,
            transaction,
            queryBuilderProvider,
            """
            SELECT id_item
            FROM ec_item
            WHERE id_set = ${setId}
              AND is_active = $EB_CONSTANT(YES)
            """,
            new Dictionary<string, object>() { ["setId"] = setId }
        );

        // query objects with eb-sql and result mapping (query should use "as" to align with DTO property names or use EBColumnName)
        var listTdTodos = await databaseProvider.Query<TdTodoDto>(
            AppContext,
            dataSourceConfig,
            connection,
            transaction,
            queryBuilderProvider,
            """
            SELECT
                maxremain AS "maxRemain",
                maxdate AS "maxDate",
                id AS "id"
            FROM td_todo
            WHERE tid = ${tId} AND $EB_IN_ARRAY_ARG(id, ${validNextTodos})
            ORDER BY listorder ASC
            """,
            new Dictionary<string, object> { ["tId"] = inputParams.TodoTId, ["validNextTodos"] = validNextTodos }
        );

        // query with defined eb-query in configs/sql.*.yml and result mapping
        var listTdTodosFromEbQuery = await DatabaseService.Query<TdTodoDto>(
            AppContext,
            "query-get-next-todo",
            new Dictionary<string, object> { ["tId"] = inputParams.TodoTId, ["validNextTodos"] = validNextTodos },
            dataSourceConfig,
            connection,
            transaction,
            queryBuilderProvider
        );


        // mutation with eb-sql
        var newCatResults = await databaseProvider.ExecuteSQLStep(
          new SqlStepDefinitionMap
          {
              Command = """
              INSERT INTO td_categories
              (
                  subtitle_de,
                  subtitle_en,
                  subtitle_fr,
                  subtitle_it,
                  isactive,
                  kindof,
                  created_at
              )
              VALUES
              (
                  ${subtitle_de},
                  ${subtitle_en},
                  ${subtitle_fr},
                  ${subtitle_it},
                  ${isactive},
                  ${kindof},
                  $EB_DATETIME_NOW
              )
              """,
              Save = "last-insert-id",
              Extra = new Dictionary<string, string> { ["identityCol"] = "catId" },
          },
          AppContext,
          dataSourceConfig,
          connection,
          transaction,
          queryBuilderProvider,
          new Dictionary<string, object>
          {
              ["subtitle_de"] = tdCategory["subtitle_de"],
              ["subtitle_en"] = tdCategory["subtitle_en"],
              ["subtitle_fr"] = tdCategory["subtitle_fr"],
              ["subtitle_it"] = tdCategory["subtitle_it"],
              ["isactive"] = int.Parse(tdCategory["isactive"]),
              ["kindof"] = int.Parse(tdCategory["kindof"])
          }
        );

        // get the result of databaseProvider.ExecuteSQLStep via `default` prop like below
        // and cast to the expected type based on the Save definition (this work the same as eb-mutation sql step `save` in configs/sql.*.yml)
        if (!newCatResults.TryGetValue("default", out var defaultNewCatResults) || defaultNewCatResults == null)
        {
            transaction.Rollback();
            return new()
            {
                ErrorMessage = "error_checklist_import_category_failed",
                StatusCode = HttpStatusCode.InternalServerError,
            };
        }

        categoryId = Convert.ToInt32(defaultNewCatResults);

        // mutation with defined eb-mutation in configs/sql.*.yml
        var (stepsResults, _) = await DatabaseService.Execute(
            AppContext,
            "mutation-import-sector",
            new Dictionary<string, object> { ["name"] = name },
            dataSourceConfig,
            connection,
            transaction,
            queryBuilderProvider
        );

        if (
            stepsResults is not IDictionary<string, object> stepsResultsDict
            || !stepsResultsDict.TryGetValue("newSectorId", out var newSectorId)
            || newSectorId == null
        )
        {
            throw new InvalidOperationException("mutation-import-sector doesn't return valid 'newSectorId' step value");
        }

        var resultNewSectorId = newSectorId.ToSafeLong();

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

## Singleton Service Pattern

Preferred approach for reusable integrations is registering singleton services in DLL projects with `EBSingletonService` and consuming them from handlers.

### 1. Define And Register Singleton Service

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
        var maxTokens = appConfigsService.ConstantConfigs[App.Name].GetInt(EB.Constant.AI.DEFAULT_MAX_TOKENS);

        logger.LogInformation("Analyzing content with singleton service");
        return Task.FromResult(content);
    }

    public void Dispose() { }
}
```

### 2. Consume Singleton In TaskLibraryHandler

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

### 3. Consume Singleton In Other Contexts

```cs
var documentIntelligenceService = dllService.GetEBSingletonService<IDocumentIntelligenceService>(EB.App.Name);
```

Notes:

- eBuilder scans loaded DLLs for `EBSingletonService` attributes and registers them in service collection.
- Constructor injection is supported for engine services (`ITaskService`, `IDatabaseService`, `ITemplateService`, `IDataCachingService`, `IAppConfigsService`, `ILogger`, etc.).
- Prefer singleton services for external API clients, expensive SDK clients, and shared cache-backed adapters.

## Handler Result Semantics

- Use `TaskLibraryHandlerResult.Body` for normal JSON response payload (`result.data`).
- Use `ErrorMessage` with appropriate `StatusCode` for predictable frontend error handling.
- For file responses, use stream fields (`BodyStreamMemory`, `BodyStreamFilePath`, `BodyStream`) based on download behavior needs.

## Checklist

- Task `activator.class` maps to a real handler class in `dll/`.
- Input mapping and validation attributes match task parameter names.
- Transaction boundaries are explicit where writes occur.
- Shared integrations use `EBSingletonService` when suitable.
- Handler returns predictable status and payload/error semantics.
