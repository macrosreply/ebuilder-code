---
name: ebuilder-eb-sql-symbols
description: 'eBuilder SQL symbol reference and engine-aware SQL patterns. USE FOR: understanding portable $EB_* symbols, cross-database compatibility, placeholders for engine-specific SQL, parameter binding, and datasource/authorization configuration. DO NOT USE FOR: query-specific or mutation-specific YAML authoring (use ebuilder-sql-query or ebuilder-sql-mutation instead).'
---

# eBuilder SQL Symbols and Engine-Aware Patterns

## Purpose

Use this skill to understand and apply **portable SQL symbols** and **engine-aware patterns** in eBuilder configurations. This ensures SQL logic is:

- **portable** across PostgreSQL, Oracle, MSSQL, MySQL
- **safe** with proper parameter binding
- **deterministic** with schema validation

## Scope

- eBuilder SQL symbols: `$EB_*` prefix
- Engine-aware SQL patterns and placeholders
- Parameter binding and datasource/authorization configuration
- Cross-database compatibility principles

## eBuilder SQL Symbols Reference

These symbols are critical for portability across database engines. Prefer them over engine-specific raw SQL where possible.

| Symbol                                          | Purpose                                                                            | Example Usage                                        |
| ----------------------------------------------- | ---------------------------------------------------------------------------------- | ---------------------------------------------------- |
| `${variable_name}`                              | Parameter binding placeholder. Always use for user input to prevent SQL injection. | `WHERE email = ${email}`                             |
| `$EB_CONSTANT(path)`                            | Read value from constants at runtime.                                              | `$EB_CONSTANT(VKB.ITEM_TYPE_IDX_CASE)`               |
| `$EB_CONTEXT(path)`                             | Read value from request/user/runtime context.                                      | `$EB_CONTEXT(user.id)`, `$EB_CONTEXT(language)`      |
| `$EB_PLACEHOLDER(name)`                         | Inject conditional or engine-specific SQL fragments from `placeholders`.           | `$EB_PLACEHOLDER(adminClause)`                       |
| `$EB_EXEC ...`                                  | Execute stored procedure/function pattern in portable form.                        | `$EB_EXEC sp_GetItem ${itemId}`                      |
| `$EB_IN_ARRAY_ARG(col, ${arr})`                 | Portable `IN (...)` handling for array args.                                       | `WHERE id IN $EB_IN_ARRAY_ARG(id, ${ids})`           |
| `$EB_DATETIME_NOW`                              | Current datetime across DB engines.                                                | `$EB_DATETIME_NOW`                                   |
| `$EB_DATETIME_ADD(exp, n, type)`                | Portable datetime arithmetic.                                                      | `$EB_DATETIME_ADD($EB_DATETIME_NOW, -7, DAY)`        |
| `$EB_GET_DATE(exp)`                             | Date extraction function abstraction.                                              | `$EB_GET_DATE(created_at)`                           |
| `$EB_DATETIME_TO_ISO_STRING(exp)`               | DateTime to ISO text conversion.                                                   | `$EB_DATETIME_TO_ISO_STRING(updated_at)`             |
| `$EB_IF_NULL(a, b)` / `$EB_COALESCE(...)`       | Null fallback logic.                                                               | `$EB_COALESCE(status, 'pending')`                    |
| `$EB_TO_LOWER(exp)` / `$EB_TO_UPPER(exp)`       | Case conversion abstraction.                                                       | `WHERE $EB_TO_LOWER(email) = $EB_TO_LOWER(${email})` |
| `$EB_STR_CONCAT` / `$EB_STRING_CONCAT(...)`     | String concatenation abstraction (`$EB_STRING_CONCAT` preferred).                  | `$EB_STRING_CONCAT(first_name, ' ', last_name)`      |
| `$EB_JSON_ARRAY_AGG(col ...)`                   | Portable JSON array aggregation.                                                   | `$EB_JSON_ARRAY_AGG(DISTINCT tags)` in GROUP BY      |
| `$EB_FORWARD_FILTER(field, expr)`               | Bridge REST query filters to SQL predicates.                                       | `$EB_FORWARD_FILTER(status, filter.status)`          |
| `$EB_IF_DEFINED(...)` / `$EB_IF_UNDEFINED(...)` | Conditional SQL fragment inclusion by param presence.                              | `$EB_IF_DEFINED(${searchTerm}): AND title LIKE ...`  |
| `$EB_IS_EMPTY_ARRAY(...)`                       | Conditional SQL fragment inclusion for empty arrays.                               | `$EB_IS_EMPTY_ARRAY(${tags}): AND tags IS NULL`      |
| `$EB_USER_ID` / `$EB_USER_LANG`                 | Current user metadata abstraction.                                                 | `WHERE owner_id = $EB_USER_ID`                       |
| `$EB_SEQ_NEXT(seq)`                             | Portable sequence/next-id expression.                                              | `$EB_SEQ_NEXT(item_id_seq)`                          |

