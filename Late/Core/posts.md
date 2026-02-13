# Posts - API Reference

## Overview
The Posts API enables creating, scheduling, managing, and publishing social media posts across all connected accounts. Posts can include text, media, platform-specific settings, and scheduling options.

---

## Endpoints

### List Posts
**GET** `/v1/posts`

Returns posts for the authenticated user with filtering and pagination support.

**Query Parameters:**
- `profileId` (string, optional): Filter by profile ID
- `accountId` (string, optional): Filter by specific account ID
- `platform` (string, optional): Filter by platform (e.g., `twitter`, `facebook`, `instagram`)
- `status` (string, optional): Filter by status - `draft`, `scheduled`, `publishing`, `published`, `failed`, `cancelled`
- `startDate` (string, optional): Filter posts scheduled after this date (ISO 8601)
- `endDate` (string, optional): Filter posts scheduled before this date (ISO 8601)
- `search` (string, optional): Search post text content
- `page` (integer, optional): Page number (default: `1`)
- `limit` (integer, optional): Results per page (default: `25`, max: `100`)
- `sort` (string, optional): Sort field - `scheduledAt`, `createdAt`, `updatedAt` (default: `scheduledAt`)
- `order` (string, optional): Sort order - `asc`, `desc` (default: `desc`)

**Response (200):**
```json
{
  "posts": [
    {
      "_id": "65a0b1c2d3e4f5a6b7c8d9e0",
      "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
      "accountIds": ["64f0a1b2c3d4e5f6a7b8c9d0"],
      "text": "Check out our latest blog post! #marketing",
      "media": [
        {
          "type": "image",
          "url": "https://cdn.late.com/media/image1.jpg",
          "altText": "Blog post banner"
        }
      ],
      "status": "scheduled",
      "scheduledAt": "2024-11-20T14:00:00Z",
      "platformSpecificData": {},
      "createdAt": "2024-11-15T10:00:00Z",
      "updatedAt": "2024-11-15T10:00:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 25,
    "total": 48,
    "totalPages": 2
  }
}
```

---

### Create Post
**POST** `/v1/posts`

Creates a new post. The post can be published immediately, scheduled for a future time, or saved as a draft.

**Request Body:**
- `accountIds` (array of strings, required): Account IDs to publish to
- `text` (string, required for most platforms): Post text content
- `media` (array of objects, optional): Media attachments
  - `type` (string): `image`, `video`, `gif`, `document`
  - `url` (string): Public URL of the media file
  - `altText` (string, optional): Accessibility text for images
  - `thumbnailUrl` (string, optional): Custom thumbnail for videos
- `scheduledAt` (string, optional): ISO 8601 datetime for scheduling. If omitted, the post is published immediately. Set to `null` for draft status.
- `status` (string, optional): `draft` or `scheduled` (default: `scheduled` if `scheduledAt` is provided)
- `profileId` (string, optional): Profile ID association
- `platformSpecificData` (object, optional): Platform-specific settings (see Platform Settings documentation)
- `autoThread` (boolean, optional): Automatically split long text into threads (Twitter/X)
- `firstComment` (string, optional): Text for an automatic first comment after publishing
- `labels` (array of strings, optional): Tags for organizing posts
- `externalId` (string, optional): External reference ID for integrations

**Response (201):**
```json
{
  "message": "Post created successfully",
  "post": {
    "_id": "65a0b1c2d3e4f5a6b7c8d9e0",
    "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
    "accountIds": ["64f0a1b2c3d4e5f6a7b8c9d0"],
    "text": "Check out our latest blog post! #marketing",
    "media": [],
    "status": "scheduled",
    "scheduledAt": "2024-11-20T14:00:00Z",
    "platformSpecificData": {},
    "createdAt": "2024-11-15T10:00:00Z"
  }
}
```

**Error Responses:**
- **400:** Invalid request body (e.g., missing text, invalid media URL, past scheduled date)
- **401:** Unauthorized access
- **403:** Insufficient permissions or account not accessible
- **404:** Account ID not found
- **422:** Post content violates platform constraints (e.g., character limit exceeded)

---

### Get Single Post
**GET** `/v1/posts/{postId}`

Retrieves detailed information about a specific post, including publishing results per account.

**Path Parameters:**
- `postId` (string, required): The post ID

**Response (200):**
```json
{
  "post": {
    "_id": "65a0b1c2d3e4f5a6b7c8d9e0",
    "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
    "accountIds": ["64f0a1b2c3d4e5f6a7b8c9d0"],
    "text": "Check out our latest blog post! #marketing",
    "media": [
      {
        "type": "image",
        "url": "https://cdn.late.com/media/image1.jpg",
        "altText": "Blog post banner"
      }
    ],
    "status": "published",
    "scheduledAt": "2024-11-20T14:00:00Z",
    "publishedAt": "2024-11-20T14:00:05Z",
    "platformSpecificData": {},
    "publishingResults": [
      {
        "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
        "platform": "twitter",
        "status": "published",
        "platformPostId": "1234567890",
        "platformPostUrl": "https://twitter.com/exampleuser/status/1234567890",
        "publishedAt": "2024-11-20T14:00:05Z"
      }
    ],
    "createdAt": "2024-11-15T10:00:00Z",
    "updatedAt": "2024-11-20T14:00:05Z"
  }
}
```

