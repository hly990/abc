# Twitter/X API Documentation

## Overview
The Late API enables posting to Twitter/X with support for tweets, threads, images, videos, and GIFs. The documentation provides quick-start examples and detailed specifications for content publishing.

## Quick Start

### Basic Tweet Example
Post a simple tweet using any of these languages:

**cURL:**
```bash
curl -X POST https://getlate.dev/api/v1/posts \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Hello from Late API! 🚀",
    "platforms": [
      {"platform": "twitter", "accountId": "YOUR_ACCOUNT_ID"}
    ],
    "publishNow": true
  }'
```

## Media Requirements

### Image Specifications
- **Maximum images per post:** 4
- **Supported formats:** JPEG, PNG, WebP, GIF
- **File size limits:** 5 MB for images, 15 MB for GIFs
- **Dimension range:** 4×4 px minimum to 8192×8192 px maximum
- **Recommended size:** 1200×675 px (16:9 aspect ratio)

### Aspect Ratio Guidelines

| Type | Ratio | Dimensions |
|------|-------|-----------|
| Landscape | 16:9 | 1200×675 px |
| Square | 1:1 | 1200×1200 px |
| Portrait | 4:5 | 1080×1350 px |

### Image Upload Example
```json
{
  "content": "Check out this photo! 📸",
  "mediaItems": [
    {"type": "image", "url": "https://example.com/photo.jpg"}
  ],
  "platforms": [
    {"platform": "twitter", "accountId": "YOUR_ACCOUNT_ID"}
  ],
  "publishNow": true
}
```

## GIF Support

Twitter/X provides native GIF functionality:
- **Maximum file size:** 15 MB
- **Maximum dimensions:** 1280×1080 px
- **Animation:** GIFs play automatically in timeline
- **Limitation:** Only 1 GIF per post (occupies all 4 image slots)

### GIF Request Example
```json
{
  "content": "Check out this animation!",
  "mediaItems": [
    {"type": "gif", "url": "https://example.com/animation.gif"}
  ],
  "platforms": [
    {"platform": "twitter", "accountId": "acc_123"}
  ]
}
```

## Video Requirements

### Video Specifications
- **Maximum videos per post:** 1
- **Supported formats:** MP4, MOV
- **File size limit:** 512 MB
- **Duration range:** 0.5 seconds to 2 minutes 20 seconds (140 seconds)
- **Dimension limits:** 32×32 px minimum to 1920×1200 px maximum
- **Frame rate:** 40 fps maximum
- **Bitrate:** 25 Mbps maximum

### Recommended Video Specs

| Property | Recommended |
|----------|-------------|
| Resolution | 1280×720 px (720p) |
| Aspect Ratio | 16:9 landscape or 1:1 square |
| Frame Rate | 30 fps |
| Codec | H.264 |
| Audio | AAC, 128 kbps |

## Threads (Multi-Tweet)

Create connected tweet sequences using the threadItems field:

```json
{
  "platforms": [{
    "platform": "twitter",
    "accountId": "YOUR_ACCOUNT_ID",
    "platformSpecificData": {
      "threadItems": [
        {
          "content": "1/ Starting a thread about API design 🧵",
          "mediaItems": [{"type": "image", "url": "https://example.com/image1.jpg"}]
        },
        {"content": "2/ First, always use proper HTTP methods..."},
        {"content": "3/ Second, version your APIs from day one..."},
        {"content": "4/ Finally, document everything! /end"}
      ]
    }
  }],
  "publishNow": true
}
```

Each thread item can include separate media attachments and content.

## Common Issues & Solutions

### Image Too Large
Twitter rejects images exceeding 5 MB. The API attempts automatic compression if needed.

### GIF Won't Animate
Verify three conditions: file size ≤15 MB, true animated GIF format (not static image with .gif extension), and note that Twitter may convert large GIFs to video format.

### Video Rejected
Common rejection causes include duration exceeding 2 minutes 20 seconds, file size over 512 MB, unsupported codec (use H.264), or frame rate exceeding 40 fps.

## Inbox Features

> Requires Inbox add-on ($1/social set/month)

### Direct Messages
- ✅ List conversations
- ✅ Fetch messages
- ✅ Send text messages
- ✅ Send attachments (images, videos—max 25 MB)
- ❌ Archive/unarchive

### Comments
- ✅ List comments on posts
- ✅ Post new comments
- ✅ Reply to comments
- ✅ Delete comments
- ✅ Like/unlike comments
- ❌ Hide/unhide comments

### Important Limitations
- DM access requires `dm.read` and `dm.write` OAuth scopes
- Reply search uses cached conversation threads with 2-minute TTL
- Cached DMs maintain 15-minute TTL for rate limit management

## Related API Endpoints
- [Connect Twitter Account](/core/connect) — OAuth authentication flow
- [Create Post](/core/posts) — Post creation and scheduling
- [Upload Media](/utilities/media) — Image and video uploads
- [Analytics](/core/analytics) — Post performance metrics
- [Messages](/core/messages) and [Comments](/core/comments) — Inbox management
