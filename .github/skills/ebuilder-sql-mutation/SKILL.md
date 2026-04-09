---
name: ebuilder-sql-mutation
description: 'Generation playbook for mutation-* definitions in configs/sql.*.yml. USE FOR: generate mutation-* YAML, design multi-step orchestration, configure transaction isolation, save data at each step, implement error handling, and return custom responses. DO NOT USE FOR: query authoring (use ebuilder-sql-query) or symbol reference (use ebuilder-eb-sql-symbols).'
---

# eBuilder Mutation Generation Playbook

## Purpose

Use this skill when generating or reviewing **`mutation-*` definitions** in `configs/sql.*.yml`. Mutations are **data-modifying** operations that:

- execute **one or multiple steps** in sequence
- operate within **transactions** with isolation levels
- **save outputs** at each step for downstream use
- support **guard checks** (before/after each step)
- return **custom response** with status code and body
- enable **side effects** (tasks, callbacks) on success/error

## Scope

- File target: `configs/sql.*.yml`
- Root key: `mutation` (not `query`)
- Mutation names must match: `mutation-[a-zA-Z0-9-]+`
- Each mutation executes **one or more steps** orchestrated in a chain
- Supports transactions with rollback on error
- Steps can call SQL, tasks, or other mutations

## Generation Order

1. Choose `mutation` root key.
2. Define unique mutation name: `mutation-<action>-<entity>`.
3. Define `inputParams` with types and rules.
4. Set `datasource` and `authorize` when needed.
5. Choose `transactionIsolationLevel` (default, read-committed, repeatable-read, serializable).
6. Build `steps` array with ordered operations:
   - Each step has `key`, `command` (or task/mutation call), and `save` strategy.
   - Add `checksBefore`/`checksAfter` for guards.
   - Add `catchErrors` for error mapping.
7. Define `response` with `statusCode` and `body`.
8. Add `on.success` and `on.error` callbacks if needed.
9. Validate all keys, naming, and cross-references.

## Mutation Blueprint

```yaml
mutation:
  mutation-update-item:
    datasource: main
    authorize:
      policy: eClient

    inputParams:
      itemId:
        type: int
        rules:
          - required: true
      title:
        type: string
        rules:
          - required: true

    transactionIsolationLevel: read-committed

    steps:
      - key: validateItem
        query: query-item-exists
        queryParams:
          inputParams:
            itemId: ${itemId}
        checksBefore:
          - if: ${{ ${validateItem} !== null }}
            elseThrowErrorCode: 404

      - key: updateItem
        command: |
          UPDATE ec_item
          SET title = ${title}, updated_at = $EB_DATETIME_NOW
          WHERE id_item = ${itemId}

      - key: result
        command: |
          SELECT id_item AS "itemId", title, updated_at AS "updatedAt"
          FROM ec_item
          WHERE id_item = ${itemId}
        save: first-row-as-object

    response:
      statusCode: 200
      body:
        item: ${result}
```

## Mutation Object Properties

| Property                    | Type                    | Required | Notes                                                           |
| --------------------------- | ----------------------- | -------- | --------------------------------------------------------------- |
| `inputParams`               | object                  | No       | Parameter schema for request input.                             |
| `datasource`                | string                  | No       | Target datasource key.                                          |
| `authorize`                 | object or array         | No       | Security policy/roles/permissions.                              |
| `transactionIsolationLevel` | enum or engine object   | No       | `default`, `read-committed`, `repeatable-read`, `serializable`. |
| `steps`                     | array of `sqlStep`      | No       | Ordered mutation orchestration.                                 |
| `command`                   | string or engine object | No       | Direct mutation command (for simple single-command mutations).  |
| `response`                  | object                  | No       | Custom `statusCode`, `message`, `body`.                         |
| `placeholders`              | object                  | No       | Named SQL fragments for `$EB_PLACEHOLDER(...)`.                 |
| `checksBefore`              | array                   | No       | Guard checks before mutation execution.                         |
| `checksAfter`               | array                   | No       | Guard checks after mutation execution.                          |
| `catchErrors`               | object                  | No       | Error-code to response mapping.                                 |
| `on`                        | object                  | No       | `success` and `error` callbacks.                                |

## Transaction Isolation Levels

Choose isolation level to control data consistency and concurrency:

