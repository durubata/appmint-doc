# Appmint AppEngine API Documentation

Welcome to the Appmint AppEngine API documentation. This comprehensive guide provides detailed information about all available API endpoints, data models, and usage examples.

## Getting Started

To get started with the Appmint AppEngine API, we recommend the following:

1. Start with the [main documentation index](index.md) for an overview of all available resources
2. Review the [data models](models.md) to understand the structure of the data
3. Check the [authentication](#authentication) section below to learn how to authenticate your requests
4. Explore the specific API modules that interest you

## Documentation Structure

The documentation is organized into the following sections:

- **API Modules**: Each API module has its own documentation with detailed endpoint information
- **Data Models**: Overview of all data structures used in the API
- **Authentication**: Information about authentication methods
- **Common Parameters**: Parameters used across multiple endpoints
- **Error Handling**: How errors are handled in the API

## API Modules

- [Users API](users/README.md) - Authentication, user management, and authorization
- [Dynamic Query API](dynamic-query/README.md) - Dynamic database queries and operations
- [CRM API](crm/README.md) - Customer relationship management features
- [Storefront API](storefront/README.md) - E-commerce functionality
- [Site API](site/README.md) - Website and page management
- [AI API](ai/README.md) - Artificial intelligence and machine learning features
- [Mintflow API](mintflow/README.md) - Workflow automation
- [Chat API](chat/README.md) - Real-time chat functionality
- [Business API](business/README.md) - Business operations
- [Tax API](tax/README.md) - Tax calculation and management
- [Tools API](tools/README.md) - Utility tools and features
- [History API](history/README.md) - Historical data and versioning
- [Batch API](batch/README.md) - Batch operations and processing
- [Connect API](connect/README.md) - Integration with external services
- [Upstream API](upstream/README.md) - Upstream data flow and processing
- [Sync API](sync/README.md) - Data synchronization
- [Repository API](repositories/README.md) - Data repository operations
- [GraphQL API](graphql/README.md) - GraphQL interface

## Authentication

Most API endpoints require authentication. The Appmint AppEngine API uses JWT (JSON Web Token) for authentication. There are two main ways to authenticate with the API:

### 1. User Authentication

This method is used when a human user is interacting with the API through a client application.

#### User Sign In

**Endpoint:** `POST /user/signin` or `POST /profile/signin`

**Request:**

```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

**Response:**

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "user123",
    "email": "user@example.com",
    "username": "user123",
    "roles": ["User"]
  }
}
```

### 2. System Authentication (App Key)

This method is used when a system or application needs to authenticate with the API programmatically.

#### Validate App Key

**Endpoint:** `POST /validate-app-key`

**Request:**

```json
{
  "appId": "app_id_123",
  "appKey": "app_key_456",
  "appSecret": "app_secret_789",
  "domain": "myapp.example.com"
}
```

**Response:**

```json
{
  "valid": true,
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "app": {
    "id": "app123",
    "name": "My App",
    "domain": "myapp.example.com"
  }
}
```

### Using the Access Token

Once you have obtained an access token (either through user authentication or system authentication), you need to include it in the `Authorization` header of your API requests:

```
orgid: your-organization-id
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

#### Important Considerations for Token Usage

- **Token Format**: The token must be prefixed with `Bearer` (including the space) in the Authorization header.
- **Case Sensitivity**: The `Authorization` header name is case-insensitive, but the `Bearer` prefix is case-sensitive.
- **Organization ID**: The `orgid` header is required for most endpoints and must match the organization ID associated with the token.
- **Token Validation**: The system validates both the token signature and the claims (exp, iss, sub, etc.).
- **Scope Limitations**: Tokens have specific scopes that limit what APIs they can access. System tokens may have different scopes than user tokens.
- **IP Restrictions**: Some tokens may be restricted to specific IP addresses for additional security.

### Token Expiration and Refresh

Access tokens have a limited lifespan (typically 1 hour). When an access token expires, you can use the refresh token to obtain a new access token without requiring the user to sign in again.

#### Refresh Token

**Endpoint:** `POST /user/refresh`

**Request:**

```json
{
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**Response:**

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

#### Token Management Best Practices

1. **Secure Storage**:
   - Store tokens securely (use secure HTTP-only cookies for web applications)
   - Never store tokens in localStorage or sessionStorage due to XSS vulnerabilities
   - For mobile apps, use secure storage mechanisms like Keychain (iOS) or KeyStore (Android)

2. **Proactive Refresh**:
   - Implement token refresh before expiration (e.g., at 75% of token lifetime)
   - Use a background process to refresh tokens to avoid disrupting the user experience
   - Include error handling for refresh failures (e.g., redirect to login)

3. **Token Revocation**:
   - Implement logout functionality that revokes tokens on the server
   - Consider implementing a token blacklist for compromised tokens
   - Revoke all tokens when changing passwords or detecting suspicious activity

4. **Error Handling**:
   - Handle 401 Unauthorized responses by attempting a token refresh
   - If refresh fails, redirect to login
   - Implement exponential backoff for retry attempts

5. **Multiple Devices**:
   - The same refresh token can be used across multiple devices
   - Be aware that revoking a refresh token affects all devices using it
   - Consider implementing device-specific tokens for more granular control

#### Common Token-Related Errors

| HTTP Status | Error Code | Description | Solution |
|-------------|------------|-------------|----------|
| 401 | token_expired | The access token has expired | Refresh the token using the refresh token |
| 401 | invalid_token | The token is malformed or invalid | Re-authenticate to obtain a new token |
| 401 | token_revoked | The token has been revoked | Re-authenticate to obtain a new token |
| 401 | insufficient_scope | The token doesn't have the required scope | Obtain a token with the required scope |
| 403 | token_org_mismatch | The token's organization doesn't match the requested resource | Use a token for the correct organization |

### Public Routes

Some endpoints are marked with `@PublicRoute()` and don't require authentication. These endpoints can be accessed without providing an access token.

## Making API Requests

All API requests should be made to the base URL of your Appmint AppEngine instance. For example:

```
https://your-instance.Appmint.com/api/
```

### Request Format

Most API endpoints accept JSON-formatted request bodies. For example:

```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

### Response Format

API responses are returned in JSON format. A typical successful response looks like:

```json
{
  "success": true,
  "data": {
    "id": "123",
    "name": "Example"
  }
}
```

Error responses include an error message and status code:

```json
{
  "statusCode": 400,
  "message": "Invalid request format"
}
```

### Authentication Flow Diagram

The following diagram illustrates the authentication flow for both user and system authentication:

```
User Authentication Flow:
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│             │     │             │     │             │
│    Client   │────▶│  /signin    │────▶│  JWT Token  │
│             │     │             │     │             │
└─────────────┘     └─────────────┘     └──────┬──────┘
                                               │
                                               ▼
                                        ┌─────────────┐
                                        │  Protected  │
                                        │  Endpoints  │
                                        │             │
                                        └─────────────┘

System Authentication Flow:
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│             │     │             │     │             │
│   System    │────▶│ /validate-  │────▶│  JWT Token  │
│             │     │  app-key    │     │             │
└─────────────┘     └─────────────┘     └──────┬──────┘
                                               │
                                               ▼
                                        ┌─────────────┐
                                        │  Protected  │
                                        │  Endpoints  │
                                        │             │
                                        └─────────────┘

Token Refresh Flow:
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│             │     │             │     │             │
│   Expired   │────▶│  /refresh   │────▶│  New JWT    │
│   Token     │     │             │     │  Token      │
└─────────────┘     └─────────────┘     └──────┬──────┘
                                               │
                                               ▼
                                        ┌─────────────┐
                                        │  Protected  │
                                        │  Endpoints  │
                                        │             │
                                        └─────────────┘
```
