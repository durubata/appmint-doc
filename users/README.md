# Users API

The Users API provides endpoints for user authentication, registration, profile management, and authorization.

## Table of Contents

- [Authentication](#authentication)
  - [User Sign In](#user-sign-in)
  - [User Sign Up](#user-sign-up)
  - [Refresh Token](#refresh-token)
  - [Password Management](#password-management)
  - [OAuth Authentication](#oauth-authentication)
- [User Management](#user-management)
  - [User Profile](#user-profile)
  - [User Update](#user-update)
  - [User Deletion](#user-deletion)
- [Customer Management](#customer-management)
  - [Customer Sign In](#customer-sign-in)
  - [Customer Sign Up](#customer-sign-up)
  - [Customer Profile](#customer-profile)
- [Role and Group Management](#role-and-group-management)
- [App Registration](#app-registration)
- [Blacklist Management](#blacklist-management)

## Authentication

### User Sign In

Authenticates a user and returns a JWT token.

**Endpoint:** `POST /profile/signin` or `POST /user/signin`

**Public Route:** Yes

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

### User Sign Up

Registers a new user.

**Endpoint:** `POST /profile/signup` or `POST /user/signup`

**Public Route:** Yes

**Request:**

```json
{
  "email": "newuser@example.com",
  "password": "password123",
  "username": "newuser",
  "firstName": "John",
  "lastName": "Doe"
}
```

**Response:**

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "user456",
    "email": "newuser@example.com",
    "username": "newuser",
    "firstName": "John",
    "lastName": "Doe",
    "roles": ["User"]
  }
}
```

### Refresh Token

Refreshes an expired JWT token.

**Endpoint:** `POST /user/refresh`

**Public Route:** Yes

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

### Password Management

#### Forgot Password

Sends a password reset email to the user.

**Endpoint:** `GET /password/forgot/:email` or `GET /user/password/forgot/:email`

**Public Route:** Yes

**Response:**

```json
{
  "success": true,
  "message": "Password reset email sent"
}
```

#### Reset Password

Resets a user's password using a reset token.

**Endpoint:** `POST /password/reset` or `POST /user/password/reset`

**Public Route:** Yes

**Request:**

```json
{
  "token": "reset-token-123",
  "password": "newpassword123"
}
```

**Response:**

```json
{
  "success": true,
  "message": "Password reset successful"
}
```

#### Validate Reset Token

Validates a password reset token.

**Endpoint:** `POST /password/validate-token` or `POST /user/password/validate-token`

**Public Route:** Yes

**Request:**

```json
{
  "token": "reset-token-123"
}
```

**Response:**

```json
{
  "valid": true
}
```

#### Change Password

Changes a user's password.

**Endpoint:** `POST /password/change` or `POST /user/password/change`

**Required Role:** ConfigAdmin

**Request:**

```json
{
  "userId": "user123",
  "password": "currentpassword",
  "newPassword": "newpassword123"
}
```

**Response:**

```json
{
  "success": true,
  "message": "Password changed successfully"
}
```

### OAuth Authentication

#### Facebook Login

**Endpoint:** `GET /facebook`

**Public Route:** Yes

#### Facebook Redirect

**Endpoint:** `GET /facebook/redirect`

**Public Route:** Yes

#### Google Login

**Endpoint:** `GET /google`

**Public Route:** Yes

#### Google Redirect

**Endpoint:** `GET /google/redirect`

**Public Route:** Yes

#### GitHub Login

**Endpoint:** `GET /github`

**Public Route:** Yes

#### GitHub Redirect

**Endpoint:** `GET /github/redirect`

**Public Route:** Yes

#### Magic Link Login

Sends a magic link to the user's email for passwordless login.

**Endpoint:** `GET /magiclink/:email`

**Public Route:** Yes

**Response:**

```json
{
  "success": true,
  "message": "Magic link sent to email"
}
```

#### Magic Link Redirect

**Endpoint:** `POST /magiclink/redirect`

**Public Route:** Yes

**Request:**

```json
{
  "token": "magic-link-token-123"
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

#### Code Login

Sends a verification code to the user's email for login.

**Endpoint:** `GET /code/:email`

**Public Route:** Yes

**Response:**

```json
{
  "success": true,
  "message": "Verification code sent to email"
}
```

## User Management

### User Profile

Gets a user's profile.

**Endpoint:** `GET /user/profile/:emailOrUsername`

**Required Role:** ConfigAdmin

**Response:**

```json
{
  "id": "user123",
  "email": "user@example.com",
  "username": "user123",
  "firstName": "John",
  "lastName": "Doe",
  "roles": ["User"],
  "groups": ["Group1"]
}
```

### User Update

Updates a user's profile.

**Endpoint:** `POST /user/update` or `POST /update`

**Required Role:** ConfigAdmin, RootAdmin, RootPowerUser

**Request:**

```json
{
  "id": "user123",
  "email": "user@example.com",
  "firstName": "John",
  "lastName": "Smith"
}
```

**Response:**

```json
{
  "success": true,
  "user": {
    "id": "user123",
    "email": "user@example.com",
    "username": "user123",
    "firstName": "John",
    "lastName": "Smith",
    "roles": ["User"],
    "groups": ["Group1"]
  }
}
```

### User Deletion

Deletes a user.

**Endpoint:** `DELETE /user/profile/:emailOrUsername`

**Required Role:** ConfigAdmin

**Response:**

```json
{
  "success": true,
  "message": "User deleted successfully"
}
```

## Customer Management

### Customer Sign In

Authenticates a customer and returns a JWT token.

**Endpoint:** `POST /customer/signin`

**Public Route:** Yes

**Request:**

```json
{
  "email": "customer@example.com",
  "password": "password123"
}
```

**Response:**

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "customer": {
    "id": "customer123",
    "email": "customer@example.com",
    "firstName": "Jane",
    "lastName": "Doe"
  }
}
```

### Customer Sign Up

Registers a new customer.

**Endpoint:** `POST /customer/signup`

**Public Route:** Yes

**Request:**

```json
{
  "email": "newcustomer@example.com",
  "password": "password123",
  "firstName": "Jane",
  "lastName": "Smith"
}
```

**Response:**

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "customer": {
    "id": "customer456",
    "email": "newcustomer@example.com",
    "firstName": "Jane",
    "lastName": "Smith"
  }
}
```

### Customer Profile

Gets a customer's profile.

**Endpoint:** `GET /customer/profile/:emailOrUsername`

**Required Role:** Owner

**Response:**

```json
{
  "id": "customer123",
  "email": "customer@example.com",
  "firstName": "Jane",
  "lastName": "Doe",
  "subscriptions": ["newsletter"]
}
```

### Customer Update

Updates a customer's profile.

**Endpoint:** `POST /customer/update`

**Request:**

```json
{
  "id": "customer123",
  "email": "customer@example.com",
  "firstName": "Jane",
  "lastName": "Smith"
}
```

**Response:**

```json
{
  "success": true,
  "customer": {
    "id": "customer123",
    "email": "customer@example.com",
    "firstName": "Jane",
    "lastName": "Smith",
    "subscriptions": ["newsletter"]
  }
}
```

### Customer Password Management

#### Customer Forgot Password

**Endpoint:** `GET /customer/password/forgot/:email`

**Public Route:** Yes

#### Customer Reset Password

**Endpoint:** `POST /customer/password/reset`

**Public Route:** Yes

#### Customer Validate Reset Token

**Endpoint:** `POST /customer/password/validate-token`

**Public Route:** Yes

#### Customer Change Password

**Endpoint:** `POST /customer/password/change`

## Role and Group Management

### Add User to Group

**Endpoint:** `POST /user/group/add` or `POST /group/add`

**Required Role:** ConfigAdmin, RootAdmin, RootPowerUser

**Request:**

```json
{
  "email": "user@example.com",
  "groups": ["Group1", "Group2"]
}
```

**Response:**

```json
{
  "success": true,
  "message": "User added to groups"
}
```

### Remove User from Group

**Endpoint:** `POST /user/group/remove` or `POST /group/remove`

**Required Role:** ConfigAdmin, RootAdmin, RootPowerUser

**Request:**

```json
{
  "email": "user@example.com",
  "groups": ["Group2"]
}
```

**Response:**

```json
{
  "success": true,
  "message": "User removed from groups"
}
```

### Add Role to User

**Endpoint:** `POST /user/role/add` or `POST /role/add`

**Required Role:** ConfigAdmin, RootAdmin, RootPowerUser

**Request:**

```json
{
  "email": "user@example.com",
  "roles": ["Editor", "Publisher"]
}
```

**Response:**

```json
{
  "success": true,
  "message": "Roles added to user"
}
```

### Remove Role from User

**Endpoint:** `POST /user/role/remove` or `POST /role/remove`

**Required Role:** ConfigAdmin, RootAdmin, RootPowerUser

**Request:**

```json
{
  "email": "user@example.com",
  "roles": ["Publisher"]
}
```

**Response:**

```json
{
  "success": true,
  "message": "Roles removed from user"
}
```

## App Registration and System Authentication

The App Registration endpoints allow you to register external applications and systems that need to interact with the Appmint AppEngine API programmatically. This is particularly useful for integrations, automated systems, and third-party applications.

### Register App

**Endpoint:** `POST /app/register`

**Required Role:** ConfigAdmin, RootAdmin, RootPowerUser

**Description:** Registers a new application and generates the necessary credentials (appId, appKey, and appSecret) for system authentication.

**Request:**

```json
{
  "name": "My App",
  "domain": "myapp.example.com",
  "description": "My application description",
  "callbackUrl": "https://myapp.example.com/auth/callback",
  "permissions": ["read", "write"]
}
```

**Response:**

```json
{
  "id": "app123",
  "name": "My App",
  "domain": "myapp.example.com",
  "description": "My application description",
  "callbackUrl": "https://myapp.example.com/auth/callback",
  "permissions": ["read", "write"],
  "appId": "app_id_123",
  "appKey": "app_key_456",
  "appSecret": "app_secret_789",
  "createdAt": "2023-01-01T12:00:00Z"
}
```

**Important:** Store the `appId`, `appKey`, and `appSecret` securely. The `appSecret` will only be shown once and cannot be retrieved later.

### Validate App Key

**Endpoint:** `POST /validate-app-key`

**Public Route:** Yes

**Description:** Validates the application credentials and returns an access token for API access. This is the primary endpoint for system authentication.

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
    "domain": "myapp.example.com",
    "permissions": ["read", "write"]
  },
  "expiresIn": 3600
}
```

### List Registered Apps

**Endpoint:** `GET /app/list`

**Required Role:** ConfigAdmin, RootAdmin, RootPowerUser

**Description:** Lists all registered applications for the organization.

**Response:**

```json
{
  "apps": [
    {
      "id": "app123",
      "name": "My App",
      "domain": "myapp.example.com",
      "description": "My application description",
      "permissions": ["read", "write"],
      "appId": "app_id_123",
      "lastUsed": "2023-01-02T10:30:00Z",
      "createdAt": "2023-01-01T12:00:00Z"
    },
    {
      "id": "app456",
      "name": "Another App",
      "domain": "anotherapp.example.com",
      "description": "Another application description",
      "permissions": ["read"],
      "appId": "app_id_789",
      "lastUsed": "2023-01-02T09:15:00Z",
      "createdAt": "2023-01-01T10:00:00Z"
    }
  ]
}
```

### Update App

**Endpoint:** `PUT /app/update/:appId`

**Required Role:** ConfigAdmin, RootAdmin, RootPowerUser

**Description:** Updates an existing application's information.

**Request:**

```json
{
  "name": "My Updated App",
  "description": "Updated application description",
  "callbackUrl": "https://myapp.example.com/new-callback",
  "permissions": ["read", "write", "admin"]
}
```

**Response:**

```json
{
  "success": true,
  "app": {
    "id": "app123",
    "name": "My Updated App",
    "domain": "myapp.example.com",
    "description": "Updated application description",
    "callbackUrl": "https://myapp.example.com/new-callback",
    "permissions": ["read", "write", "admin"],
    "appId": "app_id_123",
    "updatedAt": "2023-01-03T14:00:00Z",
    "createdAt": "2023-01-01T12:00:00Z"
  }
}
```

### Regenerate App Secret

**Endpoint:** `POST /app/regenerate-secret/:appId`

**Required Role:** ConfigAdmin, RootAdmin, RootPowerUser

**Description:** Regenerates the app secret for an application. This invalidates the previous secret.

**Response:**

```json
{
  "success": true,
  "appId": "app_id_123",
  "appSecret": "new_app_secret_789",
  "message": "App secret regenerated successfully. Store this secret securely as it won't be shown again."
}
```

### Delete App

**Endpoint:** `DELETE /app/delete/:appId`

**Required Role:** ConfigAdmin, RootAdmin, RootPowerUser

**Description:** Deletes an application registration. This immediately invalidates any tokens issued to this application.

**Response:**

```json
{
  "success": true,
  "message": "Application deleted successfully"
}
```

### System Authentication Flow

To authenticate a system or application with the Appmint AppEngine API:

1. Register your application using the `/app/register` endpoint to obtain your `appId`, `appKey`, and `appSecret`
2. Store these credentials securely in your application's environment variables or secure configuration
3. When your application needs to access the API, call the `/validate-app-key` endpoint with your credentials to obtain an access token
4. Include the access token in the `Authorization` header of your API requests:

   ```
   orgid: your-organization-id
   Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
   ```

5. When the access token expires, use the refresh token to obtain a new access token using the `/user/refresh` endpoint

**Example System Authentication Code (Node.js):**

```javascript
async function getAccessToken() {
  const response = await fetch('https://your-instance.Appmint.com/validate-app-key', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      appId: process.env.APP_ID,
      appKey: process.env.APP_KEY,
      appSecret: process.env.APP_SECRET,
      domain: process.env.APP_DOMAIN
    })
  });
  
  const data = await response.json();
  
  if (data.valid) {
    return {
      accessToken: data.access_token,
      refreshToken: data.refresh_token,
      expiresIn: data.expiresIn
    };
  } else {
    throw new Error('Authentication failed');
  }
}

async function callProtectedApi(endpoint) {
  const { accessToken } = await getAccessToken();
  
  const response = await fetch(`https://your-instance.Appmint.com${endpoint}`, {
    headers: {
      'Authorization': `Bearer ${accessToken}`,
      'orgid': 'your-organization-id'
    }
  });
  
  return await response.json();
}
```

## Blacklist Management

### Get Blacklist

**Endpoint:** `GET /blacklist/get`

**Response:**

```json
{
  "blacklist": [
    {
      "domain": "spam.example.com",
      "reason": "Spam domain"
    }
  ]
}
```

### Add to Blacklist

**Endpoint:** `POST /blacklist/add/:domain?`

**Request:**

```json
{
  "domain": "spam.example.com",
  "reason": "Spam domain"
}
```

**Response:**

```json
{
  "success": true,
  "message": "Domain added to blacklist"
}
```

### Remove from Blacklist

**Endpoint:** `DELETE /blacklist/delete/:domain`

**Response:**

```json
{
  "success": true,
  "message": "Domain removed from blacklist"
}
```

## Miscellaneous

### Who Am I

Gets the current user or customer information.

**Endpoint:** `GET /whoami`

**Public Route:** Yes

**Response:**

```json
{
  "user": {
    "id": "user123",
    "email": "user@example.com",
    "username": "user123",
    "roles": ["User"]
  },
  "customer": null
}
