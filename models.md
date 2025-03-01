# Appmint AppEngine Data Models

This document provides an overview of the data models used in the Appmint AppEngine API. These models represent the structure of the data that is sent to and received from the API endpoints.

## Base Models

### Base Model

The base model that all other models extend from. It provides common fields for all models.

```typescript
interface BaseModel {
  id: string;
  createdAt: Date;
  updatedAt: Date;
  createdBy?: string;
  updatedBy?: string;
  orgId: string;
}
```

### Base History Model

Extends the base model to include history tracking.

```typescript
interface BaseHistoryModel extends BaseModel {
  history: {
    version: number;
    changes: {
      field: string;
      oldValue: any;
      newValue: any;
    }[];
    timestamp: Date;
    userId: string;
  }[];
}
```

### Base Publish Model

Extends the base model to include publishing functionality.

```typescript
interface BasePublishModel extends BaseModel {
  published: boolean;
  publishedAt?: Date;
  publishedBy?: string;
  publishedVersion?: number;
  draft?: any;
}
```

### Base Trash Model

Extends the base model to include trash functionality.

```typescript
interface BaseTrashModel extends BaseModel {
  deleted: boolean;
  deletedAt?: Date;
  deletedBy?: string;
}
```

## User Models

### User

Represents a user in the system.

```typescript
interface User extends BaseModel {
  email: string;
  username: string;
  firstName?: string;
  lastName?: string;
  password: string; // Hashed
  roles: string[];
  groups: string[];
  permissions: string[];
  settings: {
    theme?: string;
    language?: string;
    notifications?: boolean;
  };
  lastLogin?: Date;
  active: boolean;
  verified: boolean;
  verificationToken?: string;
  resetPasswordToken?: string;
  resetPasswordExpires?: Date;
}
```

### Permission

Represents a permission in the system.

```typescript
interface Permission extends BaseModel {
  name: string;
  description?: string;
  type: string;
  actions: string[];
}
```

## Content Models

### Category

Represents a category for organizing content.

```typescript
interface Category extends BaseModel {
  name: string;
  slug: string;
  description?: string;
  parent?: string;
  children?: string[];
  metadata?: Record<string, any>;
}
```

### Collection

Represents a collection of content items.

```typescript
interface Collection extends BasePublishModel {
  name: string;
  slug: string;
  description?: string;
  type: string;
  items: string[];
  metadata?: Record<string, any>;
  rules?: CollectionRule[];
  ui?: CollectionUI;
}
```

### Collection Rule

Represents a rule for a collection.

```typescript
interface CollectionRule extends BaseModel {
  collectionId: string;
  type: string;
  condition: {
    field: string;
    operator: string;
    value: any;
  };
  action: {
    type: string;
    params: Record<string, any>;
  };
}
```

### Collection UI

Represents the UI configuration for a collection.

```typescript
interface CollectionUI extends BaseModel {
  collectionId: string;
  layout: string;
  fields: {
    name: string;
    label: string;
    type: string;
    required: boolean;
    defaultValue?: any;
    options?: any[];
  }[];
  filters: {
    field: string;
    label: string;
    type: string;
  }[];
  sorts: {
    field: string;
    label: string;
    direction: 'asc' | 'desc';
  }[];
}
```

### Post

Represents a content post.

```typescript
interface Post extends BasePublishModel, BaseHistoryModel, BaseTrashModel {
  title: string;
  slug: string;
  content: string;
  excerpt?: string;
  author: string;
  categories?: string[];
  tags?: string[];
  featuredImage?: string;
  status: 'draft' | 'published' | 'archived';
  commentStatus: 'open' | 'closed';
  commentCount: number;
  viewCount: number;
  metadata?: Record<string, any>;
}
```

### Page

Represents a web page.

