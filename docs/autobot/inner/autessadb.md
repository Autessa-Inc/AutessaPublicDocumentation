---
# Using AutessaDB API

  The AutessaDB API is a flexible client that allows you to interact with your database outside of the Autessa environment.

## API Specs

APIs within AutessaDB are table-specific: each API key can only access the details of the table it was generated for.

### Authorization Header
Include your API key in the `Authorization` header:
  ```json
  {
    "Authorization": "Bearer <API_KEY>"
  }
  ```

---

## Supported Column Types
- **UNIQUE_ID**: System-generated unique identifier for each record.
- **STRING**: Standard string data.
- **LONG_TEXT**: Extended text for large content (e.g., articles).
- **INTEGER**: Integer values.
- **FLOAT**: Floating-point numbers.
- **BOOLEAN**: True/false values.
- **TIMESTAMP**: System-generated date/time values.
- **UNKNOWN**: Fallback type, generally treated as string.

---

## Example Table Schema

A sample table `people` with ID `123-345-12312-321` with parent relationships:

| uniqueId | createdAt               | lastUpdated            | personName | parentOneId | parentTwoId |
|---------:|-------------------------|------------------------|------------|------------:|------------:|
| 1        | 2025-01-01T12:00:00Z    | 2025-01-02T08:30:00Z   | Alice      | 2           | 3           |
| 2        | 2020-05-10T15:20:00Z    | 2025-01-02T09:00:00Z   | Bob        |              |             |
| 3        | 2020-06-12T10:00:00Z    | 2025-01-02T09:05:00Z   | Carol      |              |             |

---

## Object Definitions

### Pagination
```json
{
  "currentPage": <number>,         // zero-based index
  "totalRecordsPerPage": <number>, // between 10 and 50
  "ascending": <boolean>
}
```

### SelectedColumn
```json
{
  "tableId": "<string>",
  "tableAlias": "<string|null>",
  "columnName": "<string>",
  "columnAlias": "<string|null>",
  "functions": [<string>]         // e.g., ["LENGTH", "SUM"]
}
```

### WhereFilter
```json
{
  "operand": "AND" | "OR",
  "groups": [
    {
      "operand": "AND" | "OR",
      "statements": [
        {
          "operator": "EQUALS" | "GREATER_THAN_OR_EQUALS" | ...,
          "column": <SelectedColumn>,
          "value": <any>            // value type depends on operator
        }
      ]
    }
  ]
}
```

### JoinStatement
```json
{
  "joinType": "INNER" | "LEFT" | "RIGHT" | "FULL" | "CROSS",
  "joinTableAlias": "<string>",
  "mainTableColumn": "<string>",
  "joinTableId": "<string>",
  "joinTableColumn": "<string>",
  "joinFilters": [<WhereFilter.statements>]
}
```

### ColumnValueMapping
Use for creating or updating records:
```json
{
  "tableName": "<string>",
  "tableId": "<string>",
  "columnName": "<string>",
  "value": {
    "columnType": "<STRING|INTEGER|...>",
    "value": <any>
  }
}
```

---

## API Requests & Examples

### API - View All Tables
```json
{
  "requestType": "VIEW_TABLES",
  "tableId": "123-345-12312-321"
}
```
**Response:**
```json
{
  "tables": [
    {
      "tableName": "<string>",
      "tableId": "<string>",
      "relatedTables": [
        {
          "relatedTableId": "<string>",
          "relatedTableName": "<string>",
          "relatedTableColumnMapping": [
            {
              "currentTableColumnName": "<string>",
              "relatedTableColumnName": "<string>"
            }
          ]
        }
      ],
      "schema": [
        {
          "tableName": "<string>",
          "tableId": "<string>",
          "columns": [
            {
              "unique_id": "<string>",
              "columnType": "UNIQUE_ID",
              "allowNull": false,
              "defaultValue": 0,
              "foreignKeyDefinition": {
                "relatedTableId": "<string>",
                "relatedColumnName": "<string>"
              }
            }
          ]
        }
      ]
    }
  ]
}
```
**Example:**
```bash
curl -X POST https://api.autessa.com \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "requestType": "VIEW_TABLES",
    "tableId": "123-345-12312-321"
  }'
```

