# Admin API

Administrative operations and system management.

## Clear Cache

Clear application cache for a specific resource or globally.

**Endpoint:** `POST /admin/cache/clear`

**Authentication:** Admin JWT Bearer Token

**Request Body:**
```json
{
  "cacheKey": "users",
  "all": false
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| cacheKey | string | No | Specific cache key to clear (users, loans, payments, etc.) |
| all | boolean | No | Clear all cache if true (overrides cacheKey) |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "message": "Cache cleared successfully",
    "itemsCleared": 150
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized (not admin)
- `400` - Invalid cache key

---

## Get System Health

Get system health and status information.

**Endpoint:** `GET /admin/health`

**Authentication:** Admin JWT Bearer Token

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "status": "healthy",
    "services": {
      "database": "connected",
      "firebase": "connected",
      "redis": "connected",
      "email": "operational"
    },
    "uptime": 3600000,
    "timestamp": "2024-01-23T15:30:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized (not admin)

---

## Bulk Update User Status

Update status for multiple users (admin only).

**Endpoint:** `PUT /admin/users/bulkUpdate`

**Authentication:** Admin JWT Bearer Token

**Request Body:**
```json
{
  "userIds": ["user_123", "user_456"],
  "status": "verified",
  "reason": "Bulk verification batch"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| userIds | array | Yes | Array of user IDs to update |
| status | string | Yes | New status (verified, suspended, etc.) |
| reason | string | No | Reason for bulk update |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "usersUpdated": 2,
    "timestamp": "2024-01-23T15:35:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid user IDs or status
- `401` - Unauthorized (not admin)

---

## Get API Metrics

Get API usage and performance metrics.

**Endpoint:** `GET /admin/metrics`

**Authentication:** Admin JWT Bearer Token

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| startDate | string | Start date (ISO 8601) |
| endDate | string | End date (ISO 8601) |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "totalRequests": 150000,
    "averageResponseTime": 45,
    "errorRate": 0.5,
    "topEndpoints": [
      {
        "path": "/loans",
        "requests": 25000
      }
    ],
    "period": {
      "start": "2024-01-01T00:00:00Z",
      "end": "2024-01-23T15:40:00Z"
    }
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized (not admin)