**Error Responses:**
- **401:** Unauthorized access
- **404:** Post not found

---

### Delete Post
**DELETE** `/v1/posts/{postId}`

Deletes a post. If the post is scheduled, it will be cancelled. If already published, it will only be removed from the Late platform (not from social media platforms).

**Path Parameters:**
- `postId` (string, required): The post ID

**Query Parameters:**
- `deleteFromPlatform` (boolean, optional): If `true`, also attempts to delete the post from the social media platform (only for published posts)

**Response (200):**
```json
{
  "message": "Post deleted successfully",
  "postId": "65a0b1c2d3e4f5a6b7c8d9e0"
}
```

**Error Responses:**
- **401:** Unauthorized access
- **403:** Insufficient permissions
- **404:** Post not found

---

### Update Post
**PUT** `/v1/posts/{postId}`

Updates a post. Only draft and scheduled posts can be updated. Published posts cannot be modified.

**Path Parameters:**
- `postId` (string, required): The post ID

**Request Body:**
- `text` (string, optional): Updated post text
- `media` (array of objects, optional): Updated media attachments
  - `type` (string): `image`, `video`, `gif`, `document`
  - `url` (string): Public URL of the media file
  - `altText` (string, optional): Accessibility text
- `scheduledAt` (string, optional): Updated schedule time (ISO 8601)
- `accountIds` (array of strings, optional): Updated target account IDs
- `platformSpecificData` (object, optional): Updated platform-specific settings
- `status` (string, optional): Change status (e.g., `draft` to `scheduled`)
- `firstComment` (string, optional): Updated first comment text
- `labels` (array of strings, optional): Updated tags

**Response (200):**
```json
{
  "message": "Post updated successfully",
  "post": {
    "_id": "65a0b1c2d3e4f5a6b7c8d9e0",
    "text": "Updated post content! #marketing #social",
    "status": "scheduled",
    "scheduledAt": "2024-11-21T16:00:00Z",
    "updatedAt": "2024-11-15T12:00:00Z"
  }
}
```

**Error Responses:**
- **400:** Invalid request body or attempting to update a published post
- **401:** Unauthorized access
- **403:** Insufficient permissions
- **404:** Post not found
- **422:** Updated content violates platform constraints

---

### Bulk Upload Posts
**POST** `/v1/posts/bulk-upload`

Creates multiple posts in a single request. Useful for importing content calendars or batch scheduling.

**Request Body:**
- `posts` (array of objects, required): Array of post objects, each following the same schema as the Create Post endpoint
  - `accountIds` (array of strings, required)
  - `text` (string, required for most platforms)
  - `media` (array of objects, optional)
  - `scheduledAt` (string, optional)
  - `platformSpecificData` (object, optional)
  - `labels` (array of strings, optional)
  - `externalId` (string, optional)

**Constraints:**
- Maximum 50 posts per request
- All posts must belong to the same profile

**Response (201):**
```json
{
  "message": "Bulk upload completed",
  "results": {
    "total": 10,
    "created": 9,
    "failed": 1,
    "posts": [
      {
        "_id": "65a0b1c2d3e4f5a6b7c8d9e0",
        "status": "scheduled",
        "scheduledAt": "2024-11-20T14:00:00Z"
      }
    ],
    "errors": [
      {
        "index": 4,
        "error": "Scheduled time is in the past",
        "post": { "text": "Late post..." }
      }
    ]
  }
}
```

**Error Responses:**
- **400:** Invalid request body, exceeds 50 post limit, or mixed profiles
- **401:** Unauthorized access
- **403:** Insufficient permissions
- **422:** One or more posts violate platform constraints (partial success possible)

---

### Retry Failed Post
**POST** `/v1/posts/{postId}/retry`

Retries publishing a post that previously failed. The post must have a `failed` status.

**Path Parameters:**
- `postId` (string, required): The post ID

**Request Body:**
- `accountIds` (array of strings, optional): Retry only for specific account IDs. If omitted, retries for all failed accounts.
- `scheduledAt` (string, optional): Reschedule the retry for a future time. If omitted, retries immediately.

**Response (200):**
```json
{
  "message": "Post retry initiated",
  "post": {
    "_id": "65a0b1c2d3e4f5a6b7c8d9e0",
    "status": "scheduled",
    "scheduledAt": "2024-11-15T12:30:00Z",
    "retryCount": 1
  }
}
```

**Error Responses:**
- **400:** Post is not in a failed state
- **401:** Unauthorized access
- **404:** Post not found
- **409:** Post is currently being published

---

## Error Responses Summary

| Status Code | Description |
|-------------|-------------|
| **400** | Invalid request parameters or body |
| **401** | Unauthorized - invalid or expired API key |
| **403** | Insufficient permissions |
| **404** | Post or account not found |
| **409** | Conflict - post is currently being processed |
| **422** | Content violates platform constraints |
| **429** | Rate limit exceeded |
| **500** | Internal server error |

---
