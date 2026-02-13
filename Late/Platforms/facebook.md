# Facebook API Documentation

## Overview

The Late API enables posting to Facebook with support for Pages, Stories, Reels, and multi-image posts.

## Quick Start

Post to Facebook in under 60 seconds using cURL, JavaScript, or Python:

```bash
curl -X POST https://getlate.dev/api/v1/posts \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Hello from Late API! 🚀",
    "platforms": [
      {"platform": "facebook", "accountId": "YOUR_ACCOUNT_ID"}
    ],
    "publishNow": true
  }'
```

## Image Requirements

| Property | Feed Post | Story |
|----------|-----------|-------|
| Max Images | 10 | 1 |
| Formats | JPEG, PNG, GIF, WebP | JPEG, PNG |
| Max File Size | 10 MB | 10 MB |
| Recommended | 1200 × 630 px | 1080 × 1920 px |
| Min Dimensions | 200 × 200 px | 500 × 500 px |

### Aspect Ratios

| Type | Ratio | Dimensions | Use Case |
|------|-------|-----------|----------|
| Landscape | 1.91:1 | 1200 × 630 px | Link previews, standard |
| Square | 1:1 | 1080 × 1080 px | Engagement posts |
| Portrait | 4:5 | 1080 × 1350 px | Mobile-optimized |
| Story | 9:16 | 1080 × 1920 px | Stories only |

## Video Requirements

| Property | Feed Video | Story |
|----------|-----------|-------|
| Max Videos | 1 | 1 |
| Formats | MP4, MOV | MP4, MOV |
| Max File Size | 4 GB | 4 GB |
| Max Duration | 240 minutes | 120 seconds |
| Min Duration | 1 second | 1 second |
| Resolution | Up to 4K | 1080 × 1920 px |
| Frame Rate | 30 fps recommended | 30 fps |

### Recommended Video Specs

- **Resolution:** 1280 × 720 px (720p) minimum
- **Aspect Ratio:** 16:9 (landscape), 9:16 (Stories)
- **Codec:** H.264
- **Audio:** AAC, 128 kbps stereo
- **Bitrate:** 8 Mbps for 1080p

## Multi-Image Posts

Facebook allows up to 10 images in a single post. Send multiple media items in the `mediaItems` array.

**Note:** Cannot mix images and videos in the same post.

## Facebook Stories

Stories are 24-hour ephemeral content. To post as a Story:

```json
{
  "mediaItems": [
    { "type": "image", "url": "https://example.com/story.jpg" }
  ],
  "platforms": [
    {
      "platform": "facebook",
      "accountId": "acc_123",
      "platformSpecificData": {
        "contentType": "story"
      }
    }
  ]
}
```

### Story Requirements

- Media required (image or video)
- No text captions displayed
- Disappear after 24 hours
- Recommended: 1080 × 1920 px (9:16)

## First Comment

Add an automatic first comment to your post:

```json
{
  "content": "New product launch! 🚀",
  "mediaItems": [
    { "type": "image", "url": "https://example.com/product.jpg" }
  ],
  "platforms": [
    {
      "platform": "facebook",
      "accountId": "acc_123",
      "platformSpecificData": {
        "firstComment": "Link to purchase: https://shop.example.com"
      }
    }
  ]
}
```

**Note:** First comments don't work with Stories.

## Multi-Page Posting

If your Facebook account manages multiple Pages, post to different Pages from the same account connection.

### List Available Pages

```bash
curl -X GET https://getlate.dev/api/v1/accounts/YOUR_ACCOUNT_ID/facebook-page \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### Post to Multiple Pages

Use the same `accountId` multiple times with different `pageId` values:

```json
{
  "content": "Exciting news from all our brands! 🎉",
  "platforms": [
    {
      "platform": "facebook",
      "accountId": "YOUR_ACCOUNT_ID",
      "platformSpecificData": {
        "pageId": "111111111"
      }
    },
    {
      "platform": "facebook",
      "accountId": "YOUR_ACCOUNT_ID",
      "platformSpecificData": {
        "pageId": "222222222"
      }
    }
  ],
  "publishNow": true
}
```

## Page Targeting

Specify which Page to post to:

```json
{
  "platforms": [
    {
      "platform": "facebook",
      "accountId": "acc_123",
      "platformSpecificData": {
        "pageId": "123456789"
      }
    }
  ]
}
```

## GIF Support

Facebook supports animated GIFs with these characteristics:

- Treated as videos internally
- Auto-play in feed
- Max file size: 25 MB recommended
- Loop automatically

## Common Issues

### "Cannot mix media types"

Facebook doesn't allow images and videos in the same post. Create separate posts for each.

### Story has no caption

This is expected behavior. Stories don't display text captions. Add text as an image overlay instead.

### Video processing taking long

Large videos (>100 MB) may take several minutes to process. Use scheduled posts for async processing.

### Image looks cropped

Facebook auto-crops to fit feed. Use recommended aspect ratios (1.91:1, 1:1, or 4:5) for best results.

## Inbox

**Requires Inbox add-on ($1/social set/month)**

Facebook has comprehensive inbox support for DMs, comments, and reviews.

### Direct Messages

| Feature | Supported |
|---------|-----------|
| List conversations | ✅ |
| Fetch messages | ✅ |
| Send text messages | ✅ |
| Send attachments | ✅ (images, videos, audio, files) |
| Quick replies | ✅ (up to 13, Meta quick_replies) |
| Buttons | ✅ (up to 3, generic template) |
| Carousels | ✅ (generic template, up to 10 elements) |
| Message tags | ✅ (4 types) |
| Archive/unarchive | ✅ |

**Message tags:** Use `messagingType: "MESSAGE_TAG"` with one of: `CONFIRMED_EVENT_UPDATE`, `POST_PURCHASE_UPDATE`, `ACCOUNT_UPDATE`, or `HUMAN_AGENT` to send messages outside the 24-hour messaging window.

### Persistent Menu

Manage the persistent menu shown in Facebook Messenger conversations. Max 3 top-level items, max 5 nested items.

See Account Settings for the `GET/PUT/DELETE /v1/accounts/{accountId}/messenger-menu` endpoints.

### Comments

| Feature | Supported |
|---------|-----------|
| List comments on posts | ✅ |
| Reply to comments | ✅ |
| Delete comments | ✅ |
| Like comments | ✅ |
| Hide/unhide comments | ✅ |

### Reviews (Pages)

| Feature | Supported |
|---------|-----------|
| List reviews | ✅ |
| Reply to reviews | ✅ |

## Related API Endpoints

- Connect Facebook Account — OAuth flow
- Create Post — Post creation and scheduling
- Upload Media — Image and video uploads
- Analytics — Post performance metrics
- Messages, Comments, and Reviews
- Account Settings — Persistent menu configuration
