# Comments - API Reference

## Overview
The Comments API provides endpoints for managing comments on social media posts across connected accounts. You can list posts with comments, retrieve comment threads, reply to comments, delete, hide/unhide, like/unlike comments, and send private replies.

**Platform Support:**
| Action | Facebook | Instagram | LinkedIn | YouTube | TikTok | Twitter/X |
|--------|----------|-----------|----------|---------|--------|-----------|
| List Comments | Yes | Yes | Yes | Yes | Yes | Yes |
| Reply | Yes | Yes | Yes | Yes | No | Yes |
| Delete | Yes | Yes | Yes | Yes | No | Yes |
| Hide | Yes | Yes | No | Yes | No | No |
| Unhide | Yes | Yes | No | Yes | No | No |
| Like | Yes | Yes | Yes | Yes | No | Yes |
| Unlike | Yes | Yes | Yes | Yes | No | Yes |
| Private Reply | Yes | Yes | No | No | No | No |

---

## Endpoints

### List Posts with Comments
**GET** `/v1/inbox/comments`

### Get Comments for Post
**GET** `/v1/inbox/comments/{postId}`

### Reply to Comment
**POST** `/v1/inbox/comments/{postId}/{commentId}/reply`

### Delete Comment
**DELETE** `/v1/inbox/comments/{postId}/{commentId}`

### Hide Comment
**PUT** `/v1/inbox/comments/{postId}/{commentId}/hide`

### Unhide Comment
**PUT** `/v1/inbox/comments/{postId}/{commentId}/unhide`

### Like Comment
**POST** `/v1/inbox/comments/{postId}/{commentId}/like`

### Unlike Comment
**POST** `/v1/inbox/comments/{postId}/{commentId}/unlike`

### Send Private Reply
**POST** `/v1/inbox/comments/{postId}/{commentId}/private-reply`

---

## Error Responses Summary

| Status Code | Description |
|-------------|-------------|
| **400** | Invalid request parameters or body |
| **401** | Unauthorized - invalid or expired API key |
| **403** | Insufficient permissions or action not allowed |
| **404** | Post, comment, or account not found |
| **409** | Conflict (e.g., already liked/unliked) |
| **422** | Platform does not support the requested action |
| **429** | Rate limit exceeded |
| **500** | Internal server error |
