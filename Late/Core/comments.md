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

Returns a paginated list of posts that have received comments, with comment count and latest comment preview.

**Query Parameters:**
- `profileId` (string, optional): Filter by profile ID
- `accountId` (string, optional): Filter by specific account ID
- `platform` (string, optional): Filter by platform
- `startDate` (string, optional): Filter posts published after this date (ISO 8601)
- `endDate` (string, optional): Filter posts published before this date (ISO 8601)
- `hasUnread` (boolean, optional): When `true`, returns only posts with unread comments
- `page` (integer, optional): Page number (default: `1`)
- `limit` (integer, optional): Results per page (default: `25`, max: `100`)

**Response (200):**
```json
{
  "posts": [
    {
      "postId": "65a0b1c2d3e4f5a6b7c8d9e0",
      "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
      "platform": "instagram",
      "platformPostId": "17890012345678901",
      "platformPostUrl": "https://www.instagram.com/p/ABC123/",
      "text": "Check out our latest collection! #fashion",
      "publishedAt": "2024-11-15T10:00:00Z",
      "commentCount": 24,
      "unreadCommentCount": 3,
      "latestComment": {
        "_id": "cmt_xyz789",
        "text": "Love this! Where can I buy?",
        "author": {
          "username": "fashionfan",
          "displayName": "Fashion Fan",
          "avatarUrl": "https://instagram.com/.../avatar.jpg"
        },
        "createdAt": "2024-11-15T14:30:00Z"
      }
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 25,
    "total": 12,
    "totalPages": 1
  }
}
```

---

### Get Comments for Post
**GET** `/v1/inbox/comments/{postId}`

Returns all comments for a specific post, including threaded replies.

**Path Parameters:**
- `postId` (string, required): The Late post ID

**Query Parameters:**
- `page` (integer, optional): Page number (default: `1`)
- `limit` (integer, optional): Comments per page (default: `50`, max: `100`)
- `sort` (string, optional): Sort order - `oldest`, `newest`, `top` (default: `newest`)
- `includeReplies` (boolean, optional): Include reply threads (default: `true`)

**Response (200):**
```json
{
  "postId": "65a0b1c2d3e4f5a6b7c8d9e0",
  "comments": [
    {
      "_id": "cmt_abc123",
      "platformCommentId": "17890012345678902",
      "text": "This is amazing! Great work!",
      "author": {
        "platformUserId": "111222333",
        "username": "happycustomer",
        "displayName": "Happy Customer",
        "avatarUrl": "https://instagram.com/.../avatar.jpg"
      },
      "isHidden": false,
      "isLiked": false,
      "likeCount": 5,
      "replyCount": 2,
      "createdAt": "2024-11-15T11:00:00Z",
      "replies": [
        {
          "_id": "cmt_def456",
          "platformCommentId": "17890012345678903",
          "text": "Thank you so much! We appreciate your support.",
          "author": {
            "platformUserId": "444555666",
            "username": "mybrand",
            "displayName": "My Brand",
            "avatarUrl": "https://instagram.com/.../avatar.jpg",
            "isOwner": true
          },
          "isHidden": false,
          "isLiked": false,
          "likeCount": 1,
          "createdAt": "2024-11-15T11:30:00Z"
        }
      ]
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 50,
    "total": 24,
    "totalPages": 1
  }
}
```

**Error Responses:**
- **401:** Unauthorized access
- **404:** Post not found

---

### Reply to Comment
**POST** `/v1/inbox/comments/{postId}/{commentId}/reply`

Posts a reply to a specific comment.

**Path Parameters:**
- `postId` (string, required): The Late post ID
- `commentId` (string, required): The comment ID to reply to

**Request Body:**
- `text` (string, required): Reply text content
- `media` (object, optional): Media attachment (platform support varies)
  - `type` (string): `image`, `gif`
  - `url` (string): Public URL of the media file

**Response (201):**
```json
{
  "message": "Reply posted successfully",
  "comment": {
    "_id": "cmt_ghi789",
    "platformCommentId": "17890012345678904",
    "text": "Thanks for your comment! Check out our website for more details.",
    "author": {
      "platformUserId": "444555666",
      "username": "mybrand",
      "displayName": "My Brand",
      "isOwner": true
    },
    "parentCommentId": "cmt_abc123",
    "createdAt": "2024-11-15T12:00:00Z"
  }
}
```

