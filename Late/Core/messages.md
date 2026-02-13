# Messages - API Reference

## Overview
The Messages API provides endpoints for managing inbox conversations and messages across connected social media accounts. This includes listing conversations, reading messages, sending replies, and managing conversation status.

---

## Endpoints

### List Conversations
**GET** `/v1/inbox/conversations`

Returns a paginated list of conversations across all connected accounts.

**Query Parameters:**
- `profileId` (string, optional): Filter by profile ID
- `accountId` (string, optional): Filter by specific account ID
- `platform` (string, optional): Filter by platform (e.g., `facebook`, `instagram`, `twitter`, `telegram`)
- `status` (string, optional): Filter by conversation status - `open`, `closed`, `archived` (default: all)
- `unreadOnly` (boolean, optional): When `true`, returns only conversations with unread messages (default: `false`)
- `search` (string, optional): Search conversations by participant name or message content
- `page` (integer, optional): Page number (default: `1`)
- `limit` (integer, optional): Results per page (default: `25`, max: `100`)
- `sort` (string, optional): Sort field - `lastMessageAt`, `createdAt` (default: `lastMessageAt`)
- `order` (string, optional): Sort order - `asc`, `desc` (default: `desc`)

---

### Get Conversation
**GET** `/v1/inbox/conversations/{conversationId}`

Retrieves detailed information about a specific conversation, including participant details and metadata.

### Update Conversation Status
**PUT** `/v1/inbox/conversations/{conversationId}`

Updates the status, labels, assignment, or notes of a conversation.

### Get Messages
**GET** `/v1/inbox/conversations/{conversationId}/messages`

Returns the message history for a specific conversation.

### Send Message
**POST** `/v1/inbox/conversations/{conversationId}/messages`

Sends a new message in a conversation.

### Edit Message
**PATCH** `/v1/inbox/conversations/{conversationId}/messages/{messageId}`

Edits a previously sent outgoing message.

---

## Error Responses Summary

| Status Code | Description |
|-------------|-------------|
| **400** | Invalid request parameters or body |
| **401** | Unauthorized - invalid or expired API key |
| **403** | Insufficient permissions or action not allowed |
| **404** | Conversation or message not found |
| **422** | Content violates platform constraints or unsupported action |
| **429** | Rate limit exceeded |
| **500** | Internal server error |

---

## Notes

- Message delivery status values: `sending`, `sent`, `delivered`, `read`, `failed`, `edited`
- Message direction values: `incoming` (from the customer) and `outgoing` (from your account)
- Not all platforms support all messaging features.
- Conversations are automatically created when a new incoming message is received.
