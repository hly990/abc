# Snapchat API Documentation

## Overview

The Late API enables posting to Snapchat through three distinct content types: Stories, Saved Stories, and Spotlight content.

## Supported Content Types

| Type | Description | Duration | Caption Support |
|------|-------------|----------|-----------------|
| `story` | Ephemeral snap visible for 24 hours | Temporary | No caption |
| `saved_story` | Permanent story on Public Profile | Permanent | Title (max 45 chars) |
| `spotlight` | Video in Snapchat's entertainment feed | Permanent | Description (max 160 chars) |

## Media Requirements

### Images
- **Formats:** JPEG, PNG
- **Max File Size:** 20 MB
- **Recommended Dimensions:** 1080 × 1920 px
- **Aspect Ratio:** 9:16 (portrait)

### Videos
- **Format:** MP4
- **Max File Size:** 500 MB
- **Duration:** 5-60 seconds
- **Min Resolution:** 540 × 960 px
- **Recommended Dimensions:** 1080 × 1920 px
- **Aspect Ratio:** 9:16 (portrait)

Media is encrypted using AES-256-CBC before upload.

## Key Limitations

- **Text-only posts:** Not supported
- **Multiple media items:** Only single media per post allowed
- **Public Profile requirement:** Required for API publishing
- **No inbox features:** Messaging API unavailable for third-party apps
- **No comments access:** Snap comments not retrievable via API

## Quick Start Example

Post to Snapchat with a simple POST request to `/api/v1/posts`:

```json
{
  "mediaItems": [
    {"type": "video", "url": "https://example.com/video.mp4"}
  ],
  "platforms": [{
    "platform": "snapchat",
    "accountId": "YOUR_ACCOUNT_ID",
    "platformSpecificData": {
      "contentType": "story"
    }
  }],
  "publishNow": true
}
```

## OAuth Connection Flow

Two approaches exist for account connection:

1. **Standard Flow:** Redirect users to `https://getlate.dev/connect/snapchat`
2. **Headless Mode:** Build custom UI with three-step OAuth process

## Analytics Available

The platform provides metrics including views, unique viewers, screenshots, shares, and completion rates for tracking content performance.
