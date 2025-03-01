# CRM API

The CRM API provides endpoints for managing customer relationship management features, including tickets, events, reservations, service points, chat messages, and more.

## Table of Contents

- [Contact Forms](#contact-forms)
- [Tickets](#tickets)
- [Reservations](#reservations)
- [Events](#events)
- [Service Points](#service-points)
- [Chat Messages](#chat-messages)
- [Meetings](#meetings)
- [FlexData](#flexdata)
- [Inbox](#inbox)
- [Activities and Comments](#activities-and-comments)
- [Signed Documents](#signed-documents)
- [Workspace Items](#workspace-items)

## Contact Forms

### Submit Contact Form (POST)

**Endpoint:** `POST /crm/contact-form/post/:app/:name`

**Public Route:** Yes

**Description:** Submits a contact form with form data.

**Parameters:**

- `app`: The application identifier (e.g., 'yugo' for YugoForm)
- `name`: The form name or page name
- `type` (query parameter, optional): The type of form

**Request:**

```
Content-Type: multipart/form-data

name=John Doe&email=john@example.com&message=Hello, I'm interested in your services.
```

**Response:**

```json
{
  "success": true,
  "message": "Form submitted successfully",
  "data": {
    "id": "form123",
    "submittedAt": "2023-01-01T12:00:00Z"
  }
}
```

### Submit Contact Form (JSON)

**Endpoint:** `POST /crm/contact-form/json/:app/:name`

**Public Route:** Yes

**Description:** Submits a contact form with JSON data.

**Parameters:**

- `app`: The application identifier
- `name`: The form name or page name
- `type` (query parameter, optional): The type of form

**Request:**

```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "message": "Hello, I'm interested in your services."
}
```

**Response:**

```json
{
  "success": true,
  "message": "Form submitted successfully",
  "data": {
    "id": "form123",
    "submittedAt": "2023-01-01T12:00:00Z"
  }
}
```

### Get Collection Form

**Endpoint:** `GET /crm/collection-form/get/:number`

**Public Route:** Yes

**Description:** Gets a collection form by its number.

**Parameters:**

- `number`: The form number

**Response:**

```json
{
  "id": "form123",
  "name": "Contact Form",
  "fields": [
    {
      "name": "name",
      "label": "Full Name",
      "type": "text",
      "required": true
    },
    {
      "name": "email",
      "label": "Email Address",
      "type": "email",
      "required": true
    },
    {
      "name": "message",
      "label": "Message",
      "type": "textarea",
      "required": false
    }
  ]
}
```

### Submit Collection Form

**Endpoint:** `POST /crm/collection-form/submit/:number/:email?`

**Public Route:** Yes

**Description:** Submits a collection form.

**Parameters:**

- `number`: The form number
- `email` (optional): The email address of the submitter

**Request:**

```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "message": "Hello, I'm interested in your services."
}
```

**Response:**

```json
{
  "success": true,
  "message": "Form submitted successfully",
  "data": {
    "id": "submission123",
    "submittedAt": "2023-01-01T12:00:00Z"
  }
}
```

## Tickets

### Get Ticket Collections

**Endpoint:** `GET /crm/tickets/collections`

**Public Route:** Yes

**Description:** Gets all ticket collections.

**Response:**

```json
{
  "collections": [
    {
      "id": "support",
      "name": "Support Tickets",
      "description": "Technical support tickets"
    },
    {
      "id": "sales",
      "name": "Sales Inquiries",
      "description": "Sales-related tickets"
    }
  ]
}
```

### Get Tickets by Email

**Endpoint:** `GET /crm/tickets/get-by-email/:email/:number?`

**Public Route:** Yes

**Description:** Gets tickets by email address and optionally by ticket number.

**Parameters:**

- `email`: The email address
- `number` (optional): The ticket number

**Response:**

```json
{
  "tickets": [
    {
      "id": "ticket123",
      "number": "TKT-001",
      "subject": "Technical Issue",
      "status": "open",
      "createdAt": "2023-01-01T12:00:00Z",
      "updatedAt": "2023-01-02T10:30:00Z"
    }
  ]
}
```

### Get Tickets

**Endpoint:** `GET /crm/tickets/get/:number?`

**Description:** Gets tickets, optionally filtered by ticket number.

**Parameters:**

- `number` (optional): The ticket number
- `status` (query parameter, optional): Filter by status
- `enrich` (query parameter, optional): Whether to enrich the response with additional data

**Response:**

```json
{
  "tickets": [
    {
      "id": "ticket123",
      "number": "TKT-001",
      "subject": "Technical Issue",
      "description": "I'm having trouble with the login feature.",
      "status": "open",
      "priority": "high",
      "assignee": "agent123",
      "customer": {
        "id": "customer456",
        "email": "customer@example.com",
        "name": "John Doe"
      },
      "createdAt": "2023-01-01T12:00:00Z",
      "updatedAt": "2023-01-02T10:30:00Z"
    }
  ]
}
```

### Create Ticket

**Endpoint:** `POST /crm/tickets/create`

**Description:** Creates a new ticket.

**Request:**

```json
{
  "data": {
    "subject": "Technical Issue",
    "description": "I'm having trouble with the login feature.",
    "priority": "high",
    "category": "technical"
  }
}
```

**Response:**

```json
{
  "id": "ticket123",
  "number": "TKT-001",
  "subject": "Technical Issue",
  "description": "I'm having trouble with the login feature.",
  "status": "open",
  "priority": "high",
  "createdAt": "2023-01-01T12:00:00Z"
}
```

### Create Ticket with Files

**Endpoint:** `POST /crm/tickets/create-post`

**Description:** Creates a new ticket with file attachments.

**Request:**

```
Content-Type: multipart/form-data

subject=Technical Issue&description=I'm having trouble with the login feature.&priority=high&category=technical&files[]=screenshot.png
```

**Response:**

```json
{
  "id": "ticket123",
  "number": "TKT-001",
  "subject": "Technical Issue",
  "description": "I'm having trouble with the login feature.",
  "status": "open",
  "priority": "high",
  "attachments": [
    {
      "id": "file123",
      "name": "screenshot.png",
      "url": "https://example.com/files/screenshot.png",
      "size": 1024,
      "type": "image/png"
    }
  ],
  "createdAt": "2023-01-01T12:00:00Z"
}
```

### Update Ticket

**Endpoint:** `POST /crm/tickets/update`

**Description:** Updates an existing ticket.

**Request:**

```json
{
  "id": "ticket123",
  "data": {
    "status": "in-progress",
    "priority": "medium",
    "assignee": "agent456"
  }
}
```

**Response:**

```json
{
  "id": "ticket123",
  "number": "TKT-001",
  "subject": "Technical Issue",
  "description": "I'm having trouble with the login feature.",
  "status": "in-progress",
  "priority": "medium",
  "assignee": "agent456",
  "updatedAt": "2023-01-03T09:15:00Z"
}
```

### Delete Ticket

**Endpoint:** `DELETE /crm/tickets/delete/:id`

**Description:** Deletes a ticket.

**Parameters:**

- `id`: The ticket ID

**Response:**

```json
{
  "success": true,
  "message": "Ticket deleted successfully"
}
```

## Reservations

### Get Reservation Definitions

**Endpoint:** `GET /crm/reservations/definitions`

**Public Route:** Yes

**Description:** Gets all reservation definitions.

**Response:**

```json
{
  "definitions": [
    {
      "id": "haircut",
      "name": "Haircut Appointment",
      "duration": 30,
      "price": 25,
      "description": "Standard haircut service"
    },
    {
      "id": "massage",
      "name": "Massage Session",
      "duration": 60,
      "price": 50,
      "description": "Relaxing massage therapy"
    }
  ]
}
```

### Generate Appointment Slots

**Endpoint:** `POST /crm/reservations/slots`

**Public Route:** Yes

**Description:** Generates available appointment slots.

**Request:**

```json
{
  "definitionId": "haircut",
  "date": "2023-01-15",
  "servicePointId": "salon123"
}
```

**Response:**

```json
{
  "slots": [
    {
      "time": "09:00",
      "available": true
    },
    {
      "time": "09:30",
      "available": false
    },
    {
      "time": "10:00",
      "available": true
    }
  ]
}
```

### Get Reservations by Email

**Endpoint:** `GET /crm/reservations/by-email/:email/:reservationNumber?`

**Public Route:** Yes

**Description:** Gets reservations by email address and optionally by reservation number.

**Parameters:**

- `email`: The email address
- `reservationNumber` (optional): The reservation number

**Response:**

```json
{
  "reservations": [
    {
      "id": "res123",
      "number": "RES-001",
      "definitionId": "haircut",
      "date": "2023-01-15",
      "time": "10:00",
      "status": "confirmed",
      "customer": {
        "email": "customer@example.com",
        "name": "John Doe"
      }
    }
  ]
}
```

### Get Reservations

**Endpoint:** `GET /crm/reservations/get/:id?`

**Description:** Gets reservations, optionally filtered by ID.

**Parameters:**

- `id` (optional): The reservation ID

**Response:**

```json
{
  "reservations": [
    {
      "id": "res123",
      "number": "RES-001",
      "definitionId": "haircut",
      "date": "2023-01-15",
      "time": "10:00",
      "status": "confirmed",
      "customer": {
        "id": "customer456",
        "email": "customer@example.com",
        "name": "John Doe"
      },
      "servicePoint": {
        "id": "salon123",
        "name": "Downtown Salon"
      }
    }
  ]
}
```

### Create Reservation

**Endpoint:** `POST /crm/reservations/create`

**Description:** Creates a new reservation.

**Request:**

```json
{
  "data": {
    "definitionId": "haircut",
    "date": "2023-01-15",
    "time": "10:00",
    "servicePointId": "salon123",
    "notes": "First-time customer"
  }
}
```

**Response:**

```json
{
  "id": "res123",
  "number": "RES-001",
  "definitionId": "haircut",
  "date": "2023-01-15",
  "time": "10:00",
  "status": "confirmed",
  "servicePoint": {
    "id": "salon123",
    "name": "Downtown Salon"
  },
  "createdAt": "2023-01-01T12:00:00Z"
}
```

### Update Reservation

**Endpoint:** `POST /crm/reservations/update`

**Description:** Updates an existing reservation.

**Request:**

```json
{
  "id": "res123",
  "data": {
    "date": "2023-01-16",
    "time": "11:00",
    "status": "rescheduled"
  }
}
```

**Response:**

```json
{
  "id": "res123",
  "number": "RES-001",
  "definitionId": "haircut",
  "date": "2023-01-16",
  "time": "11:00",
  "status": "rescheduled",
  "updatedAt": "2023-01-02T09:30:00Z"
}
```

### Delete Reservation

**Endpoint:** `DELETE /crm/reservations/delete/:id`

**Description:** Deletes a reservation.

**Parameters:**

- `id`: The reservation ID

**Response:**

```json
{
  "success": true,
  "message": "Reservation deleted successfully"
}
```

## Events

### Get Events

**Endpoint:** `GET /crm/events/get/:id?`

**Description:** Gets events, optionally filtered by ID.

**Parameters:**

- `id` (optional): The event ID

**Response:**

```json
{
  "events": [
    {
      "id": "event123",
      "title": "Product Launch",
      "description": "Launch of our new product line",
      "startDate": "2023-02-15T14:00:00Z",
      "endDate": "2023-02-15T16:00:00Z",
      "location": "Main Conference Room",
      "status": "scheduled"
    }
  ]
}
```

### Create Event

**Endpoint:** `POST /crm/events/create`

**Description:** Creates a new event.

**Request:**

```json
{
  "data": {
    "title": "Product Launch",
    "description": "Launch of our new product line",
    "startDate": "2023-02-15T14:00:00Z",
    "endDate": "2023-02-15T16:00:00Z",
    "location": "Main Conference Room"
  }
}
```

**Response:**

```json
{
  "id": "event123",
  "title": "Product Launch",
  "description": "Launch of our new product line",
  "startDate": "2023-02-15T14:00:00Z",
  "endDate": "2023-02-15T16:00:00Z",
  "location": "Main Conference Room",
  "status": "scheduled",
  "createdAt": "2023-01-01T12:00:00Z"
}
```

### Update Event

**Endpoint:** `POST /crm/events/update`

**Description:** Updates an existing event.

**Request:**

```json
{
  "id": "event123",
  "data": {
    "startDate": "2023-02-16T14:00:00Z",
    "endDate": "2023-02-16T16:00:00Z",
    "status": "rescheduled"
  }
}
```

**Response:**

```json
{
  "id": "event123",
  "title": "Product Launch",
  "description": "Launch of our new product line",
  "startDate": "2023-02-16T14:00:00Z",
  "endDate": "2023-02-16T16:00:00Z",
  "location": "Main Conference Room",
  "status": "rescheduled",
  "updatedAt": "2023-01-02T09:30:00Z"
}
```

### Delete Event

**Endpoint:** `DELETE /crm/events/delete/:id`

**Description:** Deletes an event.

**Parameters:**

- `id`: The event ID

**Response:**

```json
{
  "success": true,
  "message": "Event deleted successfully"
}
```

## Service Points

### Get Service Points

**Endpoint:** `GET /crm/service-point/get/:id?`

**Description:** Gets service points, optionally filtered by ID.

**Parameters:**

- `id` (optional): The service point ID

**Response:**

```json
{
  "servicePoints": [
    {
      "id": "sp123",
      "name": "Downtown Office",
      "address": "123 Main St, Downtown",
      "phone": "555-1234",
      "email": "downtown@example.com",
      "hours": {
        "monday": "9:00-17:00",
        "tuesday": "9:00-17:00",
        "wednesday": "9:00-17:00",
        "thursday": "9:00-17:00",
        "friday": "9:00-17:00"
      }
    }
  ]
}
```

### Create Service Point

**Endpoint:** `POST /crm/service-point/create`

**Description:** Creates a new service point.

**Request:**

```json
{
  "data": {
    "name": "Downtown Office",
    "address": "123 Main St, Downtown",
    "phone": "555-1234",
    "email": "downtown@example.com",
    "hours": {
      "monday": "9:00-17:00",
      "tuesday": "9:00-17:00",
      "wednesday": "9:00-17:00",
      "thursday": "9:00-17:00",
      "friday": "9:00-17:00"
    }
  }
}
```

**Response:**

```json
{
  "id": "sp123",
  "name": "Downtown Office",
  "address": "123 Main St, Downtown",
  "phone": "555-1234",
  "email": "downtown@example.com",
  "hours": {
    "monday": "9:00-17:00",
    "tuesday": "9:00-17:00",
    "wednesday": "9:00-17:00",
    "thursday": "9:00-17:00",
    "friday": "9:00-17:00"
  },
  "createdAt": "2023-01-01T12:00:00Z"
}
```

### Update Service Point

**Endpoint:** `POST /crm/service-point/update`

**Description:** Updates an existing service point.

**Request:**

```json
{
  "id": "sp123",
  "data": {
    "phone": "555-5678",
    "hours": {
      "saturday": "10:00-15:00"
    }
  }
}
```

**Response:**

```json
{
  "id": "sp123",
  "name": "Downtown Office",
  "address": "123 Main St, Downtown",
  "phone": "555-5678",
  "email": "downtown@example.com",
  "hours": {
    "monday": "9:00-17:00",
    "tuesday": "9:00-17:00",
    "wednesday": "9:00-17:00",
    "thursday": "9:00-17:00",
    "friday": "9:00-17:00",
    "saturday": "10:00-15:00"
  },
  "updatedAt": "2023-01-02T09:30:00Z"
}
```

### Delete Service Point

**Endpoint:** `DELETE /crm/service-point/delete/:id`

**Description:** Deletes a service point.

**Parameters:**

- `id`: The service point ID

**Response:**

```json
{
  "success": true,
  "message": "Service point deleted successfully"
}
```

### Get Service Requests

**Endpoint:** `GET /crm/service-request/get/:servicePoint?`

**Description:** Gets service requests, optionally filtered by service point.

**Parameters:**

- `servicePoint` (optional): The service point ID

**Response:**

```json
{
  "serviceRequests": [
    {
      "id": "sr123",
      "servicePointId": "sp123",
      "customerId": "customer456",
      "type": "consultation",
      "status": "waiting",
      "queueNumber": 5,
      "estimatedWaitTime": 15,
      "createdAt": "2023-01-01T12:00:00Z"
    }
  ]
}
```

### Join Service Request Queue

**Endpoint:** `POST /crm/service-request-queue/join`

**Description:** Joins a service request queue.

**Request:**

```json
{
  "data": {
    "servicePointId": "sp123",
    "type": "consultation",
    "notes": "First-time customer"
  }
}
```

**Response:**

```json
{
  "id": "sr123",
  "servicePointId": "sp123",
  "customerId": "customer456",
  "type": "consultation",
  "status": "waiting",
  "queueNumber": 5,
  "estimatedWaitTime": 15,
  "createdAt": "2023-01-01T12:00:00Z"
}
```

### Update Service Request

**Endpoint:** `POST /crm/service-request/update/:id`

**Description:** Updates a service request.

**Parameters:**

- `id`: The service request ID

**Request:**

```json
{
  "data": {
    "status": "in-progress",
    "assignedTo": "agent123"
  }
}
```

**Response:**

```json
{
  "id": "sr123",
  "servicePointId": "sp123",
  "customerId": "customer456",
  "type": "consultation",
  "status": "in-progress",
  "assignedTo": "agent123",
  "updatedAt": "2023-01-01T12:15:00Z"
}
```

## Chat Messages

### Get Chat Messages

**Endpoint:** `GET /crm/chat-message/get/:id?`

**Description:** Gets chat messages, optionally filtered by ID.

**Parameters:**

- `id` (optional): The chat message ID

**Response:**

```json
{
  "messages": [
    {
      "id": "msg123",
      "sender": {
        "id": "customer456",
        "name": "John Doe"
      },
      "recipient": {
        "id": "agent123",
        "name": "Support Agent"
      },
      "content": "Hello, I need help with my order.",
      "timestamp": "2023-01-01T12:00:00Z",
      "read": false
    }
  ]
}
```

### Create Chat Message

**Endpoint:** `POST /crm/chat-message/create`

**Description:** Creates a new chat message.

**Request:**

```json
{
  "data": {
    "recipientId": "agent123",
    "content": "Hello, I need help with my order."
  }
}
```

**Response:**

```json
{
  "id": "msg123",
  "sender": {
    "id": "customer456",
    "name": "John Doe"
  },
  "recipient": {
    "id": "agent123",
    "name": "Support Agent"
  },
  "content": "Hello, I need help with my order.",
  "timestamp": "2023-01-01T12:00:00Z",
  "read": false
}
```

### Update Chat Message

**Endpoint:** `POST /crm/chat-message/update`

**Description:** Updates an existing chat message.

**Request:**

```json
{
  "id": "msg123",
  "data": {
    "read": true
  }
}
```

**Response:**

```json
{
  "id": "msg123",
  "sender": {
    "id": "customer456",
    "name": "John Doe"
  },
  "recipient": {
    "id": "agent123",
    "name": "Support Agent"
  },
  "content": "Hello, I need help with my order.",
  "timestamp": "2023-01-01T12:00:00Z",
  "read": true,
  "updatedAt": "2023-01-01T12:05:00Z"
}
```

### Delete Chat Message

**Endpoint:** `DELETE /crm/chat-message/delete/:id`

**Description:** Deletes a chat message.

**Parameters:**

- `id`: The chat message ID

**Response:**

```json
{
  "success": true,
  "message": "Chat message deleted successfully"
}
```

## Meetings

### Get Meeting Token

**Endpoint:** `GET /crm/meeting/token`

**Public Route:** Yes

**Description:** Gets a token for joining a meeting.

**Response:**

```json
{
  "token": "meeting-token-123",
  "expiresAt": "2023-01-01T13:00:00Z"
}
```

### Create Meeting

**Endpoint:** `GET /crm/meeting/create`

**Description:** Creates a new meeting.

**Request:**

```json
{
  "title": "Product Demo",
  "description": "Demonstration of new features",
  "startTime": "2023-01-15T14:00:00Z",
  "duration": 60,
  "participants": ["user1@example.com", "user2@example.com"]
}
```

**Response:**

```json
{
  "id": "meeting123",
  "title": "Product Demo",
  "description": "Demonstration of new features",
  "startTime": "2023-01-15T14:00:00Z",
  "endTime": "2023-01-15T15:00:00Z",
  "joinUrl": "https://example.com/meeting/join/123",
  "participants": [
    {
      "email": "user1@example.com",
      "status": "invited"
    },
    {
      "email": "user2@example.com",
      "status": "invited"
    }
  ]
}
```

### Validate Meeting

**Endpoint:** `GET /crm/meeting/validate`

**Description:** Validates a meeting.

**Request:**

```json
{
  "meetingId": "meeting123",
  "token": "participant-token-456"
}
```

**Response:**

```json
{
  "valid": true,
  "meeting": {
    "id": "meeting123",
    "title": "Product Demo",
    "startTime": "2023-01-15T14:00:00Z",
    "endTime": "2023-01-15T15:00:00Z"
  }
}
```

## FlexData

### Get FlexData

**Endpoint:** `GET /crm/flexdata/get/:id?`

**Description:** Gets FlexData, optionally filtered by ID.

**Parameters:**

- `id` (optional): The FlexData ID
- `query` (query parameter, optional): Query string to filter results

**Response:**

```json
{
  "flexData": [
    {
      "id": "fd123",
      "type": "customer-preference",
      "data": {
        "theme": "dark",
        "notifications": true,
        "language": "en-US"
      },
      "createdAt": "2023-01-01T12:00:00Z"
    }
  ]
}
```

### Create FlexData

**Endpoint:** `POST /crm/flexdata/create`

**Description:** Creates new FlexData.

**Request:**

```json
{
  "data": {
    "type": "customer-preference",
    "data": {
      "theme": "dark",
      "notifications": true,
      "language": "en-US"
    }
  }
}
```

**Response:**

```json
{
  "id": "fd123",
  "type": "customer-preference",
  "data": {
    "theme": "dark",
    "notifications": true,
    "language": "en-US"
  },
  "createdAt": "2023-01-01T12:00:00Z"
}
```

### Update FlexData

**Endpoint:** `POST /crm/flexdata/update`

**Description:** Updates existing FlexData.

**Request:**

```json
{
  "id": "fd123",
  "data": {
    "data": {
      "theme": "light",
      "notifications": false
    }
  }
}
```

**Response:**

```json
{
  "id": "fd123",
  "type": "customer-preference",
  "data": {
    "theme": "light",
    "notifications": false,
    "language": "en-US"
  },
  "updatedAt": "2023-01-02T09:30:00Z"
}
```

### Delete FlexData

**Endpoint:** `DELETE /crm/flexdata/delete/:id`

**Description:** Deletes FlexData.

**Parameters:**

- `id`: The FlexData ID

**Response:**

```json
{
  "success": true,
  "message": "FlexData deleted successfully"
}
```

## Inbox

### Get Inbox Conversations

**Endpoint:** `GET /crm/inbox/conversations`

**Description:** Gets distinct conversations in the inbox.

**Response:**

```json
{
  "conversations": [
    {
      "otherParty": {
        "id": "user123",
        "name": "John Doe",
        "email": "john@example.com"
      },
      "lastMessage": {
        "id": "msg456",
        "content": "Hello, I need help with my order.",
        "timestamp": "2023-01-01T12:00:00Z",
        "read": false
      },
      "unreadCount": 1
    }
  ]
}
```

### Get Conversation Messages

**Endpoint:** `GET /crm/inbox/conversation/messages/:otherParty`

**Description:** Gets messages in a conversation with another party.

**Parameters:**

- `otherParty`: The ID of the other party in the conversation
- `l` (query parameter, optional): Last item ID for pagination
- `p` (query parameter, optional): Page number (default: 1)
- `ps` (query parameter, optional): Page size (default: 50)
- `s` (query parameter, optional): Sort field (default: 'modifydate')
- `st` (query parameter, optional): Sort type (default: 'desc')
- `en` (query parameter, optional): Whether to enrich the response

**Response:**

```json
{
  "messages": [
    {
      "id": "msg123",
      "sender": {
        "id": "user123",
        "name": "John Doe"
      },
      "recipient": {
        "id": "customer456",
        "name": "Support Agent"
      },
      "content": "Hello, I need help with my order.",
      "timestamp": "2023-01-01T12:00:00Z",
      "read": true
    },
    {
      "id": "msg124",
      "sender": {
        "id": "customer456",
        "name": "Support Agent"
      },
      "recipient": {
        "id": "user123",
        "name": "John Doe"
