# Account Groups - API Reference

## Overview
The Account Groups API provides endpoints for organizing connected social media accounts into logical groups. Account groups allow you to batch-select accounts when creating posts, simplifying workflows for teams managing many accounts.

---

## Endpoints

### List Account Groups
**GET** `/v1/account-groups`

Returns all account groups for the authenticated user.

**Query Parameters:**
- `profileId` (string, optional): Filter by profile ID
- `page` (integer, optional): Page number (default: `1`)
- `limit` (integer, optional): Results per page (default: `25`, max: `100`)

**Response (200):**
```json
{
  "accountGroups": [
    {
      "_id": "ag_a1b2c3d4e5f6",
      "name": "All Social Media",
      "description": "All active social media accounts for daily posting",
      "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
      "accountIds": [
        "64f0a1b2c3d4e5f6a7b8c9d0",
        "64f0b2c3d4e5f6a7b8c9d0e1",
        "64f0c3d4e5f6a7b8c9d0e1f2"
      ],
      "accounts": [
        {
          "_id": "64f0a1b2c3d4e5f6a7b8c9d0",
          "platform": "twitter",
          "platformUsername": "@exampleuser"
        },
        {
          "_id": "64f0b2c3d4e5f6a7b8c9d0e1",
          "platform": "instagram",
          "platformUsername": "exampleuser"
        },
        {
          "_id": "64f0c3d4e5f6a7b8c9d0e1f2",
          "platform": "facebook",
          "platformUsername": "My Business Page"
        }
      ],
      "createdAt": "2024-11-01T10:00:00Z",
      "updatedAt": "2024-11-10T12:00:00Z"
    },
    {
      "_id": "ag_g7h8i9j0k1l2",
      "name": "Visual Platforms",
      "description": "Instagram and Pinterest for visual content",
      "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
      "accountIds": [
        "64f0b2c3d4e5f6a7b8c9d0e1",
        "64f0d4e5f6a7b8c9d0e1f2g3"
      ],
      "accounts": [
        {
          "_id": "64f0b2c3d4e5f6a7b8c9d0e1",
          "platform": "instagram",
          "platformUsername": "exampleuser"
        },
        {
          "_id": "64f0d4e5f6a7b8c9d0e1f2g3",
          "platform": "pinterest",
          "platformUsername": "exampleuser"
        }
      ],
      "createdAt": "2024-11-05T09:00:00Z",
      "updatedAt": "2024-11-05T09:00:00Z"
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

### Create Account Group
**POST** `/v1/account-groups`

Creates a new account group.

**Request Body:**
- `name` (string, required): Group name (max 100 characters)
- `description` (string, optional): Group description (max 500 characters)
- `profileId` (string, required): The profile ID to associate the group with
- `accountIds` (array of strings, required): Array of account IDs to include in the group (minimum 1)

**Request Example:**
```json
{
  "name": "Blog Promotion",
  "description": "Accounts used for promoting blog content",
  "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
  "accountIds": [
    "64f0a1b2c3d4e5f6a7b8c9d0",
    "64f0b2c3d4e5f6a7b8c9d0e1",
    "64f0c3d4e5f6a7b8c9d0e1f2"
  ]
}
```

**Response (201):**
```json
{
  "message": "Account group created successfully",
  "accountGroup": {
    "_id": "ag_m3n4o5p6q7r8",
    "name": "Blog Promotion",
    "description": "Accounts used for promoting blog content",
    "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
    "accountIds": [
      "64f0a1b2c3d4e5f6a7b8c9d0",
      "64f0b2c3d4e5f6a7b8c9d0e1",
      "64f0c3d4e5f6a7b8c9d0e1f2"
    ],
    "accounts": [
      {
        "_id": "64f0a1b2c3d4e5f6a7b8c9d0",
        "platform": "twitter",
        "platformUsername": "@exampleuser"
      },
      {
        "_id": "64f0b2c3d4e5f6a7b8c9d0e1",
        "platform": "instagram",
        "platformUsername": "exampleuser"
      },
      {
        "_id": "64f0c3d4e5f6a7b8c9d0e1f2",
        "platform": "facebook",
        "platformUsername": "My Business Page"
      }
    ],
    "createdAt": "2024-11-15T10:00:00Z",
    "updatedAt": "2024-11-15T10:00:00Z"
  }
}
```

**Error Responses:**
- **400:** Invalid request body (missing name, empty accountIds, name too long)
- **401:** Unauthorized access
- **403:** Insufficient permissions for the specified profile
- **404:** Profile or one or more account IDs not found
- **409:** An account group with this name already exists in the profile

---

### Delete Account Group
**DELETE** `/v1/account-groups/{groupId}`

Deletes an account group. This does not affect the accounts themselves, only the grouping.

**Path Parameters:**
- `groupId` (string, required): The account group ID

**Response (200):**
```json
{
  "message": "Account group deleted successfully",
  "groupId": "ag_m3n4o5p6q7r8"
}
```

**Error Responses:**
- **401:** Unauthorized access
- **403:** Insufficient permissions
- **404:** Account group not found

---

### Update Account Group
**PUT** `/v1/account-groups/{groupId}`

Updates an existing account group's name, description, or member accounts.

**Path Parameters:**
- `groupId` (string, required): The account group ID

**Request Body:**
- `name` (string, optional): Updated group name (max 100 characters)
- `description` (string, optional): Updated description (max 500 characters)
- `accountIds` (array of strings, optional): Updated list of account IDs. This replaces the existing list entirely.

**Request Example:**
```json
{
  "name": "Blog & Newsletter Promotion",
  "accountIds": [
    "64f0a1b2c3d4e5f6a7b8c9d0",
    "64f0b2c3d4e5f6a7b8c9d0e1",
    "64f0c3d4e5f6a7b8c9d0e1f2",
    "64f0e5f6a7b8c9d0e1f2g3h4"
  ]
}
```

**Response (200):**
```json
{
  "message": "Account group updated successfully",
  "accountGroup": {
    "_id": "ag_m3n4o5p6q7r8",
    "name": "Blog & Newsletter Promotion",
    "description": "Accounts used for promoting blog content",
    "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
    "accountIds": [
      "64f0a1b2c3d4e5f6a7b8c9d0",
      "64f0b2c3d4e5f6a7b8c9d0e1",
      "64f0c3d4e5f6a7b8c9d0e1f2",
      "64f0e5f6a7b8c9d0e1f2g3h4"
    ],
    "accounts": [
      {
        "_id": "64f0a1b2c3d4e5f6a7b8c9d0",
        "platform": "twitter",
        "platformUsername": "@exampleuser"
      },
      {
        "_id": "64f0b2c3d4e5f6a7b8c9d0e1",
        "platform": "instagram",
        "platformUsername": "exampleuser"
      },
      {
        "_id": "64f0c3d4e5f6a7b8c9d0e1f2",
        "platform": "facebook",
        "platformUsername": "My Business Page"
      },
      {
        "_id": "64f0e5f6a7b8c9d0e1f2g3h4",
        "platform": "linkedin",
        "platformUsername": "Acme Corporation"
      }
    ],
    "updatedAt": "2024-11-15T12:00:00Z"
  }
}
```

**Error Responses:**
- **400:** Invalid request body (name too long, empty accountIds array)
- **401:** Unauthorized access
- **403:** Insufficient permissions
- **404:** Account group or one or more account IDs not found
- **409:** An account group with this name already exists in the profile

---

## Error Responses Summary

| Status Code | Description |
|-------------|-------------|
| **400** | Invalid request parameters or body |
| **401** | Unauthorized - invalid or expired API key |
| **403** | Insufficient permissions |
| **404** | Account group, profile, or account not found |
| **409** | Duplicate group name within the same profile |
| **429** | Rate limit exceeded |
| **500** | Internal server error |

---

## Notes

- Account groups are scoped to a single profile. An account group cannot span multiple profiles.
- When updating `accountIds`, the entire list is replaced. To add a single account, include all existing account IDs plus the new one.
- Deleting an account group does not disconnect or remove any accounts.
- Account groups can be referenced when creating posts to quickly select multiple accounts.

---
