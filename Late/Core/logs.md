# Logs - API Reference

## Overview
The Logs API provides endpoints for retrieving activity logs related to posts, connections, and other operations within the Late platform. Logs provide an audit trail of actions, errors, and status changes.

---

## Endpoints

### List All Logs (Deprecated)
**GET** `/v1/logs`

> **Deprecated:** Use the specific log endpoints instead.

### Get Log by ID
**GET** `/v1/logs/{logId}`

### Get Post Logs
**GET** `/v1/posts/logs`

### Get Connection Logs
**GET** `/v1/connections/logs`

### Get Logs for Specific Post
**GET** `/v1/posts/{postId}/logs`

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
- Logs are generated in real-time and are available immediately after an action occurs.
