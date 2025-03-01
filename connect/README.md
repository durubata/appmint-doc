# Connect API

The Connect API provides endpoints for integrating with external services and partners, including webhook handling and OAuth authentication.

## Table of Contents

- [Webhook Integration](#webhook-integration)
- [OAuth Authentication](#oauth-authentication)

## Webhook Integration

### Handle Webhook (POST)

**Endpoint:** `POST /connect/webhook/:vendor/:serviceId?`

**Public Route:** Yes

**Description:** Handles webhook POST requests from external vendors and services.

**Path Parameters:**

- `vendor`: The vendor or service name (e.g., 'stripe', 'shopify', 'github')
- `serviceId` (optional): The service ID for the specific integration

**Headers:**

- `orgid`: The organization ID

**Request:**

```json
{
  "event": "payment.succeeded",
  "data": {
    "id": "evt_123",
    "object": "event",
    "created": 1610000000,
    "data": {
      "object": {
        "id": "ch_123",
        "object": "charge",
        "amount": 2000,
        "currency": "usd",
        "status": "succeeded"
      }
    }
  }
}
```

**Response:**

```json
{
  "success": true,
  "message": "Webhook processed successfully",
  "data": {
    "id": "evt_123",
    "status": "processed"
  }
}
```

### Handle Webhook (GET)

**Endpoint:** `GET /connect/webhook/:vendor/:serviceId?`

**Public Route:** Yes

**Description:** Handles webhook GET requests from external vendors and services. This is often used for webhook verification or subscription.

**Path Parameters:**

- `vendor`: The vendor or service name (e.g., 'facebook', 'twitter', 'slack')
- `serviceId` (optional): The service ID for the specific integration

**Headers:**

- `orgid`: The organization ID

**Query Parameters:**

- Vendor-specific query parameters for verification

**Response:**

```json
{
  "success": true,
  "message": "Webhook verified successfully",
  "challenge": "challenge_token"
}
```

## OAuth Authentication

### OAuth Callback

**Endpoint:** `GET /connect/oauth2callback/:vendor`

**Public Route:** Yes

**Description:** Handles OAuth callback requests from external vendors and services.

**Path Parameters:**

- `vendor`: The vendor or service name (e.g., 'google', 'facebook', 'github')

**Headers:**

- `orgid`: The organization ID

**Query Parameters:**

- `code`: The authorization code from the OAuth provider
- `state`: The state parameter for security verification
- Additional vendor-specific parameters

**Response:**
Redirects to the specified redirect URL with authentication tokens or error information.

## Supported Vendors

The Connect API supports integration with various vendors and services, including:

### Payment Processors

- Stripe
- PayPal
- Square

### E-commerce Platforms

- Shopify
- WooCommerce
- BigCommerce

### Social Media

- Facebook
- Twitter
- Instagram
- LinkedIn

### Marketing

- Mailchimp
- HubSpot
- Klaviyo

### CRM

- Salesforce
- Zoho
- HubSpot CRM

### Communication

- Slack
- Discord
- Twilio

### File Storage

- Dropbox
- Google Drive
- OneDrive

### Development

- GitHub
- GitLab
- Bitbucket

## Integration Process

### 1. Configure Integration

Before using the Connect API, you need to configure the integration in the ContentMint AppEngine:

1. Go to the Integrations section in the admin panel
2. Select the vendor you want to integrate with
3. Enter the required credentials (API keys, client ID, client secret, etc.)
4. Save the configuration

### 2. Set Up Webhooks

For webhook-based integrations:

1. Configure the webhook URL in the vendor's dashboard:

   ```
   https://your-instance.contentmint.com/connect/webhook/{vendor}/{serviceId}
   ```

2. Select the events you want to receive
3. Save the webhook configuration

### 3. Implement OAuth (if required)

For OAuth-based integrations:

1. Configure the OAuth callback URL in the vendor's dashboard:

   ```
   https://your-instance.contentmint.com/connect/oauth2callback/{vendor}
   ```

2. Implement the authentication flow in your application
3. Handle the OAuth callback

## Error Handling

The Connect API returns appropriate HTTP status codes and error messages in case of failure:

```json
{
  "success": false,
  "error": {
    "code": "invalid_request",
    "message": "Invalid webhook signature"
  }
}
```

Common error scenarios:

- Invalid webhook signature
- Missing or invalid credentials
- Rate limiting
- Service unavailable
- Invalid OAuth state parameter
