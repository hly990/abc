# Pinterest API Documentation

## Overview

The Pinterest API via Late allows posting pins with images or videos to Pinterest boards with customizable titles, destination links, and cover images for videos.

## Quick Start

Create a pin using the POST endpoint at `https://getlate.dev/api/v1/posts` with authentication via Bearer token:

```json
{
  "content": "10 Tips for Better Photography 📸",
  "mediaItems": [{"type": "image", "url": "https://example.com/pin-image.jpg"}],
  "platforms": [{
    "platform": "pinterest",
    "accountId": "YOUR_ACCOUNT_ID",
    "platformSpecificData": {
      "title": "10 Tips for Better Photography",
      "boardId": "YOUR_BOARD_ID",
      "link": "https://myblog.com/photography-tips"
    }
  }],
  "publishNow": true
}
```

## Media Requirements

### Images
- **Maximum**: 1 per pin
- **Formats**: JPEG, PNG, WebP, GIF
- **Max File Size**: 32 MB
- **Recommended**: 1000 × 1500 px (2:3 aspect ratio)
- **Minimum**: 100 × 100 px

#### Aspect Ratios
| Ratio | Dimensions | Use Case |
|-------|-----------|----------|
| 2:3 | 1000 × 1500 px | Optimal standard pin |
| 1:1 | 1000 × 1000 px | Square pin |
| 1:2.1 | 1000 × 2100 px | Long pin (max height) |

### Videos
- **Maximum**: 1 per pin
- **Formats**: MP4, MOV
- **Max File Size**: 2 GB
- **Duration**: 4 seconds to 15 minutes
- **Aspect Ratios**: 2:3, 1:1, or 9:16
- **Resolution**: 1080p recommended
- **Frame Rate**: 25+ fps minimum

#### Video Specifications
| Property | Minimum | Recommended |
|----------|---------|------------|
| Resolution | 240p | 1080p |
| Bitrate | — | 10 Mbps |
| Audio | — | AAC, 128 kbps |

## Video Pin Example

```json
{
  "content": "Quick recipe tutorial 🍳",
  "mediaItems": [{"type": "video", "url": "https://example.com/recipe.mp4"}],
  "platforms": [{
    "platform": "pinterest",
    "accountId": "YOUR_ACCOUNT_ID",
    "platformSpecificData": {
      "title": "5-Minute Breakfast Recipe",
      "boardId": "YOUR_BOARD_ID",
      "link": "https://myrecipes.com/quick-breakfast"
    }
  }],
  "publishNow": true
}
```

## Video Cover Image

Add custom cover images for video pins:

```json
{
  "platformSpecificData": {
    "coverImageUrl": "https://example.com/cover.jpg",
    "coverImageKeyFrameTime": 5
  }
}
```

- `coverImageUrl`: URL of custom cover image
- `coverImageKeyFrameTime`: Auto-extract frame at specified seconds

## Pin Title

Configure pin titles (max 100 characters):

```json
{
  "platformSpecificData": {
    "title": "My Pin Title (max 100 chars)"
  }
}
```

Defaults to first line of content if omitted; falls back to "Pin" if unavailable.

## Board Selection

Specify target board:

```json
{
  "platformSpecificData": {
    "boardId": "board_123456"
  }
}
```

If omitted, pins to the first available board.

### Get Board IDs

Retrieve available boards via:
```
GET https://getlate.dev/api/v1/accounts/{accountId}/pinterest-boards
```

## Destination Link

Add clickable URLs to pins:

```json
{
  "platformSpecificData": {
    "link": "https://example.com/landing-page"
  }
}
```

Links must be valid URLs with `https://` protocol and open when users click the pin.

## GIF Support

Animated GIFs are supported with automatic playback in feeds, maximum 32 MB file size, treated as images (not videos), and recommended under 10 MB for faster loading.

## Common Issues & Solutions

**Image too small**: Minimum dimensions are 100 × 100 px, but Pinterest recommends at least 600 px wide for acceptable quality.

**Pin not showing in feed**: May require time for indexing; verify board isn't secret/archived and account is in good standing.

**Video processing failed**: Ensure MP4 or MOV format, duration between 4 seconds and 15 minutes, file size under 2 GB, and H.264 codec.

**Wrong aspect ratio display**: Pinterest crops to fit; use 2:3 ratio (1000 × 1500 or 600 × 900 px minimum).

**Link not clickable**: Verify URL validity and accessibility; use `https://` and avoid blocked shorteners.

## Inbox Limitations

Pinterest does not provide third-party API access to direct messages, pin comments, or review systems.

## Related Endpoints

- Connect Pinterest Account (OAuth flow)
- Create Post (scheduling and publishing)
- Upload Media (image and video uploads)
- Pinterest Boards (list available boards)
