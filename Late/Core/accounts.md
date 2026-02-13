# Accounts - API Reference

## Overview
The Accounts API provides endpoints for managing connected social media accounts. You can list accounts, retrieve follower statistics, disconnect accounts, update account settings, and check account health status.

---

## Endpoints

### List Connected Social Accounts
**GET** `/v1/accounts`

Returns all social media accounts connected to the authenticated user's profiles.

**Query Parameters:**
- `profileId` (string, optional): Filter accounts by profile ID
- `platform` (string, optional): Filter by platform (e.g., `twitter`, `facebook`, `instagram`)
- `status` (string, optional): Filter by status (`active`, `expired`, `error`)
- `page` (integer, optional): Page number for pagination (default: `1`)
- `limit` (integer, optional): Number of results per page (default: `25`, max: `100`)

**Response (200):**
```json
{
  "accounts": [
    {
      "_id": "64f0a1b2c3d4e5f6a7b8c9d0",
      "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
      "platform": "twitter",
      "platformAccountId": "123456789",
      "platformUsername": "@exampleuser",
      "displayName": "Example User",
      "avatarUrl": "https://pbs.twimg.com/profile_images/.../photo.jpg",
      "status": "active",
      "followers": 5200,
      "isHealthy": true,
      "connectedAt": "2024-11-01T10:00:00Z",
      "lastHealthCheck": "2024-11-15T08:30:00Z"
    },
    {
      "_id": "64f0b2c3d4e5f6a7b8c9d0e1",
      "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
      "platform": "instagram",
      "platformAccountId": "987654321",
      "platformUsername": "exampleuser",
      "displayName": "Example User",
      "avatarUrl": "https://instagram.com/.../avatar.jpg",
      "status": "active",
      "followers": 12400,
      "isHealthy": true,
      "connectedAt": "2024-10-15T14:00:00Z",
      "lastHealthCheck": "2024-11-15T08:30:00Z"
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

### Get Follower Stats
**GET** `/v1/accounts/follower-stats`

Returns follower count history and growth statistics for connected accounts.

**Query Parameters:**
- `accountId` (string, optional): Filter by specific account ID
- `profileId` (string, optional): Filter by profile ID
- `startDate` (string, optional): Start date in ISO 8601 format (e.g., `2024-10-01`)
- `endDate` (string, optional): End date in ISO 8601 format (e.g., `2024-11-01`)
- `granularity` (string, optional): Data granularity - `daily`, `weekly`, `monthly` (default: `daily`)

**Response (200):**
```json
{
  "stats": [
    {
      "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
      "platform": "twitter",
      "platformUsername": "@exampleuser",
      "currentFollowers": 5200,
      "followerHistory": [
        {
          "date": "2024-10-01",
          "followers": 4800
        },
        {
          "date": "2024-10-15",
          "followers": 5000
        },
        {
          "date": "2024-11-01",
          "followers": 5200
        }
      ],
      "growth": {
        "absolute": 400,
        "percentage": 8.33
      }
    }
  ]
}
```

---

### Disconnect Account
**DELETE** `/v1/accounts/{accountId}`

Disconnects a social media account from the platform. This revokes stored tokens and removes the account association. Any scheduled posts for this account will be cancelled.

**Path Parameters:**
- `accountId` (string, required): The ID of the account to disconnect

**Response (200):**
```json
{
  "message": "Account disconnected successfully",
  "accountId": "64f0a1b2c3d4e5f6a7b8c9d0"
}
```

**Error Responses:**
- **401:** Unauthorized access
- **403:** Insufficient permissions to disconnect this account
- **404:** Account not found

---

### Update Account
**PUT** `/v1/accounts/{accountId}`

Updates account settings and metadata. Can be used to move an account between profiles or update display preferences.

**Path Parameters:**
- `accountId` (string, required): The ID of the account to update

**Request Body:**
- `profileId` (string, optional): Move the account to a different profile
- `displayName` (string, optional): Custom display name override
- `labels` (array of strings, optional): Tags for organizing accounts
- `timezone` (string, optional): Timezone for scheduling (e.g., `America/New_York`)
- `autoReconnect` (boolean, optional): Attempt automatic token refresh when expired

**Response (200):**
```json
{
  "message": "Account updated successfully",
  "account": {
    "_id": "64f0a1b2c3d4e5f6a7b8c9d0",
    "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
    "platform": "twitter",
    "platformUsername": "@exampleuser",
    "displayName": "Updated Display Name",
    "labels": ["marketing", "primary"],
    "timezone": "America/New_York",
    "autoReconnect": true,
    "updatedAt": "2024-11-15T10:00:00Z"
  }
}
```

**Error Responses:**
- **400:** Invalid request body or parameters
- **401:** Unauthorized access
- **403:** Insufficient permissions
- **404:** Account not found

---

### Check Health - All Accounts
**GET** `/v1/accounts/health`

Performs a health check on all connected accounts and returns their current status. This verifies token validity and platform connectivity.

**Query Parameters:**
- `profileId` (string, optional): Filter health check by profile ID
- `platform` (string, optional): Filter by platform

**Response (200):**
```json
{
  "accounts": [
    {
      "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
      "platform": "twitter",
      "platformUsername": "@exampleuser",
      "isHealthy": true,
      "status": "active",
      "lastChecked": "2024-11-15T08:30:00Z",
      "details": null
    },
    {
      "accountId": "64f0b2c3d4e5f6a7b8c9d0e1",
      "platform": "facebook",
      "platformUsername": "My Business Page",
      "isHealthy": false,
      "status": "expired",
      "lastChecked": "2024-11-15T08:30:00Z",
      "details": "Access token has expired. Please reconnect the account."
    }
  ]
}
```

---

### Check Health - Specific Account
**GET** `/v1/accounts/{accountId}/health`

Performs a health check on a single connected account.

**Path Parameters:**
- `accountId` (string, required): The ID of the account to check

**Response (200):**
```json
{
  "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
  "platform": "twitter",
  "platformUsername": "@exampleuser",
  "isHealthy": true,
  "status": "active",
  "lastChecked": "2024-11-15T08:30:00Z",
  "tokenExpiry": "2025-02-01T00:00:00Z",
  "permissions": [
    "tweet.read",
    "tweet.write",
    "users.read"
  ],
  "rateLimits": {
    "remaining": 280,
    "limit": 300,
    "resetsAt": "2024-11-15T09:00:00Z"
  }
}
```

**Error Responses:**
- **401:** Unauthorized access
- **404:** Account not found

---

## Error Responses Summary

| Status Code | Description |
|-------------|-------------|
| **400** | Invalid request parameters |
| **401** | Unauthorized - invalid or expired API key |
| **403** | Insufficient permissions for the requested operation |
| **404** | Account not found |
| **429** | Rate limit exceeded |
| **500** | Internal server error |
