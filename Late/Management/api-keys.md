# API Keys - API Reference

## Overview
The API Keys management endpoints allow you to create, list, and delete API keys for authenticating with the Late API. API keys provide programmatic access to the platform and can be scoped to specific profiles and permissions.

---

## Endpoints

### List API Keys
**GET** `/v1/api-keys`

Returns all API keys for the authenticated user. Key values are partially masked for security.

**Query Parameters:**
- `status` (string, optional): Filter by status - `active`, `revoked`, `expired`
- `page` (integer, optional): Page number (default: `1`)
- `limit` (integer, optional): Results per page (default: `25`, max: `100`)

**Response (200):**
```json
{
  "apiKeys": [
    {
      "_id": "key_a1b2c3d4e5f6",
      "name": "Production Integration",
      "key": "late_sk_••••••••••••••••••••3f9a",
      "status": "active",
      "scopes": ["posts:read", "posts:write", "accounts:read", "analytics:read"],
      "profileIds": ["6507a1b2c3d4e5f6a7b8c9d0"],
      "lastUsedAt": "2024-11-15T14:00:00Z",
      "expiresAt": null,
      "createdAt": "2024-10-01T10:00:00Z",
      "createdBy": {
        "userId": "user_abc123",
        "name": "Jane Owner"
      }
    },
    {
      "_id": "key_g7h8i9j0k1l2",
      "name": "Analytics Dashboard",
      "key": "late_sk_••••••••••••••••••••7b2e",
      "status": "active",
      "scopes": ["analytics:read", "accounts:read"],
      "profileIds": [],
      "lastUsedAt": "2024-11-14T22:00:00Z",
      "expiresAt": "2025-10-01T10:00:00Z",
      "createdAt": "2024-10-15T08:00:00Z",
      "createdBy": {
        "userId": "user_abc123",
        "name": "Jane Owner"
      }
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 25,
    "total": 2,
    "totalPages": 1
  }
}
```

---

### Create API Key
**POST** `/v1/api-keys`

Creates a new API key. The full key value is only returned once upon creation.

**Request Body:**
- `name` (string, required): Descriptive name for the API key (max 100 characters)
- `scopes` (array of strings, optional): Permission scopes for the key. If omitted, the key inherits all permissions of the creating user.
- `profileIds` (array of strings, optional): Restrict the key to specific profiles. If omitted or empty, the key has access to all profiles the user can access.
- `expiresAt` (string, optional): Expiration date in ISO 8601 format. If omitted, the key does not expire.

**Available Scopes:**

| Scope | Description |
|-------|-------------|
| `profiles:read` | Read profile information |
| `profiles:write` | Create, update, delete profiles |
| `accounts:read` | Read connected account information |
| `accounts:write` | Connect, disconnect, update accounts |
| `posts:read` | Read post information |
| `posts:write` | Create, update, delete, schedule posts |
| `analytics:read` | Read analytics and metrics |
| `inbox:read` | Read messages, comments, reviews |
| `inbox:write` | Send messages, reply to comments/reviews |
| `webhooks:read` | Read webhook configurations |
| `webhooks:write` | Create, update, delete webhooks |
| `users:read` | Read user information |
| `users:write` | Manage users and invitations |
| `logs:read` | Read activity logs |

**Request Example:**
```json
{
  "name": "Content Scheduler Integration",
  "scopes": [
    "posts:read",
    "posts:write",
    "accounts:read",
    "profiles:read"
  ],
  "profileIds": ["6507a1b2c3d4e5f6a7b8c9d0"],
  "expiresAt": "2025-12-31T23:59:59Z"
}
```

**Response (201):**
```json
{
  "message": "API key created successfully",
  "apiKey": {
    "_id": "key_m3n4o5p6q7r8",
    "name": "Content Scheduler Integration",
    "key": "late_sk_a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6",
    "status": "active",
    "scopes": [
      "posts:read",
      "posts:write",
      "accounts:read",
      "profiles:read"
    ],
    "profileIds": ["6507a1b2c3d4e5f6a7b8c9d0"],
    "expiresAt": "2025-12-31T23:59:59Z",
    "createdAt": "2024-11-15T10:00:00Z",
    "createdBy": {
      "userId": "user_abc123",
      "name": "Jane Owner"
    }
  }
}
```

**Important:** The `key` field contains the full API key and is only returned during creation. Store it securely immediately -- it cannot be retrieved again. If lost, delete the key and create a new one.

**Error Responses:**
- **400:** Invalid request body (missing name, invalid scope, invalid expiration date)
- **401:** Unauthorized access
- **403:** Insufficient permissions to create API keys (requires admin or owner role)
- **404:** One or more profile IDs not found
- **409:** An API key with this name already exists
- **422:** Expiration date is in the past

---

### Delete API Key
**DELETE** `/v1/api-keys/{keyId}`

Permanently revokes and deletes an API key. Any requests using this key will immediately receive 401 Unauthorized responses.

**Path Parameters:**
- `keyId` (string, required): The API key ID

**Response (200):**
```json
{
  "message": "API key deleted successfully",
  "keyId": "key_m3n4o5p6q7r8"
}
```

**Error Responses:**
- **401:** Unauthorized access
- **403:** Insufficient permissions to delete this API key
- **404:** API key not found

---

## Error Responses Summary

| Status Code | Description |
|-------------|-------------|
| **400** | Invalid request parameters or body |
| **401** | Unauthorized - invalid or expired API key |
| **403** | Insufficient permissions (requires admin or owner role) |
| **404** | API key or profile not found |
| **409** | Duplicate API key name |
| **422** | Invalid expiration date |
| **429** | Rate limit exceeded |
| **500** | Internal server error |

---

## Authentication

All API requests must include the API key in the `Authorization` header:

```
Authorization: Bearer late_sk_a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6
```

## Notes

- API keys inherit the role-based permissions of the user who created them, further restricted by the specified scopes.
- Expired API keys are automatically revoked and cannot be reactivated. Create a new key instead.
- There is a maximum of 25 active API keys per user.
- API key names must be unique within a user's key set.
- Deleting a user automatically revokes all API keys created by that user.

---
