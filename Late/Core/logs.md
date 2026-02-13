# Logs - API Reference

## Overview
The Logs API provides endpoints for retrieving activity logs related to posts, connections, and other operations within the Late platform. Logs provide an audit trail of actions, errors, and status changes.

---

## Endpoints

### List All Logs (Deprecated)
**GET** `/v1/logs`

> **Deprecated:** This endpoint is deprecated and will be removed in a future version. Use the specific log endpoints (`/v1/posts/logs`, `/v1/connections/logs`) instead for better performance and filtering.

Returns a paginated list of all activity logs for the authenticated user.

**Query Parameters:**
- `profileId` (string, optional): Filter by profile ID
- `type` (string, optional): Filter by log type - `post`, `connection`, `account`, `system`
- `level` (string, optional): Filter by severity level - `info`, `warning`, `error`
- `startDate` (string, optional): Filter logs after this date (ISO 8601)
- `endDate` (string, optional): Filter logs before this date (ISO 8601)
- `page` (integer, optional): Page number (default: `1`)
- `limit` (integer, optional): Results per page (default: `50`, max: `100`)
- `sort` (string, optional): Sort field - `createdAt` (default: `createdAt`)
- `order` (string, optional): Sort order - `asc`, `desc` (default: `desc`)

**Response (200):**
```json
{
  "logs": [
    {
      "_id": "log_a1b2c3d4e5f6",
      "type": "post",
      "level": "info",
      "action": "post.published",
      "message": "Post published successfully to Twitter",
      "metadata": {
        "postId": "65a0b1c2d3e4f5a6b7c8d9e0",
        "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
        "platform": "twitter",
        "platformPostId": "1234567890"
      },
      "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
      "userId": "user_abc123",
      "createdAt": "2024-11-15T14:00:05Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 50,
    "total": 1250,
    "totalPages": 25
  }
}
```

---

### Get Log by ID
**GET** `/v1/logs/{logId}`

Retrieves a specific log entry by its ID.

**Path Parameters:**
- `logId` (string, required): The log entry ID

**Response (200):**
```json
{
  "log": {
    "_id": "log_a1b2c3d4e5f6",
    "type": "post",
    "level": "error",
    "action": "post.failed",
    "message": "Failed to publish post to Instagram: Media upload error",
    "metadata": {
      "postId": "65a0b1c2d3e4f5a6b7c8d9e0",
      "accountId": "64f0b2c3d4e5f6a7b8c9d0e1",
      "platform": "instagram",
      "errorCode": "MEDIA_UPLOAD_FAILED",
      "errorDetails": "The image format is not supported. Supported formats: JPEG, PNG.",
      "retryable": true
    },
    "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
    "userId": "user_abc123",
    "createdAt": "2024-11-15T14:00:05Z"
  }
}
```

**Error Responses:**
- **401:** Unauthorized access
- **404:** Log entry not found

---

### Get Post Logs
**GET** `/v1/posts/logs`

Returns logs specifically related to post operations (creation, scheduling, publishing, failures).

**Query Parameters:**
- `profileId` (string, optional): Filter by profile ID
- `accountId` (string, optional): Filter by account ID
- `postId` (string, optional): Filter by specific post ID
- `platform` (string, optional): Filter by platform
- `action` (string, optional): Filter by action - `post.created`, `post.scheduled`, `post.publishing`, `post.published`, `post.failed`, `post.updated`, `post.deleted`, `post.retried`
- `level` (string, optional): Filter by severity - `info`, `warning`, `error`
- `startDate` (string, optional): Filter logs after this date (ISO 8601)
- `endDate` (string, optional): Filter logs before this date (ISO 8601)
- `page` (integer, optional): Page number (default: `1`)
- `limit` (integer, optional): Results per page (default: `50`, max: `100`)

**Response (200):**
```json
{
  "logs": [
    {
      "_id": "log_post001",
      "type": "post",
      "level": "info",
      "action": "post.published",
      "message": "Post published successfully to Twitter",
      "metadata": {
        "postId": "65a0b1c2d3e4f5a6b7c8d9e0",
        "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
        "platform": "twitter",
        "platformPostId": "1234567890",
        "platformPostUrl": "https://twitter.com/exampleuser/status/1234567890"
      },
      "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
      "createdAt": "2024-11-15T14:00:05Z"
    },
    {
      "_id": "log_post002",
      "type": "post",
      "level": "error",
      "action": "post.failed",
      "message": "Post failed to publish to Facebook: Insufficient permissions",
      "metadata": {
        "postId": "65a0c2d3e4f5a6b7c8d9e0f1",
        "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
        "platform": "facebook",
        "errorCode": "PERMISSION_DENIED",
        "errorDetails": "The access token does not have the pages_manage_posts permission.",
        "retryable": false
      },
      "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
      "createdAt": "2024-11-15T14:05:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 50,
    "total": 340,
    "totalPages": 7
  }
}
```

---

### Get Connection Logs
**GET** `/v1/connections/logs`

Returns logs related to account connection and disconnection events.

**Query Parameters:**
- `profileId` (string, optional): Filter by profile ID
- `accountId` (string, optional): Filter by account ID
- `platform` (string, optional): Filter by platform
- `action` (string, optional): Filter by action - `account.connected`, `account.disconnected`, `account.reconnected`, `account.token_refreshed`, `account.token_expired`
- `level` (string, optional): Filter by severity - `info`, `warning`, `error`
- `startDate` (string, optional): Filter logs after this date (ISO 8601)
- `endDate` (string, optional): Filter logs before this date (ISO 8601)
- `page` (integer, optional): Page number (default: `1`)
- `limit` (integer, optional): Results per page (default: `50`, max: `100`)