## Symbol Usage Rules

- **Prefer `$EB_STRING_CONCAT(...)`** over `$EB_STR_CONCAT` for best cross-DB compatibility.
- **Use `$EB_PLACEHOLDER(...)`** when SQL text differs by `postgresql`/`oracle`/`mssql`/`mysql`.
- **Use `$EB_CONSTANT(...)`** and **`$EB_CONTEXT(...)`** instead of hardcoding deployment/environment values.
- **Use `$EB_USER_ID`** to safely reference current user without exposing implementation details.
- **Avoid engine-specific functions** when portable `$EB_*` symbols exist.

## Engine-Aware SQL Patterns

For `command` and `placeholders`, use either:

### Single SQL String

When SQL is identical across all engines:

```yaml
command: |
  SELECT id, name
  FROM items
  WHERE status = ${status}
```

### Engine-Target Object

When SQL differs by database engine:

```yaml
command:
  default: SELECT * FROM items WHERE id = ${id}
  postgresql: SELECT * FROM items WHERE id = ${id}::int
  oracle: SELECT * FROM items WHERE id = TO_NUMBER(${id})
  mssql: SELECT * FROM items WHERE id = CAST(${id} AS INT)
  mysql: SELECT * FROM items WHERE id = CAST(${id} AS SIGNED)
```

### Placeholders for Conditional SQL

Use `$EB_PLACEHOLDER(...)` to inject engine-specific or conditional SQL fragments:

```yaml
placeholders:
  adminClause:
    if: Boolean($EB_CONTEXT(user.roles)['admin'])
    default: AND is_admin = 1
  limitClause:
    default: LIMIT 100
    postgresql: LIMIT 100
    oracle: FETCH FIRST 100 ROWS ONLY
    mysql: LIMIT 100
```

Then reference in command:

```yaml
command: |
  SELECT * FROM items
  WHERE status = 'active'
  $EB_PLACEHOLDER(adminClause)
  $EB_PLACEHOLDER(limitClause)
```

## Parameter Binding Safety

**Always bind user-provided values via parameters:**

```yaml
# ✅ SAFE - parameter binding
command: |
  SELECT * FROM items
  WHERE email = ${email}

# ❌ UNSAFE - string interpolation
command: |
  SELECT * FROM items
  WHERE email = '${email}'
```

**Apply type constraints in `inputParams`:**

```yaml
inputParams:
  email:
    type: string
    rules:
      - required: true
  itemId:
    type: int
    rules:
      - required: true
  tags:
    type: array
    items:
      type: string
    rules:
      - required: false
```

## Configuration Properties - Common to All SQL

### Root Object

| Property   | Type   | Required | Notes                                       |
| ---------- | ------ | -------- | ------------------------------------------- |
| `query`    | object | No       | Keys must match `^query-[a-zA-Z0-9-]+$`.    |
| `mutation` | object | No       | Keys must match `^mutation-[a-zA-Z0-9-]+$`. |

### Shared Properties (Query & Mutation)

| Property       | Type            | Required | Notes                                                       |
| -------------- | --------------- | -------- | ----------------------------------------------------------- |
| `inputParams`  | object          | No       | Parameter schema for request input.                         |
| `datasource`   | string          | No       | Target datasource key (e.g., `main`, `reporting`, `cache`). |
| `authorize`    | object or array | No       | Security policy/roles/permissions.                          |
| `placeholders` | object          | No       | Named SQL fragments for `$EB_PLACEHOLDER(...)`.             |
| `on`           | object          | No       | `success` and `error` callbacks.                            |

### Input Param Schema

