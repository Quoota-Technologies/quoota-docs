# User Management API

Manage user profiles, documents, settings, and personal information.

## Get User Profile

Retrieve the authenticated user's profile information.

**Endpoint:** `GET /user/profile`

**Authentication:** JWT Bearer Token

**Query Parameters:**
None

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "id": "user_123",
    "email": "user@example.com",
    "firstName": "John",
    "lastName": "Doe",
    "phone": "+14155552671",
    "verified": true,
    "profileImage": "https://...",
    "createdAt": "2024-01-15T10:30:00Z",
    "updatedAt": "2024-01-20T15:45:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
- `404` - User not found

---

## Update User Profile

Update user's profile information.

**Endpoint:** `PUT /user/profile`

**Authentication:** JWT Bearer Token

**Request Body:**
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "phone": "+14155552671"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| firstName | string | No | User's first name |
| lastName | string | No | User's last name |
| phone | string | No | Phone number in E.164 format |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "id": "user_123",
    "firstName": "John",
    "lastName": "Doe",
    "phone": "+14155552671",
    "updatedAt": "2024-01-20T16:00:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid input data
- `401` - Unauthorized

---

## Upload Profile Picture

Upload or update user's profile picture.

**Endpoint:** `POST /user/profilePicture`

**Authentication:** JWT Bearer Token

**Request Type:** multipart/form-data

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| file | file | Yes | Image file (JPG, PNG, max 5MB) |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "url": "https://storage.googleapis.com/...",
    "updatedAt": "2024-01-20T16:15:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid file format or size
- `401` - Unauthorized

---

## Get Credit Score

Retrieve user's credit score information.

**Endpoint:** `GET /user/creditScore`

**Authentication:** JWT Bearer Token

**Query Parameters:**
None

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "score": 750,
    "rating": "Good",
    "accountAge": 24,
    "lastUpdated": "2024-01-15T10:30:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
- `404` - Credit information not available

---

## Upload Document

Upload a user document (ID, proof of address, etc.).

**Endpoint:** `POST /user/documents`

**Authentication:** JWT Bearer Token

**Request Type:** multipart/form-data

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| file | file | Yes | Document file (PDF, JPG, PNG) |
| documentType | string | Yes | Type: ID, PROOF_ADDRESS, INCOME_STATEMENT, BANK_STATEMENT |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "documentId": "doc_123",
    "documentType": "ID",
    "url": "https://storage.googleapis.com/...",
    "verified": false,
    "uploadDate": "2024-01-20T16:30:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid file or document type
- `401` - Unauthorized

---

## Get User Documents

List all documents uploaded by the user.

**Endpoint:** `GET /user/documents`

**Authentication:** JWT Bearer Token

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| documentType | string | Filter by document type |
| page | number | Page number (default: 1) |
| limit | number | Items per page (default: 20) |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": [
    {
      "documentId": "doc_123",
      "documentType": "ID",
      "url": "https://storage.googleapis.com/...",
      "verified": true,
      "uploadDate": "2024-01-15T10:30:00Z"
    }
  ],
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized

---

## Delete Document

Delete a user document.

**Endpoint:** `DELETE /user/documents/:documentId`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| documentId | string | Document ID to delete |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "message": "Document deleted successfully"
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
- `404` - Document not found

---

## Get Notifications Preferences

Retrieve user's notification settings.

**Endpoint:** `GET /user/notificationPreferences`

**Authentication:** JWT Bearer Token

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "emailNotifications": true,
    "pushNotifications": true,
    "loanUpdates": true,
    "paymentReminders": true,
    "promotions": false
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized

---

## Update Notification Preferences

Update user's notification settings.

**Endpoint:** `PUT /user/notificationPreferences`

**Authentication:** JWT Bearer Token

**Request Body:**
```json
{
  "emailNotifications": true,
  "pushNotifications": true,
  "loanUpdates": true,
  "paymentReminders": true,
  "promotions": false
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| emailNotifications | boolean | No | Enable email notifications |
| pushNotifications | boolean | No | Enable push notifications |
| loanUpdates | boolean | No | Enable loan update notifications |
| paymentReminders | boolean | No | Enable payment reminder notifications |
| promotions | boolean | No | Enable promotional notifications |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "message": "Preferences updated successfully"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid preference data
- `401` - Unauthorized

---

## Get User Gamification Stats

Retrieve user's gamification/rewards information.

**Endpoint:** `GET /user/gamification`

**Authentication:** JWT Bearer Token

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "points": 1500,
    "level": "Gold",
    "achievements": [
      {
        "id": "first_loan",
        "name": "First Loan",
        "unlockedAt": "2024-01-15T10:30:00Z"
      }
    ],
    "rewards": 250
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized

---

## Delete User Account

Delete the authenticated user's account.

**Endpoint:** `DELETE /user/account`

**Authentication:** JWT Bearer Token

**Request Body:**
```json
{
  "password": "userPassword123",
  "reason": "No longer needed"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| password | string | Yes | User's password for confirmation |
| reason | string | No | Reason for deletion |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "message": "Account deleted successfully"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid password
- `401` - Unauthorized