| Level             | PostgreSQL      | Oracle         | MSSQL           | MySQL           | Use Case                                 |
| ----------------- | --------------- | -------------- | --------------- | --------------- | ---------------------------------------- |
| `default`         | READ COMMITTED  | SERIALIZABLE   | READ COMMITTED  | REPEATABLE READ | Default engine behavior; auto-chosen.    |
| `read-committed`  | READ COMMITTED  | READ COMMITTED | READ COMMITTED  | READ COMMITTED  | Most common; dirty read prevention.      |
| `repeatable-read` | REPEATABLE READ | SERIALIZABLE   | REPEATABLE READ | REPEATABLE READ | Phantom read prevention.                 |
| `serializable`    | SERIALIZABLE    | SERIALIZABLE   | SERIALIZABLE    | SERIALIZABLE    | Strongest isolation; lowest concurrency. |

```yaml
mutation:
  mutation-transfer-funds:
    transactionIsolationLevel: serializable
    steps:
      - key: debit
        command: |
          UPDATE accounts
          SET balance = balance - ${amount}
          WHERE id = ${fromId}

      - key: credit
        command: |
          UPDATE accounts
          SET balance = balance + ${amount}
          WHERE id = ${toId}
```

## SQL Steps

Each step in a mutation defines one operation:

```yaml
steps:
  - key: uniqueStepId
    command: |
      UPDATE items SET status = 'processed'
      WHERE id = ${itemId}
    save: scalar
    checksBefore:
      - if: ${{ ${itemId} > 0 }}
        elseThrowErrorCode: 400
```

### Step Properties

| Property         | Type                    | Required | Notes                                                                                                         |
| ---------------- | ----------------------- | -------- | ------------------------------------------------------------------------------------------------------------- |
| `key`            | string                  | Yes      | Unique step id in chain.                                                                                      |
| `command`        | string or engine object | No       | SQL command for step.                                                                                         |
| `save`           | enum                    | No       | `scalar`, `last-insert-id`, `first-row`, `first-row-as-object`, `all-rows`, `column-values`, `output-params`. |
| `extra`          | object                  | No       | Save-mode options (`identityCol` for `last-insert-id`, `column` for `column-values`).                         |
| `checksBefore`   | array                   | No       | Guard checks before step execution.                                                                           |
| `checksAfter`    | array                   | No       | Guard checks after step execution.                                                                            |
| `catchErrors`    | object                  | No       | Error-code to response mapping.                                                                               |
| `defaultResult`  | any                     | No       | Result when checks fail and no throw.                                                                         |
| `omitResult`     | boolean                 | No       | Exclude step output from downstream context.                                                                  |
| `task`           | string                  | No       | Call eBuilder task.                                                                                           |
| `taskParams`     | any                     | No       | Params for `task`.                                                                                            |
| `mutation`       | string                  | No       | Call eBuilder mutation.                                                                                       |
| `mutationParams` | any                     | No       | Params for `mutation`.                                                                                        |
| `query`          | string                  | No       | Call eBuilder query.                                                                                          |
| `queryParams`    | object                  | No       | Query execution options.                                                                                      |
| `placeholders`   | object                  | No       | Per-step placeholder dictionary.                                                                              |
| `jsExpression`   | string                  | No       | Compute result via JS expression.                                                                             |
| `emitResult`     | boolean                 | No       | Emit step result in response/event context.                                                                   |
| `log`            | object                  | No       | Step entry logging definition.                                                                                |
| `datasource`     | string                  | No       | Optional datasource override.                                                                                 |
| `subSteps`       | array of `sqlStep`      | No       | Nested step chain (sub-orchestration).                                                                        |

## Save Strategies

Control how each step's result is captured for downstream steps:

| Strategy              | Result Type   | Use Case                                                 | Example                              |
| --------------------- | ------------- | -------------------------------------------------------- | ------------------------------------ |
| `scalar`              | Single value  | Single-value result (count, sum, id).                    | `SELECT COUNT(*) ...`                |
| `last-insert-id`      | Single id     | Insert operation; capture generated id.                  | Requires `extra.identityCol`         |
| `first-row`           | Object (flat) | Capture first row columns as named properties.           | `SELECT id, name FROM ... LIMIT 1`   |
| `first-row-as-object` | Object        | Same as `first-row` but emphasizes object semantics.     | `SELECT * FROM ... LIMIT 1`          |
| `all-rows`            | Array[object] | Capture all result rows as array.                        | `SELECT id, name FROM ...`           |
| `column-values`       | Array[value]  | Capture single column as array. Requires `extra.column`. | `SELECT id FROM items` → `[1, 2, 3]` |
| `output-params`       | Object        | For stored procedures; capture OUT parameters.           | SQL Server `sp_` procedures          |

### Examples

#### `scalar` - Single Value

```yaml
steps:
  - key: itemCount
    command: SELECT COUNT(*) FROM items WHERE owner_id = ${userId}
    save: scalar
    # Result: ${itemCount} = 5
```

#### `last-insert-id` - Insert and Capture ID

