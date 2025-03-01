# Site API

The Site API provides endpoints for managing websites, pages, and site-related functionality.

## Table of Contents

- [Site Management](#site-management)
- [Page Management](#page-management)
- [Site Map](#site-map)
- [App DSL](#app-dsl)

## Site Management

### Get Organization by Domain Name

**Endpoint:** `GET /site/get-org/:domainName`

**Required Permission:** `PermissionTypeContent.read`

**Description:** Gets organization information by domain name.

**Path Parameters:**

- `domainName`: The domain name

**Response:**

```json
{
  "id": "org123",
  "name": "Example Organization",
  "domain": "example.com",
  "settings": {
    "theme": "default",
    "logo": "https://example.com/logo.png"
  }
}
```

### Get Site

**Endpoint:** `GET /site/get-site/:configSiteName/:domainName?`

**Required Permission:** `PermissionTypeContent.read`

**Description:** Gets site information by site name and optionally by domain name.

**Path Parameters:**

- `configSiteName`: The site name in the configuration
- `domainName` (optional): The domain name

**Query Parameters:**

- `shared-host` (optional): The shared host name

**Response:**

```json
{
  "id": "site123",
  "name": "example-site",
  "title": "Example Site",
  "domain": "example.com",
  "settings": {
    "theme": "default",
    "logo": "https://example.com/logo.png",
    "colors": {
      "primary": "#007bff",
      "secondary": "#6c757d"
    }
  },
  "pages": [
    {
      "id": "page123",
      "title": "Home",
      "slug": "home"
    },
    {
      "id": "page456",
      "title": "About",
      "slug": "about"
    }
  ],
  "navigation": [
    {
      "id": "nav123",
      "name": "main",
      "items": [
        {
          "title": "Home",
          "url": "/"
        },
        {
          "title": "About",
          "url": "/about"
        }
      ]
    }
  ]
}
```

### Register Site Domain Name

**Endpoint:** `POST /site/register-site`

**Required Permission:** `PermissionTypeContent.create`

**Required Role:** `RootUser`, `RootSystem`, `RootAdmin`

**Description:** Registers a domain name for a site.

**Request:**

```json
{
  "siteName": "example-site",
  "domain": "example.com",
  "settings": {
    "ssl": true,
    "redirectWww": true
  }
}
```

**Response:**

```json
{
  "success": true,
  "site": {
    "id": "site123",
    "name": "example-site",
    "domain": "example.com",
    "settings": {
      "ssl": true,
      "redirectWww": true
    }
  }
}
```

## Page Management

### Get Page

**Endpoint:** `GET /site/page/:hostName/:siteName/*`

**Description:** Gets a page by host name, site name, and path.

**Path Parameters:**

- `hostName`: The host name
- `siteName`: The site name
- `*`: The page path

**Query Parameters:**

- Additional query parameters for filtering and customization

**Response:**

```json
{
  "id": "page123",
  "title": "Home",
  "slug": "home",
  "content": "<div>Welcome to our website!</div>",
  "template": "default",
  "sections": [
    {
      "id": "section123",
      "type": "hero",
      "content": {
        "title": "Welcome to Example Site",
        "subtitle": "Your one-stop solution",
        "image": "https://example.com/hero.jpg",
        "cta": {
          "text": "Learn More",
          "url": "/about"
        }
      }
    },
    {
      "id": "section456",
      "type": "features",
      "content": {
        "title": "Our Features",
        "items": [
          {
            "title": "Feature 1",
            "description": "Description of feature 1",
            "icon": "icon-feature-1"
          },
          {
            "title": "Feature 2",
            "description": "Description of feature 2",
            "icon": "icon-feature-2"
          }
        ]
      }
    }
  ],
  "seo": {
    "title": "Home | Example Site",
    "description": "Welcome to Example Site, your one-stop solution",
    "keywords": ["example", "site", "solution"]
  },
  "createdAt": "2023-01-01T12:00:00Z",
  "updatedAt": "2023-01-02T10:30:00Z"
}
```

### Get Page (POST)

**Endpoint:** `POST /site/page/:hostName/:siteName/*`

**Required Permission:** `PermissionTypeContent.read`

**Description:** Gets a page by host name, site name, and path with additional request body parameters.

**Path Parameters:**

- `hostName`: The host name
- `siteName`: The site name
- `*`: The page path

**Request:**

```json
{
  "preview": true,
  "version": "draft",
  "locale": "en-US"
}
```

**Response:**

```json
{
  "id": "page123",
  "title": "Home",
  "slug": "home",
  "content": "<div>Welcome to our website!</div>",
  "template": "default",
  "sections": [
    {
      "id": "section123",
      "type": "hero",
      "content": {
        "title": "Welcome to Example Site",
        "subtitle": "Your one-stop solution",
        "image": "https://example.com/hero.jpg",
        "cta": {
          "text": "Learn More",
          "url": "/about"
        }
      }
    }
  ],
  "seo": {
    "title": "Home | Example Site",
    "description": "Welcome to Example Site, your one-stop solution",
    "keywords": ["example", "site", "solution"]
  },
  "createdAt": "2023-01-01T12:00:00Z",
  "updatedAt": "2023-01-02T10:30:00Z"
}
```

### Get Page Section

**Endpoint:** `GET /site/page-section/:siteName/:pageId/:sectionId/:datatype?`

**Required Permission:** `PermissionTypeContent.read`

**Description:** Gets a specific section of a page.

**Path Parameters:**

- `siteName`: The site name
- `pageId`: The page ID
- `sectionId`: The section ID
- `datatype` (optional): The data type

**Query Parameters:**

- Additional query parameters for filtering and customization

**Response:**

```json
{
  "id": "section123",
  "type": "hero",
  "content": {
    "title": "Welcome to Example Site",
    "subtitle": "Your one-stop solution",
    "image": "https://example.com/hero.jpg",
    "cta": {
      "text": "Learn More",
      "url": "/about"
    }
  }
}
```

### Get Page Data

**Endpoint:** `GET /site/page-data/:datatype/:dataId?`

**Required Permission:** `PermissionTypeContent.read`

**Description:** Gets page data by data type and optionally by data ID.

**Path Parameters:**

- `datatype`: The data type
- `dataId` (optional): The data ID

**Response:**

```json
{
  "id": "data123",
  "datatype": "product",
  "data": {
    "name": "Example Product",
    "price": 99.99,
    "description": "This is an example product"
  }
}
```

## Site Map

### Create Site Map

**Endpoint:** `POST /site/site-map`

**Description:** Creates a site map for a website.

**Request:**

```json
{
  "siteName": "example-site",
  "domain": "example.com",
  "includeImages": true,
  "changefreq": "weekly",
  "priority": 0.8
}
```

**Response:**

```json
{
  "success": true,
  "siteMap": {
    "url": "https://example.com/sitemap.xml",
    "lastGenerated": "2023-01-01T12:00:00Z",
    "pageCount": 25
  }
}
```

## App DSL

### Get App DSL

**Endpoint:** `GET /site/app/dsl/:name`

**Public Route:** Yes

**Description:** Gets the Domain-Specific Language (DSL) configuration for an app.

**Path Parameters:**

- `name`: The app name

**Response:**

```json
{
  "name": "example-app",
  "version": "1.0.0",
  "components": [
    {
      "name": "Button",
      "props": [
        {
          "name": "text",
          "type": "string",
          "required": true
        },
        {
          "name": "variant",
          "type": "string",
          "enum": ["primary", "secondary", "outline"],
          "default": "primary"
        }
      ]
    },
    {
      "name": "Card",
      "props": [
        {
          "name": "title",
          "type": "string",
          "required": true
        },
        {
          "name": "content",
          "type": "string",
          "required": true
        },
        {
          "name": "image",
          "type": "string"
        }
      ]
    }
  ],
  "layouts": [
    {
      "name": "Default",
      "sections": ["header", "content", "footer"]
    },
    {
      "name": "Landing",
      "sections": ["hero", "features", "testimonials", "cta", "footer"]
    }
  ]
}