**Error Responses:**
- **400:** Invalid request body or empty text
- **401:** Unauthorized access
- **404:** Post or comment not found
- **422:** Platform does not support replies or reply text exceeds character limit

---

### Delete Comment
**DELETE** `/v1/inbox/comments/{postId}/{commentId}`

Deletes a comment. Only comments authored by the connected account can be deleted.

**Path Parameters:**
- `postId` (string, required): The Late post ID
- `commentId` (string, required): The comment ID to delete

**Response (200):**
```json
{
  "message": "Comment deleted successfully",
  "commentId": "cmt_ghi789"
}
```

**Error Responses:**
- **401:** Unauthorized access
- **403:** Cannot delete comments authored by other users
- **404:** Post or comment not found
- **422:** Platform does not support comment deletion

---

### Hide Comment
**PUT** `/v1/inbox/comments/{postId}/{commentId}/hide`

Hides a comment from public view. The comment is still visible to the author and the page/account admin.

**Path Parameters:**
- `postId` (string, required): The Late post ID
- `commentId` (string, required): The comment ID to hide

**Response (200):**
```json
{
  "message": "Comment hidden successfully",
  "comment": {
    "_id": "cmt_abc123",
    "isHidden": true,
    "hiddenAt": "2024-11-15T12:30:00Z"
  }
}
```

**Error Responses:**
- **401:** Unauthorized access
- **404:** Post or comment not found
- **422:** Platform does not support hiding comments

---

### Unhide Comment
**PUT** `/v1/inbox/comments/{postId}/{commentId}/unhide`

Restores a previously hidden comment to public view.

**Path Parameters:**
- `postId` (string, required): The Late post ID
- `commentId` (string, required): The comment ID to unhide

**Response (200):**
```json
{
  "message": "Comment unhidden successfully",
  "comment": {
    "_id": "cmt_abc123",
    "isHidden": false
  }
}
```

**Error Responses:**
- **401:** Unauthorized access
- **404:** Post or comment not found
- **422:** Platform does not support hiding/unhiding comments

---

### Like Comment
**POST** `/v1/inbox/comments/{postId}/{commentId}/like`

Likes a comment on behalf of the connected account.

**Path Parameters:**
- `postId` (string, required): The Late post ID
- `commentId` (string, required): The comment ID to like

**Response (200):**
```json
{
  "message": "Comment liked successfully",
  "comment": {
    "_id": "cmt_abc123",
    "isLiked": true,
    "likeCount": 6
  }
}
```

**Error Responses:**
- **401:** Unauthorized access
- **404:** Post or comment not found
- **409:** Comment already liked
- **422:** Platform does not support liking comments

---

### Unlike Comment
**POST** `/v1/inbox/comments/{postId}/{commentId}/unlike`

Removes a like from a comment.

**Path Parameters:**
- `postId` (string, required): The Late post ID
- `commentId` (string, required): The comment ID to unlike

**Response (200):**
```json
{
  "message": "Comment unliked successfully",
  "comment": {
    "_id": "cmt_abc123",
    "isLiked": false,
    "likeCount": 5
  }
}
```

**Error Responses:**
- **401:** Unauthorized access
- **404:** Post or comment not found
- **409:** Comment is not liked
- **422:** Platform does not support unliking comments

---

### Send Private Reply
**POST** `/v1/inbox/comments/{postId}/{commentId}/private-reply`

Sends a private/direct message to the author of a comment. Currently supported on Facebook and Instagram only.

**Path Parameters:**
- `postId` (string, required): The Late post ID
- `commentId` (string, required): The comment ID whose author will receive the private message

**Request Body:**
- `text` (string, required): Private message text content

**Response (201):**
```json
{
  "message": "Private reply sent successfully",
  "privateMessage": {
    "_id": "msg_prv001",
    "conversationId": "conv_new123",
    "text": "Hi! Thanks for your comment. I wanted to follow up privately about your question.",
    "recipient": {
      "platformUserId": "111222333",
      "username": "happycustomer"
    },
    "sentAt": "2024-11-15T13:00:00Z"
  }
}
```

**Error Responses:**
- **400:** Invalid request body or empty text
- **401:** Unauthorized access
- **403:** Cannot send private reply to this user (privacy settings)
- **404:** Post or comment not found
- **422:** Platform does not support private replies
- **429:** Rate limit exceeded (platform-specific limits apply)

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
