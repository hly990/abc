# Instagram API Documentation

## Overview
The Late API enables posting to Instagram across multiple content types including Feed posts, Stories, Reels, and Carousels. "Post to Instagram with Late API - Feed posts, Stories, Reels, and Carousels."

## Quick Start

Post to Instagram in under 60 seconds using the `/v1/posts` endpoint:

```bash
curl -X POST https://getlate.dev/api/v1/posts \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Check out this photo! 📸",
    "mediaItems": [
      {"type": "image", "url": "https://example.com/photo.jpg"}
    ],
    "platforms": [
      {"platform": "instagram", "accountId": "YOUR_ACCOUNT_ID"}
    ],
    "publishNow": true
  }'
```

**Critical Note:** Instagram requires media for all posts. Text-only posts are not supported.

## Supported Content Types

| Type | Description | Media Required |
|------|-------------|-----------------|
| **Feed Post** | Standard image/video post | Yes |
| **Carousel** | Multi-image/video post (up to 10) | Yes |
| **Story** | 24-hour ephemeral content | Yes |
| **Reel** | Short-form video (up to 90 sec) | Yes (video) |

## Image Requirements

### General Specifications

| Property | Feed Post | Story | Carousel |
|----------|-----------|-------|----------|
| Max Images | 1 | 1 | 10 |
| Formats | JPEG, PNG | JPEG, PNG | JPEG, PNG |
| Max File Size | 8 MB | 8 MB | 8 MB each |
| Recommended | 1080 × 1350 px | 1080 × 1920 px | 1080 × 1080 px |

### Aspect Ratio Requirements

Strict aspect ratio guidelines apply for feed posts:

| Orientation | Ratio | Dimensions | Use Case |
|-------------|-------|-----------|----------|
| Portrait | 4:5 | 1080 × 1350 px | Best engagement |
| Square | 1:1 | 1080 × 1080 px | Standard |
| Landscape | 1.91:1 | 1080 × 566 px | Minimum |

**Allowed range:** 0.8 (4:5) to 1.91 (1.91:1)

Valid ratios:
- ✅ 4:5 (0.8) - Portrait
- ✅ 1:1 (1.0) - Square
- ✅ 1.91:1 (1.91) - Landscape

Invalid ratios:
- ❌ 9:16 (0.56) - Too tall, use Story instead
- ❌ 16:9 (1.78) - Acceptable but cropped

Images outside the 0.8-1.91 range (like 9:16 vertical videos) must be posted as Stories.

## Video Requirements

### Feed Videos / Reels

| Property | Requirement |
|----------|-------------|
| Formats | MP4, MOV |
| Max File Size | 300 MB (auto-compressed if larger) |
| Max Duration | 90 seconds (Reels), 60 min (Feed) |
| Min Duration | 3 seconds |
| Aspect Ratio | 9:16 (Reels), 4:5 to 1.91:1 (Feed) |
| Resolution | 1080 × 1920 px (Reels) |
| Frame Rate | 30 fps recommended |
| Codec | H.264 |

### Story Videos

| Property | Requirement |
|----------|-------------|
| Max File Size | 100 MB (auto-compressed if larger) |
| Max Duration | 60 seconds |
| Aspect Ratio | 9:16 |
| Resolution | 1080 × 1920 px |

## Carousel Posts

Create carousels with up to 10 items mixing images and videos:

```json
{
  "content": "Check out these photos from my trip! 🌴",
  "mediaItems": [
    {"type": "image", "url": "https://example.com/photo1.jpg"},
    {"type": "image", "url": "https://example.com/photo2.jpg"},
    {"type": "video", "url": "https://example.com/video.mp4"},
    {"type": "image", "url": "https://example.com/photo3.jpg"}
  ],
  "platforms": [
    {"platform": "instagram", "accountId": "YOUR_ACCOUNT_ID"}
  ],
  "publishNow": true
}
```

### Carousel Requirements
- All items should have the same aspect ratio
- First item determines the ratio for all subsequent items
- Mix of images and videos is allowed
- Each item: max 8 MB (images), 100 MB (videos)

## Stories

Post as Story using platform-specific data:

```json
{
  "mediaItems": [
    {"type": "image", "url": "https://example.com/story-image.jpg"}
  ],
  "platforms": [{
    "platform": "instagram",
    "accountId": "YOUR_ACCOUNT_ID",
    "platformSpecificData": {
      "contentType": "story"
    }
  }],
  "publishNow": true
}
```

**Story Notes:**
- Stories disappear after 24 hours
- No text captions displayed (use image overlays)
- 9:16 aspect ratio recommended

Instagram's tappable link stickers for Stories are not available through the API.

## Trial Reels

Trial Reels initially share only with non-followers, allowing performance testing before broader release:

```json
{
  "mediaItems": [
    {"type": "video", "url": "https://example.com/reel.mp4"}
  ],
  "platforms": [{
    "platform": "instagram",
    "accountId": "YOUR_ACCOUNT_ID",
    "platformSpecificData": {
      "trialParams": {
        "graduationStrategy": "SS_PERFORMANCE"
      }
    }
  }],
  "publishNow": true
}
```

### Graduation Strategies

| Strategy | Description |
|----------|-------------|
| `MANUAL` | Reel graduates only via Instagram app |
| `SS_PERFORMANCE` | Auto-graduates if performs well with non-followers |

