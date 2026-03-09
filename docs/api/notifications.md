# Notifications API

Manage push notifications and communication preferences.

## Send Notification

Send a notification to the authenticated user.

**Endpoint:** `POST /notifications/send`

**Authentication:** JWT Bearer Token

**Request Body:**
```json
{
  "title": "Payment Reminder",
  "message": "Your loan payment is due in 3 days",
  "notificationType": "payment_reminder"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| title | string | Yes | Notification title |
| message | string | Yes | Notification message |
| notificationType | string | No | Type: payment_reminder, loan_update, promotion, etc. |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "notificationId": "notif_123",
    "status": "Sent",
    "sentAt": "2024-01-23T12:00:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid notification data
- `401` - Unauthorized

---

## Get Notifications

Retrieve notifications for the authenticated user.

**Endpoint:** `GET /notifications`

**Authentication:** JWT Bearer Token

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| status | string | Filter by status (Unread, Read) |
| type | string | Filter by notification type |
| page | number | Page number (default: 1) |
| limit | number | Items per page (default: 20) |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": [
    {
      "notificationId": "notif_123",
      "title": "Payment Reminder",
      "message": "Your loan payment is due in 3 days",
      "type": "payment_reminder",
      "status": "Unread",
      "createdAt": "2024-01-23T12:00:00Z"
    }
  ],
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized

---

## Get Notification Details

Retrieve details of a specific notification.

**Endpoint:** `GET /notifications/:notificationId`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| notificationId | string | Notification ID |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "notificationId": "notif_123",
    "title": "Payment Reminder",
    "message": "Your loan payment is due in 3 days",
    "type": "payment_reminder",
    "status": "Read",
    "actionUrl": "https://app.quoota.com/loans/loan_123",
    "readAt": "2024-01-23T12:05:00Z",
    "createdAt": "2024-01-23T12:00:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
- `404` - Notification not found

---

## Mark Notification as Read

Mark a notification as read.

**Endpoint:** `PUT /notifications/:notificationId/read`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| notificationId | string | Notification ID |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "notificationId": "notif_123",
    "status": "Read",
    "readAt": "2024-01-23T12:05:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
- `404` - Notification not found

---

## Register Device Token

Register a device token for push notifications.

**Endpoint:** `POST /notifications/deviceToken`

**Authentication:** JWT Bearer Token

**Request Body:**
```json
{
  "token": "device_token_here",
  "platform": "ios",
  "deviceName": "iPhone 12"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| token | string | Yes | Device push notification token |
| platform | string | Yes | Platform: ios, android, web |
| deviceName | string | No | Device name/identifier |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "deviceId": "device_123",
    "platform": "ios",
    "status": "Active"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid token or platform
- `401` - Unauthorized

---

## Unregister Device Token

Unregister a device token from push notifications.

**Endpoint:** `DELETE /notifications/deviceToken/:deviceId`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| deviceId | string | Device ID to unregister |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "message": "Device unregistered successfully"
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
- `404` - Device not found
