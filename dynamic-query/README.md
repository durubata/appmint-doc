# Dynamic Query API

The Dynamic Query API provides powerful endpoints for performing dynamic queries, updates, and deletions on collections in the database. This API is designed for flexibility, allowing clients to construct complex queries without requiring custom endpoints for each use case.

## Table of Contents

- [Dynamic Query](#dynamic-query)
- [Dynamic Update](#dynamic-update)
- [Dynamic Delete](#dynamic-delete)
- [Best Practices](#best-practices)
- [Common Pitfalls](#common-pitfalls)
- [Performance Considerations](#performance-considerations)
- [Security Considerations](#security-considerations)
- [Advanced Usage](#advanced-usage)

## Dynamic Query

Performs a dynamic query on collections in the database.

**Endpoint:** `POST /dynamic-query/query`

**Required Permission:** `PermissionTypeContent.read`

**Request:**

```json
{
  "orgId": "your-organization-id",
  "collections": {
    "users": ["name", "address", "email"],
    "orders": ["items", "status", "amount"]
  },
  "links": {
    "orders": {
      "email": "users.email"
    }
  },
  "filter": {
    "users": {
      "status": "active"
    },
    "orders": {
      "status": "pending"
    }
  },
  "sort": {
    "users": { "name": 1 }
  },
  "pagination": {
    "limit": 10,
    "skip": 0
  }
}
```

**Response:**

```json
[
  {
    "name": "John Doe",
    "address": "123 Main St",
    "email": "john@example.com",
    "orders": [
      {
        "items": ["Product A", "Product B"],
        "status": "pending",
        "amount": 100
      }
    ]
  },
  {
    "name": "Jane Smith",
    "address": "456 Oak Ave",
    "email": "jane@example.com",
    "orders": [
      {
        "items": ["Product C"],
        "status": "pending",
        "amount": 50
      }
    ]
  }
]
```

### Request Parameters

- `orgId` (required): The organization ID.
- `collections` (required): An object where keys are collection names and values are arrays of field names to retrieve.
- `links` (optional): An object defining relationships between collections. Each key is a linked collection name, and the value is an object mapping foreign fields to local fields.
- `filter` (optional): An object containing filter conditions for each collection.
- `sort` (optional): An object defining sort order for each collection. Use 1 for ascending and -1 for descending.
- `pagination` (optional): An object with `limit` and `skip` properties for pagination.

## Dynamic Update

Performs a dynamic update on a collection in the database.

**Endpoint:** `POST /dynamic-query/update`

**Required Permission:** `PermissionTypeContent.update`, `PermissionTypeContent.create`

**Request:**

```json
{
  "orgId": "your-organization-id",
  "collection": "users",
  "filter": {
    "email": "john@example.com"
  },
  "update": {
    "$set": {
      "name": "John Smith",
      "address": "789 Pine St"
    }
  },
  "options": {
    "upsert": false,
    "multi": true
  }
}
```

**Response:**

```json
{
  "acknowledged": true,
  "modifiedCount": 1,
  "upsertedId": null,
  "upsertedCount": 0,
  "matchedCount": 1
}
```

### Request Parameters

- `orgId` (required): The organization ID.
- `collection` (required): The name of the collection to update.
- `filter` (required unless `options.upsert` is true): An object containing filter conditions to identify documents to update.
- `update` (required): An object containing update operations. Must use MongoDB update operators like `$set`, `$unset`, etc.
- `options` (optional): An object with update options:
  - `upsert` (boolean): If true, creates a new document when no document matches the filter.
  - `multi` (boolean): If true, updates multiple documents that match the filter.

## Dynamic Delete

Performs a dynamic delete operation on a collection in the database.

**Endpoint:** `POST /dynamic-query/delete`

**Required Permission:** `PermissionTypeContent.delete`

**Request:**

```json
{
  "orgId": "your-organization-id",
  "collection": "users",
  "filter": {
    "email": "john@example.com"
  }
}
```

**Response:**

```json
{
  "deletedCount": 1
}
```

### Request Parameters

- `orgId` (required): The organization ID.
- `collection` (required): The name of the collection to delete from.
- `filter` (required): An object containing filter conditions to identify documents to delete.

## Error Handling

All endpoints return appropriate HTTP status codes and error messages in case of failure:

```json
{
  "statusCode": 400,
  "message": "Invalid query format: .collections must have required property 'users'"
}
```

Common error scenarios:

- Invalid request format
- Collection does not exist
- Field does not exist in collection
- Invalid filter or update operations
- Insufficient permissions

## Best Practices

### Query Construction

1. **Request Only Needed Fields**
   - Always specify only the fields you need in the `collections` object
   - Requesting unnecessary fields increases response size and processing time
   - Example: `"users": ["name", "email"]` instead of `"users": ["*"]`

2. **Use Specific Filters**
   - Create specific filters to limit the number of documents processed
   - Combine multiple conditions for more precise filtering
   - Example:

     ```json
     "filter": {
       "users": {
         "status": "active",
         "createdAt": { "$gte": "2023-01-01T00:00:00Z" }
       }
     }
     ```

3. **Implement Pagination**
   - Always use pagination for queries that might return large result sets
   - Start with a reasonable limit (e.g., 20-50 items)
   - Implement "load more" or pagination controls in your UI
   - Example:

     ```json
     "pagination": {
       "limit": 20,
       "skip": 0
     }
     ```

4. **Use Efficient Sorting**
   - Sort on indexed fields whenever possible
   - Limit the number of sort fields to improve performance
   - Consider the impact of sorting on large collections
   - Example:

     ```json
     "sort": {
       "users": { "createdAt": -1 }
     }
     ```

### Update Operations

1. **Use Atomic Operators**
   - Use atomic operators like `$set`, `$inc`, `$push` instead of replacing entire documents
   - This prevents race conditions and improves performance
   - Example:

     ```json
     "update": {
       "$inc": { "visitCount": 1 },
       "$set": { "lastVisit": "2023-01-01T12:00:00Z" }
     }
     ```

2. **Validate Before Updating**
   - Validate data on the client side before sending update requests
   - Use specific filters to ensure you're updating the correct documents
   - Consider using transactions for complex updates

3. **Use Upsert Carefully**
   - Only use `upsert: true` when you genuinely need to create or update
   - Provide all required fields when using upsert to avoid incomplete documents
   - Example:

     ```json
     "options": {
       "upsert": true
     },
     "update": {
       "$setOnInsert": {
         "createdAt": "2023-01-01T12:00:00Z",
         "status": "active"
       },
       "$set": {
         "name": "John Smith"
       }
     }
     ```

### Delete Operations

1. **Use Specific Filters**
   - Always use specific filters to avoid accidentally deleting too many documents
   - Consider using soft deletes (setting a `deleted` flag) instead of hard deletes
   - Example:

     ```json
     "filter": {
       "_id": "user123",
       "orgId": "org456"
     }
     ```

2. **Implement Safeguards**
   - Add confirmation steps in your UI for delete operations
   - Consider implementing a trash system with a recovery period
   - Use transactions for complex delete operations that affect multiple collections

## Common Pitfalls

1. **Overly Broad Queries**
   - Problem: Querying without specific filters or requesting too many fields
   - Impact: Poor performance, excessive memory usage, timeout errors
   - Solution: Always use specific filters and request only needed fields

2. **Missing Indexes**
   - Problem: Filtering or sorting on non-indexed fields
   - Impact: Slow queries, especially on large collections
   - Solution: Ensure commonly queried and sorted fields are indexed

3. **Incorrect Link Definitions**
   - Problem: Mismatched field names in link definitions
   - Impact: Missing related data or incorrect relationships
   - Solution: Double-check field names and ensure they match between collections

4. **Race Conditions**
   - Problem: Multiple updates to the same document without proper concurrency control
   - Impact: Data inconsistency or lost updates
   - Solution: Use atomic operators and consider implementing optimistic concurrency control

5. **Large Result Sets**
   - Problem: Querying without pagination or with high limits
   - Impact: Slow response times, excessive memory usage, timeout errors
   - Solution: Always implement pagination with reasonable limits

6. **Unbounded Growth**
   - Problem: Continuously adding data to arrays without limits
   - Impact: Documents exceeding size limits, degraded performance
   - Solution: Use array size limits and consider normalizing data into separate collections

## Performance Considerations

1. **Query Optimization**
   - Use indexed fields in filters and sorts
   - Limit the number of linked collections
   - Use projection to return only necessary fields
   - Consider denormalizing frequently accessed data

2. **Batch Operations**
   - For bulk updates, use the bulk API instead of multiple individual requests
   - Process large datasets in smaller batches
   - Implement retry logic for failed operations

3. **Caching Strategies**
   - Cache frequently accessed and relatively static data
   - Implement cache invalidation when data changes
   - Consider using Redis or a similar caching solution

4. **Monitoring and Profiling**
   - Monitor query performance in production
   - Identify and optimize slow queries
   - Use database profiling tools to analyze query patterns

## Security Considerations

1. **Data Access Control**
   - The API enforces organization-level isolation through the `orgId` parameter
   - Additional field-level security can be implemented through custom middleware
   - Consider implementing row-level security for sensitive data

2. **Input Validation**
   - All input is validated against schemas before processing
   - Custom validators can be added for specific business rules
   - Consider implementing additional validation on the client side

3. **Query Injection Prevention**
   - The API uses parameterized queries to prevent injection attacks
   - User input is never directly concatenated into queries
   - Additional sanitization is applied to special characters

4. **Rate Limiting**
   - API endpoints are rate-limited to prevent abuse
   - Consider implementing client-side throttling for bulk operations
   - Exponential backoff should be used when retrying failed requests

## Advanced Usage

### Aggregation Pipelines

For complex data transformations, you can use aggregation pipelines:

```json
{
  "orgId": "your-organization-id",
  "collection": "orders",
  "pipeline": [
    { "$match": { "status": "completed" } },
    { "$group": {
        "_id": "$customerId",
        "totalSpent": { "$sum": "$amount" },
        "orderCount": { "$sum": 1 }
      }
    },
    { "$sort": { "totalSpent": -1 } },
    { "$limit": 10 }
  ]
}
```

### Transactions

For operations that need to be atomic across multiple collections:

```json
{
  "orgId": "your-organization-id",
  "operations": [
    {
      "type": "update",
      "collection": "inventory",
      "filter": { "productId": "prod123" },
      "update": { "$inc": { "quantity": -1 } }
    },
    {
      "type": "insert",
      "collection": "orders",
      "document": {
        "productId": "prod123",
        "customerId": "cust456",
        "quantity": 1,
        "status": "pending"
      }
    }
  ],
  "options": {
    "readConcern": "majority",
    "writeConcern": { "w": "majority" }
  }
}
```

### Geospatial Queries

For location-based queries:

```json
{
  "orgId": "your-organization-id",
  "collection": "stores",
  "filter": {
    "location": {
      "$near": {
        "$geometry": {
          "type": "Point",
          "coordinates": [-73.9857, 40.7484]
        },
        "$maxDistance": 1000
      }
    }
  }
}
```

### Text Search

For full-text search capabilities:

```json
{
  "orgId": "your-organization-id",
  "collection": "products",
  "filter": {
    "$text": {
      "$search": "organic gluten-free",
      "$language": "english",
      "$caseSensitive": false,
      "$diacriticSensitive": false
    }
  },
  "sort": {
    "score": { "$meta": "textScore" }
  }
}
```
