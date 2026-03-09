# Currencies API

Manage exchange rates and currency information.

## Get Exchange Rate

Get current exchange rate between two currencies.

**Endpoint:** `GET /currencies/exchange`

**Authentication:** JWT Bearer Token

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| from | string | Yes | Source currency code (USD, VES, etc.) |
| to | string | Yes | Target currency code |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "from": "USD",
    "to": "VES",
    "rate": 2850.50,
    "lastUpdated": "2024-01-23T13:00:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid currency codes
- `401` - Unauthorized

---

## Get Supported Currencies

List all supported currencies.

**Endpoint:** `GET /currencies`

**Authentication:** JWT Bearer Token

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": [
    {
      "code": "USD",
      "name": "US Dollar",
      "symbol": "$"
    },
    {
      "code": "VES",
      "name": "Venezuelan Bolívar",
      "symbol": "Bs."
    }
  ],
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized

---

## Convert Amount

Convert an amount from one currency to another.

**Endpoint:** `POST /currencies/convert`

**Authentication:** JWT Bearer Token

**Request Body:**
```json
{
  "amount": 1000.00,
  "from": "USD",
  "to": "VES"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| amount | decimal | Yes | Amount to convert |
| from | string | Yes | Source currency code |
| to | string | Yes | Target currency code |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "originalAmount": 1000.00,
    "originalCurrency": "USD",
    "convertedAmount": 2850500.00,
    "convertedCurrency": "VES",
    "rate": 2850.50,
    "timestamp": "2024-01-23T13:05:00Z"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid currency codes or amount
- `401` - Unauthorized