### API - View Table Schema
```json
{
  "requestType": "VIEW_SCHEMA",
  "tableId": "123-345-12312-321"
}
```
**Response:**
```json
{
  "tableName": "<string>",
  "tableId": "<string>",
  "columns": [
    {
      "unique_id": "<string>",
      "columnType": "UNIQUE_ID",
      "allowNull": false,
      "defaultValue": 0,
      "foreignKeyDefinition": {
        "relatedTableId": "<string>",
        "relatedColumnName": "<string>"
      }
    }
  ]
}
```
**Example:**
```bash
curl -X POST https://api.autessa.com \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "requestType": "VIEW_SCHEMA",
    "tableId": "123-345-12312-321"
  }'
```

### API - View Table Data
```json
{
  "requestType": "DEFAULT_TABLE_VIEW",
  "tableId": "123-345-12312-321",
  "pagination": {
    "currentPage": 0,
    "totalRecordsPerPage": 10,
    "ascending": true
  }
}
```
**Response:**
```json
{
  "tableSchema": {
    "tableName": "<string>",
    "tableId": "<string>",
    "columns": [
      {
        "unique_id": "<string>",
        "columnType": "UNIQUE_ID",
        "allowNull": false,
        "defaultValue": 0,
        "foreignKeyDefinition": {
          "relatedTableId": "<string>",
          "relatedColumnName": "<string>"
        }
      }
    ]
  },
  "table": [
    {
      "uniqueIds": [
        {
          "tableId": "<string>",
          "uniqueId": 0
        }
      ],
      "values": [
        {
          "tableName": "<string>",
          "tableId": "<string>",
          "columnName": "<string>",
          "value": {
            "columnType": "<string>",
            "value": "<object>"
          }
        }
      ]
    }
  ],
  "totalRecords": 500
}
```

**Example:**
```bash
curl -X POST https://api.autessa.com \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "requestType": "DEFAULT_TABLE_VIEW",
    "tableId": "123-345-12312-321",
    "pagination": {"currentPage":0,"totalRecordsPerPage":10,"ascending":true}
  }'
```

### API - View Record
```json
{
  "requestType": "VIEW_RECORD",
  "tableId": "123-345-12312-321",
  "uniqueId": "1"
}
```

**Response:**
```json
{
  "record": {
    "uniqueIds": [
      {
        "tableId": "<string>",
        "uniqueId": 0
      }
    ],
    "values": [
      {
        "tableName": "<string>",
        "tableId": "<string>",
        "columnName": "<string>",
        "value": {
          "columnType": "<string>",
          "value": "<object>"
        }
      }
    ]
  }
}
```

**Example:**
```bash
curl -X POST https://api.autessa.com \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "requestType": "VIEW_RECORD",
    "tableId": "123-345-12312-321",
    "uniqueId": "1"
  }'
```

### API - Query Table
```json
{
  "requestType": "QUERY_TABLE",
  "tableId": "123-345-12312-321",
  "selectedColumns": [
    {"tableId":"people","tableAlias":null,"columnName":"personName","columnAlias":"name","functions":[]}
  ],
  "filters": {
    "operand":"AND",
    "groups":[{
      "operand":"AND",
      "statements":[{
        "operator":"LIKE",
        "column":{"tableId":"people","columnName":"personName","columnAlias":null,"functions":[]},
        "value":"A%"
      }]
    }]
  },
  "pagination":{"currentPage":0,"totalRecordsPerPage":10,"ascending":true}
}
```
**Response:**
```json
{
  "table": [
    {
      "uniqueIds": [
        {
          "tableId": "<string>",
          "uniqueId": 0
        }
      ],
      "values": [
        {
          "tableName": "<string>",
          "tableId": "<string>",
          "columnName": "<string>",
          "value": {
            "columnType": "<string>",
            "value": "<object>"
          }
        }
      ]
    }
  ],
  "totalRecords": 500
}
```
**Example:**
```bash
curl -X POST https://api.autessa.com \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "requestType":"QUERY_TABLE",
    "tableId":"people",
    "selectedColumns":[{"tableId":"people","columnName":"personName","columnAlias":"name","functions":[]}],
    "filters":{"operand":"AND","groups":[{"operand":"AND","statements":[{"operator":"LIKE","column":{"tableId":"people","columnName":"personName","columnAlias":null,"functions":[]},"value":"A%"}]}]},
    "pagination":{"currentPage":0,"totalRecordsPerPage":10,"ascending":true}
  }'
```

