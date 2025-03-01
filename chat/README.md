# Chat API

The Chat API provides endpoints for real-time chat functionality, including chat configuration, user profiles, session management, and message handling.

## Table of Contents

- [Chat Configuration](#chat-configuration)
- [User Profiles](#user-profiles)
- [Session Management](#session-management)
- [File Upload](#file-upload)
- [Live Chat](#live-chat)

## Chat Configuration

### Get Chat Configuration

**Endpoint:** `GET /chat/config/:chatId`

**Public Route:** Yes

**Description:** Gets the configuration for a specific chat.

**Path Parameters:**

- `chatId`: The chat ID

**Response:**

```json
{
  "id": "chat123",
  "name": "Support Chat",
  "type": "support",
  "settings": {
    "welcomeMessage": "Welcome to our support chat! How can we help you today?",
    "offlineMessage": "We're currently offline. Please leave a message and we'll get back to you.",
    "operatingHours": {
      "monday": "9:00-17:00",
      "tuesday": "9:00-17:00",
      "wednesday": "9:00-17:00",
      "thursday": "9:00-17:00",
      "friday": "9:00-17:00"
    },
    "theme": {
      "primaryColor": "#007bff",
      "secondaryColor": "#6c757d",
      "fontFamily": "Arial, sans-serif"
    }
  },
  "agents": [
    {
      "id": "agent123",
      "name": "John Doe",
      "avatar": "https://example.com/avatars/john.jpg",
      "status": "online"
    },
    {
      "id": "agent456",
      "name": "Jane Smith",
      "avatar": "https://example.com/avatars/jane.jpg",
      "status": "offline"
    }
  ]
}
```

## User Profiles

### Get User Profile

**Endpoint:** `GET /chat/get-profile/:userId`

**Public Route:** Yes

**Description:** Gets the profile of a chat user.

**Path Parameters:**

- `userId`: The user ID

**Response:**

```json
{
  "id": "user123",
  "name": "John Doe",
  "email": "john@example.com",
  "avatar": "https://example.com/avatars/john.jpg",
  "status": "online",
  "lastSeen": "2023-01-01T12:00:00Z",
  "preferences": {
    "notifications": true,
    "sound": true,
    "theme": "light"
  }
}
```

## Session Management

### Get Active Sessions

**Endpoint:** `GET /chat/sessions`

**Description:** Gets all active chat sessions.

**Response:**

```json
{
  "sessions": [
    {
      "id": "session123",
      "chatId": "chat123",
      "userId": "user123",
      "userName": "John Doe",
      "agentId": "agent456",
      "agentName": "Jane Smith",
      "status": "active",
      "startedAt": "2023-01-01T12:00:00Z",
      "lastActivity": "2023-01-01T12:15:00Z",
      "messageCount": 10
    },
    {
      "id": "session456",
      "chatId": "chat789",
      "userId": "user789",
      "userName": "Alice Johnson",
      "agentId": "agent123",
      "agentName": "John Doe",
      "status": "active",
      "startedAt": "2023-01-01T13:00:00Z",
      "lastActivity": "2023-01-01T13:10:00Z",
      "messageCount": 5
    }
  ]
}
```

### Delete Sessions

**Endpoint:** `POST /chat/sessions/delete`

**Description:** Deletes one or more chat sessions.

**Request:**

```json
{
  "sessionIds": ["session123", "session456"]
}
```

**Response:**

```json
{
  "success": true,
  "message": "Sessions deleted successfully",
  "deletedSessions": ["session123", "session456"]
}
```

## File Upload

### Upload Files

**Endpoint:** `POST /chat/upload/:chatId/:userId`

**Description:** Uploads files for a chat session.

**Path Parameters:**

- `chatId`: The chat ID
- `userId`: The user ID

**Request:**

```
Content-Type: multipart/form-data

files[0]: (binary file data)
files[1]: (binary file data)
```

**Response:**

```json
{
  "success": true,
  "files": [
    {
      "id": "file123",
      "name": "document.pdf",
      "url": "https://example.com/files/document.pdf",
      "type": "application/pdf",
      "size": 1024,
      "uploadedAt": "2023-01-01T12:00:00Z"
    },
    {
      "id": "file456",
      "name": "image.jpg",
      "url": "https://example.com/files/image.jpg",
      "type": "image/jpeg",
      "size": 2048,
      "uploadedAt": "2023-01-01T12:00:00Z"
    }
  ]
}
```

## Live Chat

### Send Live Chat Message

**Endpoint:** `POST /chat/live/:chatId/:userId`

**Description:** Sends a live chat message.

**Path Parameters:**

- `chatId`: The chat ID
- `userId`: The user ID

**Request:**

```json
{
  "type": "text",
  "content": "Hello, I need help with my order.",
  "metadata": {
    "orderId": "order123"
  }
}
```

**Response:**

```json
{
  "id": "msg123",
  "chatId": "chat123",
  "userId": "user123",
  "userName": "John Doe",
  "type": "text",
  "content": "Hello, I need help with my order.",
  "metadata": {
    "orderId": "order123"
  },
  "timestamp": "2023-01-01T12:00:00Z",
  "status": "sent"
}
```

## WebSocket Events

The Chat API also provides WebSocket events for real-time communication. These events are handled by the ChatGateway.

### Connection Events

- `connection`: Fired when a client connects to the WebSocket server
- `disconnect`: Fired when a client disconnects from the WebSocket server

### Chat Events

- `join-room`: Join a chat room

  ```json
  {
    "chatId": "chat123",
    "userId": "user123"
  }
  ```

- `leave-room`: Leave a chat room

  ```json
  {
    "chatId": "chat123",
    "userId": "user123"
  }
  ```

- `message`: Send a message to a chat room

  ```json
  {
    "chatId": "chat123",
    "userId": "user123",
    "type": "text",
    "content": "Hello, I need help with my order.",
    "metadata": {
      "orderId": "order123"
    }
  }
  ```

- `typing`: Indicate that a user is typing

  ```json
  {
    "chatId": "chat123",
    "userId": "user123",
    "isTyping": true
  }
  ```

- `read`: Mark messages as read

  ```json
  {
    "chatId": "chat123",
    "userId": "user123",
    "messageIds": ["msg123", "msg456"]
  }
  ```

### Response Events

- `message`: Receive a message from a chat room

  ```json
  {
    "id": "msg123",
    "chatId": "chat123",
    "userId": "user123",
    "userName": "John Doe",
    "type": "text",
    "content": "Hello, I need help with my order.",
    "metadata": {
      "orderId": "order123"
    },
    "timestamp": "2023-01-01T12:00:00Z",
    "status": "sent"
  }
  ```

- `typing`: Receive typing indicator

  ```json
  {
    "chatId": "chat123",
    "userId": "user123",
    "userName": "John Doe",
    "isTyping": true
  }
  ```

- `read`: Receive read receipt

  ```json
  {
    "chatId": "chat123",
    "userId": "user123",
    "userName": "John Doe",
    "messageIds": ["msg123", "msg456"],
    "timestamp": "2023-01-01T12:00:00Z"
  }
  ```

- `user-joined`: Receive notification when a user joins a chat room

  ```json
  {
    "chatId": "chat123",
    "userId": "user123",
    "userName": "John Doe",
    "timestamp": "2023-01-01T12:00:00Z"
  }
  ```

- `user-left`: Receive notification when a user leaves a chat room

  ```json
  {
    "chatId": "chat123",
    "userId": "user123",
    "userName": "John Doe",
    "timestamp": "2023-01-01T12:00:00Z"
  }
  ```

- `error`: Receive error notification

  ```json
  {
    "code": "unauthorized",
    "message": "You are not authorized to join this chat room"
  }