```typescript
interface Page extends BasePublishModel, BaseHistoryModel, BaseTrashModel {
  title: string;
  slug: string;
  content: string;
  template?: string;
  parent?: string;
  order?: number;
  status: 'draft' | 'published' | 'archived';
  metadata?: Record<string, any>;
  sections?: {
    id: string;
    type: string;
    content: any;
    order: number;
  }[];
  seo?: {
    title?: string;
    description?: string;
    keywords?: string[];
    ogImage?: string;
  };
}
```

### Comment

Represents a comment on a content item.

```typescript
interface Comment extends BaseModel, BaseTrashModel {
  content: string;
  author: string;
  post: string;
  parent?: string;
  children?: string[];
  status: 'pending' | 'approved' | 'spam' | 'trash';
  metadata?: Record<string, any>;
}
```

### Tag

Represents a tag for categorizing content.

```typescript
interface Tag extends BaseModel {
  name: string;
  slug: string;
  description?: string;
  count: number;
  metadata?: Record<string, any>;
}
```

## Site Models

### Site

Represents a website.

```typescript
interface Site extends BaseModel {
  name: string;
  title: string;
  description?: string;
  domain?: string;
  settings: {
    theme?: string;
    plugins?: string[];
    options?: Record<string, any>;
  };
  status: 'active' | 'inactive' | 'maintenance';
  pages?: string[];
  navigation?: Navigation[];
  metadata?: Record<string, any>;
}
```

### Site Domain

Represents a domain for a site.

```typescript
interface SiteDomain extends BaseModel {
  siteId: string;
  domain: string;
  primary: boolean;
  ssl: boolean;
  redirectWww: boolean;
  status: 'active' | 'pending' | 'error';
  dnsRecords?: {
    type: string;
    name: string;
    value: string;
    ttl: number;
  }[];
}
```

### Navigation

Represents a navigation menu for a site.

```typescript
interface Navigation extends BaseModel {
  siteId: string;
  name: string;
  location: string;
  items: {
    id: string;
    title: string;
    url: string;
    parent?: string;
    children?: string[];
    order: number;
    target?: '_self' | '_blank';
    classes?: string[];
  }[];
}
```

## CRM Models

### CRM Customer

Represents a customer in the CRM system.

```typescript
interface CrmCustomer extends BaseModel {
  email: string;
  firstName?: string;
  lastName?: string;
  phone?: string;
  company?: string;
  address?: CrmAddress[];
  tags?: string[];
  status: 'active' | 'inactive' | 'lead';
  source?: string;
  notes?: string;
  metadata?: Record<string, any>;
}
```

### CRM Address

Represents an address in the CRM system.

```typescript
interface CrmAddress extends BaseModel {
  customerId: string;
  type: 'billing' | 'shipping' | 'home' | 'work' | 'other';
  street1: string;
  street2?: string;
  city: string;
  state: string;
  postalCode: string;
  country: string;
  primary: boolean;
}
```

### CRM Lead

Represents a lead in the CRM system.

```typescript
interface CrmLead extends BaseModel {
  email: string;
  firstName?: string;
  lastName?: string;
  phone?: string;
  company?: string;
  source?: string;
  status: 'new' | 'contacted' | 'qualified' | 'converted' | 'lost';
  assignedTo?: string;
  notes?: string;
  metadata?: Record<string, any>;
}
```

### CRM Event

Represents an event in the CRM system.

```typescript
interface CrmEvent extends BaseModel {
  title: string;
  description?: string;
  startDate: Date;
  endDate: Date;
  location?: string;
  attendees?: string[];
  status: 'scheduled' | 'completed' | 'cancelled';
  metadata?: Record<string, any>;
}
```

### CRM Message

Represents a message in the CRM system.

```typescript
interface CrmMessage extends BaseModel {
  from: string;
  to: string[];
  cc?: string[];
  bcc?: string[];
  subject: string;
  body: string;
  attachments?: string[];
  status: 'draft' | 'sent' | 'delivered' | 'failed';
  metadata?: Record<string, any>;
}
```

### CRM Message Template

