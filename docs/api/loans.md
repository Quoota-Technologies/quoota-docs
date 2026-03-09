# Loans API

Manage active loans and loan details.

## Get Active Loans

List all active loans for the authenticated user.

**Endpoint:** `GET /loans`

**Authentication:** JWT Bearer Token

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| status | string | Filter by status (Active, Paused, Completed, Defaulted) |
| page | number | Page number (default: 1) |
| limit | number | Items per page (default: 20) |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": [
    {
      "loanId": "loan_123",
      "amount": 5000.00,
      "currency": "USD",
      "term": 12,
      "remainingTerm": 8,
      "monthlyPayment": 442.50,
      "status": "Active",
      "nextPaymentDate": "2024-02-20T00:00:00Z",
      "createdAt": "2024-01-15T10:30:00Z"
    }
  ],
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized

---

## Get Loan Details

Retrieve detailed information about a specific active loan.

**Endpoint:** `GET /loans/:loanId`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| loanId | string | Loan ID |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "loanId": "loan_123",
    "userId": "user_123",
    "amount": 5000.00,
    "currency": "USD",
    "term": 12,
    "remainingTerm": 8,
    "monthlyPayment": 442.50,
    "interestRate": 8.5,
    "totalInterest": 310.00,
    "paidAmount": 1768.50,
    "remainingAmount": 3310.00,
    "status": "Active",
    "nextPaymentDate": "2024-02-20T00:00:00Z",
    "lastPaymentDate": "2024-01-20T10:30:00Z",
    "disbursementDate": "2024-01-15T10:30:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
- `404` - Loan not found

---

## Get Loan Payment Info

Get payment schedule and payment information for a loan.

**Endpoint:** `GET /loans/:loanId/paymentInfo`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| loanId | string | Loan ID |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "monthlyPayment": 442.50,
    "nextPaymentDate": "2024-02-20T00:00:00Z",
    "nextPaymentAmount": 442.50,
    "totalPaymentsRemaining": 8,
    "paymentSchedule": [
      {
        "paymentNumber": 4,
        "dueDate": "2024-02-20T00:00:00Z",
        "principal": 406.67,
        "interest": 35.83,
        "balance": 3310.00,
        "status": "Pending"
      }
    ]
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
- `404` - Loan not found

---

## Check Refinance Eligibility

Check if a loan is eligible for refinancing.

**Endpoint:** `GET /loans/:loanId/refinanceEligibility`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| loanId | string | Loan ID |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "loanId": "loan_123",
    "eligible": true,
    "reason": "Loan has 8+ payments remaining",
    "estimatedSavings": 150.00,
    "currentRate": 8.5,
    "refinanceRate": 7.5
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
- `404` - Loan not found

---

## Pause Loan

Temporarily pause loan payments (if eligible).

**Endpoint:** `POST /loans/:loanId/pause`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| loanId | string | Loan ID |

**Request Body:**
```json
{
  "reason": "Temporary financial hardship",
  "duration": 3
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| reason | string | No | Reason for pause |
| duration | integer | No | Pause duration in months |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "loanId": "loan_123",
    "status": "Paused",
    "pauseStartDate": "2024-01-22T00:00:00Z",
    "pauseEndDate": "2024-04-22T00:00:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Loan not eligible for pause
- `401` - Unauthorized
- `404` - Loan not found
