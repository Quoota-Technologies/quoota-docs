# Contracts API

Manage loan contracts and contract signing.

## Get Contract Signing URL

Get URL to sign a loan contract.

**Endpoint:** `GET /contracts/:loanId/signingUrl`

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
    "signingUrl": "https://signing.quoota.com/contract/contract_123",
    "expiresAt": "2024-01-30T14:30:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
- `404` - Loan not found

---

## Update Contract Status

Update the status of a loan contract.

**Endpoint:** `PUT /contracts/:contractId/status`

**Authentication:** JWT Bearer Token

**Path Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| contractId | string | Contract ID |

**Request Body:**
```json
{
  "status": "signed",
  "signatureDate": "2024-01-23T14:30:00Z"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| status | string | Yes | Contract status: pending, signed, executed, cancelled |
| signatureDate | string | No | Date of signature (ISO 8601) |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "contractId": "contract_123",
    "status": "signed",
    "signatureDate": "2024-01-23T14:30:00Z",
    "updatedAt": "2024-01-23T14:35:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid status transition
- `401` - Unauthorized
- `404` - Contract not found
