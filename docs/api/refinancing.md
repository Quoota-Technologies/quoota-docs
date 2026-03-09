# Refinancing API

Manage loan refinancing operations.

## Create Refinance Request

Create a new refinancing request for an existing loan.

**Endpoint:** `POST /refinancing/requests`

**Authentication:** JWT Bearer Token

**Request Body:**
```json
{
  "loanId": "loan_123",
  "newTerm": 24,
  "newAmount": 6000.00,
  "reason": "Lower monthly payments"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| loanId | string | Yes | Loan ID to refinance |
| newTerm | integer | No | New loan term in months |
| newAmount | decimal | No | New loan amount |
| reason | string | No | Reason for refinancing |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "refinanceRequestId": "refin_123",
    "loanId": "loan_123",
    "status": "Pending",
    "newTerm": 24,
    "newAmount": 6000.00,
    "estimatedSavings": 250.00,
    "createdAt": "2024-01-23T15:00:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Loan not eligible for refinancing
- `401` - Unauthorized
- `404` - Loan not found

---

## Get Refinance Requests

List refinancing requests for the user.

**Endpoint:** `GET /refinancing/requests`

**Authentication:** JWT Bearer Token

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| status | string | Filter by status (Pending, Approved, Rejected) |
| page | number | Page number (default: 1) |
| limit | number | Items per page (default: 20) |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": [
    {
      "refinanceRequestId": "refin_123",
      "loanId": "loan_123",
      "status": "Approved",
      "newTerm": 24,
      "estimatedSavings": 250.00,
      "createdAt": "2024-01-23T15:00:00Z"
    }
  ],
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized

---

## Get Refinance Request Details

Retrieve details of a specific refinancing request.

**Endpoint:** `GET /refinancing/requests/:refinanceRequestId`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| refinanceRequestId | string | Refinance request ID |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "refinanceRequestId": "refin_123",
    "loanId": "loan_123",
    "originalLoan": {
      "amount": 5000.00,
      "term": 12,
      "monthlyPayment": 442.50,
      "rate": 8.5
    },
    "refinanceDetails": {
      "newAmount": 6000.00,
      "newTerm": 24,
      "newMonthlyPayment": 267.50,
      "newRate": 7.5,
      "estimatedSavings": 250.00
    },
    "status": "Approved",
    "approvalDate": "2024-01-24T10:00:00Z",
    "createdAt": "2024-01-23T15:00:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
- `404` - Refinance request not found

---

## Cancel Refinance Request

Cancel a pending refinancing request.

**Endpoint:** `DELETE /refinancing/requests/:refinanceRequestId`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| refinanceRequestId | string | Refinance request ID |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "message": "Refinance request cancelled"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Cannot cancel approved refinance request
- `401` - Unauthorized
- `404` - Refinance request not found
