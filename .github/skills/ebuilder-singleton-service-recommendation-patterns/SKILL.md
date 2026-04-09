---
name: ebuilder-singleton-service-recommendation-patterns
description: 'Recommendation playbook for moving reusable business logic into EBSingletonService classes in eBuilder DLL projects. USE FOR: designing service boundaries, registering services with EBSingletonService, dependency injection, consuming singleton services from TaskLibraryHandler, and keeping handlers thin. DO NOT USE FOR: task YAML schema authoring in configs/task.*.yml, SQL YAML structure authoring in configs/sql.*.yml, or basic TaskLibraryHandler boilerplate (use ebuilder-library-task-csharp-patterns).'
---

# eBuilder Singleton Service Recommendation Patterns

## Purpose

Use this skill when business logic in `TaskLibraryHandler` becomes large, repeated, or integration-heavy and should be extracted into reusable singleton services.

This skill covers:

- service extraction criteria and boundary decisions
- `EBSingletonService` registration patterns
- constructor injection and cross-service consumption
- handler-to-service orchestration pattern
- reliability guardrails (context, transaction, error handling)

## Scope

- Primary folder: `dll/`
- Companion handler skill: `.github/skills/ebuilder-library-task-csharp-patterns/SKILL.md`
- Related contract points: `configs/task.*.yml` `activator.assembly` and `activator.class`

## When To Recommend Singleton Extraction

Recommend extracting logic from handler into singleton service when one or more apply:

- same integration logic appears in multiple handlers
- external SDK/client setup is expensive and reusable
- business rules exceed simple request orchestration
- logic needs shared caching, policy checks, or composition across modules

Keep logic in handler when:

- operation is one-off and short
- extraction would add indirection without reuse value

## Core Pattern

### 1. Define And Register Singleton Service

```cs
// Add required usings
using ...

public interface IDbDocService
{
    Task<ReadOnlyMemory<byte>> BuildDbDocumentPdfContent(EBuilderAppContext appContext, DocumentEntryDto document);

    bool IsDbDocument(DocumentEntryDto document);
}

// Always use primary construction, so need to add `private readonly ... = ...` setup any more
[EBSingletonService<IDbDocService, DbDocService>()]
public class DbDocService(
    IAppConfigsService appConfigsService,
    IDllService dllService,
    ILogger<DbDocService> logger,
    IDatabaseService databaseService,
    Func<DataSourceType, IDatabaseProvider> getDBProvider,
    ITemplateService templateService,
    IPdfService pdfService
) : IDbDocService, IDisposable
{
    protected readonly TimeZoneInfo targetTimeZone = TimeZoneInfo.FindSystemTimeZoneById("W. Europe Standard Time");

    // appContext should be always passed from the TaskLibraryHandler to singleton service methods, don't try to add anything like like IAppContext
    public async Task<DocumentTextNote> BuildDbDocumentRawContent(EBuilderAppContext appContext, DocumentEntryDto document)
    {
        // Example of consuming another singleton service in a singleton service
        var adosDocPermissionService = dllService.GetEBSingletonService<IAdosDocPermissionService>(EB.App.Name);
        // Example of using database service to connect to datasource with databaseService.ConnectDataSource(appContext), don't try to reimplement any kind of ConnectDataSource
        var (connection, transaction, dataSourceConfig, queryBuilderProvider) = databaseService.ConnectDataSource(appContext);

        using (connection)
        using (transaction)
        {
            var hasDocRights = await adosDocPermissionService.CheckDocRights(
                appContext,
                document.DocumentId,
                connection,
                transaction,
                dataSourceConfig,
                queryBuilderProvider
            );

            if (hasDocRights == 0)
            {
                // Example of using logger in singleton service to log important information (missing document access rights in this case) before throwing an exception that can be handled by the caller (TaskLibraryHandler) to return appropriate response to frontend
                logger.LogError("Document cannot be accessed due to missing rights.");

                // Example of throwing EBResponseException with specific status code that can be handled by TaskLibraryHandler to return predictable response to frontend
                throw new EBResponseException("Document cannot be accessed due to missing rights.", 403);
            }

            // Example of using the getDBProvider function to get the right database provider based on the datasource type and use it to execute queries or commands against the datasource in this singleton service method
            var databaseProvider = getDBProvider(dataSourceConfig.Type);

            // so with databaseProvider and databaseService, you can do the same work as you do in TaskLibraryHandler
            // like Query<...DTO>, ExecuteSQLStep, or use defined eb-query and eb-mutation in configs/sql.*.yml

            // only use try/catch with transaction.commit when there are changes which are need to be committed.
            try
            {
                transaction.Commit();
            }
            catch (Exception ex)
            {
                logger.LogError(ex, "Error occurred while building DB document raw content.");
                transaction.Rollback();
                throw;
            }
        }
    }

    public bool IsDbDocument(DocumentEntryDto document)
    {
        // See how to get constant config values in singleton service constructor parameters and methods here:
        if (document.FileTypeIdx == appConfigsService.ConstantConfigs[EB.App.Name].GetInt(EB.Constant.DOK_DATEITYP_IDX.PDF))
        {
            return false;
        }

        return true;
    }

    // always use this IDisposable pattern
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    protected virtual void Dispose(bool disposing)
    {
        if (disposing)
        {
            // just ignore
        }
    }
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

## Reliability Guardrails

- Always pass `EBuilderAppContext` from caller into service methods; do not try to store ambient app context singleton state.
- Use `databaseService.ConnectDataSource(appContext)` instead of re-implementing datasource connection logic.
- Apply transaction commit/rollback only around write flows.
- Throw `EBResponseException` for predictable frontend-facing failures when status codes matter.
- Use logging at decision boundaries and failure points.

## Anti-Patterns

- Putting all handler code into a giant singleton with no clear bounded responsibilities.
- Introducing singleton state that is request-specific or user-specific.
- Hiding task-level contract validation inside deep service layers.
- Rebuilding connection/transaction helpers that already exist in engine services.

## Checklist

- Service boundary is justified by reuse/complexity.
- Service interface is focused on business capabilities, not transport concerns.
- Handler remains small and contract-focused.
- App context is passed explicitly.
- Exceptions and status semantics are intentional and consistent.
