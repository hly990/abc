# LinkedIn API Documentation

## Overview

The Late API enables posting to LinkedIn across personal profiles and company pages with support for images, videos, and documents.

## Quick Start

Post to LinkedIn in under 60 seconds using this endpoint:

**POST** `https://getlate.dev/api/v1/posts`

Required headers:
- `Authorization: Bearer YOUR_API_KEY`
- `Content-Type: application/json`

Basic payload structure:
```json
{
  "content": "Your post text",
  "platforms": [
    {"platform": "linkedin", "accountId": "YOUR_ACCOUNT_ID"}
  ],
  "publishNow": true
}
```

## Image Requirements

| Property | Requirement |
|----------|------------|
| Max Images | 20 per post |
| Formats | JPEG, PNG, GIF |
| Max File Size | 8 MB per image |
| Recommended | 1200 × 627 px |
| Min Dimensions | 552 × 276 px |
| Max Dimensions | 8192 × 8192 px |

### Aspect Ratios

- **Landscape**: 1.91:1 (1200 × 627 px) - Link shares
- **Square**: 1:1 (1080 × 1080 px) - Engagement
- **Portrait**: 1:1.25 (1080 × 1350 px) - Mobile feed

## Multi-Image Posts

LinkedIn supports carousel-style posts with up to 20 images:

```json
{
  "content": "Highlights from our team retreat! 🏔️",
  "mediaItems": [
    {"type": "image", "url": "https://example.com/photo1.jpg"},
    {"type": "image", "url": "https://example.com/photo2.jpg"},
    {"type": "image", "url": "https://example.com/photo3.jpg"}
  ],
  "platforms": [
    {"platform": "linkedin", "accountId": "YOUR_ACCOUNT_ID"}
  ],
  "publishNow": true
}
```

## Video Requirements

| Property | Requirement |
|----------|------------|
| Max Videos | 1 per post |
| Formats | MP4, MOV, AVI |
| Max File Size | 5 GB |
| Max Duration | 15 min (personal), 30 min (Pages) |
| Min Duration | 3 seconds |
| Resolution | 256 × 144 px to 4096 × 2304 px |
| Aspect Ratio | 1:2.4 to 2.4:1 |
| Frame Rate | 10-60 fps |

### Recommended Video Specs

- Resolution: 1920 × 1080 px (1080p)
- Aspect Ratio: 16:9 (landscape) or 1:1 (square)
- Frame Rate: 30 fps
- Codec: H.264
- Audio: AAC, 192 kbps
- Bitrate: 10-30 Mbps

## Document Posts

LinkedIn supports PDF and Office documents displayed as swipeable carousels:

| Property | Requirement |
|----------|------------|
| Max Documents | 1 per post |
| Formats | PDF, PPT, PPTX, DOC, DOCX |
| Max File Size | 100 MB |
| Max Pages | 300 pages |

Example:
```json
{
  "content": "Download our 2024 Industry Report 📊",
  "mediaItems": [
    {"type": "document", "url": "https://example.com/report.pdf"}
  ],
  "platforms": [
    {"platform": "linkedin", "accountId": "YOUR_ACCOUNT_ID"}
  ],
  "publishNow": true
}
```

**Document Tips:**
- First page serves as cover/preview
- Design for mobile viewing
- Keep text readable with large fonts
- Ideal length: 10-15 pages for engagement

## Multi-Organization Posting

Post to multiple company pages from a single connected account.

### List Available Organizations

**GET** `https://getlate.dev/api/v1/accounts/YOUR_ACCOUNT_ID/linkedin-organizations`

### Post to Multiple Organizations

Use the same `accountId` with different `organizationUrn` values:

```json
{
  "content": "Exciting updates from our organization! 🚀",
  "platforms": [
    {
      "platform": "linkedin",
      "accountId": "YOUR_ACCOUNT_ID",
      "platformSpecificData": {
        "organizationUrn": "urn:li:organization:111111111"
      }
    },
    {
      "platform": "linkedin",
      "accountId": "YOUR_ACCOUNT_ID",
      "platformSpecificData": {
        "organizationUrn": "urn:li:organization:222222222"
      }
    }
  ],
  "publishNow": true
}
```

**Note:** Format is `urn:li:organization:` followed by the organization ID.

## Link Previews

LinkedIn auto-generates preview cards when posting text with URLs:

```json
{
  "content": "Check out our latest blog post! https://example.com/blog/new-post",
  "platforms": [
    {"platform": "linkedin", "accountId": "acc_123"}
  ]
}
```

### Disable Link Preview

```json
{
  "platforms": [
    {
      "platform": "linkedin",
      "accountId": "acc_123",
      "platformSpecificData": {
        "disableLinkPreview": true
      }
    }
  ]
}
```

## First Comment

Add an automatic first comment to posts:

```json
{
  "platforms": [
    {
      "platform": "linkedin",
      "accountId": "acc_123",
      "platformSpecificData": {
        "firstComment": "What do you think? Share your thoughts below! 👇"
      }
    }
  ]
}
```

## GIF Support

- Converted to video format automatically
- Auto-play in feed (muted)
- Max recommended: 10 MB
- Counts as single video (1 per post limit)

## LinkedIn Articles (Long-Form)

LinkedIn's API does not support creating native long-form articles (the ones published at linkedin.com/article/new/). This is a LinkedIn platform limitation, not a Late limitation.

Supported formats:
- Text posts
- Image posts
- Video posts
- Document uploads (PDF, PPT, DOC)
- Link previews with rich metadata
- Multi-image posts

Users must publish long-form articles directly through LinkedIn's web interface.

## Common Issues

### Cannot mix media types
LinkedIn restricts images + videos or images + documents. Select one type per post.

### Document not displaying
- Verify file size ≤100 MB
- Ensure valid format (PDF recommended)
- Password-protected PDFs won't work

### Video processing failed
- Confirm codec is H.264
- Check duration limits (15 min personal, 30 min Pages)
- Verify aspect ratio between 1:2.4 and 2.4:1

### Link preview wrong image
The preview uses Open Graph meta tags. Update the `og:image` tag on your website.

## Inbox (Requires Add-On)

**Note:** Requires Inbox add-on ($1/social set/month)

LinkedIn supports comments only (no DMs via API).

### Comments

| Feature | Supported |
|---------|-----------|
| List comments on posts | ✅ |
| Reply to comments | ✅ |
| Delete comments | ✅ |
| Like comments | ❌ (API restricted) |

### Limitations

- **No DMs** — LinkedIn's messaging API unavailable for third-party apps
- **Organization accounts only** — Comments require company page accounts
- **No comment likes** — Reactions API restricted to LinkedIn partners

## Related API Endpoints

- Connect LinkedIn Account (OAuth flow)
- Create Post (creation and scheduling)
- Upload Media (images, videos, documents)
- LinkedIn Mentions (track mentions)
- Analytics (post performance metrics)
- Comments (comment management)