Represents a message template in the CRM system.

```typescript
interface CrmMessageTemplate extends BaseModel {
  name: string;
  description?: string;
  subject: string;
  body: string;
  variables?: string[];
  metadata?: Record<string, any>;
}
```

## Storefront Models

### SF Product

Represents a product in the storefront.

```typescript
interface SfProduct extends BasePublishModel, BaseHistoryModel {
  name: string;
  slug: string;
  description?: string;
  shortDescription?: string;
  sku: string;
  barcode?: string;
  price: number;
  compareAtPrice?: number;
  cost?: number;
  taxable: boolean;
  taxCode?: string;
  status: 'active' | 'draft' | 'archived';
  vendor?: string;
  collections?: string[];
  tags?: string[];
  attributes?: {
    name: string;
    value: string;
  }[];
  variants?: {
    id: string;
    name: string;
    sku: string;
    barcode?: string;
    price: number;
    compareAtPrice?: number;
    cost?: number;
    inventory: {
      quantity: number;
      policy: 'deny' | 'continue' | 'backorder';
    };
    attributes?: {
      name: string;
      value: string;
    }[];
    images?: string[];
  }[];
  images?: string[];
  seo?: {
    title?: string;
    description?: string;
    keywords?: string[];
  };
  metadata?: Record<string, any>;
}
```

### SF Collection

Represents a collection of products in the storefront.

```typescript
interface SfCollection extends BasePublishModel, BaseHistoryModel {
  name: string;
  slug: string;
  description?: string;
  image?: string;
  products?: string[];
  rules?: {
    field: string;
    operator: string;
    value: any;
  }[];
  sortOrder?: {
    field: string;
    direction: 'asc' | 'desc';
  };
  seo?: {
    title?: string;
    description?: string;
    keywords?: string[];
  };
  metadata?: Record<string, any>;
}
```

### SF Order

Represents an order in the storefront.

```typescript
interface SfOrder extends BaseModel {
  number: string;
  customer: string;
  email: string;
  phone?: string;
  shippingAddress?: {
    firstName: string;
    lastName: string;
    company?: string;
    street1: string;
    street2?: string;
    city: string;
    state: string;
    postalCode: string;
    country: string;
    phone?: string;
  };
  billingAddress?: {
    firstName: string;
    lastName: string;
    company?: string;
    street1: string;
    street2?: string;
    city: string;
    state: string;
    postalCode: string;
    country: string;
    phone?: string;
  };
  items: {
    id: string;
    productId: string;
    variantId?: string;
    name: string;
    sku: string;
    quantity: number;
    price: number;
    discount?: number;
    tax?: number;
    total: number;
    metadata?: Record<string, any>;
  }[];
  subtotal: number;
  discountTotal: number;
  taxTotal: number;
  shippingTotal: number;
  total: number;
  currency: string;
  paymentStatus: 'pending' | 'paid' | 'partially_paid' | 'refunded' | 'partially_refunded' | 'failed';
  fulfillmentStatus: 'unfulfilled' | 'partially_fulfilled' | 'fulfilled' | 'restocked';
  status: 'open' | 'closed' | 'cancelled';
  notes?: string;
  tags?: string[];
  metadata?: Record<string, any>;
}
```

### SF Cart

Represents a shopping cart in the storefront.

```typescript
interface SfCart extends BaseModel {
  customer?: string;
  email?: string;
  items: {
    id: string;
    productId: string;
    variantId?: string;
    name: string;
    sku: string;
    quantity: number;
    price: number;
    metadata?: Record<string, any>;
  }[];
  subtotal: number;
  discountTotal: number;
  taxTotal: number;
  shippingTotal: number;
  total: number;
  currency: string;
  discounts?: {
    id: string;
    code?: string;
    type: 'percentage' | 'fixed' | 'shipping';
    value: number;
    metadata?: Record<string, any>;
  }[];
  notes?: string;
  metadata?: Record<string, any>;
  expiresAt?: Date;
}
```