**Note:** Trial Reels apply only to video posts (Reels), not images, carousels, or stories.

## Thumbnail Offset for Reels

Specify a video frame as the Reel thumbnail using millisecond offset:

```json
{
  "content": "Check out my new Reel! 🎬",
  "mediaItems": [
    {"type": "video", "url": "https://example.com/reel.mp4"}
  ],
  "platforms": [{
    "platform": "instagram",
    "accountId": "YOUR_ACCOUNT_ID",
    "platformSpecificData": {
      "thumbOffset": 5000
    }
  }],
  "publishNow": true
}
```

| Property | Description |
|----------|-------------|
| `thumbOffset` | Millisecond offset from video start (e.g., 5000 = 5 seconds) |

If providing a custom thumbnail URL, it takes priority over thumbOffset.

## Custom Cover Images for Reels

Upload custom cover images instead of using video frames:

```json
{
  "content": "Check out my new Reel! 🎬",
  "mediaItems": [
    {
      "type": "video",
      "url": "https://example.com/reel.mp4",
      "instagramThumbnail": "https://example.com/custom-cover.jpg"
    }
  ],
  "platforms": [
    {"platform": "instagram", "accountId": "YOUR_ACCOUNT_ID"}
  ],
  "publishNow": true
}
```

### Cover Image Requirements

| Property | Requirement |
|----------|-------------|
| Format | JPEG, PNG |
| Recommended Size | 1080 × 1920 px (9:16) |
| Aspect Ratio | Should match Reel (typically 9:16) |

## Collaborators

Invite up to 3 collaborators on feed posts and Reels:

```json
{
  "platforms": [
    {
      "platform": "instagram",
      "accountId": "acc_123",
      "platformSpecificData": {
        "collaborators": ["username1", "username2"]
      }
    }
  ]
}
```

## Audio Name

Set custom names for original audio in Reels:

```json
{
  "mediaItems": [{"type": "video", "url": "https://example.com/reel.mp4"}],
  "platforms": [{
    "platform": "instagram",
    "accountId": "YOUR_ACCOUNT_ID",
    "platformSpecificData": {
      "audioName": "My Podcast Intro"
    }
  }],
  "publishNow": true
}
```

Audio name can only be set once and applies only for Reels.

## Automatic Compression

Late automatically compresses oversized media while preserving originals:

| Content Type | Image Limit | Video Limit | Action |
|-------------|------------|------------|--------|
| Feed/Carousel | 8 MB | 300 MB | Auto-compress |
| Story | 8 MB | 100 MB | Auto-compress |
| Reel | 8 MB | 300 MB | Auto-compress |

## Common Issues

### "Invalid aspect ratio"
Images fall outside 0.8-1.91 range. Solutions:
1. Crop to 4:5 or 1:1
2. Use `contentType: "story"` for 9:16 content

### Video rejected as Reel
Videos under 90 seconds with 9:16 aspect ratio automatically become Reels. Use different aspect ratios for feed videos.

### Carousel items showing differently
Ensure all carousel items maintain the same aspect ratio, as the first item determines the ratio for all.

## Inbox Management

Requires Inbox add-on ($1/social set/month)

### Direct Messages

| Feature | Supported |
|---------|-----------|
| List conversations | ✅ |
| Fetch messages | ✅ |
| Send text messages | ✅ |
| Send attachments | ✅ (images, videos, audio via URL) |
| Quick replies | ✅ (up to 13) |
| Buttons | ✅ (up to 3) |
| Carousels | ✅ (up to 10 elements) |
| Message tags | ✅ (`HUMAN_AGENT` only) |
| Archive/unarchive | ✅ |

**Attachment limits:** 8 MB images, 25 MB video/audio. Attachments auto-upload to temporary storage.

**Message tags:** Use `messageTag: "HUMAN_AGENT"` with `messagingType: "MESSAGE_TAG"` for 24-hour window bypass.

### Instagram Profile Data

Conversations include optional `instagramProfile` object on participants:

| Field | Type | Description |
|-------|------|-------------|
| `isFollower` | boolean | Participant follows business account |
| `isFollowing` | boolean | Business account follows participant |
| `followerCount` | integer | Participant's follower count |
| `isVerified` | boolean | Verified Instagram user status |
| `fetchedAt` | datetime | Last data fetch time |

Available in conversation endpoints and `message.received` webhooks.

### Ice Breakers

Manage ice breaker prompts in DM conversations (max 4, 80 characters each). See Account Settings endpoints.

### Comments

| Feature | Supported |
|---------|-----------|
| List comments on posts | ✅ |
| Post new top-level comment | ❌ (reply-only) |
| Reply to comments | ✅ |
| Delete comments | ✅ |
| Like comments | ❌ (deprecated since 2018) |
| Hide/unhide comments | ✅ |

### Webhooks

Subscribe to `message.received` for DM notifications.

### Limitations

- Cannot post new top-level comments, only replies to existing comments
- No comment likes (deprecated in 2018)

## Related API Endpoints

- Connect Instagram Account — OAuth flow via Facebook Business
- Create Post — Post creation and scheduling
- Upload Media — Image and video uploads
- Analytics — Post performance metrics
- Messages and Comments — DM and comment management
- Account Settings — Ice breakers configuration