```yaml
steps:
  - key: createItem
    command: |
      INSERT INTO items (title, owner_id)
      VALUES (${title}, $EB_USER_ID)
    save: last-insert-id
    extra:
      identityCol: id
    # Result: ${createItem} = 12345 (new id)
```

#### `first-row` - Column Mapping

```yaml
steps:
  - key: getItem
    command: SELECT id, title, price FROM items WHERE id = ${itemId}
    save: first-row
    # Result:
    # - ${getItem___id}
    # - ${getItem___title}
    # - ${getItem___price}
```

#### `first-row-as-object` - Object Result

```yaml
steps:
  - key: userData
    command: |
      SELECT id, first_name AS "firstName", email
      FROM users
      WHERE id = $EB_USER_ID
    save: first-row-as-object
    # Result: ${userData} = { id: 1, firstName: "John", email: "john@..." }
```

#### `all-rows` - Array Results

```yaml
steps:
  - key: allItems
    command: SELECT id, title FROM items WHERE owner_id = ${userId}
    save: all-rows
    # Result: ${allItems} = [{ id: 1, title: "A" }, { id: 2, title: "B" }]
```

#### `column-values` - Single Column as Array

```yaml
steps:
  - key: itemIds
    command: SELECT id FROM items WHERE owner_id = ${userId}
    save: column-values
    extra:
      column: id
    # Result: ${itemIds} = [1, 2, 3]
```

## Step Orchestration Patterns

### Pattern 1: Validate → Modify → Return

```yaml
steps:
  - key: validateUser
    query: query-user-exists
    queryParams:
      inputParams:
        userId: ${userId}
    checksBefore:
      - if: ${{ ${validateUser} !== null }}
        elseThrowErrorCode: 404

  - key: updateUser
    command: |
      UPDATE users
      SET last_login = $EB_DATETIME_NOW
      WHERE id = ${userId}

  - key: result
    query: query-user-profile
    queryParams:
      inputParams:
        userId: ${userId}
    save: first-row-as-object
```

### Pattern 2: Multi-Table Insert with Foreign Keys

```yaml
steps:
  - key: createOrder
    command: |
      INSERT INTO orders (customer_id, created_at)
      VALUES (${customerId}, $EB_DATETIME_NOW)
    save: last-insert-id
    extra:
      identityCol: id

  - key: addLineItem1
    command: |
      INSERT INTO order_items (order_id, product_id, quantity, price)
      VALUES (${createOrder}, ${product1Id}, ${qty1}, ${price1})

  - key: addLineItem2
    command: |
      INSERT INTO order_items (order_id, product_id, quantity, price)
      VALUES (${createOrder}, ${product2Id}, ${qty2}, ${price2})

  - key: summary
    command: |
      SELECT id, customer_id, COUNT(*) AS item_count
      FROM orders o
      LEFT JOIN order_items oi ON o.id = oi.order_id
      WHERE o.id = ${createOrder}
      GROUP BY o.id
    save: first-row-as-object
```

### Pattern 3: Call Task Between Steps

```yaml
steps:
  - key: insertRecord
    command: |
      INSERT INTO audit_log (action, user_id, details)
      VALUES ('ITEM_CREATED', $EB_USER_ID, ${description})
    save: last-insert-id
    extra:
      identityCol: id

  - key: sendNotification
    task: task-send-email
    taskParams:
      recipientId: ${userId}
      subject: Item created

  - key: result
    command: SELECT id FROM audit_log WHERE id = ${insertRecord}
    save: first-row-as-object
```

### Pattern 4: Conditional Steps with Checks

```yaml
steps:
  - key: checkInventory
    command: |
      SELECT quantity FROM inventory
      WHERE product_id = ${productId}
    save: first-row

  - key: reduceInventory
    command: |
      UPDATE inventory
      SET quantity = quantity - ${orderQty}
      WHERE product_id = ${productId}
    checksBefore:
      - if: ${{ ${checkInventory___quantity} >= ${orderQty} }}
        elseThrowErrorCode: 409
    checksAfter:
      - if: ${{ ${reduceInventory} > 0 }}
        log:
          level: information
          message: inventory_reduced

  - key: result
    command: SELECT quantity FROM inventory WHERE product_id = ${productId}
    save: first-row-as-object
```

## Guard Checks

### `checksBefore` - Validate Before Step

```yaml
steps:
  - key: deleteItem
    command: DELETE FROM items WHERE id = ${itemId}
    checksBefore:
      - if: ${{ ${itemId} > 0 && $EB_CONTEXT(user.roles)['admin'] }}
        elseThrowErrorCode: 403
        elseLog:
          level: warning
          message: unauthorized_delete_attempt
```

### `checksAfter` - Validate After Step