## FlexData Models

### FlexData

Represents flexible data that can be used for various purposes.

```typescript
interface FlexData extends BaseModel {
  name: string;
  type: string;
  data: any;
  metadata?: Record<string, any>;
}
```

### View Template

Represents a template for views.

```typescript
interface ViewTemplate extends BaseModel {
  name: string;
  description?: string;
  type: string;
  content: string;
  variables?: string[];
  metadata?: Record<string, any>;
}
```

### View Component

Represents a component for views.

```typescript
interface ViewComponent extends BaseModel {
  name: string;
  description?: string;
  type: string;
  content: string;
  props?: {
    name: string;
    type: string;
    required: boolean;
    defaultValue?: any;
  }[];
  metadata?: Record<string, any>;
}
```

## Activity Models

### Activity

Represents an activity in the system.

```typescript
interface Activity extends BaseModel {
  type: string;
  action: string;
  actor: string;
  target?: string;
  targetType?: string;
  data?: any;
  metadata?: Record<string, any>;
}
```

### Web Activity

Represents a web activity in the system.

```typescript
interface WebActivity extends BaseModel {
  sessionId: string;
  userId?: string;
  url: string;
  referrer?: string;
  userAgent?: string;
  ip?: string;
  device?: string;
  browser?: string;
  os?: string;
  country?: string;
  region?: string;
  city?: string;
  metadata?: Record<string, any>;
}
```

### Web Visit

Represents a web visit in the system.

```typescript
interface WebVisit extends BaseModel {
  sessionId: string;
  userId?: string;
  startTime: Date;
  endTime?: Date;
  duration?: number;
  pageViews: number;
  entryPage: string;
  exitPage?: string;
  referrer?: string;
  userAgent?: string;
  ip?: string;
  device?: string;
  browser?: string;
  os?: string;
  country?: string;
  region?: string;
  city?: string;
  metadata?: Record<string, any>;
}
```

## Workflow Models

### Workflow Definition

Represents a workflow definition in the system.

```typescript
interface WorkflowDefinition extends BaseModel {
  name: string;
  description?: string;
  version: string;
  status: 'draft' | 'active' | 'deprecated';
  trigger: {
    type: string;
    config: Record<string, any>;
  };
  steps: {
    id: string;
    name: string;
    type: string;
    config: Record<string, any>;
    next?: string | {
      condition: string;
      value: any;
      step: string;
    }[];
  }[];
  metadata?: Record<string, any>;
}
```

### Task

Represents a task in the system.

```typescript
interface Task extends BaseModel {
  title: string;
  description?: string;
  assignee?: string;
  dueDate?: Date;
  priority: 'low' | 'medium' | 'high' | 'urgent';
  status: 'todo' | 'in_progress' | 'done' | 'cancelled';
  tags?: string[];
  metadata?: Record<string, any>;
}
```

## Other Models

### Config

Represents a configuration in the system.

```typescript
interface Config extends BaseModel {
  name: string;
  value: any;
  type: string;
  metadata?: Record<string, any>;
}
```

### File Info

Represents a file in the system.

```typescript
interface FileInfo extends BaseModel {
  name: string;
  originalName: string;
  path: string;
  url?: string;
  size: number;
  type: string;
  extension: string;
  metadata?: Record<string, any>;
}
```

### Log

Represents a log entry in the system.

```typescript
interface Log extends BaseModel {
  level: 'debug' | 'info' | 'warn' | 'error' | 'fatal';
  message: string;
  source: string;
  data?: any;
  metadata?: Record<string, any>;
}
```

### Script

Represents a script in the system.

```typescript
interface Script extends BaseModel {
  name: string;
  description?: string;
  content: string;
  language: string;
  version?: string;
  status: 'active' | 'inactive' | 'draft';
  metadata?: Record<string, any>;
}
```

### Setting

Represents a setting in the system.

