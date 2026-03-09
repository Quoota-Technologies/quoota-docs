# Payments API

Manage loan payments and payment processing.

## Get Banks List

Get list of available banks for payments.

**Endpoint:** `GET /payments/banks`

**Authentication:** JWT Bearer Token

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| country | string | Filter by country code |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": [
    {
      "bankId": "bank_001",
      "name": "First Bank",
      "code": "001",
      "country": "VE",
      "supportsInstantTransfer": true
    }
  ],
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized

---

## Make Payment

Submit a loan payment.

**Endpoint:** `POST /payments`

**Authentication:** JWT Bearer Token

**Request Body:**
```json
{
  "loanId": "loan_123",
  "amount": 442.50,
  "paymentMethod": "instant_debit",
  "bankId": "bank_001"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| loanId | string | Yes | Loan ID to pay |
| amount | decimal | Yes | Payment amount |
| paymentMethod | string | Yes | Method: instant_debit, bank_transfer, card |
| bankId | string | No | Bank ID (required for bank_transfer) |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "paymentId": "pay_123",
    "loanId": "loan_123",
    "amount": 442.50,
    "status": "Processing",
    "transactionId": "txn_123",
    "createdAt": "2024-01-22T15:30:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid payment data
- `401` - Unauthorized
- `402` - Payment processing failed

---

## Get Payment History

Retrieve payment history for a user.

**Endpoint:** `GET /payments`

**Authentication:** JWT Bearer Token

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| loanId | string | Filter by loan ID |
| status | string | Filter by status (Completed, Failed, Pending) |
| page | number | Page number (default: 1) |
| limit | number | Items per page (default: 20) |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": [
    {
      "paymentId": "pay_123",
      "loanId": "loan_123",
      "amount": 442.50,
      "status": "Completed",
      "paymentMethod": "instant_debit",
      "transactionId": "txn_123",
      "createdAt": "2024-01-20T10:30:00Z"
    }
  ],
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized

---

## Get Payment Details

Retrieve details of a specific payment.

**Endpoint:** `GET /payments/:paymentId`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| paymentId | string | Payment ID |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "paymentId": "pay_123",
    "loanId": "loan_123",
    "amount": 442.50,
    "status": "Completed",
    "paymentMethod": "instant_debit",
    "transactionId": "txn_123",
    "bankId": "bank_001",
    "reference": "REF-123-456",
    "completedAt": "2024-01-20T10:35:00Z",
    "createdAt": "2024-01-20T10:30:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
- `404` - Payment not found

---

## Create Payment with Reference

Create a payment using a bank reference number.

**Endpoint:** `POST /payments/reference`

**Authentication:** JWT Bearer Token

**Request Body:**
```json
{
  "loanId": "loan_123",
  "referenceNumber": "REF-123-456",
  "amount": 442.50
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| loanId | string | Yes | Loan ID to pay |
| referenceNumber | string | Yes | Bank reference number |
| amount | decimal | Yes | Payment amount |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "paymentId": "pay_124",
    "loanId": "loan_123",
    "amount": 442.50,
    "status": "Pending",
    "reference": "REF-123-456"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid payment data
- `401` - Unauthorized

---

## Verify Payment

Verify and confirm a pending payment.

**Endpoint:** `POST /payments/:paymentId/verify`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| paymentId | string | Payment ID to verify |

**Request Body:**
```json
{
  "confirmationCode": "CONF-123"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| confirmationCode | string | No | Bank confirmation code |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "paymentId": "pay_124",
    "status": "Completed",
    "completedAt": "2024-01-22T16:00:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Payment verification failed
- `401` - Unauthorized
- `404` - Payment not found
