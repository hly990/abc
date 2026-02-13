# YouTube API Documentation

## Overview
Post to YouTube with Late API - Videos, Shorts, thumbnails, and visibility settings

## Quick Start

Upload a video to YouTube using the Late API with a simple POST request:

```bash
curl -X POST https://getlate.dev/api/v1/posts \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "My Latest Video\n\nIn this video, I share my thoughts on...\n\n#tutorial #howto",
    "mediaItems": [
      {"type": "video", "url": "https://example.com/video.mp4"}
    ],
    "platforms": [{
      "platform": "youtube",
      "accountId": "YOUR_ACCOUNT_ID",
      "platformSpecificData": {
        "title": "My Latest Video",
        "visibility": "public"
      }
    }],
    "publishNow": true
  }'
```

## Video Types

The API automatically detects content type based on duration:

| Duration | Type | Notes |
|----------|------|-------|
| ≤ 3 minutes | YouTube Shorts | Vertical format, no custom thumbnails via API |
| > 3 minutes | Regular Video | Supports custom thumbnails |

## Video Requirements

| Property | Shorts | Regular Video |
|----------|--------|---------------|
| **Max Duration** | 3 minutes | 12 hours |
| **Min Duration** | 1 second | 1 second |
| **Max File Size** | 256 GB | 256 GB |
| **Formats** | MP4, MOV, AVI, WMV, FLV, 3GP | MP4, MOV, AVI, WMV, FLV, 3GP |
| **Aspect Ratio** | 9:16 (vertical) | 16:9 (horizontal) |
| **Resolution** | 1080 × 1920 px | 1920 × 1080 px (1080p) |

### Recommended Specifications

| Property | Shorts | Regular Video |
|----------|--------|---------------|
| Resolution | 1080 × 1920 px | 3840 × 2160 px (4K) |
| Frame Rate | 30 fps | 24-60 fps |
| Codec | H.264 | H.264 or H.265 |
| Audio | AAC, 128 kbps | AAC, 384 kbps |
| Bitrate | 10 Mbps | 35-68 Mbps (4K) |

## Title and Description

| Property | Limit | Notes |
|----------|-------|-------|
| Title | 100 characters | Defaults to first line of content |
| Description | 5000 characters | Full content used |

The `content` field becomes the video description. The title is determined by:

1. Value specified in `platformSpecificData.title`
2. Auto-extracted from first line of content
3. "Untitled Video" as fallback

## Visibility Options

Control who can view your video:

| Visibility | Description |
|------------|-------------|
| `public` | Anyone can search and watch (default) |
| `unlisted` | Only people with link can watch |
| `private` | Only you and shared users |

Example:
```json
{
  "platformSpecificData": {
    "visibility": "unlisted"
  }
}
```

## Custom Thumbnails

For regular videos (>3 minutes), specify a custom thumbnail:

```json
{
  "mediaItems": [{
    "type": "video",
    "url": "https://example.com/video.mp4",
    "thumbnail": {
      "url": "https://example.com/thumbnail.jpg"
    }
  }]
}
```

### Thumbnail Requirements

| Property | Requirement |
|----------|-------------|
| Format | JPEG, PNG, GIF |
| Max Size | 2 MB |
| Resolution | 1280 × 720 px (16:9) |
| Min Width | 640 px |

**Note:** Custom thumbnails are not supported for Shorts via API.

## Made for Kids (COPPA Compliance)

Declare whether videos are made for kids to comply with COPPA regulations:

```json
{
  "platformSpecificData": {
    "madeForKids": true
  }
}
```

| Value | Description |
|-------|-------------|
| `true` | Video is made for kids (child-directed content) |
| `false` | Video is NOT made for kids (default) |

**Important:** Videos marked as made for kids have restricted features including disabled comments, no notification bell, and limited ad targeting.

## Video Categories

Specify a category to help YouTube organize and recommend your video:

```json
{
  "platformSpecificData": {
    "categoryId": "27"
  }
}
```

### Available Categories

| Category ID | Name |
|------------|------|
| `1` | Film & Animation |
| `2` | Autos & Vehicles |
| `10` | Music |
| `15` | Pets & Animals |
| `17` | Sports |
| `20` | Gaming |
| `22` | People & Blogs (default) |
| `23` | Comedy |
| `24` | Entertainment |
| `25` | News & Politics |
| `26` | Howto & Style |
| `27` | Education |
| `28` | Science & Technology |

## First Comment

Add an automatic pinned comment immediately after upload:

```json
{
  "platformSpecificData": {
    "firstComment": "Thanks for watching! Don't forget to subscribe."
  }
}
```

- Maximum 10,000 characters
- Posted immediately after upload
- Can include links

## Scheduled Videos

When scheduling a YouTube video:

1. Video uploads immediately with specified visibility
2. Remains in that state until scheduled time
3. At scheduled time, may change to "public" if originally set

```json
{
  "scheduledFor": "2024-12-25T10:00:00Z",
  "platforms": [
    {
      "platform": "youtube",
      "platformSpecificData": {
        "visibility": "private"
      }
    }
  ]
}
```

## Supported Formats

| Format | Extension | Notes |
|--------|-----------|-------|
| MPEG-4 | .mp4 | Recommended |
| QuickTime | .mov | Well supported |
| AVI | .avi | Supported |
| WMV | .wmv | Windows Media |
| FLV | .flv | Flash Video |
| 3GPP | .3gp | Mobile format |
| WebM | .webm | Supported |
| MPEG-PS | .mpg | Supported |

## Common Issues

### Video Stuck Processing
Large videos (>1 GB) may take 30+ minutes to process. Use scheduled posts for asynchronous handling.

### Wrong Video Type Detected
- Shorts: ≤3 min + vertical aspect ratio
- Regular: >3 min OR horizontal aspect ratio
- Ensure aspect ratio matches intent

### Thumbnail Rejected
- Must be exactly 16:9 aspect ratio
- Max 2 MB file size
- Min 640 px width
- Not available for Shorts

### Audio Issues
- Use AAC codec
- Avoid copyright-protected music
- Ensure audio bitrate is at least 128 kbps

### Resolution Too Low
Minimum recommended:
- Shorts: 720 × 1280 px
- Regular: 1280 × 720 px (720p)

## Inbox

**Requires Inbox add-on** — $1/social set/month

YouTube supports comments only (no DMs available).

### Comments

| Feature | Supported |
|---------|-----------|
| List comments on videos | ✅ |
| Reply to comments | ✅ |
| Delete comments | ✅ |
| Like comments | ❌ (no API available) |

### Limitations

- **No DMs:** YouTube does not have a direct messaging system
- **No comment likes:** No public API endpoint available for liking comments

## Related API Endpoints

- Connect YouTube Account — OAuth flow
- Create Post — Post creation and scheduling
- Upload Media — Video uploads
- YouTube Video Download — Download YouTube videos
- YouTube Transcripts — Get video transcripts
- Comments — Comments management