### API - Query Compute Value
```json
{
  "requestType": "QUERY_TABLE_COMPUTE",
  "tableId": "123-345-12312-321",
  "selectedColumns": [
    {"tableId":"people","columnName":"uniqueId","functions":["COUNT"]}
  ],
  "filters": { /* same structure as Query Table */ },
  "pagination": {"currentPage":0,"totalRecordsPerPage":1,"ascending":true}
}
```
**Response:**
Same as query response
**Example:**
```bash
curl -X POST https://api.autessa.com \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "requestType":"QUERY_TABLE_COMPUTE",
    "tableId":"people",
    "selectedColumns":[{"tableId":"people","columnName":"uniqueId","functions":["COUNT"]}],
    "filters":{"operand":"AND","groups":[]},
    "pagination":{"currentPage":0,"totalRecordsPerPage":1,"ascending":true}
  }'
```

### API - Semantic Query
```json
{
  "requestType": "SEMANTIC_QUERY_TABLE",
  "tableId": "123-345-12312-321",
  "columnName": "personName",
  "query": "Alice",
  "limit": 5
}
```
**Response:**
Same as query response
**Example:**
```bash
curl -X POST https://api.autessa.com \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "requestType":"SEMANTIC_QUERY_TABLE",
    "tableId":"people",
    "columnName":"personName",
    "query":"Alice",
    "limit":5
  }'
```

### API - Join Queries
```json
{
  "requestType": "QUERY_AND_JOIN_TABLE",
  "tableId": "123-345-12312-321",
  "selectedColumns": [
    {"tableId":"people","columnName":"uniqueId","functions":[]},
    {"tableId":"people","columnName":"personName","columnAlias":"childName","functions":[]},
    {"tableId":"people","tableAlias":"p1","columnName":"personName","columnAlias":"parentOneName","functions":[]},
    {"tableId":"people","tableAlias":"p2","columnName":"personName","columnAlias":"parentTwoName","functions":[]}
  ],
  "filters": {"operand":"AND","groups":[{"operand":"AND","statements":[]}]},
  "pagination": {"currentPage":0,"totalRecordsPerPage":10,"ascending":true},
  "joinStatements": [
    {"joinType":"LEFT","joinTableAlias":"p1","mainTableColumn":"parentOneId","joinTableId":"people","joinTableColumn":"uniqueId","joinFilters":[]},
    {"joinType":"LEFT","joinTableAlias":"p2","mainTableColumn":"parentTwoId","joinTableId":"people","joinTableColumn":"uniqueId","joinFilters":[]}
  ]
}
```
**Response:**
Same as query response
**Example:**
```bash
curl -X POST https://api.autessa.com \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "requestType":"QUERY_AND_JOIN_TABLE",
    "tableId":"people",
    "selectedColumns":[
      {"tableId":"people","columnName":"uniqueId","functions":[]},
      {"tableId":"people","columnName":"personName","columnAlias":"childName","functions":[]},
      {"tableId":"people","tableAlias":"p1","columnName":"personName","columnAlias":"parentOneName","functions":[]},
      {"tableId":"people","tableAlias":"p2","columnName":"personName","columnAlias":"parentTwoName","functions":[]}
    ],
    "filters":{"operand":"AND","groups":[{"operand":"AND","statements":[]}]},
    "pagination":{"currentPage":0,"totalRecordsPerPage":10,"ascending":true},
    "joinStatements":[
      {"joinType":"LEFT","joinTableAlias":"p1","mainTableColumn":"parentOneId","joinTableId":"people","joinTableColumn":"uniqueId","joinFilters":[]},
      {"joinType":"LEFT","joinTableAlias":"p2","mainTableColumn":"parentTwoId","joinTableId":"people","joinTableColumn":"uniqueId","joinFilters":[]}
    ]
  }'
```

