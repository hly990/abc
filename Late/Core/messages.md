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

**Response (200):**
```json
{
  "conversations": [
    {
      "_id": "conv_a1b2c3d4e5f6",
      "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
      "platform": "instagram",
      "status": "open",
      "participant": {
        "platformUserId": "987654321",
        "username": "customeruser",
        "displayName": "Customer Name",
        "avatarUrl": "https://instagram.com/.../avatar.jpg"
      },
      "lastMessage": {
        "text": "Hi, I have a question about your product.",
        "sentAt": "2024-11-15T10:30:00Z",
        "direction": "incoming"
      },
      "unreadCount": 2,
      "createdAt": "2024-11-10T08:00:00Z",
      "lastMessageAt": "2024-11-15T10:30:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 25,
    "total": 42,
    "totalPages": 2
  }
}
```

---

### Get Conversation
**GET** `/v1/inbox/conversations/{conversationId}`

Retrieves detailed information about a specific conversation, including participant details and metadata.

**Path Parameters:**
- `conversationId` (string, required): The conversation ID

**Response (200):**
```json
{
  "conversation": {
    "_id": "conv_a1b2c3d4e5f6",
    "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
    "platform": "instagram",
    "status": "open",
    "participant": {
      "platformUserId": "987654321",
      "username": "customeruser",
      "displayName": "Customer Name",
      "avatarUrl": "https://instagram.com/.../avatar.jpg",
      "followerCount": 1500,
      "isFollowing": true
    },
    "labels": ["support", "priority"],
    "assignedTo": null,
    "notes": "VIP customer - handle with care",
    "unreadCount": 2,
    "messageCount": 15,
    "createdAt": "2024-11-10T08:00:00Z",
    "lastMessageAt": "2024-11-15T10:30:00Z"
  }
}
```

**Error Responses:**
- **401:** Unauthorized access
- **404:** Conversation not found

---

### Update Conversation Status
**PUT** `/v1/inbox/conversations/{conversationId}`

Updates the status, labels, assignment, or notes of a conversation.

**Path Parameters:**
- `conversationId` (string, required): The conversation ID

**Request Body:**
- `status` (string, optional): New status - `open`, `closed`, `archived`
- `labels` (array of strings, optional): Updated labels/tags for the conversation
- `assignedTo` (string, optional): User ID to assign the conversation to, or `null` to unassign
- `notes` (string, optional): Internal notes about the conversation

**Response (200):**
```json
{
  "message": "Conversation updated successfully",
  "conversation": {
    "_id": "conv_a1b2c3d4e5f6",
    "status": "closed",
    "labels": ["support", "resolved"],
    "assignedTo": "user_abc123",
    "notes": "Issue resolved - refund processed",
    "updatedAt": "2024-11-15T11:00:00Z"
  }
}
```

**Error Responses:**
- **400:** Invalid status value or request body
- **401:** Unauthorized access
- **404:** Conversation not found

---

### Get Messages
**GET** `/v1/inbox/conversations/{conversationId}/messages`

Returns the message history for a specific conversation, ordered chronologically.

**Path Parameters:**
- `conversationId` (string, required): The conversation ID

**Query Parameters:**
- `page` (integer, optional): Page number (default: `1`)
- `limit` (integer, optional): Messages per page (default: `50`, max: `100`)
- `before` (string, optional): Return messages before this message ID (for cursor-based pagination)
- `after` (string, optional): Return messages after this message ID (for cursor-based pagination)

**Response (200):**
```json
{
  "messages": [
    {
      "_id": "msg_001",
      "conversationId": "conv_a1b2c3d4e5f6",
      "direction": "incoming",
      "text": "Hi, I have a question about your product.",
      "media": [],
      "sender": {
        "platformUserId": "987654321",
        "username": "customeruser",
        "displayName": "Customer Name"
      },
      "status": "delivered",
      "sentAt": "2024-11-15T10:30:00Z",
      "readAt": null
    },
    {
      "_id": "msg_002",
      "conversationId": "conv_a1b2c3d4e5f6",
      "direction": "outgoing",
      "text": "Hello! Happy to help. What would you like to know?",
      "media": [],
      "sender": {
        "userId": "user_abc123",
        "displayName": "Support Agent"
      },
      "status": "sent",
      "sentAt": "2024-11-15T10:35:00Z",
      "readAt": "2024-11-15T10:36:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 50,
    "total": 15,
    "totalPages": 1
  }
}
```

**Error Responses:**
- **401:** Unauthorized access
- **404:** Conversation not found

---

### Send Message
**POST** `/v1/inbox/conversations/{conversationId}/messages`

Sends a new message in a conversation. The message is delivered through the platform associated with the conversation.

**Path Parameters:**
- `conversationId` (string, required): The conversation ID

**Request Body:**
- `text` (string, required unless media is provided): Message text content
- `media` (array of objects, optional): Media attachments
  - `type` (string): `image`, `video`, `audio`, `file`
  - `url` (string): Public URL of the media file
- `quickReplies` (array of objects, optional): Quick reply buttons (platform support varies)
  - `title` (string): Button label text
  - `payload` (string): Payload sent when the button is tapped

**Response (201):**
```json
{
  "message": {
    "_id": "msg_003",
    "conversationId": "conv_a1b2c3d4e5f6",
    "direction": "outgoing",
    "text": "Here is the information you requested.",
    "media": [
      {
        "type": "image",
        "url": "https://cdn.late.com/media/product-info.jpg"
      }
    ],
    "sender": {
      "userId": "user_abc123",
      "displayName": "Support Agent"
    },
    "status": "sending",
    "sentAt": "2024-11-15T10:40:00Z"
  }
}
```

**Error Responses:**
- **400:** Invalid request body (missing text and media, invalid media URL)
- **401:** Unauthorized access
- **403:** Cannot send messages in a closed or archived conversation
- **404:** Conversation not found
- **422:** Message content exceeds platform character limit
- **429:** Rate limit exceeded

---

### Edit Message
**PATCH** `/v1/inbox/conversations/{conversationId}/messages/{messageId}`

Edits a previously sent outgoing message. Support varies by platform.

**Path Parameters:**
- `conversationId` (string, required): The conversation ID
- `messageId` (string, required): The message ID to edit

**Request Body:**
- `text` (string, required): Updated message text

**Response (200):**
```json
{
  "message": {
    "_id": "msg_002",
    "conversationId": "conv_a1b2c3d4e5f6",
    "direction": "outgoing",
    "text": "Hello! Happy to help. What specific product are you interested in?",
    "status": "edited",
    "sentAt": "2024-11-15T10:35:00Z",
    "editedAt": "2024-11-15T10:42:00Z"
  }
}
```

**Error Responses:**
- **400:** Invalid request body or message text is empty
- **401:** Unauthorized access
- **403:** Cannot edit incoming messages or messages older than the platform's edit window
- **404:** Conversation or message not found
- **422:** Platform does not support message editing

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
- Not all platforms support all messaging features (e.g., quick replies, message editing, media types).
- Conversations are automatically created when a new incoming message is received from a platform user.
- The inbox must be enabled for the connected account to receive and send messages.
