# GraphQL API

The ContentMint AppEngine provides a GraphQL API that allows you to query and mutate data using GraphQL syntax.

## Table of Contents

- [Overview](#overview)
- [Queries](#queries)
  - [Hello World](#hello-world)
  - [Get Sample](#get-sample)
- [Mutations](#mutations)
  - [Create Message](#create-message)
- [Schema](#schema)

## Overview

The GraphQL API is available at `/graphql`. You can also access the GraphQL Playground at the same URL to explore the API interactively.

## Queries

### Hello World

Returns a simple greeting message.

**Query:**

```graphql
query {
  helloWorld
}
```

**Response:**

```json
{
  "data": {
    "helloWorld": "Hello, World!"
  }
}
```

### Get Sample

Returns a sample data object.

**Query:**

```graphql
query {
  getSample {
    id
    name
    isActive
    url
  }
}
```

**Response:**

```json
{
  "data": {
    "getSample": {
      "id": 1,
      "name": "Sample Item",
      "isActive": true,
      "url": "https://example.com/sample"
    }
  }
}
```

## Mutations

### Create Message

Creates a new message.

**Mutation:**

```graphql
mutation {
  createMessage(message: "Hello from GraphQL!")
}
```

**Response:**

```json
{
  "data": {
    "createMessage": "Message received: Hello from GraphQL!"
  }
}
```

## Schema

The GraphQL schema defines the available types, queries, and mutations.

```graphql
# ------------------------------------------------------
# THIS FILE WAS AUTOMATICALLY GENERATED (DO NOT MODIFY)
# ------------------------------------------------------

type SampleDTO {
  id: Int!
  name: String!
  isActive: Boolean!
  url: String
}

type Query {
  helloWorld: String!
  getSample: SampleDTO!
}

type Mutation {
  createMessage(message: String!): String!
}
```

### Types

#### SampleDTO

Represents a sample data object.

| Field    | Type     | Description                                |
|----------|----------|--------------------------------------------|
| id       | Int!     | The unique identifier for the sample       |
| name     | String!  | The name of the sample                     |
| isActive | Boolean! | Whether the sample is active               |
| url      | String   | Optional URL associated with the sample    |

### Queries

| Name       | Return Type | Description                           |
|------------|-------------|---------------------------------------|
| helloWorld | String!     | Returns a simple greeting message     |
| getSample  | SampleDTO!  | Returns a sample data object          |

### Mutations

| Name          | Parameters                | Return Type | Description                |
|---------------|---------------------------|-------------|----------------------------|
| createMessage | message: String! (required) | String!     | Creates a new message     |