```typescript
interface Setting extends BaseModel {
  key: string;
  value: any;
  type: string;
  group?: string;
  metadata?: Record<string, any>;
}
```

### Translation

Represents a translation in the system.

```typescript
interface Translation extends BaseModel {
  key: string;
  locale: string;
  value: string;
  namespace?: string;
  metadata?: Record<string, any>;
}
```

### Workspace

Represents a workspace in the system.

```typescript
interface Workspace extends BaseModel {
  name: string;
  description?: string;
  owner: string;
  members: {
    userId: string;
    role: string;
  }[];
  items?: WorkspaceItem[];
  metadata?: Record<string, any>;
}
```

### Workspace Item

Represents an item in a workspace.

```typescript
interface WorkspaceItem extends BaseModel {
  workspaceId: string;
  type: string;
  itemId: string;
  order?: number;
  metadata?: Record<string, any>;
}
```

## Enums

### Permission Type

```typescript
enum PermissionType {
  create = 'create',
  read = 'read',
  update = 'update',
  delete = 'delete',
  publish = 'publish',
  unpublish = 'unpublish',
  admin = 'admin'
}
```

### Role Type

```typescript
enum RoleType {
  System = 'System',
  RootSystem = 'RootSystem',
  RootAdmin = 'RootAdmin',
  RootPowerUser = 'RootPowerUser',
  Owner = 'Owner',
  ConfigAdmin = 'ConfigAdmin',
  ContentAdmin = 'ContentAdmin',
  Publisher = 'Publisher',
  PowerUser = 'PowerUser',
  User = 'User',
  Guest = 'Guest'
}
```

## Relationships

The models in the Appmint AppEngine API are related to each other in various ways. Here are some of the key relationships:

- **User** has many **Permissions**
- **User** belongs to many **Groups**
- **User** has many **Roles**
- **Post** belongs to many **Categories**
- **Post** has many **Tags**
- **Post** has many **Comments**
- **Page** has many **Sections**
- **Site** has many **Pages**
- **Site** has many **Navigations**
- **Site** has many **SiteDomains**
- **CrmCustomer** has many **CrmAddresses**
- **CrmCustomer** has many **CrmEvents**
- **CrmCustomer** has many **CrmMessages**
- **SfProduct** belongs to many **SfCollections**
- **SfProduct** has many **Variants**
- **SfOrder** belongs to **CrmCustomer**
- **SfOrder** has many **Items**
- **SfCart** belongs to **CrmCustomer**
- **SfCart** has many **Items**
- **Workspace** belongs to **User** (owner)
- **Workspace** has many **WorkspaceItems**
- **WorkspaceItem** belongs to **Workspace**

## Data Validation

The Appmint AppEngine API validates data according to the model definitions. Here are some common validation rules:

- **Required fields**: Fields marked as required must be provided
- **Type validation**: Fields must match the specified type
- **Enum validation**: Fields with enum values must match one of the specified values
- **Length validation**: String fields may have minimum and maximum length requirements
- **Pattern validation**: String fields may need to match a specific pattern (e.g., email, URL)
- **Numeric validation**: Numeric fields may have minimum and maximum value requirements
- **Date validation**: Date fields may have minimum and maximum date requirements
- **Custom validation**: Some fields may have custom validation rules

## Data Serialization

When sending data to or receiving data from the API, the data is serialized according to the model definitions. Here are some common serialization rules:

- **Date fields**: Dates are serialized as ISO 8601 strings (e.g., "2023-01-01T12:00:00Z")
- **Object fields**: Objects are serialized as JSON objects
- **Array fields**: Arrays are serialized as JSON arrays
- **Enum fields**: Enums are serialized as strings
- **ID fields**: IDs are serialized as strings
- **Boolean fields**: Booleans are serialized as true or false
- **Numeric fields**: Numbers are serialized as JSON numbers
- **String fields**: Strings are serialized as JSON strings
