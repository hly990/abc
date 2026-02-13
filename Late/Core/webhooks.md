# Webhooks - API Reference

## Overview
The Webhooks API allows you to subscribe to real-time event notifications from the Late platform. When an event occurs (e.g., a post is published, a comment is received), Late sends an HTTP POST request to your configured endpoint with event details. Webhooks use HMAC signatures for payload verification.

---

## Endpoints

### List Webhook Settings
**GET** `/v1/webhooks/settings`

Returns all configured webhook subscriptions for the authenticated user.

**Query Parameters:**
- `profileId` (string, optional): Filter by profile ID
- `status` (string, optional): Filter by status - `active`, `inactive`, `failed`

**Response (200):**
```json
{
  "webhooks": [
    {
      "_id": "wh_a1b2c3d4e5f6",
      "url": "https://example.com/webhooks/late",
      "events": ["post.published", "post.failed", "comment.created"],
      "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
      "status": "active",
      "secret": "whsec_••••••••••••",
      "createdAt": "2024-11-01T10:00:00Z",
      "updatedAt": "2024-11-01T10:00:00Z",
      "lastTriggeredAt": "2024-11-15T14:00:00Z",
      "failureCount": 0
    }
  ]
}
```

---

### Create Webhook
**POST** `/v1/webhooks/settings`

Creates a new webhook subscription.

**Request Body:**
- `url` (string, required): The HTTPS endpoint URL to receive webhook payloads
- `events` (array of strings, required): List of event types to subscribe to (see Available Events)
- `profileId` (string, optional): Scope the webhook to a specific profile. If omitted, the webhook receives events for all profiles.
- `secret` (string, optional): Custom HMAC signing secret. If omitted, a secret is automatically generated.

**Request Example:**
```json
{
  "url": "https://example.com/webhooks/late",
  "events": [
    "post.published",
    "post.failed",
    "comment.created",
    "message.received"
  ],
  "profileId": "6507a1b2c3d4e5f6a7b8c9d0"
}
```

**Response (201):**
```json
{
  "message": "Webhook created successfully",
  "webhook": {
    "_id": "wh_a1b2c3d4e5f6",
    "url": "https://example.com/webhooks/late",
    "events": ["post.published", "post.failed", "comment.created", "message.received"],
    "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
    "status": "active",
    "secret": "whsec_abc123def456ghi789jkl012mno345",
    "createdAt": "2024-11-01T10:00:00Z"
  }
}
```

**Note:** The `secret` is only returned in full upon creation. Store it securely as it cannot be retrieved again.

**Error Responses:**
- **400:** Invalid URL, empty events array, or invalid event type
- **401:** Unauthorized access
- **409:** A webhook with this URL already exists for the same profile
- **422:** URL is not HTTPS or is unreachable

---

### Delete Webhook
**DELETE** `/v1/webhooks/settings/{webhookId}`

Deletes a webhook subscription.

**Path Parameters:**
- `webhookId` (string, required): The webhook ID

**Response (200):**
```json
{
  "message": "Webhook deleted successfully",
  "webhookId": "wh_a1b2c3d4e5f6"
}
```

**Error Responses:**
- **401:** Unauthorized access
- **404:** Webhook not found

---

### Update Webhook
**PUT** `/v1/webhooks/settings/{webhookId}`

Updates an existing webhook subscription.

**Path Parameters:**
- `webhookId` (string, required): The webhook ID

**Request Body:**
- `url` (string, optional): Updated endpoint URL (must be HTTPS)
- `events` (array of strings, optional): Updated list of event types
- `status` (string, optional): Enable or disable the webhook - `active`, `inactive`
- `secret` (string, optional): New HMAC signing secret

**Request Example:**
```json
{
  "events": [
    "post.published",
    "post.failed",
    "post.scheduled",
    "comment.created",
    "review.created"
  ],
  "status": "active"
}
```

**Response (200):**
```json
{
  "message": "Webhook updated successfully",
  "webhook": {
    "_id": "wh_a1b2c3d4e5f6",
    "url": "https://example.com/webhooks/late",
    "events": ["post.published", "post.failed", "post.scheduled", "comment.created", "review.created"],
    "status": "active",
    "updatedAt": "2024-11-15T10:00:00Z"
  }
}
```

**Error Responses:**
- **400:** Invalid event type or URL
- **401:** Unauthorized access
- **404:** Webhook not found
- **422:** URL is not HTTPS

---

### Test Webhook
**POST** `/v1/webhooks/test`

Sends a test payload to a webhook endpoint to verify connectivity and payload processing.

**Request Body:**
- `webhookId` (string, optional): Test an existing webhook by ID
- `url` (string, optional): Test a URL directly (must be HTTPS). Either `webhookId` or `url` is required.
- `event` (string, optional): The event type to simulate (default: `test.ping`)

**Request Example:**
```json
{
  "webhookId": "wh_a1b2c3d4e5f6",
  "event": "post.published"
}
```

**Response (200):**
```json
{
  "message": "Test webhook sent successfully",
  "result": {
    "url": "https://example.com/webhooks/late",
    "statusCode": 200,
    "responseTime": 245,
    "success": true
  }
}
```

**Response when endpoint fails (200):**
```json
{
  "message": "Test webhook sent but endpoint returned an error",
  "result": {
    "url": "https://example.com/webhooks/late",
    "statusCode": 500,
    "responseTime": 1200,
    "success": false,
    "error": "Endpoint returned HTTP 500"
  }
}
```

