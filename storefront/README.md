# Storefront API

The Storefront API provides endpoints for e-commerce functionality, including products, collections, brands, shopping carts, orders, and payment processing.

## Table of Contents

- [Search and Find](#search-and-find)
- [Products](#products)
- [Brands](#brands)
- [Collections](#collections)
- [Categories](#categories)
- [Attributes](#attributes)
- [Cart Management](#cart-management)
- [Orders](#orders)
- [Subscriptions](#subscriptions)
- [Payment Processing](#payment-processing)
- [Checkout](#checkout)
- [Coupons and Discounts](#coupons-and-discounts)

## Search and Find

### Search

**Endpoint:** `POST /storefront/search`

**Public Route:** Yes

**Description:** Searches for products and other storefront items.

**Request:**

```json
{
  "query": {
    "keyword": "t-shirt",
    "category": "clothing"
  },
  "options": {
    "limit": 10,
    "skip": 0,
    "sort": {
      "field": "price",
      "order": "asc"
    }
  }
}
```

**Response:**

```json
{
  "results": [
    {
      "id": "product123",
      "name": "Cotton T-Shirt",
      "description": "Comfortable cotton t-shirt",
      "price": 19.99,
      "category": "clothing",
      "images": ["https://example.com/images/tshirt1.jpg"]
    },
    {
      "id": "product456",
      "name": "Graphic T-Shirt",
      "description": "T-shirt with graphic print",
      "price": 24.99,
      "category": "clothing",
      "images": ["https://example.com/images/tshirt2.jpg"]
    }
  ],
  "total": 42,
  "page": 1,
  "pageSize": 10
}
```

### Find

**Endpoint:** `POST /storefront/find`

**Public Route:** Yes

**Description:** Finds products and other storefront items based on specific criteria.

**Request:**

```json
{
  "query": {
    "brand": "acme",
    "inStock": true
  },
  "options": {
    "limit": 10,
    "skip": 0,
    "sort": {
      "field": "createdAt",
      "order": "desc"
    }
  }
}
```

**Response:**

```json
{
  "results": [
    {
      "id": "product789",
      "name": "Acme Running Shoes",
      "description": "Professional running shoes",
      "price": 89.99,
      "brand": "acme",
      "inStock": true,
      "images": ["https://example.com/images/shoes1.jpg"]
    }
  ],
  "total": 15,
  "page": 1,
  "pageSize": 10
}
```

## Products

### Get Products

**Endpoint:** `GET /storefront/products`

**Public Route:** Yes

**Description:** Gets a list of products with optional filtering and pagination.

**Query Parameters:**

- `l` (optional): Last item ID for pagination
- `p` (optional): Page number (default: 1)
- `ps` (optional): Page size (default: 50)
- `s` (optional): Sort field (default: 'modifydate')
- `st` (optional): Sort type (default: 'desc')
- Additional query parameters for filtering (e.g., `category`, `brand`, `price_min`, `price_max`)

**Response:**

```json
{
  "products": [
    {
      "id": "product123",
      "name": "Cotton T-Shirt",
      "description": "Comfortable cotton t-shirt",
      "price": 19.99,
      "category": "clothing",
      "images": ["https://example.com/images/tshirt1.jpg"],
      "variants": [
        {
          "id": "variant1",
          "name": "Small / Blue",
          "attributes": {
            "size": "S",
            "color": "Blue"
          },
          "price": 19.99,
          "sku": "TS-S-BLU",
          "inStock": true,
          "quantity": 25
        }
      ]
    }
  ],
  "total": 150,
  "page": 1,
  "pageSize": 50
}
```

### Search Products

**Endpoint:** `GET /storefront/search/products`

**Public Route:** Yes

**Description:** Searches for products with full-text search capabilities.

**Query Parameters:**

- `q` (required): Search query
- `l` (optional): Last item ID for pagination
- `p` (optional): Page number (default: 1)
- `ps` (optional): Page size (default: 50)
- `s` (optional): Sort field (default: 'modifydate')
- `st` (optional): Sort type (default: 'desc')

**Response:**

```json
{
  "products": [
    {
      "id": "product123",
      "name": "Cotton T-Shirt",
      "description": "Comfortable cotton t-shirt",
      "price": 19.99,
      "category": "clothing",
      "images": ["https://example.com/images/tshirt1.jpg"]
    }
  ],
  "total": 5,
  "page": 1,
  "pageSize": 50
}
```

### Get Products by Category

**Endpoint:** `GET /storefront/products/category/:category`

**Public Route:** Yes

**Description:** Gets products by category.

**Path Parameters:**

- `category`: The category ID or slug

**Query Parameters:**

- `category` (optional): Additional categories to include (comma-separated)
- `l` (optional): Last item ID for pagination
- `p` (optional): Page number (default: 1)
- `ps` (optional): Page size (default: 50)
- `s` (optional): Sort field (default: 'modifydate')
- `st` (optional): Sort type (default: 'desc')

**Response:**

```json
{
  "products": [
    {
      "id": "product123",
      "name": "Cotton T-Shirt",
      "description": "Comfortable cotton t-shirt",
      "price": 19.99,
      "category": "clothing",
      "images": ["https://example.com/images/tshirt1.jpg"]
    }
  ],
  "total": 42,
  "page": 1,
  "pageSize": 50
}
```

### Get Product by ID

**Endpoint:** `GET /storefront/product/:id`

**Public Route:** Yes

**Description:** Gets a product by its ID or slug.

**Path Parameters:**

- `id`: The product ID or slug

**Response:**

```json
{
  "id": "product123",
  "name": "Cotton T-Shirt",
  "description": "Comfortable cotton t-shirt",
  "price": 19.99,
  "category": "clothing",
  "images": ["https://example.com/images/tshirt1.jpg"],
  "variants": [
    {
      "id": "variant1",
      "name": "Small / Blue",
      "attributes": {
        "size": "S",
        "color": "Blue"
      },
      "price": 19.99,
      "sku": "TS-S-BLU",
      "inStock": true,
      "quantity": 25
    },
    {
      "id": "variant2",
      "name": "Medium / Blue",
      "attributes": {
        "size": "M",
        "color": "Blue"
      },
      "price": 19.99,
      "sku": "TS-M-BLU",
      "inStock": true,
      "quantity": 15
    }
  ],
  "attributes": {
    "material": "100% Cotton",
    "care": "Machine wash cold"
  },
  "relatedProducts": ["product456", "product789"]
}
```

### Get Related Products

**Endpoint:** `GET /storefront/product/:id/related`

**Public Route:** Yes

**Description:** Gets products related to a specific product.

**Path Parameters:**

- `id`: The product ID or slug

**Response:**

```json
{
  "products": [
    {
      "id": "product456",
      "name": "Graphic T-Shirt",
      "description": "T-shirt with graphic print",
      "price": 24.99,
      "category": "clothing",
      "images": ["https://example.com/images/tshirt2.jpg"]
    },
    {
      "id": "product789",
      "name": "V-Neck T-Shirt",
      "description": "V-neck cotton t-shirt",
      "price": 22.99,
      "category": "clothing",
      "images": ["https://example.com/images/tshirt3.jpg"]
    }
  ]
}
```

## Brands

### Get Brands

**Endpoint:** `GET /storefront/brands/:brand?`

**Public Route:** Yes

**Description:** Gets a list of brands or a specific brand.

**Path Parameters:**

- `brand` (optional): The brand ID or slug

**Query Parameters:**

- `l` (optional): Last item ID for pagination
- `p` (optional): Page number (default: 1)
- `ps` (optional): Page size (default: 50)
- `s` (optional): Sort field (default: 'modifydate')
- `st` (optional): Sort type (default: 'desc')

**Response:**

```json
{
  "brands": [
    {
      "id": "brand123",
      "name": "Acme",
      "description": "Quality products for everyone",
      "logo": "https://example.com/images/acme-logo.jpg",
      "website": "https://acme.example.com"
    },
    {
      "id": "brand456",
      "name": "TechGear",
      "description": "Innovative tech products",
      "logo": "https://example.com/images/techgear-logo.jpg",
      "website": "https://techgear.example.com"
    }
  ],
  "total": 25,
  "page": 1,
  "pageSize": 50
}
```

## Collections

### Get Collections

**Endpoint:** `GET /storefront/collections/:collection?`

**Public Route:** Yes

**Description:** Gets a list of collections or a specific collection.

**Path Parameters:**

- `collection` (optional): The collection ID or slug

**Query Parameters:**

- `l` (optional): Last item ID for pagination
- `p` (optional): Page number (default: 1)
- `ps` (optional): Page size (default: 50)
- `s` (optional): Sort field (default: 'modifydate')
- `st` (optional): Sort type (default: 'desc')

**Response:**

```json
{
  "collections": [
    {
      "id": "collection123",
      "name": "Summer Sale",
      "description": "Hot deals for the summer",
      "image": "https://example.com/images/summer-sale.jpg",
      "startDate": "2023-06-01T00:00:00Z",
      "endDate": "2023-08-31T23:59:59Z",
      "productCount": 42
    },
    {
      "id": "collection456",
      "name": "New Arrivals",
      "description": "Latest products in our catalog",
      "image": "https://example.com/images/new-arrivals.jpg",
      "productCount": 15
    }
  ],
  "total": 10,
  "page": 1,
  "pageSize": 50
}
```

## Categories

### Get Categories

**Endpoint:** `GET /storefront/categories/:category?`

**Public Route:** Yes

**Description:** Gets a list of categories or a specific category.

**Path Parameters:**

- `category` (optional): The category ID or slug

**Response:**

```json
{
  "categories": [
    {
      "id": "category123",
      "name": "Clothing",
      "description": "Apparel and accessories",
      "image": "https://example.com/images/clothing.jpg",
      "parent": null,
      "children": [
        {
          "id": "category124",
          "name": "T-Shirts",
          "description": "All types of t-shirts",
          "image": "https://example.com/images/tshirts.jpg",
          "parent": "category123",
          "productCount": 50
        },
        {
          "id": "category125",
          "name": "Pants",
          "description": "Jeans, trousers, and more",
          "image": "https://example.com/images/pants.jpg",
          "parent": "category123",
          "productCount": 35
        }
      ],
      "productCount": 150
    }
  ]
}
```

## Attributes

### Get Attributes

**Endpoint:** `GET /storefront/attributes/:attribute?`

**Public Route:** Yes

**Description:** Gets a list of product attributes or a specific attribute.

**Path Parameters:**

- `attribute` (optional): The attribute ID or name

**Response:**

```json
{
  "attributes": [
    {
      "id": "attr123",
      "name": "size",
      "displayName": "Size",
      "type": "select",
      "values": ["XS", "S", "M", "L", "XL", "XXL"],
      "filterable": true,
      "visible": true
    },
    {
      "id": "attr456",
      "name": "color",
      "displayName": "Color",
      "type": "color",
      "values": [
        {"name": "Red", "value": "#ff0000"},
        {"name": "Blue", "value": "#0000ff"},
        {"name": "Green", "value": "#00ff00"}
      ],
      "filterable": true,
      "visible": true
    }
  ]
}
```

## Cart Management

### Get Cart

**Endpoint:** `GET /storefront/cart/get/:authorid/:cartid?`

**Public Route:** Yes

**Description:** Gets a shopping cart by author ID and optionally by cart ID.

**Path Parameters:**

- `authorid`: The author ID (customer ID)
- `cartid` (optional): The cart ID

**Response:**

```json
{
  "id": "cart123",
  "customer": {
    "id": "customer456",
    "email": "customer@example.com",
    "name": "John Doe"
  },
  "items": [
    {
      "id": "item123",
      "product": {
        "id": "product789",
        "name": "Cotton T-Shirt",
        "price": 19.99,
        "image": "https://example.com/images/tshirt1.jpg"
      },
      "variant": {
        "id": "variant1",
        "name": "Small / Blue",
        "attributes": {
          "size": "S",
          "color": "Blue"
        }
      },
      "quantity": 2,
      "price": 19.99,
      "total": 39.98
    }
  ],
  "subtotal": 39.98,
  "tax": 3.20,
  "shipping": 5.00,
  "discount": 0,
  "total": 48.18,
  "createdAt": "2023-01-01T12:00:00Z",
  "updatedAt": "2023-01-01T12:15:00Z"
}
```

### Update Cart

**Endpoint:** `POST /storefront/cart/update/:cartid`

**Public Route:** Yes

**Description:** Updates a shopping cart.

**Path Parameters:**

- `cartid`: The cart ID

**Request:**

```json
{
  "items": [
    {
      "productId": "product789",
      "variantId": "variant1",
      "quantity": 3
    }
  ]
}
```

**Response:**

```json
{
  "id": "cart123",
  "items": [
    {
      "id": "item123",
      "product": {
        "id": "product789",
        "name": "Cotton T-Shirt",
        "price": 19.99,
        "image": "https://example.com/images/tshirt1.jpg"
      },
      "variant": {
        "id": "variant1",
        "name": "Small / Blue",
        "attributes": {
          "size": "S",
          "color": "Blue"
        }
      },
      "quantity": 3,
      "price": 19.99,
      "total": 59.97
    }
  ],
  "subtotal": 59.97,
  "tax": 4.80,
  "shipping": 5.00,
  "discount": 0,
  "total": 69.77,
  "updatedAt": "2023-01-01T12:30:00Z"
}
```

### Clear Cart

**Endpoint:** `GET /storefront/cart/clear/:cartid`

**Public Route:** Yes

**Description:** Clears all items from a shopping cart.

**Path Parameters:**

- `cartid`: The cart ID

**Response:**

```json
{
  "id": "cart123",
  "items": [],
  "subtotal": 0,
  "tax": 0,
  "shipping": 0,
  "discount": 0,
  "total": 0,
  "updatedAt": "2023-01-01T12:45:00Z"
}
```

## Orders

### Get Order by Email

**Endpoint:** `GET /storefront/order/email/:email/:orderNumber`

**Public Route:** Yes

**Description:** Gets an order by customer email and order number.

**Path Parameters:**

- `email`: The customer email
- `orderNumber`: The order number

**Response:**

```json
{
  "id": "order123",
  "number": "ORD-001",
  "customer": {
    "email": "customer@example.com",
    "name": "John Doe"
  },
  "items": [
    {
      "product": {
        "id": "product789",
        "name": "Cotton T-Shirt",
        "price": 19.99,
        "image": "https://example.com/images/tshirt1.jpg"
      },
      "variant": {
        "id": "variant1",
        "name": "Small / Blue"
      },
      "quantity": 2,
      "price": 19.99,
      "total": 39.98
    }
  ],
  "subtotal": 39.98,
  "tax": 3.20,
  "shipping": 5.00,
  "discount": 0,
  "total": 48.18,
  "status": "processing",
  "paymentStatus": "paid",
  "shippingAddress": {
    "name": "John Doe",
    "address1": "123 Main St",
    "city": "Anytown",
    "state": "CA",
    "postalCode": "12345",
    "country": "US"
  },
  "billingAddress": {
    "name": "John Doe",
    "address1": "123 Main St",
    "city": "Anytown",
    "state": "CA",
    "postalCode": "12345",
    "country": "US"
  },
  "createdAt": "2023-01-01T12:00:00Z",
  "updatedAt": "2023-01-01T12:15:00Z"
}
```

### Get Order

**Endpoint:** `GET /storefront/order/get/:author/:orderNumber`

**Public Route:** Yes

**Description:** Gets an order by author ID and order number.

**Path Parameters:**

- `author`: The author ID (customer ID)
- `orderNumber`: The order number

**Response:**

```json
{
  "id": "order123",
  "number": "ORD-001",
  "customer": {
    "id": "customer456",
    "email": "customer@example.com",
    "name": "John Doe"
  },
  "items": [
    {
      "product": {
        "id": "product789",
        "name": "Cotton T-Shirt",
        "price": 19.99,
        "image": "https://example.com/images/tshirt1.jpg"
      },
      "variant": {
        "id": "variant1",
        "name": "Small / Blue"
      },
      "quantity": 2,
      "price": 19.99,
      "total": 39.98
    }
  ],
  "subtotal": 39.98,
  "tax": 3.20,
  "shipping": 5.00,
  "discount": 0,
  "total": 48.18,
  "status": "processing",
  "paymentStatus": "paid",
  "shippingAddress": {
    "name": "John Doe",
    "address1": "123 Main St",
    "city": "Anytown",
    "state": "CA",
    "postalCode": "12345",
    "country": "US"
  },
  "billingAddress": {
    "name": "John Doe",
    "address1": "123 Main St",
    "city": "Anytown",
    "state": "CA",
    "postalCode": "12345",
    "country": "US"
  },
  "createdAt": "2023-01-01T12:00:00Z",
  "updatedAt": "2023-01-01T12:15:00Z"
}
```

### Get Orders

**Endpoint:** `GET /storefront/orders/get/:author`

**Public Route:** Yes

**Description:** Gets all orders for a specific author (customer).

**Path Parameters:**

- `author`: The author ID (customer ID)

**Response:**

```json
{
  "orders": [
    {
      "id": "order123",
      "number": "ORD-001",
      "total": 48.18,
      "status": "processing",
      "paymentStatus": "paid",
      "createdAt": "2023-01-01T12:00:00Z"
    },
    {
      "id": "order456",
      "number": "ORD-002",
      "total": 75.50,
      "status": "shipped",
      "paymentStatus": "paid",
      "createdAt": "2022-12-15T10:30:00Z"
    }
  ]
}
```

### Remove Order

**Endpoint:** `GET /storefront/order/remove/:author/:orderNumber?`

**Public Route:** Yes

**Description:** Removes an order or all orders for a specific author.

**Path Parameters:**

- `author`: The author ID (customer ID)
- `orderNumber` (optional): The order number

**Response:**

```json
{
  "success": true,
  "message": "Order(s) removed successfully"
}
```

### Send Welcome Email

**Endpoint:** `GET /storefront/order/send-welcome/:orderNumber`

**Public Route:** Yes

**Description:** Sends a welcome email to the customer for a specific order.

**Path Parameters:**

- `orderNumber`: The order number

**Response:**

```json
{
  "success": true,
  "message": "Welcome email sent successfully"
}
```

### Refund Order

**Endpoint:** `POST /storefront/order/refund/:orderNumber`

**Public Route:** Yes

**Description:** Processes a refund for a specific order.

**Path Parameters:**

- `orderNumber`: The order number

**Request:**

```json
{
  "amount": 48.18,
  "reason": "Customer request",
  "items": [
    {
      "id": "item123",
      "quantity": 2
    }
  ]
}
```

**Response:**

```json
{
  "success": true,
  "refund": {
    "id": "refund123",
    "orderNumber": "ORD-001",
    "amount": 48.18,
    "reason": "Customer request",
    "status": "processed",
    "createdAt": "2023-01-05T14:30:00Z"
  }
}
```

## Subscriptions

### Get Subscriptions

**Endpoint:** `GET /storefront/subscriptions/get/:author/:subscriptionid?`

**Public Route:** Yes

**Description:** Gets subscriptions for a specific author, optionally filtered by subscription ID.

**Path Parameters:**

- `author`: The author ID (customer ID)
- `subscriptionid` (optional): The subscription ID

**Query Parameters:**

- `enrich` (optional): Whether to enrich the response with additional data (default: false)

**Response:**

```json
{
  "subscriptions": [
    {
      "id": "sub123",
      "customer": {
        "id": "customer456",
        "email": "customer@example.com",
        "name": "John Doe"
      },
      "plan": {
        "id": "plan789",
        "name": "Premium Plan",
        "price": 19.99,
        "interval": "month",
        "features": ["Feature 1", "Feature 2", "Feature 3"]
      },
      "status": "active",
      "startDate": "2023-01-01T00:00:00Z",
      "nextBillingDate": "2023-02-01T00:00:00Z",
      "paymentMethod": {
        "type": "card",
        "last4": "4242",
        "brand": "visa",
        "expMonth": 12,
        "expYear": 2025
      }
    }
  ]
}
```

## Payment Processing

### Verify Payment

**Endpoint:** `GET /storefront/verify-payment/:provider/:configId/:paymentId`

**Required Role:** User

**Description:** Verifies a payment with a specific provider.

**Path Parameters:**

- `provider`: The payment provider (e.g., 'stripe', 'paypal')
- `configId`: The payment configuration ID
- `paymentId`: The payment ID

**Response:**

```json
{
  "success": true,
  "payment": {
    "id": "payment123",
    "amount": 48.18,
    "currency": "USD",
    "status": "succeeded",
    "provider": "stripe",
    "createdAt": "2023-01-01T12:30:00Z"
  }
}
```

### Create Stripe Payment Intent

**Endpoint:** `POST /storefront/stripe/intent`

**Public Route:** Yes

**Description:** Creates a Stripe payment intent.

**Request:**

```json
{
  "amount": 4818,
  "currency": "usd",
  "description": "Order ORD-001",
  "metadata": {
    "orderId": "order123"
  }
}
```

**Response:**

```json
{
  "clientSecret": "pi_1234_secret_5678",
  "amount": 4818,
  "currency": "usd",
  "id": "pi_1234"
}
```

### Create Stripe Subscription Session

**Endpoint:** `POST /storefront/stripe/subscription-session`

**Public Route:** Yes

**Description:** Creates a Stripe subscription session.

**Request:**

```json
{
  "planId": "plan789",
  "successUrl": "https://example.com/success",
  "cancelUrl": "https://example.com/cancel"
}
```

**Response:**

```json
{
  "sessionId": "cs_1234",
  "url": "https://checkout.stripe.com/pay/cs_1234"
}
```

### Create Stripe Checkout Session

**Endpoint:** `POST /storefront/stripe/checkout-session`

**Public Route:** Yes

**Description:** Creates a Stripe checkout session.

**Request:**

```json
{
  "items": [
    {
      "name": "Cotton T-Shirt",
      "description": "Small / Blue",
      "amount": 1999,
      "currency": "usd",
      "quantity": 2
    }
  ],
  "successUrl": "https://example.com/success",
  "cancelUrl": "https://example.com/cancel"
}
```

**Response:**

```json
{
  "sessionId": "cs_5678",
  "url": "https://checkout.stripe.com/pay/cs_5678"
}
```

### Get PayPal Config

**Endpoint:** `GET /storefront/paypal/config`

**Public Route:** Yes

**Description:** Gets the PayPal configuration.

**Response:**

```json
{
  "clientId": "paypal-client-id",
  "currency": "USD",
  "intent": "capture"
}
```

### List Payment Gateways

**Endpoint:** `GET /storefront/payment-gateways`

**Public Route:** Yes

**Description:** Lists all available payment gateways.

**Response:**

```json
{
  "gateways": [
    {
      "id": "stripe",
      "name": "Stripe",
      "description": "Credit card payments via Stripe",
      "enabled": true,
      "supportedCurrencies": ["USD", "EUR", "GBP"]
    },
    {
      "id": "paypal",
      "name": "PayPal",
      "description": "Payments via PayPal",
      "enabled": true,
      "supportedCurrencies": ["USD", "EUR", "GBP"]
    }
  ]
}
```

### Take Payment

**Endpoint:** `POST /storefront/take-payment`

**Public Route:** Yes

**Description:** Processes a payment.

**Request:**

```json
{
  "gateway": "stripe",
  "amount": 4818,
  "currency": "usd",
  "paymentMethodId": "pm_1234",
  "orderId": "order123"
}
```

**Response:**

```json
{
  "success": true,
  "payment": {
    "id": "payment123",
    "amount": 48.18,
    "currency": "USD",
    "status": "succeeded",
    "provider": "stripe",
    "createdAt": "2023-01-01T12:30:00Z"
  }
}
```

## Checkout

### Checkout Cart

**Endpoint:** `POST /storefront/checkout-cart`

**Public Route:** Yes

**Description:** Checks out a shopping cart and creates an order.

**Request:**

```json
{
  "cartId": "cart123",
  "customer": {
    "email": "customer@example.com",
    "firstName": "John",
    "lastName": "Doe",
    "phone": "555-1234"
  },
  "shippingAddress": {
    "firstName": "John",
    "lastName": "Doe",
    "address1": "123 Main St",
    "city": "Anytown",
    "state": "CA",
    "postalCode": "12345",
    "country": "US"
  },
  "billingAddress": {
    "firstName": "John",
    "lastName": "Doe",
    "address1": "123 Main St",
    "city": "Anytown",
    "state": "CA",
    "postalCode": "12345",
    "country": "US"
  },
  "shippingMethod": "standard",
  "paymentMethod": "stripe",
  "paymentData": {
    "paymentIntentId": "pi_1234"
  }
}
```

**Response:**

```json
{
  "success": true,
  "order": {
    "id": "order123",
    "number": "ORD-001",
    "total": 48.18,
    "status": "processing",
