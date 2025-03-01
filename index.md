# ContentMint AppEngine API Documentation

Welcome to the ContentMint AppEngine API documentation. This index provides a comprehensive overview of all available API endpoints, organized by module.

## Authentication

Before using most API endpoints, you need to authenticate. The ContentMint AppEngine API supports two authentication methods:

1. **[User Authentication](users/README.md#authentication)** - For human users interacting with the API through a client application
2. **[System Authentication](users/README.md#app-registration-and-system-authentication)** - For systems and applications that need to interact with the API programmatically

For detailed information about authentication, including how to obtain and use access tokens, see the [Authentication section](README.md#authentication) in the main documentation.

## API Modules

### Core Modules

| Module | Description |
|--------|-------------|
| [Users API](users/README.md) | Authentication, user management, and authorization |
| [Dynamic Query API](dynamic-query/README.md) | Dynamic database queries and operations |
| [GraphQL API](graphql/README.md) | GraphQL interface for data queries and mutations |

### Content Management

| Module | Description |
|--------|-------------|
| [Site API](site/README.md) | Website and page management |
| [Batch API](batch/README.md) | Batch operations and site repositories |
| [History API](history/README.md) | Historical data and versioning |

### Customer Relationship Management

| Module | Description |
|--------|-------------|
| [CRM API](crm/README.md) | Customer relationship management features |
| [Chat API](chat/README.md) | Real-time chat functionality |

### E-commerce

| Module | Description |
|--------|-------------|
| [Storefront API](storefront/README.md) | E-commerce functionality |
| [Tax API](tax/README.md) | Tax calculation and management |

### Integration and Synchronization

| Module | Description |
|--------|-------------|
| [Connect API](connect/README.md) | Integration with external services |
| [Upstream API](upstream/README.md) | Upstream data flow and processing |
| [Sync API](sync/README.md) | Data synchronization |

### Artificial Intelligence

| Module | Description |
|--------|-------------|
| [AI API](ai/README.md) | Artificial intelligence and machine learning features |

### Business Operations

| Module | Description |
|--------|-------------|
| [Business API](business/README.md) | Business operations |
| [Mintflow API](mintflow/README.md) | Workflow automation |

### Utilities

| Module | Description |
|--------|-------------|
| [Tools API](tools/README.md) | Utility tools and features |
| [Repository API](repositories/README.md) | Data repository operations |

## Common Concepts

### Data Models

The ContentMint AppEngine API uses a variety of data models to represent entities and their relationships. For detailed information about these models, see the [Data Models](models.md) documentation.

### Authentication and Authorization

Most API endpoints require authentication and specific permissions or roles. The authentication and authorization system is based on JWT (JSON Web Token) and role-based access control (RBAC).

For more information, see:

- [Authentication](README.md#authentication)
- [Role and Group Management](users/README.md#role-and-group-management)

## Common API Patterns

### Pagination

Most endpoints that return collections of items support pagination to limit the number of results returned in a single request. This is important for performance and usability when dealing with large datasets.

#### Pagination Parameters

| Parameter | Type | Description | Default |
|-----------|------|-------------|---------|
| `page` | number | The page number (1-based) | 1 |
| `limit` | number | The number of items per page | 20 |
| `offset` | number | Alternative to page, the number of items to skip | 0 |

#### Pagination Response Format

Paginated responses include metadata about the pagination state:

```json
{
  "data": [...],
  "pagination": {
    "total": 100,
    "page": 1,
    "limit": 20,
    "pages": 5,
    "hasNext": true,
    "hasPrev": false
  }
}
```

#### Important Pagination Considerations

- The maximum value for `limit` is 100. Requests with larger values will be capped.
- For large datasets, using cursor-based pagination (where available) is more efficient than offset-based pagination.
- Some endpoints support cursor-based pagination using `after` and `before` parameters instead of `page` and `limit`.
- When iterating through all pages in automated scripts, implement rate limiting to avoid API throttling.

### Filtering and Sorting

Many endpoints support filtering and sorting to retrieve specific subsets of data.

#### Common Filter Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `search` | Full-text search across relevant fields | `search=example` |
| `filter[field]` | Filter by a specific field | `filter[status]=active` |
| `filter[field][operator]` | Filter with a specific operator | `filter[createdAt][gt]=2023-01-01` |
| `include` | Include related resources | `include=author,comments` |
| `fields` | Limit fields returned (sparse fieldsets) | `fields=id,title,createdAt` |

#### Common Sort Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `sort` | Field to sort by (prefix with `-` for descending) | `sort=createdAt` or `sort=-createdAt` |
| `sort[]` | Multiple sort fields | `sort[]=name&sort[]=-createdAt` |

#### Filter Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `eq` | Equal to (default) | `filter[status][eq]=active` |
| `ne` | Not equal to | `filter[status][ne]=deleted` |
| `gt` | Greater than | `filter[price][gt]=100` |
| `gte` | Greater than or equal to | `filter[price][gte]=100` |
| `lt` | Less than | `filter[price][lt]=100` |
| `lte` | Less than or equal to | `filter[price][lte]=100` |
| `in` | In array | `filter[status][in]=active,pending` |
| `nin` | Not in array | `filter[status][nin]=deleted,archived` |
| `like` | Contains substring (case-insensitive) | `filter[name][like]=example` |
| `exists` | Field exists | `filter[middleName][exists]=true` |

#### Important Filtering Considerations

- Not all fields support all operators
- Complex filters may impact performance
- Some fields may not be filterable for security or performance reasons
- Case sensitivity varies by field and operator
- Date fields should be formatted as ISO 8601 strings (e.g., `2023-01-01T12:00:00Z`)

### Bulk Operations

Some endpoints support bulk operations to perform the same action on multiple resources in a single request.

#### Bulk Create

```json
POST /api/users/bulk
{
  "users": [
    { "email": "user1@example.com", "name": "User 1" },
    { "email": "user2@example.com", "name": "User 2" }
  ]
}
```

#### Bulk Update

```json
PUT /api/users/bulk
{
  "users": [
    { "id": "user1", "name": "Updated User 1" },
    { "id": "user2", "name": "Updated User 2" }
  ]
}
```

#### Bulk Delete

```json
DELETE /api/users/bulk
{
  "ids": ["user1", "user2", "user3"]
}
```

#### Important Bulk Operation Considerations

- Bulk operations are atomic by default (all succeed or all fail)
- Some endpoints support non-atomic bulk operations with the `atomic=false` parameter
- Bulk operations may have stricter rate limits than individual operations
- The maximum number of items in a bulk operation is typically 100
- Responses include individual results for each item in the bulk operation

### Error Handling

API errors are returned with appropriate HTTP status codes and error messages. The error response format is consistent across all endpoints:

```json
{
  "statusCode": 400,
  "message": "Invalid request format",
  "error": "Bad Request",
  "details": [
    {
      "field": "email",
      "message": "Email is required",
      "code": "required_field"
    }
  ],
  "requestId": "req_123456",
  "documentation": "https://docs.contentmint.com/errors/required_field"
}
```

#### Common HTTP Status Codes

- `200 OK`: The request was successful
- `201 Created`: The resource was created successfully
- `204 No Content`: The request was successful but there is no response body
- `400 Bad Request`: The request was invalid
- `401 Unauthorized`: Authentication is required or failed
- `403 Forbidden`: The authenticated user doesn't have the required permissions
- `404 Not Found`: The requested resource was not found
- `409 Conflict`: The request conflicts with the current state of the resource
- `422 Unprocessable Entity`: The request was well-formed but contains semantic errors
- `429 Too Many Requests`: Rate limit exceeded
- `500 Internal Server Error`: An error occurred on the server
- `503 Service Unavailable`: The service is temporarily unavailable

#### Error Fields

| Field | Description |
|-------|-------------|
| `statusCode` | The HTTP status code |
| `message` | A human-readable error message |
| `error` | The error type |
| `details` | An array of detailed error information (field-level errors) |
| `requestId` | A unique identifier for the request (useful for troubleshooting) |
| `documentation` | A link to documentation about the error |

#### Error Handling Best Practices

1. **Check the status code first**: Different status codes require different handling strategies
2. **Look for field-level errors**: The `details` array contains specific information about validation errors
3. **Log the requestId**: This helps with troubleshooting and support
4. **Implement retry logic**: For 429 and 5xx errors, implement exponential backoff
5. **Handle authentication errors**: For 401 errors, refresh the token or redirect to login
6. **Display user-friendly messages**: Don't show raw error messages to end users
7. **Check for rate limiting headers**: Look for `X-RateLimit-*` headers to avoid hitting limits

## Getting Started

To get started with the ContentMint AppEngine API:

1. Review the [Authentication section](README.md#authentication) to understand how to authenticate your requests
2. Explore the specific API modules that interest you
3. Try out the API endpoints with the provided request and response examples
4. Refer to the [Data Models](models.md) documentation for information about the data structures

## API Versioning

The ContentMint AppEngine API uses versioning to ensure backward compatibility. The current version is v1, which is the default version for all endpoints.

## Rate Limiting

The API implements rate limiting to prevent abuse. The rate limits vary by endpoint and authentication status. If you exceed the rate limit, you'll receive a `429 Too Many Requests` response.

## Support

If you encounter any issues or have questions about the API, please contact support at <support@contentmint.com>.
