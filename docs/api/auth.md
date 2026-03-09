# Authentication API

Manage user authentication, including sign-up, login, password recovery, and token management.

## Sign Up

Create a new user account with email and password.

**Endpoint:** `POST /auth/signup`

**Authentication:** None (Public)

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "securePassword123",
  "firstName": "John",
  "lastName": "Doe"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| email | string | Yes | User's email address |
| password | string | Yes | Password (must be at least 8 characters) |
| firstName | string | Yes | User's first name |
| lastName | string | Yes | User's last name |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "uid": "user_id_123",
    "email": "user@example.com",
    "token": "jwt_token_here"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid email format or password too weak
- `409` - Email already exists

---

## Login

Authenticate a user and retrieve JWT token.

**Endpoint:** `POST /auth/login`

**Authentication:** None (Public)

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "securePassword123"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| email | string | Yes | User's email address |
| password | string | Yes | User's password |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "uid": "user_id_123",
    "email": "user@example.com",
    "token": "jwt_token_here"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Missing email or password
- `401` - Invalid credentials

---

## Send Password Reset Email

Request a password reset email for a user account.

**Endpoint:** `POST /auth/forgotPassword`

**Authentication:** None (Public)

**Request Body:**
```json
{
  "email": "user@example.com"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| email | string | Yes | Email address associated with account |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": null,
  "error": null
}
```

**Error Responses:**
- `400` - Invalid email format
- `404` - User not found

---

## Verify Email

Verify user's email address with OTP or token.

**Endpoint:** `POST /auth/verifyEmail`

**Authentication:** Firebase Auth

**Request Body:**
```json
{
  "code": "123456"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| code | string | Yes | OTP code sent to email |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "verified": true
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid or expired code
- `401` - Unauthorized

---

## Change Password

Change the password for authenticated user.

**Endpoint:** `POST /auth/changePassword`

**Authentication:** Firebase Auth

**Request Body:**
```json
{
  "oldPassword": "currentPassword123",
  "newPassword": "newPassword456"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| oldPassword | string | Yes | Current password |
| newPassword | string | Yes | New password (must be different from old) |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "message": "Password updated successfully"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Password validation failed
- `401` - Incorrect current password

---

## Verify Phone Number

Verify user's phone number with OTP.

**Endpoint:** `POST /auth/verifyPhone`

**Authentication:** Firebase Auth

**Request Body:**
```json
{
  "phone": "+14155552671",
  "code": "123456"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| phone | string | Yes | Phone number in E.164 format |
| code | string | Yes | OTP code sent to phone |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "verified": true
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid phone format or OTP
- `401` - Unauthorized

---

## Request Phone Verification

Send OTP to user's phone number.

**Endpoint:** `POST /auth/requestPhoneVerification`

**Authentication:** Firebase Auth

**Request Body:**
```json
{
  "phone": "+14155552671"
}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| phone | string | Yes | Phone number in E.164 format |

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "message": "OTP sent to phone"
  },
  "error": null
}
```

**Error Responses:**
- `400` - Invalid phone format
- `429` - Too many requests

---

## Refresh Token

Get a new JWT token using current authentication.

**Endpoint:** `POST /auth/refreshToken`

**Authentication:** Firebase Auth

**Request Body:**
```json
{}
```

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "token": "new_jwt_token_here"
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized

---

## Logout

Invalidate the current session/token.

**Endpoint:** `POST /auth/logout`

**Authentication:** Firebase Auth

**Request Body:**
```json
{}
```

**Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "message": "Logged out successfully"
  },
  "error": null
}
```

**Error Responses:**
- `401` - Unauthorized