**Response (200):**
```json
{
  "logs": [
    {
      "_id": "log_conn001",
      "type": "connection",
      "level": "info",
      "action": "account.connected",
      "message": "Instagram account @exampleuser connected successfully",
      "metadata": {
        "accountId": "64f0b2c3d4e5f6a7b8c9d0e1",
        "platform": "instagram",
        "platformUsername": "exampleuser",
        "connectionMethod": "oauth"
      },
      "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
      "userId": "user_abc123",
      "createdAt": "2024-11-10T08:00:00Z"
    },
    {
      "_id": "log_conn002",
      "type": "connection",
      "level": "warning",
      "action": "account.token_expired",
      "message": "Facebook access token has expired for My Business Page",
      "metadata": {
        "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
        "platform": "facebook",
        "platformUsername": "My Business Page",
        "tokenExpiredAt": "2024-11-15T00:00:00Z",
        "autoReconnectAttempted": true,
        "autoReconnectSuccess": false
      },
      "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
      "createdAt": "2024-11-15T00:05:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 50,
    "total": 28,
    "totalPages": 1
  }
}
```

---

### Get Logs for Specific Post
**GET** `/v1/posts/{postId}/logs`

Returns all log entries associated with a specific post, providing a complete timeline of the post's lifecycle.

**Path Parameters:**
- `postId` (string, required): The post ID

**Query Parameters:**
- `level` (string, optional): Filter by severity - `info`, `warning`, `error`
- `page` (integer, optional): Page number (default: `1`)
- `limit` (integer, optional): Results per page (default: `50`, max: `100`)

**Response (200):**
```json
{
  "postId": "65a0b1c2d3e4f5a6b7c8d9e0",
  "logs": [
    {
      "_id": "log_pl001",
      "level": "info",
      "action": "post.created",
      "message": "Post created and scheduled for 2024-11-20T14:00:00Z",
      "metadata": {
        "scheduledAt": "2024-11-20T14:00:00Z",
        "accountIds": ["64f0a1b2c3d4e5f6a7b8c9d0"],
        "platforms": ["twitter"]
      },
      "userId": "user_abc123",
      "createdAt": "2024-11-15T10:00:00Z"
    },
    {
      "_id": "log_pl002",
      "level": "info",
      "action": "post.updated",
      "message": "Post text updated",
      "metadata": {
        "changedFields": ["text", "media"]
      },
      "userId": "user_abc123",
      "createdAt": "2024-11-16T09:00:00Z"
    },
    {
      "_id": "log_pl003",
      "level": "info",
      "action": "post.publishing",
      "message": "Post publishing initiated",
      "metadata": {
        "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
        "platform": "twitter"
      },
      "createdAt": "2024-11-20T14:00:00Z"
    },
    {
      "_id": "log_pl004",
      "level": "info",
      "action": "post.published",
      "message": "Post published successfully to Twitter",
      "metadata": {
        "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
        "platform": "twitter",
        "platformPostId": "1234567890",
        "platformPostUrl": "https://twitter.com/exampleuser/status/1234567890"
      },
      "createdAt": "2024-11-20T14:00:05Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 50,
    "total": 4,
    "totalPages": 1
  }
}
```

**Error Responses:**
- **401:** Unauthorized access
- **404:** Post not found

---

## Example Log Entry

A complete log entry contains the following fields:

```json
{
  "_id": "log_a1b2c3d4e5f6",
  "type": "post",
  "level": "error",
  "action": "post.failed",
  "message": "Failed to publish post to Instagram: Media upload error",
  "metadata": {
    "postId": "65a0b1c2d3e4f5a6b7c8d9e0",
    "accountId": "64f0b2c3d4e5f6a7b8c9d0e1",
    "platform": "instagram",
    "errorCode": "MEDIA_UPLOAD_FAILED",
    "errorDetails": "The image format is not supported. Supported formats: JPEG, PNG.",
    "retryable": true,
    "retryCount": 0
  },
  "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
  "userId": "user_abc123",
  "createdAt": "2024-11-15T14:00:05Z"
}
```

**Log Entry Fields:**

| Field | Type | Description |
|-------|------|-------------|
| `_id` | string | Unique log entry identifier |
| `type` | string | Log category: `post`, `connection`, `account`, `system` |
| `level` | string | Severity level: `info`, `warning`, `error` |
| `action` | string | Specific action that triggered the log |
| `message` | string | Human-readable description of the event |
| `metadata` | object | Additional context data specific to the action |
| `profileId` | string | Associated profile ID |
| `userId` | string | User who triggered the action (if applicable) |
| `createdAt` | string | ISO 8601 timestamp of when the log was created |

---

## Error Responses Summary

| Status Code | Description |
|-------------|-------------|
| **400** | Invalid request parameters |
| **401** | Unauthorized - invalid or expired API key |
| **404** | Log entry or post not found |
| **429** | Rate limit exceeded |
| **500** | Internal server error |

---

## Notes

- Log entries are retained for 90 days. Older logs are automatically purged.
- The deprecated `/v1/logs` endpoint has a higher latency than the specific log endpoints.
- Log metadata varies by action type and may include platform-specific error codes.
- Logs are generated in real-time and are available immediately after an action occurs.
