# Media Downloads - Late API Documentation

## Overview
The Media Downloads tool allows you to download videos from seven major social media platforms. Each platform has its own dedicated endpoint with platform-specific parameters.

**Plan Requirement**: Build, Accelerate, or Unlimited plan required.

### Rate Limits
- **Build**: 50 requests/day
- **Accelerate**: 500 requests/day
- **Unlimited**: Unlimited requests

---

## Standard Response Format

All download endpoints return a consistent response structure:

```json
{
  "success": true,
  "title": "string",
  "downloadUrl": "string",
  "thumbnail": "string",
  "duration": 0,
  "formats": [
    {
      "formatId": "string",
      "quality": "string",
      "mimeType": "string",
      "fileSize": 0
    }
  ]
}
```

### Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `success` | boolean | Whether the request was processed successfully |
| `title` | string | Title of the media content |
| `downloadUrl` | string | Direct URL to download the media file |
| `thumbnail` | string | URL of the video thumbnail image |
| `duration` | number | Duration of the video in seconds |
| `formats` | array | Available download formats and quality options |
| `formats[].formatId` | string | Unique identifier for the format, used with `formatId` parameter |
| `formats[].quality` | string | Human-readable quality label (e.g., "720p", "1080p") |
| `formats[].mimeType` | string | MIME type of the file (e.g., "video/mp4") |
| `formats[].fileSize` | number | Approximate file size in bytes |

---

## 1. YouTube Download

**GET** `/v1/tools/youtube/download`

Download videos from YouTube with format and quality selection.

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `url` | string | Yes | YouTube video URL or video ID |
| `action` | string | No | Set to `"formats"` to retrieve available formats without downloading. Omit to download directly. |
| `format` | string | No | Desired format: `"mp4"`, `"webm"`, `"mp3"`, `"m4a"`. Default: `"mp4"` |
| `quality` | string | No | Desired quality: `"lowest"`, `"360"`, `"480"`, `"720"`, `"1080"`, `"1440"`, `"2160"`, `"highest"`. Default: `"highest"` |
| `formatId` | string | No | Specific format ID from the formats list. Overrides `format` and `quality` when provided. |

### Example: List Available Formats

```bash
curl -X GET "https://getlate.dev/api/v1/tools/youtube/download?url=https://youtube.com/watch?v=dQw4w9WgXcQ&action=formats" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### Example: Download Video

```bash
curl -X GET "https://getlate.dev/api/v1/tools/youtube/download?url=https://youtube.com/watch?v=dQw4w9WgXcQ&format=mp4&quality=1080" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### Example: Download by Format ID

```bash
curl -X GET "https://getlate.dev/api/v1/tools/youtube/download?url=https://youtube.com/watch?v=dQw4w9WgXcQ&formatId=137" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

---

## 2. Instagram Download

**GET** `/v1/tools/instagram/download`

Download videos and reels from Instagram.

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `url` | string | Yes | Instagram post or reel URL |

### Example

```bash
curl -X GET "https://getlate.dev/api/v1/tools/instagram/download?url=https://www.instagram.com/reel/ABC123/" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

---

## 3. TikTok Download

**GET** `/v1/tools/tiktok/download`

Download TikTok videos with optional format selection.

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `url` | string | Yes | TikTok video URL |
| `action` | string | No | Set to `"formats"` to list available formats without downloading |
| `formatId` | string | No | Specific format ID from the formats list |

### Example: List Available Formats

```bash
curl -X GET "https://getlate.dev/api/v1/tools/tiktok/download?url=https://www.tiktok.com/@user/video/1234567890&action=formats" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### Example: Download Video

```bash
curl -X GET "https://getlate.dev/api/v1/tools/tiktok/download?url=https://www.tiktok.com/@user/video/1234567890" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### Example: Download by Format ID

```bash
curl -X GET "https://getlate.dev/api/v1/tools/tiktok/download?url=https://www.tiktok.com/@user/video/1234567890&formatId=hd_watermark_removed" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

---

## 4. Twitter/X Download

**GET** `/v1/tools/twitter/download`

Download videos from Twitter/X posts.

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `url` | string | Yes | Twitter/X post URL |
| `action` | string | No | Set to `"formats"` to list available formats without downloading |
| `formatId` | string | No | Specific format ID from the formats list |

### Example: List Available Formats

```bash
curl -X GET "https://getlate.dev/api/v1/tools/twitter/download?url=https://x.com/user/status/1234567890&action=formats" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### Example: Download Video

```bash
curl -X GET "https://getlate.dev/api/v1/tools/twitter/download?url=https://x.com/user/status/1234567890" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### Example: Download by Format ID

```bash
curl -X GET "https://getlate.dev/api/v1/tools/twitter/download?url=https://x.com/user/status/1234567890&formatId=720x1280" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

---

## 5. Facebook Download

**GET** `/v1/tools/facebook/download`

Download videos from Facebook posts and reels.

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `url` | string | Yes | Facebook video or reel URL |

### Example

```bash
curl -X GET "https://getlate.dev/api/v1/tools/facebook/download?url=https://www.facebook.com/watch/?v=1234567890" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

---

## 6. LinkedIn Download

**GET** `/v1/tools/linkedin/download`

Download videos from LinkedIn posts.

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `url` | string | Yes | LinkedIn post URL containing video |

### Example

```bash
curl -X GET "https://getlate.dev/api/v1/tools/linkedin/download?url=https://www.linkedin.com/posts/user_activity-1234567890" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

---

## 7. Bluesky Download

**GET** `/v1/tools/bluesky/download`

Download videos from Bluesky posts.

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `url` | string | Yes | Bluesky post URL |

### Example

```bash
curl -X GET "https://getlate.dev/api/v1/tools/bluesky/download?url=https://bsky.app/profile/user.bsky.social/post/abc123" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

---

## Error Responses

| Status | Description |
|--------|------------|
| 400 | Bad Request — Missing or malformed `url` parameter |
| 401 | Unauthorized — Missing or invalid API key |
| 403 | Forbidden — Your plan does not include Tools API access |
| 404 | Not Found — Video not found or URL is invalid |
| 429 | Rate Limit Exceeded — Daily request limit reached |
| 500 | Internal Server Error — Platform-side issue; retry later |

### Error Response Body

```json
{
  "success": false,
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Daily request limit reached. Resets at 2025-01-15T00:00:00Z."
  }
}
```
