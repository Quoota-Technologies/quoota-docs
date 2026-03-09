# Support API

Manage support tickets and customer service interactions.

## Create Support Ticket

Submit a new support ticket.

**Endpoint:** `POST /support/tickets`

**Authentication:** JWT Bearer Token

**Request Body:**
```json
{
  "subject": "Unable to make payment",
  "description": "I'm experiencing issues making a payment on my loan",
  "categoryId": "category_001",
  "priority": "High",
  "attachments": ["file_id_123"]
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subject | string | Yes | Ticket subject |
| description | string | Yes | Detailed description |
| categoryId | string | Yes | Support category ID |
| priority | string | No | Priority: Low, Medium, High, Critical |
| attachments | array | No | Array of attachment file IDs |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "ticketId": "ticket_123",
    "subject": "Unable to make payment",
    "status": "Open",
    "priority": "High",
    "createdAt": "2024-01-22T16:30:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid ticket data
- `401` - Unauthorized

---

## Get Support Tickets

List support tickets for the authenticated user.

**Endpoint:** `GET /support/tickets`

**Authentication:** JWT Bearer Token

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| status | string | Filter by status (Open, In Progress, Resolved, Closed) |
| priority | string | Filter by priority |
| page | number | Page number (default: 1) |
| limit | number | Items per page (default: 20) |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": [
    {
      "ticketId": "ticket_123",
      "subject": "Unable to make payment",
      "status": "In Progress",
      "priority": "High",
      "createdAt": "2024-01-22T16:30:00Z",
      "updatedAt": "2024-01-23T09:00:00Z"
    }
  ],
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized

---

## Get Ticket Details

Retrieve detailed information about a support ticket.

**Endpoint:** `GET /support/tickets/:ticketId`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| ticketId | string | Support ticket ID |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "ticketId": "ticket_123",
    "userId": "user_123",
    "subject": "Unable to make payment",
    "description": "I'm experiencing issues making a payment on my loan",
    "status": "In Progress",
    "priority": "High",
    "categoryId": "category_001",
    "assignedAgent": "agent_123",
    "messages": [
      {
        "messageId": "msg_001",
        "sender": "user_123",
        "content": "I can't make a payment",
        "timestamp": "2024-01-22T16:30:00Z"
      }
    ],
    "createdAt": "2024-01-22T16:30:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
- `404` - Ticket not found

---

## Add Ticket Message

Add a message/reply to a support ticket.

**Endpoint:** `POST /support/tickets/:ticketId/messages`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| ticketId | string | Support ticket ID |

**Request Body:**
```json
{
  "message": "I've tried multiple times but still getting an error",
  "attachments": ["file_id_456"]
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| message | string | Yes | Message content |
| attachments | array | No | Array of attachment file IDs |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "messageId": "msg_002",
    "ticketId": "ticket_123",
    "content": "I've tried multiple times but still getting an error",
    "timestamp": "2024-01-23T10:15:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid message
- `401` - Unauthorized
- `404` - Ticket not found

---

## Close Ticket

Close a support ticket.

**Endpoint:** `POST /support/tickets/:ticketId/close`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| ticketId | string | Support ticket ID |

**Request Body:**
```json
{
  "resolution": "Issue was resolved by updating payment method"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| resolution | string | No | How the issue was resolved |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "ticketId": "ticket_123",
    "status": "Closed",
    "closedAt": "2024-01-23T11:00:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
- `404` - Ticket not found

---

## Get Support Categories

Get list of support ticket categories.

**Endpoint:** `GET /support/categories`

**Authentication:** JWT Bearer Token

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": [
    {
      "categoryId": "category_001",
      "name": "Payment Issues",
      "description": "Help with payment problems"
    },
    {
      "categoryId": "category_002",
      "name": "Loan Questions",
      "description": "Questions about loans"
    }
  ],
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
