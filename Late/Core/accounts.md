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

---

### Disconnect Account
**DELETE** `/v1/accounts/{accountId}`

Disconnects a social media account from the platform. This revokes stored tokens and removes the account association.

### Update Account
**PUT** `/v1/accounts/{accountId}`

Updates account settings and metadata. Can be used to move an account between profiles.

### Check Health - All Accounts
**GET** `/v1/accounts/health`

Performs a health check on all connected accounts and returns their current status.

### Check Health - Specific Account
**GET** `/v1/accounts/{accountId}/health`

Performs a health check on a single connected account.

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