### API - Join Query Compute Value
```json
{
  "requestType": "QUERY_AND_JOIN_TABLE_COMPUTE",
  /* same structure as Join Queries but with aggregation functions */
}
```
**Response:**
Same as query response
**Example:**
```bash
curl -X POST https://api.autessa.com \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "requestType":"QUERY_AND_JOIN_TABLE_COMPUTE",
    "tableId":"people",
    "selectedColumns":[{"tableId":"people","columnName":"uniqueId","functions":["COUNT"]}],
    "filters":{"operand":"AND","groups":[]},
    "pagination":{"currentPage":0,"totalRecordsPerPage":1,"ascending":true},
    "joinStatements":[
      {"joinType":"LEFT","joinTableAlias":"p1","mainTableColumn":"parentOneId","joinTableId":"people","joinTableColumn":"uniqueId","joinFilters":[]},
      {"joinType":"LEFT","joinTableAlias":"p2","mainTableColumn":"parentTwoId","joinTableId":"people","joinTableColumn":"uniqueId","joinFilters":[]}
    ]
  }'
```

### API - Create Record
```json
{
  "requestType": "CREATE_RECORD",
  "tableId": "123-345-12312-321",
  "columnValueMapping": [
    {"tableName":"people","tableId":"people","columnName":"personName","value":{"columnType":"STRING","value":"Dave"}},
    {"tableName":"people","tableId":"people","columnName":"parentOneId","value":{"columnType":"UNIQUE_ID","value":1}},
    {"tableName":"people","tableId":"people","columnName":"parentTwoId","value":{"columnType":"UNIQUE_ID","value":2}}
  ]
}
```
**Response:**
```json
{
  "recordsImpacted": "<number>"
}
```
**Example:**
```bash
curl -X POST https://api.autessa.com \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "requestType":"CREATE_RECORD",
    "tableId":"people",
    "columnValueMapping":[
      {"tableName":"people","columnName":"personName","value":{"columnType":"STRING","value":"Dave"}},
      {"tableName":"people","columnName":"parentOneId","value":{"columnType":"UNIQUE_ID","value":1}},
      {"tableName":"people","columnName":"parentTwoId","value":{"columnType":"UNIQUE_ID","value":2}}
    ]
  }'
```

### API - Update Record
```json
{
  "requestType": "UPDATE_RECORD",
  "tableId": "123-345-12312-321",
  "uniqueId": 1,
  "updatedColumns": [
    {"tableName":"people","tableId":"people","columnName":"personName","value":{"columnType":"STRING","value":"Alice Smith"}}
  ]
}
```
**Response:**
```json
{
  "recordsImpacted": "<number>"
}
```
**Example:**
```bash
curl -X POST https://api.autessa.com \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "requestType":"UPDATE_RECORD",
    "tableId":"people",
    "uniqueId":1,
    "updatedColumns":[{"tableName":"people","columnName":"personName","value":{"columnType":"STRING","value":"Alice Smith"}}]
  }'
```

### API - Delete Record
```json
{
  "requestType": "DELETE_RECORD",
  "tableId": "123-345-12312-321",
  "uniqueId": 1
}
```
**Response:**
```json
{
  "recordsImpacted": "<number>"
}
```
**Example:**
```bash
curl -X POST https://api.autessa.com \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "requestType":"DELETE_RECORD",
    "tableId":"people",
    "uniqueId":1
  }'
```

### API - Upload Batch
Step 1 - generate an upload link
```json
{
  "requestType": "GENERATE_UPLOAD_LINK",
  "tableId": "123-345-12312-321",
  "fileType": "CSV",
  "fileName": "people_batch.csv"
}
```
**Example:**
```bash
curl -X POST https://api.autessa.com \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "requestType":"GENERATE_UPLOAD_LINK",
    "tableId":"people",
    "fileType":"CSV",
    "fileName":"people_batch.csv"
  }'
```

Step 2 - upload the file to the generated link
```bash
curl -X PUT -T "path/to/your/local/file" "presignedUrl"
```

Step 3 - process the file
```json
{
  "requestType": "BATCH_INSERT_RECORD",
  "tableId": "123-345-12312-321",
  "fileDelimiter": "COMMA",
  "tempPath": "s3://bucket/.../people_batch.csv"
}
```
**Example:**
```bash
curl -X POST https://api.autessa.com \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "requestType":"BATCH_INSERT_RECORD",
    "tableId":"people",
    "fileDelimiter":"COMMA",
    "tempPath":"s3://bucket/.../people_batch.csv"
  }'
```
