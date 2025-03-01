# AI API

The AI API provides endpoints for artificial intelligence and machine learning features, including model management, assistants, threads, and content generation.

## Table of Contents

- [Models](#models)
- [Assistants](#assistants)
- [Threads](#threads)
- [Content Generation](#content-generation)
- [Classification](#classification)
- [File Management](#file-management)

## Models

### List Models

**Endpoint:** `GET /ai/list-models`

**Description:** Lists available AI models.

**Query Parameters:**

- `configId` (optional): The configuration ID
- `apiKey` (optional): The API key

**Response:**

```json
{
  "models": [
    {
      "id": "gpt-4",
      "name": "GPT-4",
      "description": "Most capable GPT-4 model",
      "maxTokens": 8192,
      "trainingData": "Up to Apr 2023"
    },
    {
      "id": "gpt-3.5-turbo",
      "name": "GPT-3.5 Turbo",
      "description": "Most capable GPT-3.5 model",
      "maxTokens": 4096,
      "trainingData": "Up to Sep 2021"
    }
  ]
}
```

## Assistants

### Create Assistant

**Endpoint:** `POST /ai/assistants/create`

**Description:** Creates a new AI assistant.

**Request:**

```json
{
  "name": "Customer Support Assistant",
  "model": "gpt-4",
  "instructions": "You are a customer support assistant for Example Inc. Be helpful and concise.",
  "tools": [
    {
      "type": "retrieval"
    }
  ],
  "metadata": {
    "team": "support",
    "department": "customer-service"
  }
}
```

**Response:**

```json
{
  "id": "asst_123",
  "name": "Customer Support Assistant",
  "model": "gpt-4",
  "instructions": "You are a customer support assistant for Example Inc. Be helpful and concise.",
  "tools": [
    {
      "type": "retrieval"
    }
  ],
  "metadata": {
    "team": "support",
    "department": "customer-service"
  },
  "createdAt": "2023-01-01T12:00:00Z"
}
```

### Get Assistant

**Endpoint:** `GET /ai/assistants/get/:assistantId?`

**Description:** Gets an assistant by ID or lists all assistants if no ID is provided.

**Path Parameters:**

- `assistantId` (optional): The assistant ID

**Query Parameters:**

- `configId` (optional): The configuration ID
- `apiKey` (optional): The API key

**Response:**

```json
{
  "id": "asst_123",
  "name": "Customer Support Assistant",
  "model": "gpt-4",
  "instructions": "You are a customer support assistant for Example Inc. Be helpful and concise.",
  "tools": [
    {
      "type": "retrieval"
    }
  ],
  "metadata": {
    "team": "support",
    "department": "customer-service"
  },
  "createdAt": "2023-01-01T12:00:00Z"
}
```

### Run Assistant

**Endpoint:** `POST /ai/assistants/run`

**Description:** Runs an assistant with a specific input.

**Request:**

```json
{
  "assistantId": "asst_123",
  "threadId": "thread_456",
  "input": "How do I reset my password?",
  "stream": false
}
```

**Response:**

```json
{
  "id": "run_789",
  "assistantId": "asst_123",
  "threadId": "thread_456",
  "status": "completed",
  "response": {
    "id": "msg_abc",
    "role": "assistant",
    "content": "To reset your password, please follow these steps:\n\n1. Go to the login page\n2. Click on 'Forgot Password'\n3. Enter your email address\n4. Follow the instructions sent to your email",
    "createdAt": "2023-01-01T12:05:00Z"
  },
  "startedAt": "2023-01-01T12:00:00Z",
  "completedAt": "2023-01-01T12:05:00Z"
}
```

## Threads

### Create Thread

**Endpoint:** `POST /ai/thread/create`

**Description:** Creates a new thread for conversation.

**Query Parameters:**

- `configId` (optional): The configuration ID
- `apiKey` (optional): The API key

**Response:**

```json
{
  "id": "thread_123",
  "createdAt": "2023-01-01T12:00:00Z"
}
```

### Add Message to Thread

**Endpoint:** `POST /ai/thread/add/message`

**Description:** Adds a message to a thread.

**Request:**

```json
{
  "threadId": "thread_123",
  "role": "user",
  "content": "How do I reset my password?",
  "metadata": {
    "userId": "user_456"
  }
}
```

**Response:**

```json
{
  "id": "msg_789",
  "threadId": "thread_123",
  "role": "user",
  "content": "How do I reset my password?",
  "metadata": {
    "userId": "user_456"
  },
  "createdAt": "2023-01-01T12:05:00Z"
}
```

### Get Thread Runs

**Endpoint:** `GET /ai/thread/runs/:threadId/:runId?`

**Description:** Gets runs for a thread, optionally filtered by run ID.

**Path Parameters:**

- `threadId`: The thread ID
- `runId` (optional): The run ID

**Query Parameters:**

- `configId` (optional): The configuration ID
- `apiKey` (optional): The API key

**Response:**

```json
{
  "runs": [
    {
      "id": "run_123",
      "threadId": "thread_456",
      "assistantId": "asst_789",
      "status": "completed",
      "startedAt": "2023-01-01T12:00:00Z",
      "completedAt": "2023-01-01T12:05:00Z"
    }
  ]
}
```

### Get Thread Run Status

**Endpoint:** `GET /ai/thread/run/status/:threadId/:runId`

**Description:** Gets the status of a specific run in a thread.

**Path Parameters:**

- `threadId`: The thread ID
- `runId`: The run ID

**Query Parameters:**

- `configId` (optional): The configuration ID
- `apiKey` (optional): The API key

**Response:**

```json
{
  "id": "run_123",
  "threadId": "thread_456",
  "assistantId": "asst_789",
  "status": "completed",
  "startedAt": "2023-01-01T12:00:00Z",
  "completedAt": "2023-01-01T12:05:00Z"
}
```

### Get Thread Messages

**Endpoint:** `GET /ai/thread/messages/:threadId/`

**Description:** Gets messages in a thread.

**Path Parameters:**

- `threadId`: The thread ID

**Query Parameters:**

- `configId` (optional): The configuration ID
- `apiKey` (optional): The API key

**Response:**

```json
{
  "messages": [
    {
      "id": "msg_123",
      "threadId": "thread_456",
      "role": "user",
      "content": "How do I reset my password?",
      "createdAt": "2023-01-01T12:00:00Z"
    },
    {
      "id": "msg_789",
      "threadId": "thread_456",
      "role": "assistant",
      "content": "To reset your password, please follow these steps:\n\n1. Go to the login page\n2. Click on 'Forgot Password'\n3. Enter your email address\n4. Follow the instructions sent to your email",
      "createdAt": "2023-01-01T12:05:00Z"
    }
  ]
}
```

## Content Generation

### Create Assist

**Endpoint:** `POST /ai/assist/create`

**Description:** Creates a new assist session.

**Request:**

```json
{
  "model": "gpt-4",
  "instructions": "You are a helpful assistant.",
  "metadata": {
    "userId": "user_123"
  }
}
```

**Response:**

```json
{
  "id": "assist_123",
  "model": "gpt-4",
  "instructions": "You are a helpful assistant.",
  "metadata": {
    "userId": "user_123"
  },
  "createdAt": "2023-01-01T12:00:00Z"
}
```

### Assist Completion

**Endpoint:** `POST /ai/assist/completion`

**Description:** Gets a completion from an assist session.

**Request:**

```json
{
  "assistId": "assist_123",
  "prompt": "Write a short poem about technology.",
  "maxTokens": 100,
  "temperature": 0.7
}
```

**Response:**

```json
{
  "id": "completion_456",
  "assistId": "assist_123",
  "prompt": "Write a short poem about technology.",
  "completion": "Silicon dreams in digital streams,\nConnecting hearts across the void.\nTechnology's gleam, a modern theme,\nHuman and machine, forever alloyed.",
  "finishReason": "stop",
  "usage": {
    "promptTokens": 7,
    "completionTokens": 28,
    "totalTokens": 35
  }
}
```

### Generate Content

**Endpoint:** `POST /ai/generate/:what`

**Description:** Generates specific content based on the provided type.

**Path Parameters:**

- `what`: The type of content to generate (e.g., 'text', 'image', 'code')

**Request:**

```json
{
  "prompt": "Generate a JavaScript function that calculates the Fibonacci sequence.",
  "options": {
    "language": "javascript",
    "comments": true
  }
}
```

**Response:**

```json
{
  "content": "/**\n * Calculates the Fibonacci sequence up to n numbers\n * @param {number} n - The number of Fibonacci numbers to generate\n * @returns {Array} - Array containing the Fibonacci sequence\n */\nfunction fibonacci(n) {\n  // Initialize the sequence with the first two numbers\n  const sequence = [0, 1];\n  \n  // Generate the sequence up to n numbers\n  for (let i = 2; i < n; i++) {\n    sequence[i] = sequence[i-1] + sequence[i-2];\n  }\n  \n  return sequence;\n}",
  "language": "javascript",
  "tokens": {
    "prompt": 12,
    "completion": 245,
    "total": 257
  }
}
```

## Classification

### Classify Content

**Endpoint:** `POST /ai/classify`

**Description:** Classifies content into predefined categories.

**Request:**

```json
{
  "text": "I'm having trouble logging into my account. It says my password is incorrect, but I'm sure I'm using the right one.",
  "categories": ["login-issues", "password-reset", "account-creation", "billing", "feature-request"],
  "multiLabel": false
}
```

**Response:**

```json
{
  "classification": "login-issues",
  "confidence": 0.92,
  "allCategories": [
    {
      "category": "login-issues",
      "confidence": 0.92
    },
    {
      "category": "password-reset",
      "confidence": 0.78
    },
    {
      "category": "account-creation",
      "confidence": 0.12
    },
    {
      "category": "billing",
      "confidence": 0.05
    },
    {
      "category": "feature-request",
      "confidence": 0.02
    }
  ]
}
```

## File Management

### Delete Files

**Endpoint:** `POST /ai/delete-files`

**Description:** Deletes files used for AI processing.

**Request:**

```json
{
  "fileIds": ["file_123", "file_456"]
}
```

**Response:**

```json
{
  "success": true,
  "deletedFiles": ["file_123", "file_456"]
}
