# Threads API Documentation

## Overview

The Late API provides comprehensive support for posting to Threads (Meta's text-based platform). You can publish text content, images, videos, and connected thread sequences.

## Quick Start

Post to Threads in under 60 seconds using this endpoint:

```
POST https://getlate.dev/api/v1/posts
```

**Required Headers:**
- `Authorization: Bearer YOUR_API_KEY`
- `Content-Type: application/json`

**Basic Request Body:**
```json
{
  "content": "Your message here",
  "platforms": [
    {
      "platform": "threads",
      "accountId": "YOUR_ACCOUNT_ID"
    }
  ],
  "publishNow": true
}
```

## Image Specifications

| Property | Details |
|----------|---------|
| Maximum images | 20 per post |
| Supported formats | JPEG, PNG, WebP, GIF |
| Max file size | 8 MB per image |
| Recommended dimensions | 1080 × 1350 px (4:5 ratio) |

### Aspect Ratios
- **4:5** – 1080 × 1350 px (portrait, recommended)
- **1:1** – 1080 × 1080 px (square)
- **16:9** – 1080 × 608 px (landscape)

## Video Specifications

| Property | Requirement |
|----------|-------------|
| Maximum videos | 1 per post |
| Supported formats | MP4, MOV |
| Max file size | 1 GB |
| Max duration | 5 minutes |
| Min duration | 0 seconds |
| Aspect ratios | 9:16 (vertical), 16:9 (landscape), 1:1 |
| Recommended resolution | 1080p |

**Recommended specs:** 1080 × 1920 px (vertical), 30 fps, H.264 codec, AAC audio at 128 kbps

## Image Post Example

```json
{
  "content": "Sharing some thoughts on building in public 🚀",
  "mediaItems": [
    { "type": "image", "url": "https://example.com/photo1.jpg" },
    { "type": "image", "url": "https://example.com/photo2.jpg" }
  ],
  "platforms": [
    { "platform": "threads", "accountId": "YOUR_ACCOUNT_ID" }
  ],
  "publishNow": true
}
```

## Thread Sequences

Create connected posts with a root post followed by reply items:

```json
{
  "platforms": [{
    "platform": "threads",
    "accountId": "YOUR_ACCOUNT_ID",
    "platformSpecificData": {
      "threadItems": [
        {
          "content": "Here is a thread about API design 🧵",
          "mediaItems": [{"type": "image", "url": "https://example.com/cover.jpg"}]
        },
        { "content": "1/ First, let us talk about REST principles..." },
        {
          "content": "2/ Authentication is crucial...",
          "mediaItems": [{"type": "image", "url": "https://example.com/auth-diagram.jpg"}]
        },
        { "content": "3/ Finally, always version your API! /end" }
      ]
    }
  }],
  "publishNow": true
}
```

**Notes:**
- First item is the root post
- Subsequent items become replies in order
- Each item can include its own media
- No limit on thread length

## Carousel Posts

Display multiple images in a swipeable carousel:

```json
{
  "content": "Product launch day! Here are all the new features:",
  "mediaItems": [
    { "type": "image", "url": "https://example.com/feature1.jpg" },
    { "type": "image", "url": "https://example.com/feature2.jpg" },
    { "type": "image", "url": "https://example.com/feature3.jpg" }
  ],
  "platforms": [
    { "platform": "threads", "accountId": "acc_123" }
  ]
}
```

Maximum of 20 images per carousel.

## GIF Support

Threads supports animated GIFs with these characteristics:
- Auto-play in feed
- Max 8 MB file size
- Counts toward the image limit
- May be converted to video internally by Threads

## Link Previews

URLs in post text automatically generate preview cards:

```json
{
  "content": "Just published a new blog post! https://myblog.com/new-post",
  "platforms": [
    { "platform": "threads", "accountId": "acc_123" }
  ]
}
```

## Text Limits

| Property | Limit |
|----------|-------|
| Post text | 500 characters |
| With link | Link may consume additional space |

## Common Issues & Solutions

**Too many images:** Maximum is 20 images per post. Use thread sequences for additional content.

**Video too long:** Maximum duration is 5 minutes. Trim or split longer videos.

**Image rejected:** Verify file size (≤8 MB), valid format, and reasonable aspect ratio.

**Thread sequence failed:** Ensure first item has content, check each item's media individually, and verify account permissions.

**Media type mismatch:** Unlike some platforms, Threads allows mixing images and videos in thread sequences, but not multiple media types in a single post.

## Inbox Management

Requires Inbox add-on ($1/social set/month)

### Comment Features

| Feature | Supported |
|---------|-----------|
| List comments on posts | ✅ |
| Post new comments | ❌ |
| Reply to comments | ✅ |
| Delete comments | ✅ |
| Like/unlike comments | ❌ |
| Hide/unhide comments | ✅ |

### Limitations

- No direct messages supported
- Cannot like comments
- Can only reply to existing comments, not create top-level ones

See Comments API Reference for endpoint details.

## Related API Endpoints

- **Connect Threads Account** – Via Instagram authentication
- **Create Post** – Post creation and scheduling
- **Upload Media** – Image and video uploads
- **Comments** – Comment management
