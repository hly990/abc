# Bluesky API Documentation

## Overview
The Late API enables posting to Bluesky with support for text posts, images, and videos across their platform.

## Quick Start

Post to Bluesky in under 60 seconds using any of these languages:

**cURL:**
```bash
curl -X POST https://getlate.dev/api/v1/posts \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Hello from Late API! 🦋",
    "platforms": [
      {"platform": "bluesky", "accountId": "YOUR_ACCOUNT_ID"}
    ],
    "publishNow": true
  }'
```

## Authentication

Bluesky uses App Passwords instead of OAuth. Connection requires:

1. Navigate to Bluesky Settings > App Passwords
2. Create a new App Password
3. Use the connect endpoint with handle and app password

**Connection Endpoint:**
```
POST /api/v1/connect/bluesky
```

Required parameters: `profileId`, `handle` (format: yourhandle.bsky.social), and `appPassword`.

## Image Requirements

| Property | Requirement |
|----------|-------------|
| Max Images | 4 per post |
| Formats | JPEG, PNG, WebP, GIF |
| Max File Size | 1 MB per image |
| Max Dimensions | 2000 × 2000 px |
| Recommended | 1200 × 675 px (16:9) |

**Aspect Ratios:**
- Landscape (16:9): 1200 × 675 px
- Square (1:1): 1000 × 1000 px
- Portrait (4:5): 800 × 1000 px

## Video Requirements

| Property | Requirement |
|----------|-------------|
| Max Videos | 1 per post |
| Formats | MP4 |
| Max File Size | 50 MB |
| Max Duration | 60 seconds |
| Max Dimensions | 1920 × 1080 px |
| Frame Rate | 30 fps recommended |

**Recommended Specs:** 1280 × 720 px (720p), 16:9 or 1:1 aspect ratio, H.264 codec, AAC audio.

## Character Limits

- Post text: 300 characters
- Alt text for images: 1000 characters

Platform automatically renders URLs as link cards, mentions (@handle.bsky.social), and hashtags.

## Link Cards

When posts contain URLs, Bluesky automatically generates preview cards. Best practices include placing URLs at the end and ensuring proper Open Graph meta tags on target pages.

## Common Issues

**Image Too Large:** Bluesky enforces a strict 1 MB limit per image. Compress before upload or use automatic compression.

**App Password Invalid:** Ensure you're using an App Password (format: xxxx-xxxx-xxxx-xxxx), not your main account password. Create a new one if needed.

**Post Too Long:** Exceeding 300 characters requires shortening URLs, splitting into multiple posts, or moving content to linked pages.

## Inbox Features

*Requires Inbox add-on ($1/social set/month)*

**Direct Messages:**
- List conversations ✅
- Fetch messages ✅
- Send text messages ✅
- Send attachments ❌ (API limitation)
- Archive/unarchive ✅

**Comments:**
- List comments on posts ✅
- Reply to comments ✅
- Delete comments ✅
- Like comments ✅ (requires CID)
- Unlike comments ✅ (requires likeUri)

**Limitations:**
- No DM attachments (Bluesky Chat API restriction)
- Liking requires content identifier (CID)
- Unliking requires storing the likeUri from the like operation

## Related API Endpoints

- Connect Bluesky Account — App Password authentication
- Create Post — Post creation and scheduling
- Upload Media — Image and video uploads
- Messages & Comments — Full inbox API reference
