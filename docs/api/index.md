# API Reference

Welcome to the Quoota API documentation. This is a comprehensive REST API reference for the Quoota lending platform, built with Fiber v2 and backed by Airtable and Firebase.

## API Overview

### Base URL
```
https://api.quoota.com/api/v1
```

### Authentication
The API uses two authentication methods:
- **Firebase Authentication** - For user sign-up, login, and public endpoints
- **JWT Bearer Tokens** - For authenticated requests after login

Include your JWT token in the Authorization header:
```
Authorization: Bearer <your_jwt_token>
```

### Response Format
All API responses follow a standard format:

**Success Response (200)**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    // Response data
  },
  "error": null
}
```

**Error Response (4xx/5xx)**
```json
{
  "success": false,
  "statusCode": 400,
  "data": null,
  "error": {
    "message": "Error description"
  }
}
```

### HTTP Methods
- `GET` - Retrieve data
- `POST` - Create new resources
- `PUT/PATCH` - Update existing resources
- `DELETE` - Cancel/remove resources

## API Sections

### Core Features
1. **[Authentication](./auth.md)** - User sign-up, login, password recovery
2. **[User Management](./users.md)** - Profile, documents, settings
3. **[Loan Requests](./loan-requests.md)** - Create and manage loan applications
4. **[Loans](./loans.md)** - Active loan management
5. **[Payments](./payments.md)** - Payment processing and history
6. **[Refinancing](./refinancing.md)** - Loan refinancing operations

### Additional Features
7. **[Support](./support.md)** - Support tickets and categories
8. **[Notifications](./notifications.md)** - Push notifications and messaging
9. **[Currencies](./currencies.md)** - Exchange rates
10. **[Treasury](./treasury.md)** - Treasury operations
11. **[Contracts](./contracts.md)** - Contract management
12. **[Admin](./admin.md)** - Administrative operations

## Rate Limiting
Rate limits are applied per user/IP. Current limits:
- Standard endpoints: 100 requests per hour
- Authentication endpoints: 10 requests per hour per IP

## Error Codes

| Code | Status | Description |
|------|--------|-------------|
| 400 | Bad Request | Invalid request parameters |
| 401 | Unauthorized | Missing or invalid authentication |
| 404 | Not Found | Resource not found |
| 409 | Conflict | Resource conflict (e.g., duplicate email) |
| 500 | Internal Server Error | Server error |

## Environment Variables
When implementing the API client, ensure you have:
- `FIREBASE_PROJECT_ID` - Firebase project ID
- `JWT_SECRET` - For JWT token validation
- `API_BASE_URL` - Base URL of the API

## Timezone & Dates
- All timestamps are in UTC (ISO 8601 format)
- Dates should be sent in `YYYY-MM-DD` format

## Pagination
List endpoints support pagination via query parameters:
- `page` - Page number (default: 1)
- `limit` - Items per page (default: 20, max: 100)

## Getting Help
For support and questions:
- Check the specific endpoint documentation
- Review error messages and status codes
- Contact support at support@quoota.com
