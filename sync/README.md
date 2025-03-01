# Sync API

The Sync API provides endpoints for data synchronization, job queue management, and synchronization status information.

## Table of Contents

- [Synchronization Information](#synchronization-information)
- [Job Queue Management](#job-queue-management)
- [Synchronization Control](#synchronization-control)

## Synchronization Information

### Get Sync Info

**Endpoint:** `GET /sync`

**Description:** Gets general synchronization information.

**Response:**

```json
{
  "version": "1.0.0",
  "status": "active",
  "activeJobs": 5,
  "completedJobs": 120,
  "failedJobs": 2,
  "uptime": "5d 12h 30m"
}
```

### Get Organization Sync Info

**Endpoint:** `GET /sync/info`

**Required Permission:** `PermissionTypeContent.read`

**Description:** Gets synchronization information for a specific organization.

**Headers:**

- `orgid`: The organization ID

**Response:**

```json
{
  "organization": {
    "id": "org123",
    "name": "Example Organization"
  },
  "syncStatus": {
    "lastSync": "2023-01-01T12:00:00Z",
    "activeJobs": 2,
    "completedJobs": 45,
    "failedJobs": 1
  },
  "syncJobs": [
    {
      "id": "job123",
      "name": "product-sync",
      "status": "active",
      "progress": 75,
      "startedAt": "2023-01-01T12:00:00Z",
      "estimatedCompletion": "2023-01-01T12:15:00Z"
    },
    {
      "id": "job456",
      "name": "order-sync",
      "status": "queued",
      "progress": 0,
      "queuedAt": "2023-01-01T12:05:00Z"
    }
  ]
}
```

## Job Queue Management

### Queue Job

**Endpoint:** `POST /sync/queue`

**Required Permission:** `PermissionTypeContent.update`

**Description:** Queues a new synchronization job.

**Headers:**

- `orgid`: The organization ID

**Request:**

```json
{
  "name": "product-sync",
  "type": "full",
  "priority": "high",
  "data": {
    "source": "shopify",
    "target": "local",
    "filters": {
      "updatedSince": "2023-01-01T00:00:00Z",
      "collections": ["summer-collection", "new-arrivals"]
    }
  },
  "options": {
    "concurrency": 5,
    "retryOnFail": true,
    "maxRetries": 3
  }
}
```

**Response:**

```json
{
  "success": true,
  "job": {
    "id": "job789",
    "name": "product-sync",
    "status": "queued",
    "queuedAt": "2023-01-01T12:10:00Z",
    "estimatedStart": "2023-01-01T12:12:00Z"
  }
}
```

### Purge Sync Jobs

**Endpoint:** `GET /sync/purge`

**Required Permission:** `PermissionTypeContent.delete`

**Description:** Purges all completed and failed synchronization jobs for an organization.

**Headers:**

- `orgid`: The organization ID

**Response:**

```json
{
  "success": true,
  "message": "Sync jobs purged successfully",
  "purgedJobs": {
    "completed": 45,
    "failed": 1
  }
}
```

## Synchronization Control

### Pause Sync Job

**Endpoint:** `GET /sync/pause/:name`

**Description:** Pauses a running synchronization job.

**Headers:**

- `orgid`: The organization ID

**Path Parameters:**

- `name`: The job name

**Response:**

```json
{
  "success": true,
  "message": "Sync job paused successfully",
  "job": {
    "id": "job123",
    "name": "product-sync",
    "status": "paused",
    "progress": 75,
    "pausedAt": "2023-01-01T12:08:00Z"
  }
}
```

### Resume Sync Job

**Endpoint:** `GET /sync/resume/:name`

**Description:** Resumes a paused synchronization job.

**Headers:**

- `orgid`: The organization ID

**Path Parameters:**

- `name`: The job name

**Response:**

```json
{
  "success": true,
  "message": "Sync job resumed successfully",
  "job": {
    "id": "job123",
    "name": "product-sync",
    "status": "active",
    "progress": 75,
    "resumedAt": "2023-01-01T12:10:00Z",
    "estimatedCompletion": "2023-01-01T12:15:00Z"
  }
}
```

## Synchronization Types

The Sync API supports various types of synchronization jobs:

### Full Sync

A full synchronization that processes all data.

```json
{
  "name": "full-sync",
  "type": "full",
  "data": {
    "source": "shopify",
    "target": "local"
  }
}
```

### Incremental Sync

An incremental synchronization that only processes data that has changed since the last sync.

```json
{
  "name": "incremental-sync",
  "type": "incremental",
  "data": {
    "source": "shopify",
    "target": "local",
    "filters": {
      "updatedSince": "2023-01-01T00:00:00Z"
    }
  }
}
```

### Entity Sync

A synchronization for specific entities or collections.

```json
{
  "name": "product-sync",
  "type": "entity",
  "data": {
    "source": "shopify",
    "target": "local",
    "entity": "products",
    "filters": {
      "collections": ["summer-collection"]
    }
  }
}
```

### Scheduled Sync

A synchronization that is scheduled to run at a specific time.

```json
{
  "name": "scheduled-sync",
  "type": "scheduled",
  "schedule": {
    "startAt": "2023-01-15T00:00:00Z",
    "repeat": "daily",
    "time": "00:00"
  },
  "data": {
    "source": "shopify",
    "target": "local"
  }
}
```

## Error Handling

The Sync API returns appropriate HTTP status codes and error messages in case of failure:

```json
{
  "statusCode": 400,
  "message": "Empty Data",
  "error": "Bad Request"
}
```

Common error scenarios:

- Empty request body
- Invalid job configuration
- Job not found
- Insufficient permissions
