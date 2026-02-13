# Users - API Reference

## Overview
The Users API provides endpoints for retrieving user information within the Late platform. Users represent team members who have access to profiles and accounts.

---

## Endpoints

### List Users
**GET** `/v1/users`

Returns a paginated list of users accessible to the authenticated user. The results depend on the API key's scope and the user's role.

**Query Parameters:**
- `page` (integer, optional): Page number (default: `1`)
- `limit` (integer, optional): Results per page (default: `25`, max: `100`)
- `search` (string, optional): Search by name or email
- `role` (string, optional): Filter by role - `owner`, `admin`, `editor`, `viewer`
- `status` (string, optional): Filter by status - `active`, `invited`, `suspended`
- `sort` (string, optional): Sort field - `name`, `email`, `createdAt`, `lastLoginAt` (default: `createdAt`)
- `order` (string, optional): Sort order - `asc`, `desc` (default: `asc`)

**Response (200):**
```json
{
  "users": [
    {
      "_id": "user_abc123",
      "email": "owner@example.com",
      "name": "Jane Owner",
      "avatarUrl": "https://cdn.late.com/avatars/user_abc123.jpg",
      "role": "owner",
      "status": "active",
      "profiles": [
        {
          "_id": "6507a1b2c3d4e5f6a7b8c9d0",
          "name": "Marketing Team"
        }
      ],
      "permissions": {
        "canManageUsers": true,
        "canManageProfiles": true,
        "canManageBilling": true,
        "canPublish": true,
        "canApprove": true
      },
      "lastLoginAt": "2024-11-15T08:00:00Z",
      "createdAt": "2024-01-15T10:00:00Z"
    },
    {
      "_id": "user_def456",
      "email": "editor@example.com",
      "name": "John Editor",
      "avatarUrl": "https://cdn.late.com/avatars/user_def456.jpg",
      "role": "editor",
      "status": "active",
      "profiles": [
        {
          "_id": "6507a1b2c3d4e5f6a7b8c9d0",
          "name": "Marketing Team"
        }
      ],
      "permissions": {
        "canManageUsers": false,
        "canManageProfiles": false,
        "canManageBilling": false,
        "canPublish": true,
        "canApprove": false
      },
      "lastLoginAt": "2024-11-14T16:30:00Z",
      "createdAt": "2024-06-01T12:00:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 25,
    "total": 8,
    "totalPages": 1
  }
}
```

---

### Get User by ID
**GET** `/v1/users/{userId}`

Retrieves detailed information about a specific user.

**Path Parameters:**
- `userId` (string, required): The user ID

**Response (200):**
```json
{
  "user": {
    "_id": "user_abc123",
    "email": "owner@example.com",
    "name": "Jane Owner",
    "avatarUrl": "https://cdn.late.com/avatars/user_abc123.jpg",
    "role": "owner",
    "status": "active",
    "profiles": [
      {
        "_id": "6507a1b2c3d4e5f6a7b8c9d0",
        "name": "Marketing Team",
        "color": "#4CAF50"
      },
      {
        "_id": "6508b2c3d4e5f6a7b8c9d0e1",
        "name": "Personal Brand",
        "color": "#ffeda0"
      }
    ],
    "permissions": {
      "canManageUsers": true,
      "canManageProfiles": true,
      "canManageBilling": true,
      "canPublish": true,
      "canApprove": true
    },
    "timezone": "America/New_York",
    "locale": "en-US",
    "lastLoginAt": "2024-11-15T08:00:00Z",
    "createdAt": "2024-01-15T10:00:00Z",
    "updatedAt": "2024-11-10T12:00:00Z"
  }
}
```

**Error Responses:**
- **401:** Unauthorized access
- **403:** Insufficient permissions to view this user
- **404:** User not found

---

## User Object Properties

| Property | Type | Description |
|----------|------|-------------|
| `_id` | string | Unique user identifier |
| `email` | string | User's email address |
| `name` | string | User's display name |
| `avatarUrl` | string | URL to the user's avatar image |
| `role` | string | User's role: `owner`, `admin`, `editor`, `viewer` |
| `status` | string | Account status: `active`, `invited`, `suspended` |
| `profiles` | array | Profiles the user has access to |
| `profiles[]._id` | string | Profile ID |
| `profiles[].name` | string | Profile name |
| `profiles[].color` | string | Profile color (hex code) |
| `permissions` | object | Detailed permission flags |
| `permissions.canManageUsers` | boolean | Can invite, remove, or change roles of other users |
| `permissions.canManageProfiles` | boolean | Can create, update, or delete profiles |
| `permissions.canManageBilling` | boolean | Can access and modify billing settings |
| `permissions.canPublish` | boolean | Can create and publish posts |
| `permissions.canApprove` | boolean | Can approve posts created by other users |
| `timezone` | string | User's timezone (IANA format) |
| `locale` | string | User's locale/language preference |
| `lastLoginAt` | string | ISO 8601 timestamp of the user's last login |
| `createdAt` | string | ISO 8601 timestamp of when the user was created |
| `updatedAt` | string | ISO 8601 timestamp of the last update to the user record |

---

## Role Permissions

| Permission | Owner | Admin | Editor | Viewer |
|-----------|-------|-------|--------|--------|
| View posts and analytics | Yes | Yes | Yes | Yes |
| Create and edit posts | Yes | Yes | Yes | No |
| Publish posts | Yes | Yes | Yes | No |
| Approve posts | Yes | Yes | No | No |
| Manage accounts | Yes | Yes | No | No |
| Manage profiles | Yes | Yes | No | No |
| Manage users | Yes | Yes | No | No |
| Manage billing | Yes | No | No | No |
| Manage API keys | Yes | Yes | No | No |

---

## Error Responses Summary

| Status Code | Description |
|-------------|-------------|
| **400** | Invalid request parameters |
| **401** | Unauthorized - invalid or expired API key |
| **403** | Insufficient permissions |
| **404** | User not found |
| **429** | Rate limit exceeded |
| **500** | Internal server error |

---