```yaml
steps:
  - key: updateStatus
    command: |
      UPDATE items SET status = 'processed'
      WHERE id = ${itemId}
    save: scalar
    checksAfter:
      - if: ${{ ${updateStatus} > 0 }}
        log:
          level: debug
          message: update_successful
        elseLog:
          level: warning
          message: no_rows_updated
```

## Error Handling

### `catchErrors` - Map Error Codes

```yaml
steps:
  - key: insertUnique
    command: |
      INSERT INTO users (email, name)
      VALUES (${email}, ${name})
    catchErrors:
      2627: # SQL Server unique constraint
        statusCode: 409
        message: EMAIL_EXISTS
      23505: # PostgreSQL unique constraint
        statusCode: 409
        message: EMAIL_EXISTS
```

### Response Error Mapping

```yaml
mutation:
  mutation-create-item:
    steps:
      - key: create
        command: INSERT INTO items (title) VALUES (${title})
        save: last-insert-id
        extra:
          identityCol: id

    response:
      statusCode: 201
      body:
        itemId: ${create}

    catchErrors:
      1:
        statusCode: 500
        body:
          message: database_error
```

## Response Definition

Define custom response for mutation result:

```yaml
response:
  statusCode: 201
  message: Item created successfully
  body:
    itemId: ${create}
    title: ${getItem___title}
    createdAt: $EB_DATETIME_NOW
```

## Authorization

Control mutation access:

```yaml
mutation:
  mutation-delete-item:
    authorize:
      policy: eClient
    # OR by role
    authorize:
      - role: admin
      - role: owner

    steps:
      - key: delete
        command: DELETE FROM items WHERE id = ${itemId}
```

## Callbacks (After-Mutation Actions)

Execute tasks or mutations after mutation succeeds or fails:

```yaml
mutation:
  mutation-process-order:
    steps:
      - key: processStep
        command: UPDATE orders SET status = 'processing' WHERE id = ${orderId}

    on:
      success:
        - task: task-notify-customer
          params:
            orderId: ${orderId}
        - task: task-log-activity
          params:
            action: order_processed

      error:
        - task: task-alert-admin
          checks:
            - if: ${{ ${errorCode} === 500 }}
          params:
            errorCode: ${errorCode}
            orderId: ${orderId}
```

## Inline JavaScript Patterns

### `jsExpression` - Compute Step Result

```yaml
steps:
  - key: pricing
    jsExpression: |
      ${{
        const basePrice = ${baseAmount};
        const discount = $EB_CONTEXT(user.discountPercent) || 0;
        const tax = 0.10;
        
        const finalPrice = basePrice * (1 - discount / 100) * (1 + tax);
        
        return {
          basePrice,
          discount,
          tax,
          finalPrice
        };
      }}
```

Rules:

- Always provide **multi-line function body** string.
- Always include explicit **`return`** statement.
- Keep logic **deterministic** and **side-effect free**.

## Deterministic Checklist

Before finalizing a mutation:

1. ✅ Mutation name matches `^mutation-[a-zA-Z0-9-]+$` pattern.
2. ✅ `datasource` references valid entry in `configs/app.yml`.
3. ✅ All `inputParams` have `type` and `rules` defined.
4. ✅ `transactionIsolationLevel` is one of: `default`, `read-committed`, `repeatable-read`, `serializable`.
5. ✅ Every step has unique `key`; keys don't collide with input param names.
6. ✅ All user inputs in `command` are parameter-bound (`${paramName}`).
7. ✅ `save` strategy is valid (`scalar`, `last-insert-id`, `first-row`, etc).
8. ✅ `first-row` steps reference step outputs with format `${stepKey___columnName}`.
9. ✅ `last-insert-id` steps include `extra.identityCol`.
10. ✅ `column-values` steps include `extra.column`.
11. ✅ `checksBefore`/`checksAfter` use `if` instead of deprecated `param/op/value`.
12. ✅ `response` includes `statusCode` and valid `body` structure.
13. ✅ `authorize` policy name exists in `configs/security.yml`.
14. ✅ Called `task`, `mutation`, `query` names actually exist.
15. ✅ `catchErrors` maps only expected error codes.
16. ✅ SQL uses `$EB_*` symbols where available for portability.
17. ✅ No deprecated keys (`param`, `op`, `value`, `throw`, `silentThrow`).

## Do Not Generate

- ❌ Query operations (use `ebuilder-sql-query`).
- ❌ Unknown top-level keys outside `mutation`.
- ❌ Invalid mutation name patterns.
- ❌ Raw SQL interpolation from unsanitized input.
- ❌ Steps without `key` or with colliding keys.
- ❌ `save` strategy for steps without result data.
- ❌ Ambiguous column references in multi-row results.
