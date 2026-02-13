# TikTok API Documentation

## Overview
Post to TikTok using the Late API with support for videos, photo carousels, and creator tools.

## Quick Start

Post to TikTok in under 60 seconds using a simple REST API call:

```bash
curl -X POST https://getlate.dev/api/v1/posts \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Check out this amazing sunset! 🌅",
    "mediaItems": [{"type": "video", "url": "https://example.com/video.mp4"}],
    "platforms": [{"platform": "tiktok", "accountId": "YOUR_ACCOUNT_ID"}],
    "tiktokSettings": {
      "privacy_level": "PUBLIC_TO_EVERYONE",
      "allow_comment": true,
      "allow_duet": true,
      "allow_stitch": true,
      "content_preview_confirmed": true,
      "express_consent_given": true
    },
    "publishNow": true
  }'
```

## Content Types

TikTok supports two distinct formats:

| Type | Description | Limit |
|------|-------------|-------|
| **Video** | Single video post | 1 per post |
| **Photo Carousel** | Multiple images | Up to 35 |

**Important constraint:** Photos and videos cannot be mixed in the same post.

## Video Requirements

| Property | Specification |
|----------|--------------|
| **Formats** | MP4, MOV, WebM |
| **Max File Size** | 4 GB |
| **Duration Range** | 3 seconds to 10 minutes |
| **Minimum Resolution** | 720 × 1280 px |
| **Recommended Aspect Ratio** | 9:16 (vertical) |
| **Frame Rate** | 24-60 fps |
| **Video Codec** | H.264 |

### Recommended Specifications
- Resolution: 1080 × 1920 px
- Aspect Ratio: 9:16 vertical
- Frame Rate: 30 fps
- Bitrate: 10-20 Mbps
- Audio: AAC at 128 kbps

## Video Cover/Thumbnail

Customize the thumbnail by specifying a timestamp:

```json
{
  "tiktokSettings": {
    "video_cover_timestamp_ms": 3000
  }
}
```

- Value specified in milliseconds
- Default: 1000 ms (1 second)
- Must fall within video duration

## Photo Carousel

Create multi-image posts with up to 35 photos:

```json
{
  "content": "My travel highlights ✈️",
  "mediaItems": [
    {"type": "image", "url": "https://example.com/photo1.jpg"},
    {"type": "image", "url": "https://example.com/photo2.jpg"},
    {"type": "image", "url": "https://example.com/photo3.jpg"}
  ],
  "platforms": [{"platform": "tiktok", "accountId": "YOUR_ACCOUNT_ID"}],
  "tiktokSettings": {
    "privacy_level": "PUBLIC_TO_EVERYONE",
    "allow_comment": true,
    "media_type": "photo",
    "photo_cover_index": 0,
    "content_preview_confirmed": true,
    "express_consent_given": true
  },
  "publishNow": true
}
```

### Photo Requirements

| Property | Specification |
|----------|--------------|
| **Maximum Photos** | 35 images |
| **Formats** | JPEG, PNG, WebP |
| **Max File Size** | 20 MB per image |
| **Recommended Aspect Ratio** | 9:16 |
| **Recommended Resolution** | 1080 × 1920 px |

## Caption Limits

| Content Type | Character Limit | Notes |
|-------------|-----------------|-------|
| **Video** | 2200 characters | Full content used as title |
| **Photo** | 90 characters | Auto-truncated (hashtags/URLs stripped) |

For longer captions on photo carousels, use the description field:

```json
{
  "tiktokSettings": {
    "description": "Extended description up to 4000 characters..."
  }
}
```

## Optional Features

### Auto-Add Music
Let TikTok automatically select music for photo carousels:

```json
{
  "tiktokSettings": {
    "auto_add_music": true
  }
}
```

*Note: Only applicable to photo carousels, not videos.*

### AI Disclosure
Mark content as AI-generated:

```json
{
  "tiktokSettings": {
    "video_made_with_ai": true
  }
}
```

### Draft Mode
Send to creator inbox as draft instead of publishing:

```json
{
  "tiktokSettings": {
    "draft": true
  }
}
```

## Required Settings

Due to TikTok Direct Post API requirements, these fields are mandatory:

| Field | Scope | Details |
|-------|-------|---------|
| `privacy_level` | All posts | Must align with creator account permissions |
| `allow_comment` | All posts | Enable/disable commenting |
| `allow_duet` | Videos only | Enable/disable duet feature |
| `allow_stitch` | Videos only | Enable/disable stitch feature |
| `content_preview_confirmed` | All posts | Must be `true` |
| `express_consent_given` | All posts | Must be `true` |

## Troubleshooting

### "Invalid privacy_level" Error
The selected privacy level must match one of the values permitted for the creator account. Retrieve allowed values from the TikTok creator information endpoint.

### "Missing required fields" Error
Ensure both `content_preview_confirmed` and `express_consent_given` are set to `true`.

### Cannot Mix Media Types
TikTok's API prevents combining photos and videos in one post. Use either all images or a single video.

### Video Rejected
Verify that your video:
- Falls within the 3-second to 10-minute duration range
- Uses MP4 format (recommended)
- Maintains vertical aspect ratio (9:16)

### Photo Carousel Title Truncated
Titles are limited to 90 characters for photo posts. Utilize the `description` field for longer text.

## Inbox Management

> Requires Inbox add-on ($1/social set/month)

TikTok inbox support is limited to comment operations only.

### Comment Operations

| Operation | Support |
|-----------|---------|
| List comments on posts | ❌ Not available |
| Post new comment | ✅ Supported |
| Reply to comments | ✅ Supported |
| Delete comments | ✅ Supported |

### Limitations

- **Read-only restriction:** TikTok's API only allows posting comments, not retrieving them
- **No DM support:** Direct messaging API access is unavailable

See the Comments API Reference for complete endpoint documentation.

## Related Endpoints

- [Connect TikTok Account](/core/connect) — OAuth authentication flow
- [Create Post](/core/posts) — Post creation and scheduling
- [Upload Media](/utilities/media) — Image and video uploads
- [TikTok Video Download](/tools/downloads) — Video retrieval tool
- [Comments](/core/comments) — Comment management
