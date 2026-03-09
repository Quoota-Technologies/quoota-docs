# Loan Requests API

Create and manage loan applications/requests.

## Create Loan Request

Submit a new loan request/application.

**Endpoint:** `POST /requests`

**Authentication:** JWT Bearer Token

**Request Body:**
```json
{
  "amount": 5000.00,
  "currency": "USD",
  "term": 12,
  "purpose": "Personal",
  "employmentStatus": "Employed",
  "companyName": "Tech Corp"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| amount | decimal | Yes | Loan amount requested |
| currency | string | Yes | Loan currency (USD, VES, etc.) |
| term | integer | Yes | Loan term in months (3-60) |
| purpose | string | No | Loan purpose |
| employmentStatus | string | No | Employment status (Employed, Self-employed, Unemployed) |
| companyName | string | No | Company name if employed |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "requestId": "req_123",
    "userId": "user_123",
    "amount": 5000.00,
    "currency": "USD",
    "term": 12,
    "status": "Pending",
    "createdAt": "2024-01-20T17:00:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid request parameters
- `401` - Unauthorized

---

## Get Loan Requests

List all loan requests for the authenticated user.

**Endpoint:** `GET /requests`

**Authentication:** JWT Bearer Token

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| status | string | Filter by status (Pending, Approved, Rejected, etc.) |
| page | number | Page number (default: 1) |
| limit | number | Items per page (default: 20) |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": [
    {
      "requestId": "req_123",
      "amount": 5000.00,
      "currency": "USD",
      "term": 12,
      "status": "Approved",
      "createdAt": "2024-01-20T17:00:00Z"
    }
  ],
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized

---

## Get Loan Request Details

Retrieve detailed information about a specific loan request.

**Endpoint:** `GET /requests/:requestId`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| requestId | string | Loan request ID |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "requestId": "req_123",
    "userId": "user_123",
    "amount": 5000.00,
    "currency": "USD",
    "term": 12,
    "interestRate": 8.5,
    "monthlyPayment": 442.50,
    "status": "Approved",
    "approvalDate": "2024-01-21T10:30:00Z",
    "createdAt": "2024-01-20T17:00:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
- `404` - Request not found

---

## Update Loan Request

Update an existing loan request (only if still pending).

**Endpoint:** `PUT /requests/:requestId`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| requestId | string | Loan request ID |

**Request Body:**
```json
{
  "amount": 6000.00,
  "term": 18,
  "purpose": "Business Expansion"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| amount | decimal | No | Updated loan amount |
| term | integer | No | Updated loan term |
| purpose | string | No | Updated loan purpose |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "requestId": "req_123",
    "amount": 6000.00,
    "term": 18,
    "updatedAt": "2024-01-22T11:00:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Cannot update non-pending request
- `401` - Unauthorized
- `404` - Request not found

---

## Cancel Loan Request

Cancel a pending loan request.

**Endpoint:** `DELETE /requests/:requestId`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| requestId | string | Loan request ID |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "message": "Request cancelled successfully"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Cannot cancel approved or rejected request
- `401` - Unauthorized
- `404` - Request not found

---

## Get Amortization Preview

Calculate loan amortization schedule before approval.

**Endpoint:** `POST /requests/amortization`

**Authentication:** JWT Bearer Token

**Request Body:**
```json
{
  "amount": 5000.00,
  "term": 12,
  "interestRate": 8.5
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| amount | decimal | Yes | Loan amount |
| term | integer | Yes | Loan term in months |
| interestRate | decimal | Yes | Annual interest rate |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "totalAmount": 5510.00,
    "monthlyPayment": 459.17,
    "totalInterest": 510.00,
    "schedule": [
      {
        "month": 1,
        "payment": 459.17,
        "principal": 417.45,
        "interest": 41.72,
        "balance": 4582.55
      }
    ]
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid calculation parameters
- `401` - Unauthorized

---

## Get Loan Request Parameters

Get available parameters for creating a loan request.

**Endpoint:** `GET /requests/parameters`

**Authentication:** JWT Bearer Token

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "minAmount": 100.00,
    "maxAmount": 50000.00,
    "minTerm": 3,
    "maxTerm": 60,
    "currencies": ["USD", "VES"],
    "purposes": ["Personal", "Business", "Emergency"],
    "employmentStatuses": ["Employed", "Self-employed", "Unemployed"]
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
