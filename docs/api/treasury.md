# Treasury API

Treasury operations and financial transaction management.

## Get Processed Transactions

Get list of processed treasury transactions.

**Endpoint:** `GET /treasury/transactions`

**Authentication:** JWT Bearer Token

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| status | string | Filter by status (Processed, Pending, Failed) |
| type | string | Filter by transaction type |
| page | number | Page number (default: 1) |
| limit | number | Items per page (default: 20) |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": [
    {
      "transactionId": "txn_123",
      "type": "disbursement",
      "amount": 5000.00,
      "currency": "USD",
      "status": "Processed",
      "processedAt": "2024-01-20T10:30:00Z"
    }
  ],
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized

---

## Get Transaction Details

Retrieve details of a specific treasury transaction.

**Endpoint:** `GET /treasury/transactions/:transactionId`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| transactionId | string | Transaction ID |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "transactionId": "txn_123",
    "type": "disbursement",
    "amount": 5000.00,
    "currency": "USD",
    "status": "Processed",
    "loanId": "loan_123",
    "recipient": {
      "accountNumber": "****1234",
      "bankCode": "001"
    },
    "processedAt": "2024-01-20T10:30:00Z",
    "createdAt": "2024-01-20T10:00:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
- `404` - Transaction not found

---

## Get Treasury Balance

Get current treasury account balance.

**Endpoint:** `GET /treasury/balance`

**Authentication:** JWT Bearer Token

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "balance": 500000.00,
    "currency": "USD",
    "lastUpdated": "2024-01-23T14:00:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
