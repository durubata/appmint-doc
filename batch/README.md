# Batch API

The Batch API provides endpoints for managing batch operations, site repositories, and Kubernetes resources.

## Table of Contents

- [Site Repository Management](#site-repository-management)
  - [Publish Status](#publish-status)
  - [Repository Creation](#repository-creation)
  - [Repository Templates](#repository-templates)
  - [Repository Operations](#repository-operations)
  - [Publishing](#publishing)
  - [Domain Management](#domain-management)
  - [Site Operations](#site-operations)
- [Kubernetes Management](#kubernetes-management)
  - [Service Management](#service-management)
  - [Namespace Management](#namespace-management)
  - [Resource Status](#resource-status)

## Site Repository Management

### Publish Status

#### Get Publish Status

**Endpoint:** `GET /batch/sr/publish/status/:name`

**Required Role:** `System`, `ConfigAdmin`, `ContentAdmin`, `Publisher`, `PowerUser`, `RootAdmin`, `RootPowerUser`

**Description:** Gets the publish status of a site repository.

**Path Parameters:**

- `name`: The site repository name

**Response:**

```json
{
  "status": "success",
  "publishStatus": "published",
  "lastPublished": "2023-01-01T12:00:00Z",
  "publishedBy": "user@example.com",
  "environment": "production",
  "url": "https://example.com"
}
```

#### Get Publish API Status

**Endpoint:** `GET /batch/sr/publish-api/status/:hostOrgId/:name`

**Required Role:** `System`, `ConfigAdmin`, `ContentAdmin`, `Publisher`, `PowerUser`, `RootAdmin`, `RootPowerUser`

**Description:** Gets the publish API status of a site repository.

**Path Parameters:**

- `hostOrgId`: The host organization ID
- `name`: The site repository name

**Response:**

```json
{
  "status": "success",
  "publishStatus": "published",
  "lastPublished": "2023-01-01T12:00:00Z",
  "publishedBy": "user@example.com",
  "environment": "production",
  "apiUrl": "https://api.example.com"
}
```

### Repository Creation

#### Create Repository (PUT)

**Endpoint:** `PUT /batch/sr/create`

**Required Role:** `System`, `ConfigAdmin`, `ContentAdmin`, `RootAdmin`, `RootPowerUser`

**Description:** Creates a new site repository by cloning a template. The organization and site must already exist.

**Request:**

```json
{
  "siteName": "example-site",
  "templateRepo": "default-template"
}
```

**Response:**

```json
{
  "status": "success",
  "message": "Repository created successfully",
  "repository": {
    "name": "example-site",
    "url": "https://git.example.com/example-site",
    "branch": "main"
  }
}
```

#### Create Repository (POST)

**Endpoint:** `POST /batch/sr/create`

**Required Role:** `System`, `ConfigAdmin`, `ContentAdmin`, `RootAdmin`, `RootPowerUser`

**Description:** Creates a new site repository by cloning a template. The organization and site must already exist.

**Request:**

```json
{
  "siteName": "example-site",
  "templateRepo": "default-template"
}
```

**Response:**

```json
{
  "status": "success",
  "message": "Repository created successfully",
  "repository": {
    "name": "example-site",
    "url": "https://git.example.com/example-site",
    "branch": "main"
  }
}
```

### Repository Templates

#### Get Repository Templates

**Endpoint:** `GET /batch/sr/templates/:name?`

**Public Route:** Yes

**Description:** Gets available repository templates, optionally filtered by name.

**Path Parameters:**

- `name` (optional): The template name

**Response:**

```json
{
  "templates": [
    {
      "name": "default-template",
      "description": "Default site template",
      "tags": ["default", "starter"],
      "preview": "https://example.com/previews/default-template.jpg"
    },
    {
      "name": "blog-template",
      "description": "Blog site template",
      "tags": ["blog", "content"],
      "preview": "https://example.com/previews/blog-template.jpg"
    }
  ]
}
```

### Repository Operations

#### Get Repository by Name

**Endpoint:** `GET /batch/sr/name/:name`

**Required Role:** `System`, `ConfigAdmin`, `ContentAdmin`, `PowerUser`, `RootAdmin`, `RootPowerUser`

**Description:** Gets a site repository by name.

**Path Parameters:**

- `name`: The repository name

**Response:**

```json
{
  "name": "example-site",
  "url": "https://git.example.com/example-site",
  "branch": "main",
  "lastCommit": {
    "hash": "abc123",
    "message": "Update homepage",
    "author": "user@example.com",
    "date": "2023-01-01T12:00:00Z"
  },
  "status": "active"
}
```

#### Delete Repository

**Endpoint:** `DELETE /batch/sr/delete/:siteOrgId/:name`

**Required Role:** `System`, `ConfigAdmin`, `ContentAdmin`, `RootAdmin`, `RootPowerUser`

**Description:** Deletes a site repository.

**Path Parameters:**

- `siteOrgId`: The site organization ID
- `name`: The repository name

**Query Parameters:**

- `force` (optional): Whether to force deletion
- `deleteHost` (optional): Whether to delete the host
- `deleteOrg` (optional): Whether to delete the organization

**Response:**

```json
{
  "status": "success",
  "message": "Repository deleted successfully"
}
```

#### Delete Local Repository

**Endpoint:** `DELETE /batch/sr/delete-local/:name`

**Required Role:** `System`, `ConfigAdmin`, `ContentAdmin`, `RootAdmin`, `RootPowerUser`

**Description:** Deletes a local site repository.

**Path Parameters:**

- `name`: The repository name

**Response:**

```json
{
  "status": "success",
  "message": "Local repository deleted successfully"
}
```

### Publishing

#### Publish Site

**Endpoint:** `POST /batch/sr/publish/:name`

**Required Role:** `System`, `ConfigAdmin`, `ContentAdmin`, `Publisher`, `PowerUser`, `RootAdmin`, `RootPowerUser`

**Description:** Publishes a site.

**Path Parameters:**

- `name`: The site name

**Request:**

```json
{
  "environment": "production",
  "message": "Publishing site to production"
}
```

**Response:**

```json
{
  "status": "success",
  "message": "Site published successfully",
  "url": "https://example.com",
  "publishedAt": "2023-01-01T12:00:00Z"
}
```

#### Publish Shared Site

**Endpoint:** `POST /batch/sr/publish-shared/:name`

**Required Role:** `System`, `ConfigAdmin`, `ContentAdmin`, `Publisher`, `PowerUser`, `RootAdmin`, `RootPowerUser`

**Description:** Publishes a shared site.

**Path Parameters:**

- `name`: The site name

**Request:**

```json
{
  "environment": "production",
  "message": "Publishing shared site to production"
}
```

**Response:**

```json
{
  "status": "success",
  "message": "Shared site published successfully",
  "url": "https://example.com",
  "publishedAt": "2023-01-01T12:00:00Z"
}
```

#### Publish Site with API

**Endpoint:** `POST /batch/sr/publish-api/:name`

**Required Role:** `System`, `ConfigAdmin`, `ContentAdmin`, `Publisher`, `PowerUser`, `RootAdmin`, `RootPowerUser`

**Description:** Publishes a site with API.

**Path Parameters:**

- `name`: The site name

**Request:**

```json
{
  "environment": "production",
  "message": "Publishing site with API to production"
}
```

**Response:**

```json
{
  "status": "success",
  "message": "Site with API published successfully",
  "url": "https://example.com",
  "apiUrl": "https://api.example.com",
  "publishedAt": "2023-01-01T12:00:00Z"
}
```

#### Build Site

**Endpoint:** `POST /batch/sr/build/:name`

**Required Role:** `System`, `ConfigAdmin`, `ContentAdmin`, `Publisher`, `PowerUser`, `RootAdmin`, `RootPowerUser`

**Description:** Builds a site.

**Path Parameters:**

- `name`: The site name

**Request:**

```json
{
  "environment": "production",
  "message": "Building site for production"
}
```

**Response:**

```json
{
  "status": "success",
  "message": "Site built successfully",
  "buildId": "build123",
  "buildAt": "2023-01-01T12:00:00Z"
}
```

### Domain Management

#### Update Domain

**Endpoint:** `POST /batch/sr/update-domain/:name`

**Required Role:** `System`, `ConfigAdmin`, `ContentAdmin`, `Publisher`, `PowerUser`, `RootAdmin`, `RootPowerUser`

**Description:** Updates a site domain.

**Path Parameters:**

- `name`: The site name

**Request:**

```json
{
  "domain": "example.com",
  "ssl": true,
  "redirectWww": true
}
```

**Response:**

```json
{
  "status": "success",
  "message": "Domain updated successfully",
  "domain": "example.com",
  "url": "https://example.com"
}
```

#### Update Domain API

**Endpoint:** `POST /batch/sr/update-domain-api/:name`

**Required Role:** `System`, `ConfigAdmin`, `ContentAdmin`, `Publisher`, `PowerUser`, `RootAdmin`, `RootPowerUser`

**Description:** Updates a site domain API.

**Path Parameters:**

- `name`: The site name

**Request:**

```json
{
  "domain": "api.example.com",
  "ssl": true
}
```

**Response:**

```json
{
  "status": "success",
  "message": "Domain API updated successfully",
  "domain": "api.example.com",
  "url": "https://api.example.com"
}
```

### Site Operations

#### Clone Site

**Endpoint:** `POST /batch/sr/site/clone`

**Required Role:** `System`, `RootAdmin`, `RootPowerUser`, `Owner`, `ConfigAdmin`

**Description:** Clones a site.

**Request:**

```json
{
  "sourceSiteName": "source-site",
  "targetSiteName": "target-site",
  "includeContent": true
}
```

**Response:**

```json
{
  "status": "success",
  "message": "Site cloned successfully",
  "site": {
    "name": "target-site",
    "url": "https://target-site.example.com"
  }
}
```

#### Create Site

**Endpoint:** `POST /batch/sr/site/create`

**Required Role:** `System`, `RootAdmin`, `RootPowerUser`, `Owner`, `ConfigAdmin`

**Description:** Creates a site with repository, hosting, and deployment.

**Request:**

```json
{
  "siteName": "new-site",
  "template": "default-template",
  "domain": "new-site.example.com",
  "settings": {
    "theme": "default",
    "logo": "https://example.com/logo.png"
  }
}
```

**Response:**

```json
{
  "status": "success",
  "message": "Site created successfully",
  "site": {
    "name": "new-site",
    "url": "https://new-site.example.com",
    "repository": {
      "name": "new-site",
      "url": "https://git.example.com/new-site"
    }
  }
}
```

#### Create Organization

**Endpoint:** `POST /batch/sr/org/create`

**Required Role:** `System`, `RootAdmin`, `RootPowerUser`

**Description:** Creates a new organization, site, and repository.

**Request:**

```json
{
  "orgName": "new-org",
  "siteName": "new-site",
  "template": "default-template",
  "domain": "new-org.example.com",
  "settings": {
    "theme": "default",
    "logo": "https://example.com/logo.png"
  }
}
```

**Response:**

```json
{
  "status": "success",
  "message": "Organization created successfully",
  "organization": {
    "name": "new-org",
    "id": "org123"
  },
  "site": {
    "name": "new-site",
    "url": "https://new-org.example.com",
    "repository": {
      "name": "new-site",
      "url": "https://git.example.com/new-org/new-site"
    }
  }
}
```

## Kubernetes Management

### Service Management

#### Delete Service

**Endpoint:** `DELETE /batch/kube/delete/service/:namespace/:kind/:name`

**Required Role:** `System`, `RootAdmin`, `RootPowerUser`

**Description:** Deletes a Kubernetes service.

**Path Parameters:**

- `namespace`: The Kubernetes namespace
- `kind`: The Kubernetes resource kind
- `name`: The service name

**Response:**

```json
{
  "status": "success",
  "message": "Service deleted successfully"
}
```

#### Delete Site Services

**Endpoint:** `DELETE /batch/kube/delete/site/:namespace/:siteName`

**Required Role:** `System`, `RootAdmin`, `RootPowerUser`

**Description:** Deletes all services for a site.

**Path Parameters:**

- `namespace`: The Kubernetes namespace
- `siteName`: The site name

**Query Parameters:**

- `services` (optional): Comma-separated list of services to delete

**Response:**

```json
{
  "status": "success",
  "message": "Site services deleted successfully",
  "deletedServices": ["service1", "service2"]
}
```

### Namespace Management

#### Delete Namespace

**Endpoint:** `DELETE /batch/kube/delete/namespace/:name`

**Required Role:** `System`, `RootAdmin`, `RootPowerUser`

**Description:** Deletes a Kubernetes namespace.

**Path Parameters:**

- `name`: The namespace name

**Response:**

```json
{
  "status": "success",
  "message": "Namespace deleted successfully"
}
```

#### Get Pods in Namespace

**Endpoint:** `GET /batch/kube/namespaces/:namespaceName/pods`

**Required Role:** `System`, `RootAdmin`, `RootPowerUser`

**Description:** Gets all pods in a namespace.

**Path Parameters:**

- `namespaceName`: The namespace name

**Response:**

```json
{
  "items": [
    {
      "metadata": {
        "name": "pod1",
        "namespace": "example-namespace",
        "labels": {
          "app": "example-app"
        }
      },
      "status": {
        "phase": "Running",
        "conditions": [
          {
            "type": "Ready",
            "status": "True"
          }
        ]
      }
    }
  ]
}
```

#### Get Namespace

**Endpoint:** `GET /batch/kube/namespaces/:namespaceName`

**Required Role:** `System`, `RootAdmin`, `RootPowerUser`

**Description:** Gets a namespace.

**Path Parameters:**

- `namespaceName`: The namespace name

**Response:**

```json
{
  "metadata": {
    "name": "example-namespace",
    "labels": {
      "name": "example"
    }
  },
  "status": {
    "phase": "Active"
  }
}
```

#### List Namespaces

**Endpoint:** `GET /batch/kube/namespaces`

**Description:** Lists all namespaces.

**Response:**

```json
[
  "default",
  "kube-system",
  "example-namespace"
]
```

### Resource Status

#### Check Resource Status

**Endpoint:** `GET /batch/kube/check-status/:namespace/:kind/:resource`

**Required Role:** `System`, `RootAdmin`, `RootPowerUser`

**Description:** Checks the status of a Kubernetes resource.

**Path Parameters:**

- `namespace`: The namespace name
- `kind`: The resource kind
- `resource`: The resource name

**Response:**

```json
{
  "status": "Running",
  "conditions": [
    {
      "type": "Ready",
      "status": "True"
    }
  ],
  "message": "Resource is running"
}