| Property      | Type   | Notes                                                                                            |
| ------------- | ------ | ------------------------------------------------------------------------------------------------ |
| `description` | string | Parameter description.                                                                           |
| `type`        | enum   | `int`, `decimal`, `number`, `string`, `bool`, `dateTime`, `datetime`, `array`, `object`, `uuid`. |
| `items`       | object | Nested schema for `array` item type.                                                             |
| `properties`  | object | Nested schema for `object` members.                                                              |
| `rules`       | array  | Includes `required`, `nullable`, `enum`, `enumFromConstant`.                                     |

### SQL Step Common Properties

| Property       | Type                    | Required | Notes                                                         |
| -------------- | ----------------------- | -------- | ------------------------------------------------------------- |
| `key`          | string                  | Yes      | Unique step id in chain.                                      |
| `datasource`   | string                  | No       | Optional datasource override.                                 |
| `command`      | string or engine object | No       | SQL command for step (single string or engine-target object). |
| `placeholders` | object                  | No       | Per-step placeholder dictionary.                              |
| `checksBefore` | array                   | No       | Guard checks before execution.                                |
| `checksAfter`  | array                   | No       | Guard checks after execution.                                 |
| `catchErrors`  | object                  | No       | Error-code to response mapping.                               |
| `log`          | object                  | No       | Step entry logging definition.                                |

### Check Object (`checksBefore` / `checksAfter` item)

| Property             | Type   | Required | Notes                                                    |
| -------------------- | ------ | -------- | -------------------------------------------------------- |
| `if`                 | string | No       | Preferred conditional expression (JS expression string). |
| `elseThrowErrorCode` | number | No       | Error code when check fails.                             |
| `log`                | object | No       | Log when condition is true.                              |
| `elseLog`            | object | No       | Log when condition is false.                             |

### SQL Step Log

| Property  | Type   | Required | Notes                                                            |
| --------- | ------ | -------- | ---------------------------------------------------------------- |
| `level`   | enum   | Yes      | `trace`, `debug`, `information`, `warning`, `error`, `critical`. |
| `message` | string | No       | Log message.                                                     |
| `data`    | object | No       | Structured log payload.                                          |

## Cross-Database Compatibility Examples

### Example 1: Datetime Handling

```yaml
# ✅ Portable across all engines
command: |
  SELECT id, created_at
  FROM items
  WHERE created_at >= $EB_DATETIME_ADD($EB_DATETIME_NOW, -7, DAY)
```

### Example 2: Null Coalescing

```yaml
# ✅ Portable across all engines
command: |
  SELECT 
    id,
    $EB_COALESCE(nickname, first_name, 'Unknown') AS display_name
  FROM users
```

### Example 3: String Concatenation

```yaml
# ✅ Portable across all engines
command: |
  SELECT 
    id,
    $EB_STRING_CONCAT(first_name, ' ', last_name) AS full_name
  FROM users
```

### Example 4: Case-Insensitive Search

```yaml
# ✅ Portable across all engines
command: |
  SELECT id, email
  FROM users
  WHERE $EB_TO_LOWER(email) = $EB_TO_LOWER(${searchEmail})
```

### Example 5: Array Handling

```yaml
# ✅ Portable across all engines
inputParams:
  statusList:
    type: array
    items:
      type: string

command: |
  SELECT * FROM items
  WHERE status IN $EB_IN_ARRAY_ARG(status, ${statusList})
```

## Safety Checklist

1. ✅ All user inputs bound via `inputParams` parameters.
2. ✅ No raw string interpolation in SQL `command`.
3. ✅ Engine-specific SQL uses `engine-target` object shape or `$EB_PLACEHOLDER`.
4. ✅ `$EB_*` symbols used where available for cross-DB compatibility.
5. ✅ Parameterized types match actual input parameter types.
6. ✅ Authorization policy names align with `configs/security.yml`.
7. ✅ Datasource keys reference valid entries in `configs/app.yml`.

## Do Not Do

- ❌ Hardcode environment values—use `$EB_CONSTANT(...)` or `$EB_CONTEXT(...)`.
- ❌ Concatenate user input into SQL—always use parameters.
- ❌ Mix engine-specific raw SQL without using placeholders or engine-target objects.
- ❌ Ignore `$EB_*` symbols when equivalent portable solutions exist.
