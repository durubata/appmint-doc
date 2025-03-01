# Upstream API

The Upstream API provides endpoints for managing integrations with external services, including configuration, operation calls, and testing.

## Table of Contents

- [Integration Types](#integration-types)
- [Integration Configuration](#integration-configuration)
- [Active Integrations](#active-integrations)
- [Integration Operations](#integration-operations)
- [Integration Management](#integration-management)

## Integration Types

### Get Integration Types

**Endpoint:** `GET /upstream/integration-types/:type?`

**Description:** Gets available integration types, optionally filtered by type.

**Path Parameters:**

- `type` (optional): The integration type to filter by

**Headers:**

- `orgid`: The organization ID

**Response:**

```json
{
  "types": [
    {
      "id": "payment",
      "name": "Payment Processor",
      "description": "Integrations for processing payments",
      "providers": [
        {
          "id": "stripe",
          "name": "Stripe",
          "description": "Stripe payment processing",
          "icon": "https://example.com/icons/stripe.png"
        },
        {
          "id": "paypal",
          "name": "PayPal",
          "description": "PayPal payment processing",
          "icon": "https://example.com/icons/paypal.png"
        }
      ]
    },
    {
      "id": "shipping",
      "name": "Shipping Provider",
      "description": "Integrations for shipping and fulfillment",
      "providers": [
        {
          "id": "fedex",
          "name": "FedEx",
          "description": "FedEx shipping services",
          "icon": "https://example.com/icons/fedex.png"
        },
        {
          "id": "ups",
          "name": "UPS",
          "description": "UPS shipping services",
          "icon": "https://example.com/icons/ups.png"
        }
      ]
    }
  ]
}
```

## Integration Configuration

### Get Integration Configuration

**Endpoint:** `GET /upstream/get-config/:type?/:configId?`

**Description:** Gets the configuration for a specific integration type and ID.

**Path Parameters:**

- `type` (optional): The integration type
- `configId` (optional): The configuration ID

**Query Parameters:**

- `create` (optional): Whether to create a new configuration if one doesn't exist (default: false)

**Headers:**

- `orgid`: The organization ID

**Response:**

```json
{
  "id": "config123",
  "type": "payment",
  "provider": "stripe",
  "name": "Stripe Payment Processing",
  "description": "Stripe integration for processing payments",
  "config": {
    "apiKey": "sk_test_...",
    "webhookSecret": "whsec_...",
    "environment": "test"
  },
  "status": "active",
  "createdAt": "2023-01-01T12:00:00Z",
  "updatedAt": "2023-01-02T10:30:00Z"
}
```

### Save Integration

**Endpoint:** `POST /upstream/save-integration`

**Description:** Saves an integration configuration.

**Headers:**

- `orgid`: The organization ID

**Request:**

```json
{
  "type": "payment",
  "provider": "stripe",
  "name": "Stripe Payment Processing",
  "description": "Stripe integration for processing payments",
  "config": {
    "apiKey": "sk_test_...",
    "webhookSecret": "whsec_...",
    "environment": "test"
  }
}
```

**Response:**

```json
{
  "success": true,
  "integration": {
    "id": "config123",
    "type": "payment",
    "provider": "stripe",
    "name": "Stripe Payment Processing",
    "description": "Stripe integration for processing payments",
    "status": "active",
    "createdAt": "2023-01-01T12:00:00Z",
    "updatedAt": "2023-01-01T12:00:00Z"
  }
}
```

## Active Integrations

### List Active Integrations by Type

**Endpoint:** `GET /upstream/active/:type?`

**Description:** Lists active integrations, optionally filtered by type.

**Path Parameters:**

- `type` (optional): The integration type to filter by

**Headers:**

- `orgid`: The organization ID

**Response:**

```json
{
  "integrations": [
    {
      "id": "integration123",
      "type": "payment",
      "provider": "stripe",
      "name": "Stripe Payment Processing",
      "status": "active",
      "lastUsed": "2023-01-01T12:00:00Z"
    },
    {
      "id": "integration456",
      "type": "shipping",
      "provider": "fedex",
      "name": "FedEx Shipping",
      "status": "active",
      "lastUsed": "2023-01-01T11:30:00Z"
    }
  ]
}
```

### Get Active Integration Details

**Endpoint:** `GET /upstream/active/detail/:id?`

**Description:** Gets detailed information about active integrations, optionally filtered by ID.

**Path Parameters:**

- `id` (optional): The integration ID to filter by

**Headers:**

- `orgid`: The organization ID

**Response:**

```json
{
  "integrations": [
    {
      "id": "integration123",
      "type": "payment",
      "provider": "stripe",
      "name": "Stripe Payment Processing",
      "description": "Stripe integration for processing payments",
      "status": "active",
      "config": {
        "environment": "test",
        "webhookEnabled": true
      },
      "operations": [
        {
          "id": "create-payment",
          "name": "Create Payment",
          "description": "Create a new payment"
        },
        {
          "id": "refund-payment",
          "name": "Refund Payment",
          "description": "Refund an existing payment"
        }
      ],
      "metrics": {
        "totalCalls": 150,
        "successfulCalls": 145,
        "failedCalls": 5,
        "averageResponseTime": 250
      },
      "lastUsed": "2023-01-01T12:00:00Z",
      "createdAt": "2022-12-01T00:00:00Z",
      "updatedAt": "2023-01-01T12:00:00Z"
    }
  ]
}
```

### Get Integration

**Endpoint:** `POST /upstream/get-integration`

**Description:** Gets information about a specific integration.

**Headers:**

- `orgid`: The organization ID

**Request:**

```json
{
  "id": "integration123"
}
```

**Response:**

```json
{
  "id": "integration123",
  "type": "payment",
  "provider": "stripe",
  "name": "Stripe Payment Processing",
  "description": "Stripe integration for processing payments",
  "status": "active",
  "config": {
    "environment": "test",
    "webhookEnabled": true
  },
  "operations": [
    {
      "id": "create-payment",
      "name": "Create Payment",
      "description": "Create a new payment"
    },
    {
      "id": "refund-payment",
      "name": "Refund Payment",
      "description": "Refund an existing payment"
    }
  ],
  "createdAt": "2022-12-01T00:00:00Z",
  "updatedAt": "2023-01-01T12:00:00Z"
}
```

## Integration Operations

### Call Integration Operation (POST)

**Endpoint:** `POST /upstream/call/:integration/:operation`

**Description:** Calls an operation on an integration.

**Path Parameters:**

- `integration`: The integration ID or name
- `operation`: The operation ID or name

**Headers:**

- `orgid`: The organization ID

**Request:**

```json
{
  "amount": 1000,
  "currency": "usd",
  "description": "Test payment",
  "source": "tok_visa"
}
```

**Response:**

```json
{
  "success": true,
  "result": {
    "id": "ch_123",
    "object": "charge",
    "amount": 1000,
    "currency": "usd",
    "status": "succeeded",
    "description": "Test payment",
    "source": {
      "id": "card_123",
      "object": "card",
      "brand": "Visa",
      "last4": "4242"
    }
  }
}
```

### Call Integration Operation (GET)

**Endpoint:** `GET /upstream/call/:integration/:operation`

**Description:** Calls a GET operation on an integration.

**Path Parameters:**

- `integration`: The integration ID or name
- `operation`: The operation ID or name

**Headers:**

- `orgid`: The organization ID

**Response:**

```json
{
  "success": true,
  "result": {
    "balance": {
      "available": [
        {
          "amount": 10000,
          "currency": "usd"
        }
      ],
      "pending": [
        {
          "amount": 500,
          "currency": "usd"
        }
      ]
    }
  }
}
```

### Test Integration Operation

**Endpoint:** `POST /upstream/test/:integration/:operation`

**Description:** Tests an operation on an integration without actually executing it.

**Path Parameters:**

- `integration`: The integration ID or name
- `operation`: The operation ID or name

**Headers:**

- `orgid`: The organization ID

**Request:**

```json
{
  "amount": 1000,
  "currency": "usd",
  "description": "Test payment",
  "source": "tok_visa"
}
```

**Response:**

```json
{
  "success": true,
  "message": "Test successful",
  "validRequest": true,
  "expectedResponse": {
    "id": "ch_test",
    "object": "charge",
    "amount": 1000,
    "currency": "usd",
    "status": "succeeded"
  }
}
```

## Integration Management

### Shutdown Integration

**Endpoint:** `POST /upstream/shutdown/:id`

**Description:** Shuts down an active integration.

**Path Parameters:**

- `id`: The integration ID

**Headers:**

- `orgid`: The organization ID

**Response:**

```json
{
  "success": true,
  "message": "Integration shutdown successfully",
  "integration": {
    "id": "integration123",
    "status": "inactive",
    "shutdownAt": "2023-01-02T12:00:00Z"
  }
}
```

## Integration Providers

The Upstream API supports various integration providers, including:

### Payment Processors

- Stripe
- PayPal
- Square
- Braintree
- Authorize.Net

### Shipping Providers

- FedEx
- UPS
- USPS
- DHL
- ShipStation

### E-commerce Platforms

- Shopify
- WooCommerce
- Magento
- BigCommerce
- Etsy

### Marketing Platforms

- Mailchimp
- HubSpot
- Klaviyo
- Constant Contact
- SendGrid

### CRM Systems

- Salesforce
- HubSpot CRM
- Zoho CRM
- Microsoft Dynamics
- Pipedrive

### Accounting Systems

- QuickBooks
- Xero
- FreshBooks
- Sage
- Wave

## Error Handling

The Upstream API returns appropriate HTTP status codes and error messages in case of failure:

```json
{
  "success": false,
  "error": {
    "code": "integration_error",
    "message": "Failed to connect to integration service",
    "details": {
      "reason": "Authentication failed",
      "suggestion": "Check your API credentials"
    }
  }
}
```

Common error scenarios:

- Integration not found
- Operation not found
- Invalid request parameters
- Integration service unavailable
- Authentication failure
- Rate limiting