**Error Responses:**
- **400:** Neither `webhookId` nor `url` provided
- **401:** Unauthorized access
- **404:** Webhook not found
- **422:** URL is not HTTPS or is invalid

---

### Get Webhook Logs
**GET** `/v1/webhooks/logs`

Returns delivery logs for webhook events, including success/failure status and response details.

**Query Parameters:**
- `webhookId` (string, optional): Filter logs by webhook ID
- `event` (string, optional): Filter by event type
- `status` (string, optional): Filter by delivery status - `success`, `failed`, `pending`
- `startDate` (string, optional): Filter logs after this date (ISO 8601)
- `endDate` (string, optional): Filter logs before this date (ISO 8601)
- `page` (integer, optional): Page number (default: `1`)
- `limit` (integer, optional): Results per page (default: `50`, max: `100`)

**Response (200):**
```json
{
  "logs": [
    {
      "_id": "whl_001",
      "webhookId": "wh_a1b2c3d4e5f6",
      "event": "post.published",
      "url": "https://example.com/webhooks/late",
      "status": "success",
      "statusCode": 200,
      "responseTime": 180,
      "payload": {
        "event": "post.published",
        "timestamp": "2024-11-15T14:00:05Z",
        "data": {
          "postId": "65a0b1c2d3e4f5a6b7c8d9e0",
          "platform": "twitter",
          "status": "published"
        }
      },
      "attempt": 1,
      "createdAt": "2024-11-15T14:00:05Z"
    },
    {
      "_id": "whl_002",
      "webhookId": "wh_a1b2c3d4e5f6",
      "event": "comment.created",
      "url": "https://example.com/webhooks/late",
      "status": "failed",
      "statusCode": 503,
      "responseTime": 5000,
      "error": "Endpoint returned HTTP 503 Service Unavailable",
      "attempt": 3,
      "nextRetryAt": null,
      "createdAt": "2024-11-15T14:30:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 50,
    "total": 245,
    "totalPages": 5
  }
}
```

---

## Available Events

| Event | Description |
|-------|-------------|
| `test.ping` | Test event for verifying webhook connectivity |
| `post.created` | A new post has been created |
| `post.scheduled` | A post has been scheduled |
| `post.published` | A post has been successfully published |
| `post.failed` | A post failed to publish |
| `post.updated` | A post has been updated |
| `post.deleted` | A post has been deleted |
| `comment.created` | A new comment was received on a post |
| `comment.replied` | A reply was sent to a comment |
| `comment.deleted` | A comment was deleted |
| `message.received` | A new direct message was received |
| `message.sent` | A direct message was sent |
| `review.created` | A new review was received |
| `review.replied` | A reply was posted to a review |
| `account.connected` | A new social account was connected |
| `account.disconnected` | A social account was disconnected |
| `account.health.changed` | An account's health status changed (e.g., token expired) |
| `profile.created` | A new profile was created |
| `profile.deleted` | A profile was deleted |

---

## HMAC Signature Verification

All webhook payloads include an HMAC-SHA256 signature in the `X-Late-Signature` header. Use this signature to verify that the payload was sent by Late and has not been tampered with.

### Signature Header
```
X-Late-Signature: sha256=a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2
```

### Verification Process

1. Extract the signature from the `X-Late-Signature` header
2. Compute the HMAC-SHA256 hash of the raw request body using your webhook secret
3. Compare the computed hash with the signature from the header

### Verification Examples

**Node.js:**
```javascript
const crypto = require('crypto');

function verifyWebhookSignature(payload, signature, secret) {
  const expectedSignature = 'sha256=' +
    crypto.createHmac('sha256', secret)
      .update(payload, 'utf8')
      .digest('hex');

  return crypto.timingSafeEqual(
    Buffer.from(signature),
    Buffer.from(expectedSignature)
  );
}
```

**Python:**
```python
import hmac
import hashlib

def verify_webhook_signature(payload: bytes, signature: str, secret: str) -> bool:
    expected = 'sha256=' + hmac.new(
        secret.encode('utf-8'),
        payload,
        hashlib.sha256
    ).hexdigest()

    return hmac.compare_digest(signature, expected)
```

### Webhook Payload Format

All webhook payloads follow this structure:

```json
{
  "event": "post.published",
  "timestamp": "2024-11-15T14:00:05Z",
  "webhookId": "wh_a1b2c3d4e5f6",
  "data": {
    "postId": "65a0b1c2d3e4f5a6b7c8d9e0",
    "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
    "platform": "twitter",
    "status": "published",
    "platformPostUrl": "https://twitter.com/exampleuser/status/1234567890"
  }
}
```

### Retry Policy

Failed webhook deliveries are retried with exponential backoff:
- **Attempt 1:** Immediate
- **Attempt 2:** After 1 minute
- **Attempt 3:** After 5 minutes
- **Attempt 4:** After 30 minutes
- **Attempt 5:** After 2 hours

After 5 failed consecutive attempts, the webhook is marked as `failed` and no further retries are attempted. The webhook must be manually re-enabled by updating its status to `active`.

---

## Error Responses Summary

| Status Code | Description |
|-------------|-------------|
| **400** | Invalid request parameters or body |
| **401** | Unauthorized - invalid or expired API key |
| **404** | Webhook not found |
| **409** | Duplicate webhook URL for the same profile |
| **422** | Invalid URL or unreachable endpoint |
| **429** | Rate limit exceeded |
| **500** | Internal server error |

---
